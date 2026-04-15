# Pertemuan 14: Advanced Refactoring dengan Design Patterns

## 1. Learning Outcomes
Setelah perkuliahan ini, mahasiswa mampu:
- Mengidentifikasi ketika design patterns bisa memperbaiki desain
- Menerapkan refactoring bertahap untuk menghindari breaking changes
- Menggunakan design patterns untuk menyelesaikan kompleksitas domain
- Membuat keputusan trade-off dalam refactoring

## 2. Konsep Utama

Refactoring bukan cuma "clean code" — tapi meningkatkan desain sambil mempertahankan behavior. Design patterns adalah tools untuk refactoring yang sophisticated.

### Kapan Refactor dengan Pattern?

1. **Terlalu banyak if-else** → Strategy atau Factory
2. **Class terlalu besar** → Extract class, Observer
3. **Dependency hardcoded** → Dependency Injection
4. **Duplikasi behavior** → Decorator atau Template Method
5. **Kompleksitas tersembunyi** → Facade

## 3. Studi Kasus Real: Payment Processing Refactoring

### Before: God Class

```javascript
// BEFORE — 300+ lines, 5+ tanggung jawab
class PaymentProcessor {
    async processPayment(amount, method, userId) {
        // Validasi
        if (amount <= 0) throw new Error('Invalid amount');
        if (!['credit_card', 'gopay', 'ovo'].includes(method)) throw new Error('Unknown method');
        
        // Check balance
        const user = await this.db.getUser(userId);
        if (method === 'gopay' && user.goPayBalance < amount) {
            throw new Error('Insufficient GoPay balance');
        }
        
        // Process berdasarkan method
        let result;
        if (method === 'credit_card') {
            result = await this.chargeCard(amount, user.cardToken);
        } else if (method === 'gopay') {
            result = await this.chargeGoPay(amount, user.goPayId);
        } else if (method === 'ovo') {
            result = await this.chargeOVO(amount, user.ovoPhone);
        }
        
        if (!result.success) {
            // Log failure
            await this.logger.log('payment_failed', { userId, amount, method });
            throw new Error(result.error);
        }
        
        // Save to DB
        const order = {
            userId,
            amount,
            method,
            transactionId: result.transactionId,
            status: 'SUCCESS',
            createdAt: new Date(),
        };
        await this.db.saveOrder(order);
        
        // Send confirmation
        const user = await this.db.getUser(userId);
        await this.emailService.sendConfirmation(user.email, order);
        
        // Update wallet balance
        if (method === 'gopay') {
            user.goPayBalance -= amount;
            await this.db.saveUser(user);
        }
        
        return order;
    }
    
    async chargeCard(amount, token) { /* 20 lines */ }
    async chargeGoPay(amount, id) { /* 20 lines */ }
    async chargeOVO(amount, phone) { /* 20 lines */ }
}
```

### After: Refactored dengan Patterns

#### Step 1: Extract Strategy (Payment Method)

```javascript
// Strategy pattern untuk method pembayaran
class PaymentMethod {
    async validate(user) { throw new Error('Abstract'); }
    async charge(amount, userDetails) { throw new Error('Abstract'); }
}

class CreditCardPayment extends PaymentMethod {
    async validate(user) {
        if (!user.cardToken) throw new Error('Card not linked');
    }
    async charge(amount, user) {
        return await this.gateway.charge(amount, user.cardToken);
    }
}

class GoPayPayment extends PaymentMethod {
    async validate(user) {
        if (user.goPayBalance < amount) throw new Error('Insufficient balance');
    }
    async charge(amount, user) {
        return await this.gateway.charge(amount, user.goPayId);
    }
}

// Factory untuk create payment method
class PaymentMethodFactory {
    static create(method, dependencies) {
        const methods = {
            'credit_card': CreditCardPayment,
            'gopay': GoPayPayment,
            'ovo': OVOPayment,
        };
        const Method = methods[method];
        return new Method(dependencies);
    }
}
```

#### Step 2: Extract Service (Separation of Concerns)

```javascript
// Validator service
class PaymentValidator {
    constructor(paymentRepository) {
        this.paymentRepository = paymentRepository;
    }
    
    validate(amount, paymentMethod) {
        if (amount <= 0) throw new Error('Invalid amount');
        if (!paymentMethod) throw new Error('Payment method required');
    }
}

// Repository (persistence)
class PaymentRepository {
    async saveOrder(order) { /* DB logic */ }
    async getUser(userId) { /* DB logic */ }
}

// Notification service
class NotificationService {
    async sendConfirmation(email, order) { /* Email */ }
}

// Main processor (now orchestrator)
class PaymentProcessor {
    constructor(validator, repository, notificationService, paymentMethodFactory) {
        this.validator = validator;
        this.repository = repository;
        this.notificationService = notificationService;
        this.paymentMethodFactory = paymentMethodFactory;
    }
    
    async processPayment(amount, methodType, userId) {
        this.validator.validate(amount, methodType);
        
        const user = await this.repository.getUser(userId);
        const paymentMethod = this.paymentMethodFactory.create(methodType, { user });
        
        await paymentMethod.validate(user);
        const result = await paymentMethod.charge(amount, user);
        
        if (!result.success) throw new Error(result.error);
        
        const order = await this.repository.saveOrder({
            userId, amount, method: methodType,
            transactionId: result.transactionId,
            status: 'SUCCESS',
        });
        
        await this.notificationService.sendConfirmation(user.email, order);
        
        return order;
    }
}
```

