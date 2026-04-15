# Pertemuan 16: Review & Persiapan UAS — Pemrograman Web Lanjutan

## 1. Learning Outcomes & Course Recap

Mahasiswa yang telah menyelesaikan course ini mampu:

### Modern JavaScript & Async (P2)
- ✅ ES6+ features (arrow functions, destructuring, spread)
- ✅ Promises dan async/await
- ✅ Promise.all, Promise.race, Promise.allSettled
- ✅ Event loop dan microtask queue

### Express.js Backend (P3)
- ✅ Middleware architecture dan execution flow
- ✅ Request/response validation dengan Joi
- ✅ File upload dengan multer
- ✅ Security headers (helmet, CORS)
- ✅ Error handling middleware
- ✅ API documentation dengan Swagger

### Database & ORM (P4)
- ✅ Database relationships (1-to-many, many-to-many)
- ✅ Sequelize/Prisma untuk ORM
- ✅ Migrations dan schema management
- ✅ Eager loading dan N+1 prevention
- ✅ Query optimization dengan indexes

### Authentication & Authorization (P5)
- ✅ Password hashing (bcrypt, Argon2)
- ✅ JWT token strategy
- ✅ Refresh token rotation
- ✅ Role-Based Access Control (RBAC)
- ✅ Multi-device session management

### Vue.js Frontend (P6-P8)
- ✅ Composition API (ref, reactive, computed, watch)
- ✅ Component lifecycle hooks
- ✅ Props, emits, v-model
- ✅ Form handling dan validation
- ✅ Conditional rendering dan lists

### State Management & Routing (P7)
- ✅ Vue Router dengan nested routes
- ✅ Route guards dan lazy loading
- ✅ Pinia stores (state, getters, actions)
- ✅ Cross-component state sharing

### API Integration (P9)
- ✅ Axios setup dan interceptors
- ✅ Request/response transformation
- ✅ Token refresh dan error recovery
- ✅ Retry logic dengan exponential backoff
- ✅ File upload progress tracking

### Testing (P10)
- ✅ Jest unit tests
- ✅ Vue Test Utils component testing
- ✅ Mocking dan fixtures
- ✅ Code coverage measurement

### Advanced Features (P11-P13)
- ✅ Pagination (offset-based dan cursor-based)
- ✅ File upload dan cloud storage (S3)
- ✅ Real-time features dengan Socket.io
- ✅ WebSocket events dan rooms

### Production (P14)
- ✅ Code splitting dan lazy loading
- ✅ Performance optimization
- ✅ Docker containerization
- ✅ Deployment strategies (Vercel, Railway)
- ✅ Monitoring dan logging

## 2. Concept Map — Full-Stack Development

```
FRONTEND (Vue.js 3)
├── Components
│   ├── Composition API (setup, hooks)
│   ├── Template directives (v-if, v-for, v-model)
│   └── Props, emits, slots
├── State Management
│   ├── Reactive state (ref, reactive)
│   ├── Computed properties
│   ├── Watchers
│   └── Pinia stores
├── Routing
│   ├── Vue Router
│   ├── Dynamic routes
│   ├── Route guards
│   └── Lazy loading
├── API Integration
│   ├── Axios client
│   ├── Interceptors
│   ├── Error handling
│   └── Request retry
└── Styling & UX
    ├── Tailwind CSS
    ├── Responsive design
    ├── Form validation
    └── Loading states

BACKEND (Express.js)
├── Routing & Middleware
│   ├── Route handlers
│   ├── Middleware pipeline
│   ├── Error handling
│   └── Validation (Joi)
├── Authentication
│   ├── Password hashing
│   ├── JWT tokens
│   ├── Token refresh
│   └── RBAC
├── Database
│   ├── Models & associations
│   ├── Migrations
│   ├── Query optimization
│   └── Transactions
├── File Handling
│   ├── Multer upload
│   ├── AWS S3 storage
│   └── Virus scanning
└── Real-time
    ├── Socket.io
    ├── Events & rooms
    └── Broadcasting

DATABASE (SQL)
├── Schema Design
│   ├── Normalization
│   ├── Relationships
│   └── Constraints
├── Optimization
│   ├── Indexes
│   ├── Query plans
│   └── Connection pooling
├── Data Integrity
│   ├── Transactions
│   ├── Locks
│   └── Backups
└── Security
    ├── Encryption
    ├── Role permissions
    └── Audit logging
```

## 3. Quick Reference Tables

### Framework Comparison

| Framework | Purpose | Complexity | When Use |
|-----------|---------|-----------|----------|
| Vue.js | Frontend UI | Low-Medium | Building interactive UIs |
| Express.js | Backend API | Low-Medium | REST APIs, web servers |
| Sequelize | ORM | Medium | Database abstraction |
| Socket.io | Real-time | Medium | Chat, notifications |
| Pinia | State mgmt | Low | Global state sharing |
| Jest | Testing | Low | Unit & component tests |

