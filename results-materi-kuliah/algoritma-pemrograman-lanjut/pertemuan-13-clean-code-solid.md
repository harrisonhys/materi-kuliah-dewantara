# Pertemuan 13: Clean Code & SOLID Principles

## 1. Learning Outcomes
Setelah perkuliahan ini, mahasiswa mampu:
- Menerapkan prinsip Clean Code untuk readability dan maintainability
- Memahami dan mengimplementasikan 5 SOLID principles
- Mengidentifikasi code smells dan refactor dengan aman
- Menulis kode yang mudah dipahami dan dimodifikasi

## 2. Pengantar: Hook

Kode yang bekerja ≠ kode yang baik. Di Tokopedia, senior engineer bisa menulis fitur dalam 2 jam atau 2 hari — tergantung apakah menerapkan Clean Code.

**Kode buruk:** "Code that works today but breaks tomorrow"
**Kode baik:** "Code written so the next person (or future you) understands immediately"

SOLID principles — akronim dari 5 prinsip desain — adalah peta jalan untuk kode yang fleksibel, testable, dan maintainable.

## 3. Clean Code Prinsip Utama

### 3.1 Naming Conventions

```javascript
// ❌ SALAH
const a = [1, 2, 3, 4, 5];
const b = a.filter(x => x > 2).map(x => x * 2);

// ✅ BENAR
const numbers = [1, 2, 3, 4, 5];
const doubledLargeNumbers = numbers
    .filter(num => num > 2)
    .map(num => num * 2);

// ❌ Ambiguous
const proc = (d) => { return d.map(x => x.a); };

// ✅ Clear intent
const extractUserIds = (users) => users.map(user => user.id);
```

### 3.2 Function Length & Complexity

```javascript
// ❌ 50-line god function
function processOrder(order) {
    if (!order) return null;
    let total = 0;
    // ... 45 lines of logic ...
    return total;
}

// ✅ Decomposed, single responsibility
function calculateOrderTotal(order) {
    return order.items.reduce((sum, item) => sum + item.price, 0);
}

function applyDiscount(total, discountRate) {
    return total * (1 - discountRate);
}

function addTax(subtotal, taxRate) {
    return subtotal * (1 + taxRate);
}
```

### 3.3 Comments

```javascript
// ❌ Obvious comments (noise)
const user = { name: 'Budi' }; // Create user object
const age = calculateAge(birthDate); // Calculate age from birth date

// ✅ Necessary comments (WHY, not WHAT)
// Discount limited to 20% due to company policy (Finance Dept)
const MAX_DISCOUNT_RATE = 0.20;

// Fetch from cache first to avoid unnecessary DB calls
// See: JIRA-1234 (Performance bottleneck)
const user = cache.get(userId) || db.getUser(userId);
```

## 4. SOLID Principles

### 4.1 S — Single Responsibility

**Setiap class punya ONE reason to change.**

```javascript
// ❌ Multiple responsibilities
class User {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }
    
    save() { /* DB logic */ }
    sendWelcomeEmail() { /* Email logic */ }
    validateEmail() { /* Validation logic */ }
}

// ✅ Separated responsibilities
class User {
    constructor(name, email) { this.name = name; this.email = email; }
}

class UserRepository {
    save(user) { /* DB logic */ }
}

class EmailService {
    sendWelcome(user) { /* Email logic */ }
}

class EmailValidator {
    validate(email) { /* Validation */ }
}
```

### 4.2 O — Open/Closed

**Open for extension, closed for modification.**

```javascript
// ❌ Closed for extension (perlu ubah class setiap tambah payment method)
class PaymentProcessor {
    process(payment) {
        if (payment.method === 'credit_card') { /* CC logic */ }
        else if (payment.method === 'gopay') { /* GoPay logic */ }
        // Tambah provider? Ubah class ini!
    }
}

// ✅ Open for extension (tambah provider = add class, tidak ubah existing)
class PaymentStrategy {
    process(amount) { throw new Error('Abstract'); }
}

class CreditCardStrategy extends PaymentStrategy {
    process(amount) { /* CC logic */ }
}

class GoPayStrategy extends PaymentStrategy {
    process(amount) { /* GoPay logic */ }
}

class PaymentProcessor {
    constructor(strategy) { this.strategy = strategy; }
    process(amount) { return this.strategy.process(amount); }
}
```

### 4.3 L — Liskov Substitution

**Subclass bisa replace parent class tanpa break behavior.**

```javascript
// ❌ Violates LSP
class Bird {
    fly() { return 'flying'; }
}

class Penguin extends Bird {
    fly() { throw new Error('Penguins cannot fly!'); } // breaks contract
}

// ✅ Respects LSP
class Bird {
    move() { return 'moving'; } // general contract
}

class Sparrow extends Bird {
    move() { return 'flying'; }
}

class Penguin extends Bird {
    move() { return 'swimming'; }
}

// Setiap subclass bisa implement move() tanpa surprises
```

### 4.4 I — Interface Segregation

**Client tidak boleh depend on interface yang tidak digunakan.**

```javascript
// ❌ Fat interface
interface Worker {
    work(): void;
    eat(): void;
    sleep(): void;
}

class Robot implements Worker {
    work() { /* ok */ }
    eat() { throw new Error('Robots don\'t eat'); } // forced to implement
    sleep() { throw new Error('Robots don\'t sleep'); }
}

// ✅ Segregated interfaces
interface Worker {
    work(): void;
}

interface Mortal {
    eat(): void;
    sleep(): void;
}

class Human implements Worker, Mortal {
    work() { /* ok */ }
    eat() { /* ok */ }
    sleep() { /* ok */ }
}

class Robot implements Worker {
    work() { /* ok */ }
    // tidak perlu implement eat/sleep
}
```

