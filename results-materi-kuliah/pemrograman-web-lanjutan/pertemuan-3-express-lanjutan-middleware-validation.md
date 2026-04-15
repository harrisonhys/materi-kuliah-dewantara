# Pertemuan 3: Express.js Lanjutan — Middleware, Validation, Security

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Memahami middleware architecture dan execution flow di Express.js
- Mengimplementasikan custom middleware untuk logging, authentication, error handling
- Melakukan input validation menggunakan Joi dan express-validator
- Menangani file upload dengan multer dan validasi file
- Menerapkan security headers dengan helmet dan CORS
- Mendokumentasikan API dengan Swagger/OpenAPI
- Mengimplementasikan rate limiting dan request throttling

## 2. Pengantar: Hook

Express.js adalah "micro-framework" — dia hanya provide routing dan middleware system. Sisanya tergantung kamu: validation, security, documentation, error handling.

DANA backend menangani jutaan API calls per hari. Kalau validation lemah, attacker bisa kirim data gila. Kalau error handling buruk, crash terjadi di production. Kalau API tidak documented, frontend developer bingung. Kualitas backend adalah **middleware quality**.

Express middleware adalah "gatekeeper" — request masuk melalui middleware chain sebelum sampe ke route handler. Ini adalah tempat logic reusable dikerjakan: logging, auth, rate limiting, validation.

## 3. Konsep Utama

### 3.1 Middleware Architecture

```
Request → [Middleware 1] → [Middleware 2] → [Route Handler] → Response
                ↓               ↓                    ↓
            logging         authentication      business logic
            body parsing    rate limiting       database query
```

**Express Middleware Flow:**

```javascript
app.use(middleware1);      // Runs for EVERY request
app.use('/api', middleware2); // Runs only for /api routes
app.post('/transfer', middleware3, handler); // Runs for this route only

// Handler response (next must NOT be called)
// If handler calls next() anyway, goes to next middleware
```

### 3.2 Built-in vs Custom Middleware

| Middleware | Purpose | Example |
|-----------|---------|---------|
| express.json() | Parse JSON body | `app.use(express.json())` |
| express.urlencoded() | Parse form data | `app.use(express.urlencoded())` |
| express.static() | Serve static files | `app.use(express.static('public'))` |
| Custom logging | Log every request | `app.use((req, res, next) => {...})` |
| Custom auth | Protect routes | `app.use(verifyToken)` |

### 3.3 Error Handling Middleware

```
Normal Middleware: (req, res, next)
Error Middleware: (err, req, res, next) ← 4 parameters!
```

Error middleware harus **didefinisikan di akhir** setelah semua route.

### 3.4 Joi Validation Schema

```javascript
const schema = Joi.object({
    email: Joi.string().email().required(),
    password: Joi.string().min(8).required(),
    phone: Joi.string().pattern(/^08\d{9,11}$/).optional()
});

// Validation returns: { error, value }
const { error, value } = schema.validate(data);
```

## 4. Ilustrasi & Analogi

**Analogi: Airport Security**

```
Passenger arrives at airport
↓
[Middleware 1] Passport check → error? reject
↓
[Middleware 2] Baggage scan → error? reject
↓
[Middleware 3] Metal detector → error? reject
↓
[Handler] Board plane
↓
Passenger in seat
```

Sama seperti express middleware. Setiap middleware bisa `next()` (lanjut) atau reject.

## 5. Contoh Teknis

### 5.1 Custom Middleware

