# Pertemuan 5: Autentikasi & Otorisasi — JWT, Password Hashing, RBAC

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Memahami perbedaan autentikasi vs otorisasi
- Mengimplementasikan password hashing dengan bcrypt dan Argon2
- Membuat dan memverifikasi JWT tokens
- Menggunakan refresh tokens untuk extended sessions
- Mengimplementasikan Role-Based Access Control (RBAC)
- Menangani token expiration dan revocation
- Membangun secure authentication flow dari login sampai logout

## 2. Pengantar: Hook

Password plaintext = disaster. Tapi hashing password juga punya cara yang benar dan salah. Di OVO, jika satu developer tidak tahu tentang password salting, jutaan user accounts terancam.

Autentikasi bukan hanya "login/password". Hari ini ada:
- JWT tokens untuk stateless authentication
- Refresh tokens untuk extended sessions tanpa re-login
- RBAC untuk kontrol granular (user vs seller vs admin)
- Token expiration untuk security

Setiap endpoint harus protect dengan middleware authentication. Setiap action harus check authorization (apakah user ini berhak?).

## 3. Konsep Utama

### 3.1 Autentikasi vs Otorisasi

| Aspect | Autentikasi | Otorisasi |
|--------|------------|-----------|
| **Purpose** | Verify WHO you are | Verify WHAT you can do |
| **Question** | "Are you really Budi?" | "Can Budi access /admin/users?" |
| **How** | Username + password + validation | JWT payload + role check |
| **Example** | Login page | Permission middleware |

### 3.2 Password Hashing

```
❌ WRONG:
password = "12345"  → save to DB
User attacks DB → gets plaintext
```

```
✅ RIGHT:
password = "12345"
hashed = bcrypt.hash(password)  → save to DB
User attacks DB → gets hash, cannot reverse
```

### 3.3 JWT Anatomy

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

[HEADER].[PAYLOAD].[SIGNATURE]

Header: { alg: 'HS256', typ: 'JWT' }
Payload: { sub: '123', name: 'John', admin: true, exp: 1716239022 }
Signature: HMAC-SHA256(header.payload, secret)
```

### 3.4 Token Flow

```
Login Request
    ↓
Verify Username + Password (bcrypt compare)
    ↓
Create Access Token (short-lived, 15 min)
Create Refresh Token (long-lived, 7 days)
    ↓
Return both tokens to client
    ↓
Client stores:
  - Access Token: in memory (safer)
  - Refresh Token: in httpOnly cookie (secure)
    ↓
Protected request:
  - Attach Access Token in header
  - Server verifies JWT
    ↓
Access Token expired?
  - Use Refresh Token to get new Access Token
    ↓
Logout:
  - Invalidate Refresh Token (remove from DB)
```

## 4. Ilustrasi & Analogi

**Analogi: Hotel Key Card**

- **Password (login):** Your ID and PIN to get the key card
- **Access Token:** The key card itself (short-lived, expires in 15 min)
- **Refresh Token:** The receipt showing you paid for 7 days (get new card anytime)
- **RBAC:** Key card only opens rooms on floor 3 (role-based)

## 5. Contoh Teknis

### 5.1 Password Hashing with Bcrypt

```javascript
const bcrypt = require('bcrypt');

// Hashing password during registration
async function registerUser(email, password) {
    // Hash dengan 12 rounds (more rounds = more secure but slower)
    const hashedPassword = await bcrypt.hash(password, 12);
    
    // Save to database
    const user = await User.create({
        email,
        passwordHash: hashedPassword
    });
    
    return user;
}

// Verifying password during login
async function loginUser(email, password) {
    const user = await User.findOne({ where: { email } });
    
    if (!user) {
        throw new Error('User not found');
    }
    
    // Compare plaintext password with hash
    const isPasswordCorrect = await bcrypt.compare(password, user.passwordHash);
    
    if (!isPasswordCorrect) {
        throw new Error('Invalid password');
    }
    
    return user;
}

// Complexity benchmark:
// rounds: 8 → 10ms
// rounds: 12 → 250ms (recommended)
// rounds: 15 → 1 second (overkill)
```

### 5.2 JWT Token Generation & Verification

```javascript
const jwt = require('jsonwebtoken');

// Generate Access Token (short-lived)
function generateAccessToken(userId, role) {
    return jwt.sign(
        {
            sub: userId,        // subject (user ID)
            role: role,
            type: 'access'
        },
        process.env.JWT_SECRET,
        { expiresIn: '15m' }    // expires in 15 minutes
    );
}

