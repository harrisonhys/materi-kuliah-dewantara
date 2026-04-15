# Pertemuan 2: JavaScript ES6+ & Asynchronous Programming

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Menerapkan fitur ES6+ modern: arrow functions, destructuring, spread operator, template literals
- Memahami perbedaan callback, Promise, async/await dan kapan menggunakannya
- Menghindari callback hell dan mengelola error handling asynchronous code
- Mengoptimalkan concurrent operations dengan Promise.all, Promise.race, Promise.allSettled
- Membangun real-world async flow patterns seperti retry, timeout, dan rate limiting

## 2. Pengantar: Hook

10 tahun lalu, JavaScript hanya menjalankan satu task pada satu waktu (single-threaded). Kalau network request lambat, UI freeze. Hari ini, asynchronous JavaScript adalah fondasi dari setiap aplikasi responsif — dari mobile app sampai server.

Di GoPay, ketika user tap "Transfer", app tidak bisa block UI untuk menunggu response server (5-10 detik). Backend harus handle 1000+ concurrent requests tanpa bottleneck. Asynchronous code bukan optional — itu **survival skill**.

JavaScript ES6+ membuat async programming lebih elegant: dari callback pyramid → Promise → async/await. Semakin baik kamu menguasai ini, semakin powerful aplikasi yang bisa kamu build.

## 3. Konsep Utama

### 3.1 Event Loop & Call Stack

```
┌─────────────────────────────────────────────────┐
│  JavaScript Engine (Single-threaded)            │
│  ┌─────────────────────────────────────────┐   │
│  │ Call Stack (LIFO)                       │   │
│  │ ┌──────────┐                            │   │
│  │ │function1 │  ← sedang dijalankan       │   │
│  │ └──────────┘                            │   │
│  └─────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────┐   │
│  │ Microtask Queue (Priority High)         │   │
│  │ - Promise callbacks (then/catch)        │   │
│  │ - async/await                           │   │
│  └─────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────┐   │
│  │ Macrotask Queue (Priority Low)          │   │
│  │ - setTimeout, setInterval               │   │
│  │ - I/O operations (fetch, fs)            │   │
│  └─────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

**Event Loop Priority:** Call Stack → Microtask Queue → Macrotask Queue

### 3.2 ES6+ Features

| Feature | Purpose | Example |
|---------|---------|---------|
| **Arrow Functions** | Concise syntax, implicit return | `const add = (a, b) => a + b` |
| **Destructuring** | Extract values from objects/arrays | `const { name, age } = user` |
| **Spread Operator** | Expand array/object | `const arr2 = [...arr1, 4]` |
| **Template Literals** | String interpolation | `` const msg = `Hello ${name}` `` |
| **Default Parameters** | Function parameter defaults | `function fetch(url, timeout = 5000)` |
| **Rest Parameters** | Capture multiple arguments | `function log(...args) {}` |

### 3.3 Callback vs Promise vs Async/Await

```javascript
// ❌ Callback Hell (Pyramid of Doom)
function getTransactionHistory(userId, callback) {
    fetchUser(userId, (err, user) => {
        if (err) return callback(err);
        fetchTransactions(user.id, (err, txs) => {
            if (err) return callback(err);
            fetchDetails(txs[0].id, (err, detail) => {
                if (err) return callback(err);
                callback(null, detail); // deeply nested!
            });
        });
    });
}

// ✅ Promise Chain (Better)
function getTransactionHistory(userId) {
    return fetchUser(userId)
        .then(user => fetchTransactions(user.id))
        .then(txs => fetchDetails(txs[0].id))
        .catch(err => console.error(err));
}

// ✅✅ Async/Await (Best)
async function getTransactionHistory(userId) {
    try {
        const user = await fetchUser(userId);
        const txs = await fetchTransactions(user.id);
        const detail = await fetchDetails(txs[0].id);
        return detail;
    } catch (err) {
        console.error(err);
    }
}
```

### 3.4 Promise States

```
┌───────────────────────────────────────────┐
│ Promise States                            │
├───────────────────────────────────────────┤
│ PENDING → FULFILLED (resolve)             │
│        → REJECTED (reject)                │
│        → SETTLED (either fulfilled/rejected)
└───────────────────────────────────────────┘
```

## 4. Ilustrasi & Analogi

**Analogi: Restoran**

- **Callback:** Kamu tunggu di meja, waiter cek status setiap 5 detik. Inefficient!
- **Promise:** Kamu dapat nomor antrian. Sistem panggil kamu ketika makanan siap. Better!
- **Async/Await:** Kamu santai, sistem otomatis kasih tahu. Best experience!

**Analogi: Concurrent Tasks**

GoPay process 1000 transfers simultaneously:
- **Sequential (Callback):** 1000 tasks × 100ms = 100 detik. Terlalu lambat!
- **Parallel (Promise.all):** Run semua bersamaan = 100ms total. Perfect!

## 5. Contoh Teknis

### 5.1 ES6+ Fundamentals

```javascript
// Arrow Functions & Implicit Return
const multiply = (a, b) => a * b;
console.log(multiply(5, 3)); // 15