```javascript
// Logging Middleware
const requestLogger = (req, res, next) => {
    const start = Date.now();
    
    res.on('finish', () => {
        const duration = Date.now() - start;
        console.log(`${req.method} ${req.path} → ${res.statusCode} (${duration}ms)`);
    });
    
    next(); // HARUS dipanggil untuk lanjut
};

// Authentication Middleware
const authenticateToken = (req, res, next) => {
    const token = req.headers['authorization']?.split(' ')[1];
    
    if (!token) {
        return res.status(401).json({ error: 'No token' });
    }
    
    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        req.user = decoded;
        next();
    } catch (err) {
        return res.status(403).json({ error: 'Invalid token' });
    }
};

// Rate Limiting Middleware
const rateLimit = (maxRequests = 10, windowMs = 60000) => {
    const clients = new Map();
    
    return (req, res, next) => {
        const clientIp = req.ip;
        const now = Date.now();
        const windowStart = now - windowMs;
        
        // Get client's request times
        let requests = clients.get(clientIp) || [];
        requests = requests.filter(time => time > windowStart);
        
        if (requests.length >= maxRequests) {
            return res.status(429).json({ error: 'Too many requests' });
        }
        
        requests.push(now);
        clients.set(clientIp, requests);
        next();
    };
};

// Apply middleware
app.use(requestLogger);
app.use(express.json());
app.post('/api/transfer', authenticateToken, rateLimit(5, 60000), transferHandler);
```

### 5.2 Input Validation with Joi

```javascript
const Joi = require('joi');

// Define validation schema
const transferSchema = Joi.object({
    fromAccountId: Joi.string().alphanum().length(16).required(),
    toAccountId: Joi.string().alphanum().length(16).required(),
    amount: Joi.number().min(1000).max(100000000).required(),
    description: Joi.string().max(200).optional(),
    pin: Joi.string().length(6).pattern(/^\d+$/).required()
});

// Middleware for validation
const validateTransfer = (req, res, next) => {
    const { error, value } = transferSchema.validate(req.body);
    
    if (error) {
        return res.status(400).json({
            error: 'Validation failed',
            details: error.details.map(d => ({
                field: d.path.join('.'),
                message: d.message
            }))
        });
    }
    
    req.validatedData = value;
    next();
};

// Usage
app.post('/api/transfer', validateTransfer, async (req, res) => {
    const { fromAccountId, toAccountId, amount } = req.validatedData;
    // Process transfer...
});
```

### 5.3 File Upload with Multer

```javascript
const multer = require('multer');
const path = require('path');

// Configure storage
const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'uploads/'); // folder to save files
    },
    filename: (req, file, cb) => {
        const unique = Date.now() + '-' + Math.random().toString(36).substring(7);
        cb(null, unique + path.extname(file.originalname));
    }
});

// File filter (only images)
const fileFilter = (req, file, cb) => {
    const allowedMimes = ['image/jpeg', 'image/png', 'image/webp'];
    
    if (!allowedMimes.includes(file.mimetype)) {
        return cb(new Error('Only JPEG, PNG, WebP allowed'));
    }
    
    if (file.size > 5 * 1024 * 1024) {
        return cb(new Error('File too large (max 5MB)'));
    }
    
    cb(null, true);
};

const upload = multer({
    storage,
    fileFilter,
    limits: { fileSize: 5 * 1024 * 1024 }
});

// Upload endpoint
app.post('/api/profile/photo', authenticateToken, upload.single('photo'), (req, res) => {
    if (!req.file) {
        return res.status(400).json({ error: 'No file uploaded' });
    }
    
    const photoUrl = `/uploads/${req.file.filename}`;
    
    // Update database
    db.query('UPDATE users SET photo_url = ? WHERE id = ?', 
        [photoUrl, req.user.id]);
    
    res.json({ success: true, photoUrl });
});

// Error handling for multer
app.post('/api/profile/photo', 
    authenticateToken, 
    upload.single('photo'),
    (req, res) => { /* ... */ },
    (err, req, res, next) => {
        if (err instanceof multer.MulterError) {
            return res.status(400).json({ error: err.message });
        }
        res.status(500).json({ error: err.message });
    }
);
```

### 5.4 Security Middleware (Helmet, CORS)