// Generate Refresh Token (long-lived)
function generateRefreshToken(userId) {
    return jwt.sign(
        {
            sub: userId,
            type: 'refresh'
        },
        process.env.JWT_REFRESH_SECRET,
        { expiresIn: '7d' }     // expires in 7 days
    );
}

// Verify & decode token
function verifyAccessToken(token) {
    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        return decoded;
    } catch (err) {
        if (err.name === 'TokenExpiredError') {
            throw new Error('Token expired');
        } else if (err.name === 'JsonWebTokenError') {
            throw new Error('Invalid token');
        }
        throw err;
    }
}

// Complete login flow
async function login(email, password) {
    // 1. Verify credentials
    const user = await User.findOne({ where: { email } });
    const isValid = await bcrypt.compare(password, user.passwordHash);
    
    if (!isValid) {
        throw new Error('Invalid credentials');
    }
    
    // 2. Generate tokens
    const accessToken = generateAccessToken(user.id, user.role);
    const refreshToken = generateRefreshToken(user.id);
    
    // 3. Save refresh token to DB
    await RefreshToken.create({
        userId: user.id,
        token: refreshToken,
        expiresAt: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)
    });
    
    return { accessToken, refreshToken };
}

// Token refresh flow
async function refreshAccessToken(refreshToken) {
    // 1. Verify refresh token
    const decoded = jwt.verify(refreshToken, process.env.JWT_REFRESH_SECRET);
    
    // 2. Check if token exists in DB
    const tokenRecord = await RefreshToken.findOne({
        where: { token: refreshToken }
    });
    
    if (!tokenRecord) {
        throw new Error('Refresh token not found or revoked');
    }
    
    // 3. Generate new access token
    const newAccessToken = generateAccessToken(
        decoded.sub,
        tokenRecord.user.role
    );
    
    return { accessToken: newAccessToken };
}
```

### 5.3 Authentication Middleware

```javascript
// Middleware to verify JWT token
const authenticateToken = (req, res, next) => {
    // Get token from Authorization header
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1]; // "Bearer token"
    
    if (!token) {
        return res.status(401).json({ error: 'No token provided' });
    }
    
    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        
        // Check token type
        if (decoded.type !== 'access') {
            throw new Error('Wrong token type');
        }
        
        // Attach user to request
        req.user = decoded;
        next();
    } catch (err) {
        return res.status(403).json({ error: 'Invalid or expired token' });
    }
};

// Usage
app.post('/api/transfer', authenticateToken, transferHandler);
```

### 5.4 Role-Based Access Control (RBAC)

```javascript
// Authorization middleware for specific roles
const authorizeRole = (...allowedRoles) => {
    return (req, res, next) => {
        if (!req.user) {
            return res.status(401).json({ error: 'Not authenticated' });
        }
        
        if (!allowedRoles.includes(req.user.role)) {
            return res.status(403).json({ 
                error: 'Insufficient permissions',
                required: allowedRoles,
                current: req.user.role
            });
        }
        
        next();
    };
};

// Specific role middlewares
const adminOnly = authorizeRole('admin');
const sellerOrAdmin = authorizeRole('seller', 'admin');
const buyerOrHigher = authorizeRole('buyer', 'seller', 'admin');

// Usage
app.delete('/users/:id', authenticateToken, adminOnly, deleteUserHandler);
app.post('/products', authenticateToken, sellerOrAdmin, createProductHandler);
app.get('/orders', authenticateToken, buyerOrHigher, getOrdersHandler);
```

### 5.5 Permission-Based Access (more granular)

```javascript
// Define permissions by role
const permissions = {
    admin: ['user:read', 'user:write', 'user:delete', 'product:write'],
    seller: ['product:write', 'order:read'],
    buyer: ['product:read', 'order:read']
};

// Middleware for specific permissions
const authorize = (requiredPermission) => {
    return (req, res, next) => {
        if (!req.user) {
            return res.status(401).json({ error: 'Not authenticated' });
        }
        
        const userPermissions = permissions[req.user.role] || [];
        
        if (!userPermissions.includes(requiredPermission)) {
            return res.status(403).json({ error: 'Permission denied' });
        }
        
        next();
    };
};

// Usage
app.delete('/users/:id', authenticateToken, authorize('user:delete'), handler);
app.post('/products', authenticateToken, authorize('product:write'), handler);
```

### 5.6 Logout & Token Revocation

```javascript
// Logout: invalidate refresh token
app.post('/logout', authenticateToken, async (req, res) => {
    try {
        // Remove refresh token from DB
        await RefreshToken.destroy({
            where: { userId: req.user.sub }
        });
        
        res.json({ success: true, message: 'Logged out' });
    } catch (err) {
        res.status(500).json({ error: err.message });
    }
});