### Common Patterns

| Pattern | Use Case | Example |
|---------|----------|---------|
| **Composition API** | Reusable logic | useAuth, useForm |
| **Middleware** | Cross-cutting concern | logging, validation |
| **Interceptor** | Request transform | add token to header |
| **Observer** | Event emission | Socket.io rooms |
| **Repository** | Data access | UserRepository |
| **Service** | Business logic | PaymentService |

### Performance Checklist

| Aspect | Target | How |
|--------|--------|-----|
| Bundle size | <1.5 MB | Code splitting, tree-shaking |
| LCP | <2.5 sec | Lazy load, optimize images |
| Time to interactive | <3.5 sec | Minimize blocking JS |
| Database queries | <100ms | Indexes, eager loading |
| API response | <200ms | Caching, CDN |

## 4. UAS Question Types & Preparation

### Tipe 1: Conceptual Questions (20%)

```
Example:
1. Jelaskan perbedaan HTTP polling vs WebSocket. 
   Kapan menggunakan masing-masing?

2. Mengapa async/await lebih baik dari callback? 
   Berikan contoh code.

3. Apa itu N+1 problem dalam database? 
   Bagaimana solusinya?

Persiapan:
- Pahami fundamental concepts
- Bersiaplah untuk membandingkan
- Siap memberikan contoh konkret
```

### Tipe 2: Implementation Questions (40%)

```
Example:
1. Implement login endpoint yang:
   - Validate email & password
   - Hash password dengan bcrypt
   - Return JWT token
   - Handle errors

2. Implement Vue component dengan:
   - Form submission
   - Error display
   - Loading state
   - API integration

Persiapan:
- Siap code dari scratch
- Tested, runnable code
- Handle edge cases
- Proper error handling
```

### Tipe 3: Problem Solving (25%)

```
Example:
Diberikan problem:
"User complains checkout is slow (3 seconds).
Diagnose dan propose solution."

Atau:
"Design authentication for mobile app 
dengan 1M users."

Persiapan:
- Think like architect
- Identify bottlenecks
- Propose scalable solutions
- Trade-offs analysis
```

### Tipe 4: Code Review (15%)

```
Example:
Review code yang diberikan.
Identifikasi:
- Code smells
- Security issues
- Performance problems
- Improvements

Persiapan:
- Hafal SOLID principles
- Tahu common pitfalls
- Bisa explain WHY
```

## 5. Mock Exam (180 minutes)

### Section A: Theory (60 minutes, 30 points)

```
1. (10 pts) Jelaskan lifecycle komponen Vue.js.
   Kapan setup(), onMounted(), onUnmounted() dijalankan?

2. (10 pts) Design database schema untuk e-commerce.
   Tentukan tabel, relationships, dan indexes.

3. (10 pts) Implement middleware authentication di Express.js.
   Code harus handle JWT verification dan error cases.
```

### Section B: Implementation (90 minutes, 50 points)

```
1. (25 pts) Backend: User registration endpoint
   - Accept email, password, name
   - Validate input (Joi)
   - Hash password (bcrypt)
   - Save to database
   - Return JWT token
   - Handle errors

2. (25 pts) Frontend: Login form component
   - Email & password inputs
   - Form validation
   - Show loading state
   - Display error messages
   - Redirect on success
   - Integrate with API
```

### Section C: Architecture (30 minutes, 20 points)

```
Problem:
"Build real-time notification system untuk 
e-commerce platform dengan 1M users.
Notification types:
- Order status change
- Payment confirmation
- New messages
- Price drop alerts"

Deliverables:
- Architecture diagram
- Technology choices (Socket.io? Redis? etc)
- Scalability considerations
- Code snippet untuk key component
```

## 6. Study Tips

### Do's ✅

- [ ] Code dari scratch 3+ kali
- [ ] Pahami concept, jangan hafal
- [ ] Test code kamu (run it!)
- [ ] Trace manual untuk verify
- [ ] Review teman punya code
- [ ] Solve past year problems
- [ ] Pahami error messages
- [ ] Know when to use which tool
- [ ] Practice time management
- [ ] Ask clarifying questions

### Don'ts ❌

- [ ] Hafal syntax tanpa paham
- [ ] Copy-paste dari tutorial
- [ ] Only understand "happy path"
- [ ] Skip error handling
- [ ] Ignore edge cases
- [ ] Stay up all night before exam
- [ ] Panic jika stuck (keep calm!)
- [ ] Use deprecated methods
- [ ] Write untested code
- [ ] Ignore performance

## 7. Common Mistakes & How to Avoid