```javascript
const helmet = require('helmet');
const cors = require('cors');

// Helmet: Set security headers
app.use(helmet());
// Sets: Content-Security-Policy, X-Frame-Options, X-Content-Type-Options, etc.

// CORS: Allow requests from specific origins
const corsOptions = {
    origin: process.env.FRONTEND_URL || 'http://localhost:3000',
    credentials: true, // allow cookies
    methods: ['GET', 'POST', 'PUT', 'DELETE'],
    allowedHeaders: ['Content-Type', 'Authorization']
};

app.use(cors(corsOptions));

// Custom security headers
app.use((req, res, next) => {
    res.setHeader('X-Content-Type-Options', 'nosniff');
    res.setHeader('X-Frame-Options', 'DENY');
    res.setHeader('X-XSS-Protection', '1; mode=block');
    next();
});
```

### 5.5 Error Handling

```javascript
// Custom Error Class
class ApiError extends Error {
    constructor(statusCode, message) {
        super(message);
        this.statusCode = statusCode;
    }
}

// Route handlers throw errors
app.post('/api/transfer', async (req, res, next) => {
    try {
        const user = await db.getUser(req.user.id);
        
        if (!user) {
            throw new ApiError(404, 'User not found');
        }
        
        if (user.balance < req.body.amount) {
            throw new ApiError(400, 'Insufficient balance');
        }
        
        // Process...
        res.json({ success: true });
    } catch (err) {
        next(err); // Pass to error middleware
    }
});

// Error middleware (MUST be last)
app.use((err, req, res, next) => {
    const statusCode = err.statusCode || 500;
    const message = err.message || 'Internal server error';
    
    console.error(`[${statusCode}] ${message}`);
    
    res.status(statusCode).json({
        error: message,
        ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    });
});
```

### 5.6 Swagger Documentation

```javascript
const swaggerJsdoc = require('swagger-jsdoc');
const swaggerUi = require('swagger-ui-express');

const swaggerOptions = {
    definition: {
        openapi: '3.0.0',
        info: {
            title: 'DANA API',
            version: '1.0.0',
            description: 'Digital wallet API'
        },
        servers: [
            { url: 'http://localhost:3000/api', description: 'Dev' },
            { url: 'https://api.dana.co.id', description: 'Production' }
        ]
    },
    apis: ['./routes/*.js']
};

const swaggerSpec = swaggerJsdoc(swaggerOptions);
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));

// In routes file:
/**
 * @swagger
 * /transfer:
 *   post:
 *     summary: Transfer money
 *     tags: [Transfer]
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             properties:
 *               fromAccountId:
 *                 type: string
 *               toAccountId:
 *                 type: string
 *               amount:
 *                 type: number
 *     responses:
 *       200:
 *         description: Transfer successful
 *       400:
 *         description: Validation error
 */
app.post('/transfer', validateTransfer, transferHandler);
```

## 6. Studi Kasus Nyata: DANA Payment API Architecture

DANA backend API harus handle:
- Millions of requests/day
- Strict validation (invalid data = fraud risk)
- Security (JWT, rate limiting, encryption)
- File upload (KYC documents)
- Detailed documentation (for mobile teams)