// Destructuring Objects
const user = { name: 'Budi', age: 25, email: 'budi@gojek.com' };
const { name, age, email } = user;
console.log(`${name} is ${age} years old`); // Budi is 25 years old

// Destructuring Arrays
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first, second, rest); // 1, 2, [3, 4, 5]

// Spread Operator
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }

// Template Literals
const msg = `Transaction to ${user.name}: Rp${1000000}`;
console.log(msg);

// Default Parameters
function createOrder(amount, currency = 'IDR', timeout = 30000) {
    console.log(`Order: ${amount} ${currency}, timeout: ${timeout}ms`);
}
createOrder(100000); // Order: 100000 IDR, timeout: 30000ms

// Rest Parameters
const sum = (...numbers) => numbers.reduce((a, b) => a + b, 0);
console.log(sum(1, 2, 3, 4, 5)); // 15
```

### 5.2 Promise Patterns

```javascript
// Basic Promise
const fetchTransaction = (id) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (id > 0) {
                resolve({ id, amount: 100000, status: 'success' });
            } else {
                reject(new Error('Invalid transaction ID'));
            }
        }, 500);
    });
};

// Promise Chaining
fetchTransaction(1)
    .then(tx => {
        console.log('Transaction:', tx);
        return tx.amount * 1.1; // add 10% fee
    })
    .then(finalAmount => {
        console.log('Final amount:', finalAmount); // 110000
    })
    .catch(err => console.error('Error:', err.message));

// Promise.all (wait for ALL)
async function processMultiplePayments(txIds) {
    try {
        const payments = await Promise.all(
            txIds.map(id => fetchTransaction(id))
        );
        console.log('All payments processed:', payments.length);
    } catch (err) {
        console.error('One or more payments failed:', err);
    }
}
processMultiplePayments([1, 2, 3]);

// Promise.race (wait for FIRST)
const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error('Timeout')), 5000)
);
Promise.race([fetchTransaction(1), timeout])
    .then(result => console.log('Got result:', result))
    .catch(err => console.error('Timeout or error:', err.message));

// Promise.allSettled (wait for ALL, even if some fail)
const txIds = [1, -1, 2]; // -1 will fail
Promise.allSettled(txIds.map(id => fetchTransaction(id)))
    .then(results => {
        results.forEach((result, i) => {
            if (result.status === 'fulfilled') {
                console.log(`${i}: Success -`, result.value.id);
            } else {
                console.log(`${i}: Failed -`, result.reason.message);
            }
        });
    });
```

### 5.3 Async/Await Patterns

```javascript
// Basic Async Function
async function getWalletBalance(userId) {
    try {
        const user = await fetchUser(userId);
        const wallet = await fetchWallet(user.walletId);
        console.log(`Balance: Rp${wallet.balance}`);
        return wallet.balance;
    } catch (err) {
        console.error('Failed to fetch balance:', err.message);
        return 0;
    }
}

// Async with Retry Logic
async function fetchWithRetry(url, maxRetries = 3, delay = 1000) {
    for (let i = 1; i <= maxRetries; i++) {
        try {
            console.log(`Attempt ${i}...`);
            const response = await fetch(url);
            if (!response.ok) throw new Error(`HTTP ${response.status}`);
            return await response.json();
        } catch (err) {
            if (i === maxRetries) throw err;
            await new Promise(r => setTimeout(r, delay * i)); // exponential backoff
        }
    }
}

// Async with Timeout
async function fetchWithTimeout(url, timeoutMs = 5000) {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), timeoutMs);
    
    try {
        const response = await fetch(url, { signal: controller.signal });
        return await response.json();
    } finally {
        clearTimeout(timeoutId);
    }
}

// Async Parallel vs Sequential
// ❌ SLOW - Sequential (3 seconds)
async function slowWay() {
    const user = await fetchUser(1); // 1 sec
    const txs = await fetchTransactions(user.id); // 1 sec
    const summary = await fetchSummary(user.id); // 1 sec
    return { user, txs, summary }; // total: 3 sec
}

// ✅ FAST - Parallel (1 second)
async function fastWay() {
    const user = await fetchUser(1);
    const [txs, summary] = await Promise.all([
        fetchTransactions(user.id),
        fetchSummary(user.id)
    ]);
    return { user, txs, summary }; // total: 1 sec
}