#### Step 3: Add Decorators (Optional Behavior)

```javascript
// Decorator untuk logging
class LoggingPaymentProcessor {
    constructor(processor, logger) {
        this.processor = processor;
        this.logger = logger;
    }
    
    async processPayment(amount, method, userId) {
        this.logger.info(`Processing payment: ${method} for user ${userId}`);
        try {
            const result = await this.processor.processPayment(amount, method, userId);
            this.logger.info(`Payment successful: ${result.id}`);
            return result;
        } catch (error) {
            this.logger.error(`Payment failed: ${error.message}`);
            throw error;
        }
    }
}

// Decorator untuk retry
class RetryPaymentProcessor {
    constructor(processor, maxRetries = 3) {
        this.processor = processor;
        this.maxRetries = maxRetries;
    }
    
    async processPayment(amount, method, userId) {
        for (let i = 1; i <= this.maxRetries; i++) {
            try {
                return await this.processor.processPayment(amount, method, userId);
            } catch (error) {
                if (i === this.maxRetries) throw error;
                console.log(`Retry attempt ${i}...`);
                await new Promise(r => setTimeout(r, 1000 * i)); // exponential backoff
            }
        }
    }
}
```

#### Step 4: Dependency Injection

```javascript
// Setup (di container/factory)
const paymentRepository = new PaymentRepository(database);
const validator = new PaymentValidator();
const notificationService = new NotificationService(emailService);
const paymentMethodFactory = new PaymentMethodFactory(paymentGateways);

let processor = new PaymentProcessor(
    validator,
    paymentRepository,
    notificationService,
    paymentMethodFactory
);

// Add logging decorator
processor = new LoggingPaymentProcessor(processor, logger);

// Add retry decorator
processor = new RetryPaymentProcessor(processor, 3);

// Usage (sederhana dan modular!)
const order = await processor.processPayment(500000, 'gopay', 'USER-123');
```

## 4. Refactoring Checklist

### Sebelum Refactor
- [ ] Semua test pass?
- [ ] Sudah ada test coverage?
- [ ] Punya version control (git)?
- [ ] Team siap untuk review?

### Selama Refactor
- [ ] Refactor in small steps
- [ ] Test setelah setiap change
- [ ] Jangan ubah behavior (regression test!)
- [ ] Keep failing tests to 0

### Setelah Refactor
- [ ] Semua test pass?
- [ ] Code review approve?
- [ ] Documentation updated?
- [ ] Team understand new design?

## 5. Trade-offs: Over-engineering

```javascript
// ✅ BENAR — simple untuk masalah simple
function getUserName(userId) {
    return users[userId]?.name;
}

// ❌ OVER-ENGINEERING — kompleks untuk kebutuhan simple
// (perlu 5 patterns, 10 classes, for a simple function!)
class UserNameProvider {
    constructor(repository, cache, logger) { /* ... */ }
    async getUserName(userId) { /* overkill! */ }
}

// Rule: Complexity harus proportional dengan problem complexity
```

## 6. Pattern Selection Guide

```
Problem: Multiple algorithms
├─ Need to switch at runtime? → STRATEGY
└─ Need to extend independently? → COMMAND

Problem: Object too complex
├─ Too many responsibilities? → EXTRACT CLASS + SRP
├─ Terlalu banyak if-else? → FACTORY + STRATEGY
└─ Nested creation logic? → BUILDER

Problem: Data structure kompleks
├─ Part-whole hierarchy? → COMPOSITE
├─ Perlu traverse banyak cara? → ITERATOR
└─ Perlu notify banyak observer? → OBSERVER

Problem: Coupling terlalu tinggi
├─ Depend on concrete class? → DEPENDENCY INJECTION
├─ Subsystem too complex? → FACADE
└─ Perlu proxy untuk control? → PROXY
```

## 7. Real-World Example: Tokopedia Notification System

**Dari:** Hardcoded email + SMS di berbagai method
**Ke:** Observer pattern + Strategy untuk channel

**Refactoring steps:**
1. Extract notification logic (Facade)
2. Add Observer untuk subscribe
3. Strategy untuk email vs SMS vs push
4. Decorator untuk rate limiting
5. Factory untuk create channels

**Hasil:**
- Mudah tambah channel baru (no modification)
- Easy to test (mock observers)
- Decouple notification dari business logic

## 8. Referensi

- Martin Fowler — *Refactoring* (chap. Introducing Patterns)
- Kent Beck — *Extreme Programming*
- Sandi Metz — *99 Bottles of OOP* (refactoring masterclass)
- Design Patterns Applied Refactoring — Refactoring.guru