```javascript
// src/app.js - Application setup
const express = require('express');
const helmet = require('helmet');
const cors = require('cors');
const swaggerUi = require('swagger-ui-express');
const requestLogger = require('./middleware/logger');
const errorHandler = require('./middleware/errorHandler');

const app = express();

// Security & parsing
app.use(helmet());
app.use(cors({ origin: process.env.FRONTEND_URL }));
app.use(express.json({ limit: '10mb' }));

// Logging & rate limiting
app.use(requestLogger);
app.use('/api/auth/login', rateLimit(5, 900000)); // 5 req per 15 min

// API Documentation
const swaggerSpec = require('./swagger');
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));

// Routes
app.use('/api/auth', require('./routes/auth'));
app.use('/api/wallet', require('./routes/wallet'));
app.use('/api/transfer', require('./routes/transfer'));

// Error handling
app.use(errorHandler);

module.exports = app;

// src/routes/transfer.js
const express = require('express');
const Joi = require('joi');
const router = express.Router();

const transferSchema = Joi.object({
    toPhone: Joi.string().pattern(/^08\d{9,11}$/).required(),
    amount: Joi.number().min(10000).max(5000000).required(),
    pin: Joi.string().length(6).pattern(/^\d+$/).required()
});

const validateTransfer = (req, res, next) => {
    const { error, value } = transferSchema.validate(req.body);
    if (error) {
        return res.status(400).json({ error: error.details[0].message });
    }
    req.validatedData = value;
    next();
};

/**
 * @swagger
 * /transfer:
 *   post:
 *     security:
 *       - bearerAuth: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             properties:
 *               toPhone:
 *                 type: string
 *                 example: "08123456789"
 *               amount:
 *                 type: number
 *               pin:
 *                 type: string
 *     responses:
 *       200:
 *         description: Transfer successful
 */
router.post('/', authenticateToken, validateTransfer, async (req, res, next) => {
    try {
        const { toPhone, amount, pin } = req.validatedData;
        
        // Verify PIN
        const wallet = await db.getWallet(req.user.id);
        const pinMatch = await bcrypt.compare(pin, wallet.pin_hash);
        
        if (!pinMatch) {
            return res.status(401).json({ error: 'Invalid PIN' });
        }
        
        // Process transfer
        const tx = await db.createTransfer({
            from_user_id: req.user.id,
            to_phone: toPhone,
            amount,
            status: 'PENDING'
        });
        
        // Async operations (don't block response)
        notificationService.sendConfirmation(req.user.email, amount);
        auditService.log('TRANSFER', req.user.id, amount);
        
        res.json({ success: true, transactionId: tx.id });
    } catch (err) {
        next(err);
    }
});

module.exports = router;

// src/middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
    console.error(err.stack);
    
    const statusCode = err.statusCode || 500;
    const message = err.message || 'Internal server error';
    
    // Don't leak internal errors to client
    const clientMessage = statusCode === 500 
        ? 'Something went wrong' 
        : message;
    
    res.status(statusCode).json({
        error: clientMessage,
        requestId: req.id // for debugging
    });
};

module.exports = errorHandler;
```

## 7. Visualisasi: Request Lifecycle

```
1. Request arrives
   ↓
2. [helmet] Add security headers
   ↓
3. [cors] Check origin
   ↓
4. [express.json] Parse body
   ↓
5. [requestLogger] Log request
   ↓
6. [rateLimit] Check rate limit
   ↓
7. [authenticateToken] Verify JWT
   ↓
8. [validateTransfer] Validate input
   ↓
9. Route Handler → Business logic
   ↓
10. Response sent
    ↓
11. [errorHandler] (if error thrown)
    ↓
12. Send error response
```

## 8. Kesalahan Umum

### ❌ Middleware not calling next()

```javascript
// ❌ WRONG - request hangs
app.use((req, res, next) => {
    console.log(req.path);
    // forgot next()!
});

// ✅ CORRECT
app.use((req, res, next) => {
    console.log(req.path);
    next(); // MUST call
});
```

### ❌ Validation happening in wrong place

```javascript
// ❌ WRONG - validation loses valid data
app.post('/transfer', (req, res) => {
    const { amount } = req.body;
    if (amount < 0) return res.status(400).json({ error: 'Invalid' });
    // Lots of code here, amount might be undefined later
});

// ✅ CORRECT - validation in middleware, sets req.validatedData
app.post('/transfer', validateMiddleware, (req, res) => {
    const { amount } = req.validatedData; // guaranteed valid
});
```

### ❌ Error middleware not handling all errors

```javascript
// ❌ WRONG - async error not caught
app.post('/transfer', async (req, res) => {
    const result = await someAsyncFn(); // if rejects, not caught!
    res.json(result);
});

// ✅ CORRECT - wrap in try-catch, pass to next()
app.post('/transfer', async (req, res, next) => {
    try {
        const result = await someAsyncFn();
        res.json(result);
    } catch (err) {
        next(err); // pass to error middleware
    }
});
```