// Async Iterator
async function* fetchPages(baseUrl) {
    let page = 1;
    while (page <= 10) {
        const data = await fetch(`${baseUrl}?page=${page}`).then(r => r.json());
        yield data;
        page++;
    }
}

// Usage
for await (const pageData of fetchPages('https://api.gopay.com/transactions')) {
    console.log(`Page ${pageData.page}: ${pageData.items.length} items`);
}
```

## 6. Studi Kasus Nyata: GoPay Transfer Processing

GoPay memproses ribuan transfer per detik. Flownya:

```javascript
// src/services/transferService.js
class TransferService {
    constructor(walletService, notificationService, auditService) {
        this.wallet = walletService;
        this.notification = notificationService;
        this.audit = auditService;
    }
    
    // Main transfer flow with error handling
    async processTransfer(fromUser, toUser, amount) {
        try {
            // 1. Validate concurrently
            const [senderWallet, recipientExists] = await Promise.all([
                this.wallet.getBalance(fromUser.id),
                this.wallet.userExists(toUser.id)
            ]);
            
            if (senderWallet < amount) {
                throw new Error('Insufficient balance');
            }
            if (!recipientExists) {
                throw new Error('Recipient not found');
            }
            
            // 2. Deduct from sender
            const deductResult = await this.wallet.deduct(fromUser.id, amount);
            
            // 3. Add to recipient (with retry)
            const creditResult = await this.retryOperation(
                () => this.wallet.credit(toUser.id, amount),
                3,
                500
            );
            
            // 4. Notify both users in parallel
            await Promise.all([
                this.notification.sendTransferConfirm(fromUser, amount),
                this.notification.sendTransferReceived(toUser, amount)
            ]);
            
            // 5. Log to audit (fire-and-forget, don't block)
            this.audit.log({
                type: 'TRANSFER',
                from: fromUser.id,
                to: toUser.id,
                amount,
                timestamp: new Date()
            }).catch(err => console.error('Audit log failed:', err));
            
            return { success: true, txId: deductResult.txId };
        } catch (err) {
            // Rollback if needed
            console.error('Transfer failed:', err.message);
            throw err;
        }
    }
    
    // Retry helper with exponential backoff
    async retryOperation(fn, maxRetries = 3, baseDelay = 100) {
        for (let i = 1; i <= maxRetries; i++) {
            try {
                return await fn();
            } catch (err) {
                if (i === maxRetries) throw err;
                const delay = baseDelay * Math.pow(2, i - 1);
                await new Promise(r => setTimeout(r, delay));
            }
        }
    }
}

// Usage
const transferService = new TransferService(
    new WalletService(db),
    new NotificationService(email, sms),
    new AuditService(logger)
);

transferService.processTransfer(userA, userB, 100000)
    .then(result => console.log('Transfer success:', result.txId))
    .catch(err => console.error('Transfer failed:', err.message));
```

## 7. Visualisasi: Async Flow Diagram

```
Transfer Request → Validate (parallel)
                  ├─ Check sender balance
                  └─ Check recipient exists
                ↓ (if OK)
            Deduct from sender
                ↓
            Credit recipient (with retry)
                ↓
            Send notifications (parallel)
                ├─ Confirm to sender
                ├─ Notify recipient
                └─ Log to audit
                ↓
            Return success
```

**Event Loop Execution Order:**

```
1. Call getBalance() → returns Promise → goes to Microtask
2. Call userExists() → returns Promise → goes to Microtask
3. Promise.all waits for both → resolves
4. Continue to deduct → returns Promise
5. Try credit with retry → Promise loops
6. Promise.all for notifications → parallel send
7. Audit log (fire-and-forget) → added to queue but don't wait
8. Return result
```

## 8. Kesalahan Umum

### ❌ Forgetting await

```javascript
// ❌ WRONG - function returns Promise, not value
async function getBalance() {
    const balance = fetchBalance(userId); // forgot await!
    console.log(balance); // logs Promise object, not number
}

// ✅ CORRECT
async function getBalance() {
    const balance = await fetchBalance(userId);
    console.log(balance); // logs actual number
}
```

### ❌ Error handling in Promise chain

```javascript
// ❌ WRONG - error gets lost
fetchUser(1)
    .then(user => fetchWallet(user.id)) // no catch here!
    .then(wallet => console.log(wallet));

// ✅ CORRECT - always add .catch()
fetchUser(1)
    .then(user => fetchWallet(user.id))
    .then(wallet => console.log(wallet))
    .catch(err => console.error('Error:', err));