### 4.5 D — Dependency Inversion

**Depend on abstractions, not concretions.**

```javascript
// ❌ Tightly coupled
class OrderService {
    constructor() {
        this.paymentGateway = new MidtransGateway(); // hardcoded!
    }
    
    pay(amount) {
        return this.paymentGateway.charge(amount);
    }
}

// ✅ Loosely coupled
class OrderService {
    constructor(paymentGateway) {
        this.paymentGateway = paymentGateway; // injected!
    }
    
    pay(amount) {
        return this.paymentGateway.charge(amount);
    }
}

// Usage
const service1 = new OrderService(new MidtransGateway());
const service2 = new OrderService(new StripeGateway()); // swap easily!
```

## 5. Code Smells & Refactoring

### Common Code Smells

| Smell | Gejala | Fix |
|-------|--------|-----|
| **Duplicate Code** | Kode sama di 2+ tempat | Extract method/class |
| **Long Method** | Method > 20 lines | Decompose into smaller methods |
| **Large Class** | Class punya terlalu banyak tanggung jawab | Split into multiple classes |
| **Long Parameter List** | Method punya 3+ params | Introduce parameter object |
| **Divergent Change** | Class berubah untuk multiple reasons | Single Responsibility |
| **Speculative Generality** | Kode untuk "future use" yang tidak ada | Delete dead code |
| **Data Clumps** | Field yang selalu digunakan bersama | Extract class |
| **Feature Envy** | Method lebih banyak akses field class lain | Move to that class |
| **Comments** | Banyak komentar untuk explain | Refactor untuk self-explanatory |
| **Primitive Obsession** | Primitive types instead of objects | Introduce value object |

### Refactoring Contoh

```javascript
// ❌ Code smell: Duplicate code, long method
function calculateOrderPrice() {
    const item1Price = item1.quantity * item1.price;
    const item2Price = item2.quantity * item2.price;
    // repeated pattern...
    let total = item1Price + item2Price + ...;
    
    let discount = 0;
    if (customer.isMember) discount += total * 0.1;
    if (customer.isVIP) discount += total * 0.05;
    
    let tax = total * 0.1;
    return total - discount + tax;
}

// ✅ Refactored
function calculateOrderPrice() {
    const subtotal = calculateSubtotal();
    const discount = calculateDiscount(subtotal);
    const tax = calculateTax(subtotal);
    return subtotal - discount + tax;
}

function calculateSubtotal() {
    return items.reduce((sum, item) => sum + calculateItemTotal(item), 0);
}

function calculateItemTotal(item) {
    return item.quantity * item.price;
}

function calculateDiscount(subtotal) {
    let discount = 0;
    if (customer.isMember) discount += subtotal * 0.1;
    if (customer.isVIP) discount += subtotal * 0.05;
    return discount;
}

function calculateTax(subtotal) {
    return subtotal * 0.1;
}
```

## 6. Testing & Maintainability

Clean code harus mudah ditest:

```javascript
// ❌ Sulit ditest (hardcoded dependencies)
class PaymentService {
    process() {
        const api = new HttpClient(); // hardcoded
        const result = api.post('/charge', ...);
        const db = new Database(); // hardcoded
        db.save(result);
    }
}

// ✅ Mudah ditest (injected dependencies)
class PaymentService {
    constructor(httpClient, database) {
        this.http = httpClient;
        this.db = database;
    }
    
    process() {
        const result = this.http.post('/charge', ...);
        this.db.save(result);
    }
}

// Test
const mockHttp = { post: () => ({ id: '123' }) };
const mockDb = { save: () => {} };
const service = new PaymentService(mockHttp, mockDb);
service.process(); // dapat dikontrol sepenuhnya
```

## 7. Checklist Code Review

- [ ] Setiap class/function punya SATU tanggung jawab?
- [ ] Nama variabel jelas dan meaningful?
- [ ] Function tidak terlalu panjang (< 20 lines ideal)?
- [ ] Ada duplikasi kode yang bisa di-extract?
- [ ] Dependency injection digunakan (tidak hardcoded)?
- [ ] SOLID principles diterapkan?
- [ ] Error handling memadai?
- [ ] Code bisa dengan mudah ditambah fitur baru?
- [ ] Testable (mudah dibuat unit test)?
- [ ] Ada dokumentasi untuk bagian kompleks?

## 8. Studi Kasus: Order Processing Refactoring

**Sebelum:**
- 500-line Order class
- Dependency hardcoded (DB, Email, Payment)
- Validasi bertebaran di beberapa method
- Sulit ditest

**Sesudah:**
- Order class (domain model, ~50 lines)
- OrderRepository (persistence)
- OrderValidator (validation)
- OrderProcessor (orchestration)
- Dependency injection
- Mudah ditest dengan mock

## 9. Referensi

- Robert C. Martin — *Clean Code: A Handbook of Agile Software Craftsmanship*
- Martin Fowler — *Refactoring: Improving the Design of Existing Code*
- SOLID Principles — https://en.wikipedia.org/wiki/SOLID
- Refactoring.guru — Code Smells & Refactoring Techniques