| Mistake | Impact | Fix |
|---------|--------|-----|
| **Not validating input** | Security risk | Always validate on server |
| **Hardcoded credentials** | Breach | Use environment variables |
| **No error handling** | Crash | Try-catch, error middleware |
| **N+1 queries** | Slow | Eager load relationships |
| **No tests** | Regressions | Aim for 80%+ coverage |
| **Weak passwords** | Hacked | Use bcrypt, Argon2 |
| **Plaintext passwords** | Disaster | Hash everything |
| **No pagination** | Crash | Always paginate |
| **Local file storage** | Unscalable | Use S3 |
| **Missing indexes** | Slow queries | Profile & index |

## 8. Last-Minute Checklist (1 hari sebelum)

### Pahami Ini (jangan hafal):

- [ ] Async/await flow dan event loop
- [ ] Middleware execution order
- [ ] Database relationships
- [ ] JWT token lifecycle
- [ ] Component lifecycle
- [ ] State mutation vs action
- [ ] Request/response flow
- [ ] Error handling strategy
- [ ] Pagination math
- [ ] Security best practices

### Bisa Coding Ini:

- [ ] Login endpoint (validation, hashing, JWT)
- [ ] Protected route middleware
- [ ] Vue component dengan forms
- [ ] Database query dengan association
- [ ] API client dengan interceptors
- [ ] Error handling (try-catch)
- [ ] Input validation (Joi)
- [ ] Real-time event (Socket.io)
- [ ] File upload
- [ ] Pagination

### Tahu Ini:

- [ ] Complexity comparison table
- [ ] Technology selection guide
- [ ] SOLID principles list
- [ ] Security checklist
- [ ] Performance metrics
- [ ] Best practices

## 9. Exam Strategy

1. **Baca semua soal dulu** (10 min)
   - Pahami scope
   - Identifikasi difficulty
   - Alokasikan waktu

2. **Mulai dari yang familiar** (build confidence)
   - Solve easier problems first
   - Get momentum

3. **Code dengan teliti**
   - Trace manually sebelum run
   - Test edge cases
   - Handle errors

4. **Explain your thinking**
   - Write comments
   - Explain WHY, not just WHAT
   - Show understanding

5. **Manage waktu**
   - 60% theory, 40% implementation
   - Keep track of time
   - Don't get stuck

6. **Double-check sebelum submit**
   - Syntax errors?
   - Logic errors?
   - Edge cases?

## 10. If You Get Stuck

| Situation | Action |
|-----------|--------|
| **Lupa syntax** | Write pseudo-code dulu, then code |
| **Code error** | Trace dengan contoh kecil |
| **Lupa concept** | Write apa yang kamu tahu, get partial credit |
| **Blank mind** | Deep breath, start with simplest case |
| **Time running out** | Submit apa yang ada, explain di comment |
| **Not sure** | Ask instructor untuk clarification |

## 11. Final Words

> "The secret of getting ahead is getting started." — Mark Twain

Ini bukan hanya exam. Ini adalah culmination dari skills yang akan kamu gunakan dalam karir.

**Key mindset:**
- Fundamentals > fancy syntax
- Understanding > memorization  
- Testing > assumptions
- Performance > premature optimization
- Security > convenience
- User > developer comfort

**Remember:**
- You've learned a lot this semester
- Real-world applications use these exact patterns
- Your first few projects will be rough — that's normal
- Debugging is 90% of development
- Google is your friend (and will be forever)
- Ask questions, read errors, experiment

**Good practices untuk career:**
1. Write code that's easy to understand (future you will thank present you)
2. Test your code (confidence to refactor)
3. Read others' code (learn patterns)
4. Document as you go (no time later)
5. Ask for code reviews (grow faster)
6. Deploy early, often (learn real problems)
7. Monitor in production (understand users)
8. Never stop learning (this tech changes fast)

---

## 12. Resources untuk Review

**Quick References:**
- [MDN Web Docs](https://developer.mozilla.org) — JS fundamentals
- [Vue.js Docs](https://vuejs.org) — framework reference
- [Express Guide](https://expressjs.com) — backend patterns
- [HTTP Status Codes](https://httpwg.org/specs/rfc7231.html) — when to use which

**Practice:**
- LeetCode (algorithm practice)
- HackerRank (problem solving)
- Codewars (daily katas)
- Your own projects (best learning)

**Visualization:**
- [Visualgo.net](https://visualgo.net) — algorithm visualization
- [db-fiddle.com](https://db-fiddle.com) — test SQL queries
- [excalidraw.com](https://excalidraw.com) — architecture diagrams

---

**Sukses untuk UAS! Kalian semua punya capability untuk pass dengan baik. Trust the process.** 🚀

---

**Status:** ✅ Pertemuan 16 selesai — SELURUH MATERI MU5602 LENGKAP