### ❌ CORS not configured properly

```javascript
// ❌ WRONG - allows any origin
app.use(cors());

// ✅ CORRECT - restrict to trusted origins
app.use(cors({
    origin: process.env.FRONTEND_URL,
    credentials: true
}));
```

## 9. Latihan & Studi Kasus

### Latihan 1: Custom Auth Middleware
```javascript
// Task: Create middleware that checks if user has 'admin' role

// Expected behavior:
// - If no token → 401
// - If token invalid → 403
// - If user not admin → 403
// - If admin → next()
```

**Solusi:**
```javascript
const adminOnly = (req, res, next) => {
    const token = req.headers.authorization?.split(' ')[1];
    if (!token) return res.status(401).json({ error: 'No token' });
    
    try {
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        if (decoded.role !== 'admin') {
            return res.status(403).json({ error: 'Not admin' });
        }
        req.user = decoded;
        next();
    } catch {
        res.status(403).json({ error: 'Invalid token' });
    }
};

app.delete('/users/:id', adminOnly, (req, res) => {
    // Only admins can reach here
});
```

### Latihan 2: Joi Validation
```javascript
// Task: Validate registration form
// - email: valid email, required
// - password: min 8 chars, required
// - phone: pattern 08XXXXXXXXX, optional
// - agreeTerms: must be true

// If validation fails, return 400 with field-level errors
```

**Solusi:**
```javascript
const registerSchema = Joi.object({
    email: Joi.string().email().required(),
    password: Joi.string().min(8).required(),
    phone: Joi.string().pattern(/^08\d{9,11}$/).optional().allow(''),
    agreeTerms: Joi.boolean().valid(true).required()
});

app.post('/register', (req, res) => {
    const { error, value } = registerSchema.validate(req.body);
    
    if (error) {
        return res.status(400).json({
            errors: error.details.map(d => ({
                field: d.path[0],
                message: d.message
            }))
        });
    }
    
    // Register user...
});
```

### Latihan 3: Error Handling
```javascript
// Task: Create error handler middleware that:
// - Catches all errors from route handlers
// - Logs error to file
// - Returns different response based on status code
// - Hides stack trace from client (unless dev mode)
```

**Solusi:**
```javascript
const errorHandler = (err, req, res, next) => {
    const statusCode = err.statusCode || 500;
    const isDev = process.env.NODE_ENV === 'development';
    
    // Log to file
    fs.appendFileSync('logs/errors.log', 
        `[${new Date().toISOString()}] ${statusCode}: ${err.message}\n`);
    
    const response = {
        error: statusCode === 500 && !isDev 
            ? 'Something went wrong' 
            : err.message
    };
    
    if (isDev) response.stack = err.stack;
    
    res.status(statusCode).json(response);
};

// Must be last!
app.use(errorHandler);
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami middleware execution order
- [ ] Bisa membuat custom middleware (logging, auth, etc)
- [ ] Bisa validate input dengan Joi
- [ ] Bisa handle file upload dengan multer
- [ ] Bisa apply security headers (helmet, CORS)
- [ ] Bisa implement rate limiting
- [ ] Bisa handle errors dengan error middleware
- [ ] Bisa document API dengan Swagger
- [ ] Tahu kapan middleware harus dipanggil (order matters!)
- [ ] Bisa debug middleware issues (request hanging, etc)

## 11. Referensi

- [Express.js Documentation](https://expressjs.com)
- [Helmet.js — Express Security](https://helmetjs.github.io/)
- [Joi Validation](https://joi.dev)
- [Multer — File Upload](https://github.com/expressjs/multer)
- [CORS — MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [Swagger/OpenAPI](https://swagger.io)
- [Express Error Handling](https://expressjs.com/en/guide/error-handling.html)

**Status:** ✅ Pertemuan 3 selesai