// Optional: Blacklist access tokens (for immediate logout)
const tokenBlacklist = new Set();

const logout = async (req, res) => {
    const token = req.headers['authorization'].split(' ')[1];
    
    // Add to blacklist
    tokenBlacklist.add(token);
    
    // Also remove refresh token
    await RefreshToken.destroy({ where: { userId: req.user.sub } });
    
    res.json({ success: true });
};

// Check blacklist in auth middleware
const authenticateToken = (req, res, next) => {
    const token = req.headers['authorization']?.split(' ')[1];
    
    if (tokenBlacklist.has(token)) {
        return res.status(401).json({ error: 'Token has been revoked' });
    }
    
    // ... rest of verification
};
```

## 6. Studi Kasus Nyata: OVO Multi-Account Security

OVO users dapat memiliki multiple linked accounts. System harus:
- Authenticate per-device
- Allow account switching tanpa re-login
- Maintain separate sessions per device
- Logout dari satu device tidak affect devices lain

```javascript
// models/Session.js
const Session = sequelize.define('Session', {
    userId: DataTypes.INTEGER,
    deviceId: DataTypes.STRING(100), // unique device identifier
    refreshToken: DataTypes.STRING(500),
    userAgent: DataTypes.STRING(500),
    ipAddress: DataTypes.STRING(50),
    expiresAt: DataTypes.DATE,
    isActive: { type: DataTypes.BOOLEAN, defaultValue: true }
}, { timestamps: true });

// services/AuthService.js
class AuthService {
    async login(email, password, deviceId, userAgent, ipAddress) {
        // 1. Verify credentials
        const user = await User.findOne({ where: { email } });
        const isValid = await bcrypt.compare(password, user.passwordHash);
        
        if (!isValid) throw new Error('Invalid credentials');
        
        // 2. Create session
        const accessToken = jwt.sign(
            { sub: user.id, deviceId, role: user.role },
            process.env.JWT_SECRET,
            { expiresIn: '15m' }
        );
        
        const refreshToken = jwt.sign(
            { sub: user.id, deviceId, type: 'refresh' },
            process.env.JWT_REFRESH_SECRET,
            { expiresIn: '7d' }
        );
        
        // 3. Save to database
        await Session.create({
            userId: user.id,
            deviceId,
            refreshToken,
            userAgent,
            ipAddress,
            expiresAt: new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)
        });
        
        return { accessToken, refreshToken, userId: user.id };
    }
    
    async logoutDevice(userId, deviceId) {
        // Invalidate session for specific device
        await Session.update(
            { isActive: false },
            { where: { userId, deviceId } }
        );
    }
    
    async logoutAllDevices(userId) {
        // Invalidate all sessions
        await Session.update(
            { isActive: false },
            { where: { userId } }
        );
    }
    
    async getSessions(userId) {
        return await Session.findAll({
            where: { userId, isActive: true },
            attributes: ['deviceId', 'userAgent', 'ipAddress', 'createdAt']
        });
    }
}

// routes/auth.js
router.post('/login', async (req, res, next) => {
    try {
        const { email, password } = req.body;
        const deviceId = req.headers['x-device-id'];
        const userAgent = req.headers['user-agent'];
        const ipAddress = req.ip;
        
        const tokens = await authService.login(
            email, password, deviceId, userAgent, ipAddress
        );
        
        // Set refresh token in httpOnly cookie
        res.cookie('refreshToken', tokens.refreshToken, {
            httpOnly: true,
            secure: process.env.NODE_ENV === 'production',
            maxAge: 7 * 24 * 60 * 60 * 1000
        });
        
        res.json({
            accessToken: tokens.accessToken,
            userId: tokens.userId
        });
    } catch (err) {
        next(err);
    }
});

router.post('/logout/device', authenticateToken, async (req, res) => {
    await authService.logoutDevice(req.user.sub, req.user.deviceId);
    res.json({ success: true });
});

router.post('/logout/all', authenticateToken, async (req, res) => {
    await authService.logoutAllDevices(req.user.sub);
    res.json({ success: true });
});
```

## 7. Visualisasi: Auth Flow Diagram

```
Login Request
    ↓
[Verify Email + Password]
    ↓ ✓ Valid
[Generate Access Token (15m)]
[Generate Refresh Token (7d)]
    ↓
[Save Refresh Token to DB]
    ↓
Return tokens
    ↓
Client stores:
├─ Access Token → in memory
└─ Refresh Token → httpOnly cookie
    ↓