```

### ❌ Mixing callbacks with Promises

```javascript
// ❌ CONFUSING - mixing styles
function transfer(amount, callback) {
    return new Promise((resolve, reject) => {
        processPayment(amount)
            .then(result => callback(null, result)) // callback inside Promise!
            .catch(err => callback(err));
    });
}

// ✅ CONSISTENT - stick to one style
function transfer(amount) {
    return processPayment(amount);
}

// Usage: one way only
await transfer(100000); // async/await style
```

### ❌ Promise.all with partial failures

```javascript
// ❌ WRONG - if any Promise rejects, whole thing fails
Promise.all([
    fetchTransaction(1),
    fetchTransaction(-1), // this will throw
    fetchTransaction(2)
]).catch(err => console.error('One failed, all failed'));

// ✅ CORRECT - use allSettled if you need partial results
Promise.allSettled([
    fetchTransaction(1),
    fetchTransaction(-1),
    fetchTransaction(2)
]).then(results => {
    const successful = results.filter(r => r.status === 'fulfilled');
    const failed = results.filter(r => r.status === 'rejected');
    console.log(`${successful.length} succeeded, ${failed.length} failed`);
});
```

## 9. Latihan & Studi Kasus

### Latihan 1: Destructuring & Spread
```javascript
// Task: Extract data dan combine arrays
const transaction = {
    id: 'TX-001',
    amount: 100000,
    user: { name: 'Budi', phone: '08123456789' },
    timestamp: '2024-01-15T10:30:00Z'
};

// Extract id, amount, user.name
// Expected: id = 'TX-001', amount = 100000, name = 'Budi'

const array1 = [1, 2, 3];
const array2 = [4, 5, 6];

// Combine into [1, 2, 3, 4, 5, 6]
```

**Solusi:**
```javascript
const { id, amount, user: { name } } = transaction;
console.log(id, amount, name);

const combined = [...array1, ...array2];
console.log(combined); // [1, 2, 3, 4, 5, 6]
```

### Latihan 2: Promise Chaining
```javascript
// Task: Fetch user → fetch wallet → calculate total

function fetchUser(id) {
    return new Promise(resolve => 
        setTimeout(() => resolve({ id, name: 'Rani' }), 100)
    );
}

function fetchWallet(userId) {
    return new Promise(resolve =>
        setTimeout(() => resolve({ balance: 5000000, userId }), 100)
    );
}

// Use Promise chaining (then/then/then)
// Expected output: "Rani has balance Rp5000000"
```

**Solusi:**
```javascript
fetchUser(1)
    .then(user => {
        console.log(`User: ${user.name}`);
        return fetchWallet(user.id);
    })
    .then(wallet => {
        console.log(`Balance: Rp${wallet.balance}`);
    })
    .catch(err => console.error(err));
```

### Latihan 3: Async/Await with Error Handling
```javascript
// Task: Implement payment with retry logic
// - If fails, retry up to 3 times
// - Wait 1 second between retries
// - Log each attempt

async function makePayment(amount) {
    // Simulate payment (70% success rate)
    if (Math.random() > 0.7) {
        return { success: true, transactionId: 'TX-123' };
    } else {
        throw new Error('Payment gateway timeout');
    }
}

// Implement retryPayment with retry logic
```

**Solusi:**
```javascript
async function retryPayment(amount, maxAttempts = 3) {
    for (let attempt = 1; attempt <= maxAttempts; attempt++) {
        try {
            console.log(`Attempt ${attempt}...`);
            const result = await makePayment(amount);
            console.log('Payment successful:', result.transactionId);
            return result;
        } catch (err) {
            console.log(`Attempt ${attempt} failed: ${err.message}`);
            if (attempt === maxAttempts) throw err;
            await new Promise(r => setTimeout(r, 1000));
        }
    }
}

retryPayment(100000);
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Pahami arrow functions dan kapan menggunakannya
- [ ] Bisa destructuring objects dan arrays
- [ ] Mengerti perbedaan callback vs Promise vs async/await
- [ ] Bisa menulis Promise chain dengan .then/.catch
- [ ] Bisa menulis async functions dengan error handling
- [ ] Mengerti Promise.all vs Promise.race vs Promise.allSettled
- [ ] Mengerti event loop: Call Stack → Microtask → Macrotask
- [ ] Bisa implement retry logic dengan exponential backoff
- [ ] Bisa handle concurrent vs sequential operations
- [ ] Tahu kapan menggunakan await vs fire-and-forget

## 11. Referensi

- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [MDN: Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
- [MDN: Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: Async/Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- [MDN: Event Loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
- [Jake Archibald: In The Loop (Video)](https://www.youtube.com/watch?v=cCOL7MC4Pl0)

**Status:** ✅ Pertemuan 2 selesai