Protected Request
├─ Attach Access Token
    ↓
[Verify Token]
├─ Not expired? → Allow
├─ Expired? → Request new token
    ↓
[Use Refresh Token]
    ↓
[Check if exists in DB]
    ↓
[Generate new Access Token]
    ↓
Continue request
```

## 8. Kesalahan Umum

### ❌ Storing plaintext passwords

```javascript
// ❌ WRONG
const user = await User.create({
    email: 'budi@tok.com',
    password: 'secretpassword'  // NEVER store plaintext!
});

// ✅ CORRECT
const hashedPassword = await bcrypt.hash(password, 12);
const user = await User.create({
    email: 'budi@tok.com',
    passwordHash: hashedPassword
});
```

### ❌ Token in URL

```javascript
// ❌ WRONG - Token visible in URL history
GET /api/data?token=eyJhbGc...

// ✅ CORRECT - Token in Authorization header
GET /api/data
Authorization: Bearer eyJhbGc...
```

### ❌ Long-lived access tokens

```javascript
// ❌ WRONG - if token stolen, attacker has access for days
jwt.sign(payload, secret, { expiresIn: '30d' });

// ✅ CORRECT - short-lived, rotate with refresh
jwt.sign(payload, secret, { expiresIn: '15m' }); // access
jwt.sign(payload, secret, { expiresIn: '7d' }); // refresh
```

### ❌ Storing tokens in localStorage

```javascript
// ❌ RISKY - vulnerable to XSS
localStorage.setItem('token', accessToken);

// ✅ SAFER - httpOnly cookie (cannot access via JS)
res.cookie('token', accessToken, { httpOnly: true });

// BEST - In-memory for access, httpOnly for refresh
// Access token: stored in memory variable
// Refresh token: httpOnly cookie
```

## 9. Latihan & Studi Kasus

### Latihan 1: Password Hashing
```javascript
// Task: Create register and login functions
// - Hash password with bcrypt
// - Store in database
// - On login, compare with hash

async function register(email, password) {
    // TODO: hash password and save
}

async function login(email, password) {
    // TODO: verify password and return user
}
```

**Solusi:**
```javascript
async function register(email, password) {
    const hash = await bcrypt.hash(password, 12);
    return await User.create({ email, passwordHash: hash });
}

async function login(email, password) {
    const user = await User.findOne({ where: { email } });
    const valid = await bcrypt.compare(password, user.passwordHash);
    if (!valid) throw new Error('Invalid password');
    return user;
}
```

### Latihan 2: JWT Token Generation
```javascript
// Task: Create tokens for user ID 123, role 'seller'
// - Access token: expires in 15 minutes
// - Refresh token: expires in 7 days
```

**Solusi:**
```javascript
const accessToken = jwt.sign(
    { sub: 123, role: 'seller' },
    process.env.JWT_SECRET,
    { expiresIn: '15m' }
);

const refreshToken = jwt.sign(
    { sub: 123, type: 'refresh' },
    process.env.JWT_REFRESH_SECRET,
    { expiresIn: '7d' }
);
```

### Latihan 3: RBAC Middleware
```javascript
// Task: Create middleware that allows only admins and sellers

function authorizeSellers() {
    return (req, res, next) => {
        // Check if user is authenticated
        // Check if user role is seller or admin
        // If not, return 403
        // If yes, next()
    }
}
```

**Solusi:**
```javascript
function authorizeSellers() {
    return (req, res, next) => {
        if (!req.user) return res.status(401).json({ error: 'Not auth' });
        if (!['seller', 'admin'].includes(req.user.role)) {
            return res.status(403).json({ error: 'Not seller' });
        }
        next();
    };
}
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami perbedaan autentikasi vs otorisasi
- [ ] Bisa hash dan verify passwords dengan bcrypt
- [ ] Bisa generate dan verify JWT tokens
- [ ] Memahami token expiration dan refresh
- [ ] Bisa implement authentication middleware
- [ ] Bisa implement RBAC (role-based access)
- [ ] Bisa implement permission-based access
- [ ] Mengerti token storage best practices
- [ ] Bisa handle logout dan token revocation
- [ ] Bisa implement multi-device sessions

## 11. Referensi

- [JWT Introduction](https://jwt.io)
- [bcryptjs Documentation](https://github.com/dcodeIO/bcrypt.js)
- [jsonwebtoken NPM](https://www.npmjs.com/package/jsonwebtoken)
- [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
- [RFC 7519: JWT](https://tools.ietf.org/html/rfc7519)

**Status:** ✅ Pertemuan 5 selesai
