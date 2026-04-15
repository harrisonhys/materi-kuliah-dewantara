# Pertemuan 12: Design Patterns — Behavioral

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Mengimplementasikan Observer/EventEmitter untuk event-driven architecture
- Menerapkan Strategy Pattern untuk algoritma yang interchangeable
- Menggunakan Command Pattern untuk undo/redo dan task queuing
- Memahami Iterator dan Template Method patterns

## 2. Pengantar: Hook

Sistem inventory Tokopedia perlu memberitahu "semua yang berminat" saat stok produk berubah:
- Payment gateway perlu tahu untuk verifikasi
- Email service perlu kirim notifikasi
- Analytics service perlu track
- Rekomendasi engine perlu update

Jangan hardcode "panggil 4 service ini" — gunakan **Observer Pattern** (publish-subscribe). Tambah observer baru tanpa ubah kode lama.

Atau: Algoritma sorting bisa selection sort, merge sort, quick sort. Pilih saat runtime. Itu adalah **Strategy Pattern**.

Behavioral Patterns adalah tentang **komunikasi antar objek** dan **alur eksekusi**.

## 3. Konsep Utama

### 3.1 Lima Behavioral Patterns Utama

| Pattern | Masalah | Solusi |
|---------|---------|--------|
| **Observer** | Notify banyak objek tentang state change | Publish-subscribe architecture |
| **Strategy** | Algoritma yang bisa ditukar saat runtime | Encapsulate dalam strategy objects |
| **Command** | Encapsulate permintaan sebagai object | Enable undo/redo/queue/logging |
| **Iterator** | Traverse struktur kompleks tanpa expose | Unified interface untuk iterasi |
| **Template Method** | Outline algoritma, details di subclass | Inversion of control |

### 3.2 Observer dalam JavaScript — EventEmitter

JavaScript punya `EventTarget` (browser) dan Node.js `EventEmitter`. Pattern yang sama di mana-mana!

## 4. Contoh Teknis

### 4.1 Observer — Inventory Events

```javascript
/**
 * Observer / Pub-Sub Pattern
 * 
 * JavaScript native: EventEmitter (Node.js) atau EventTarget (browser)
 */

class InventoryEventEmitter {
    constructor() {
        this.listeners = new Map(); // { eventName: [callback1, callback2, ...] }
    }
    
    on(eventName, callback) {
        if (!this.listeners.has(eventName)) {
            this.listeners.set(eventName, []);
        }
        this.listeners.get(eventName).push(callback);
        return this; // method chaining
    }
    
    once(eventName, callback) {
        const wrapper = (...args) => {
            callback(...args);
            this.off(eventName, wrapper);
        };
        this.on(eventName, wrapper);
        return this;
    }
    
    off(eventName, callback) {
        if (this.listeners.has(eventName)) {
            const cbs = this.listeners.get(eventName);
            const idx = cbs.indexOf(callback);
            if (idx !== -1) cbs.splice(idx, 1);
        }
        return this;
    }
    
    emit(eventName, data) {
        if (this.listeners.has(eventName)) {
            this.listeners.get(eventName).forEach(cb => cb(data));
        }
        return this;
    }
}

// Inventory yang emit events
class Inventory extends InventoryEventEmitter {
    constructor() {
        super();
        this.products = new Map();
    }
    
    addProduct(id, name, stock) {
        this.products.set(id, { id, name, stock });
    }
    
    updateStock(productId, newStock) {
        const product = this.products.get(productId);
        if (!product) throw new Error(`Product ${productId} not found`);
        
        const oldStock = product.stock;
        product.stock = newStock;
        
        // Emit event dengan data perubahan
        this.emit('stock-changed', {
            productId,
            productName: product.name,
            oldStock,
            newStock,
            timestamp: new Date(),
        });
        
        // Emit conditional events
        if (newStock === 0) {
            this.emit('out-of-stock', { productId, productName: product.name });
        }
        if (oldStock === 0 && newStock > 0) {
            this.emit('back-in-stock', { productId, productName: product.name });
        }
    }
}

// Observers
class NotificationService {
    notify(event) {
        console.log(`[Notification] Sending email: "${event.productName}" stock updated`);
    }
}

class PaymentGateway {
    onStockChange(event) {
        console.log(`[Payment] Verifying inventory for ${event.productId}`);
    }
}

class AnalyticsService {
    trackStockChange(event) {
        console.log(`[Analytics] Logged: ${event.productName} ${event.oldStock}→${event.newStock}`);
    }
}

class RecommendationEngine {
    onBackInStock(event) {
        console.log(`[Recommendation] Re-enabling recommendations for ${event.productName}`);
    }
}

// Setup
const inventory = new Inventory();
const notif = new NotificationService();
const payment = new PaymentGateway();
const analytics = new AnalyticsService();
const recommendation = new RecommendationEngine();

// Subscribe
inventory
    .on('stock-changed', e => notif.notify(e))
    .on('stock-changed', e => payment.onStockChange(e))
    .on('stock-changed', e => analytics.trackStockChange(e))
    .on('back-in-stock', e => recommendation.onBackInStock(e));

// Test
console.log('\n=== Observer Pattern ===');
inventory.addProduct('PROD-001', 'Laptop Gaming', 5);
inventory.updateStock('PROD-001', 2);
inventory.updateStock('PROD-001', 0); // out-of-stock event
inventory.updateStock('PROD-001', 3); // back-in-stock event
```

### 4.2 Strategy — Sorting Algorithm

```javascript
/**
 * Strategy Pattern
 * 
 * Masalah: Kode client harus mendukung multiple sorting algorithms
 * tanpa if-else yang panjang.
 * 
 * Strategy: encapsulate algorithm, biarkan client pilih saat runtime.
 */

// Strategy Interface
class SortStrategy {
    sort(arr) { throw new Error('Abstract'); }
}

// Concrete Strategies
class BubbleSort extends SortStrategy {
    sort(arr) {
        const n = arr.length;
        for (let i = 0; i < n; i++) {
            for (let j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
                }
            }
        }
        return arr;
    }
}

class MergeSort extends SortStrategy {
    sort(arr) {
        if (arr.length <= 1) return arr;
        const mid = Math.floor(arr.length / 2);
        const left = this.sort(arr.slice(0, mid));
        const right = this.sort(arr.slice(mid));
        return this._merge(left, right);
    }
    
    _merge(left, right) {
        const result = [];
        let i = 0, j = 0;
        while (i < left.length && j < right.length) {
            if (left[i] <= right[j]) result.push(left[i++]);
            else result.push(right[j++]);
        }
        return result.concat(left.slice(i)).concat(right.slice(j));
    }
}

class QuickSort extends SortStrategy {
    sort(arr) {
        if (arr.length <= 1) return arr;
        const pivot = arr[0];
        const left = arr.slice(1).filter(x => x <= pivot);
        const right = arr.slice(1).filter(x => x > pivot);
        return [...this.sort(left), pivot, ...this.sort(right)];
    }
}

// Context yang menggunakan strategy
class Sorter {
    constructor(strategy = new MergeSort()) {
        this.strategy = strategy;
    }
    
    setStrategy(strategy) {
        this.strategy = strategy;
        return this;
    }
    
    sort(arr) {
        console.log(`Sorting dengan ${this.strategy.constructor.name}...`);
        return this.strategy.sort([...arr]); // clone array
    }
    
    benchmark(arr) {
        const start = performance.now();
        const result = this.sort([...arr]);
        const time = performance.now() - start;
        console.log(`  Time: ${time.toFixed(2)}ms`);
        return result;
    }
}

// Usage
console.log('\n=== Strategy Pattern ===');
const data = [64, 34, 25, 12, 22, 11, 90, 88];
const sorter = new Sorter();

sorter.setStrategy(new BubbleSort());
sorter.benchmark(data);

sorter.setStrategy(new MergeSort());
sorter.benchmark(data);

sorter.setStrategy(new QuickSort());
sorter.benchmark(data);

// Benchmark besar
const largeData = Array.from({length: 10000}, () => Math.random() * 1000);

console.log('\nBenchmark 10000 items:');
sorter.setStrategy(new BubbleSort());
sorter.benchmark(largeData);

sorter.setStrategy(new MergeSort());
sorter.benchmark(largeData);

sorter.setStrategy(new QuickSort());
sorter.benchmark(largeData);
```

### 4.3 Command — Undo/Redo

```javascript
/**
 * Command Pattern
 * 
 * Encapsulate permintaan sebagai object → enable:
 * - Undo/Redo
 * - Queuing
 * - Logging
 * - Scheduling
 */

// Command Interface
class Command {
    execute() { throw new Error('Abstract'); }
    undo() { throw new Error('Abstract'); }
}

// Receiver — object yang melakukan action
class BankAccount {
    constructor(initialBalance = 0) {
        this.balance = initialBalance;
    }
    
    deposit(amount) {
        this.balance += amount;
        console.log(`Deposited Rp ${amount}. Balance: Rp ${this.balance}`);
    }
    
    withdraw(amount) {
        if (this.balance < amount) throw new Error('Insufficient funds');
        this.balance -= amount;
        console.log(`Withdrew Rp ${amount}. Balance: Rp ${this.balance}`);
    }
    
    getBalance() { return this.balance; }
}

// Concrete Commands
class DepositCommand extends Command {
    constructor(account, amount) {
        super();
        this.account = account;
        this.amount = amount;
    }
    
    execute() {
        this.account.deposit(this.amount);
    }
    
    undo() {
        this.account.withdraw(this.amount);
    }
}

class WithdrawCommand extends Command {
    constructor(account, amount) {
        super();
        this.account = account;
        this.amount = amount;
    }
    
    execute() {
        this.account.withdraw(this.amount);
    }
    
    undo() {
        this.account.deposit(this.amount);
    }
}

// Invoker — eksekutor command dengan history
class CommandInvoker {
    constructor() {
        this.history = [];
        this.undoneHistory = [];
    }
    
    execute(command) {
        command.execute();
        this.history.push(command);
        this.undoneHistory = []; // clear redo history
    }
    
    undo() {
        if (this.history.length === 0) {
            console.log('Nothing to undo');
            return;
        }
        const command = this.history.pop();
        command.undo();
        this.undoneHistory.push(command);
    }
    
    redo() {
        if (this.undoneHistory.length === 0) {
            console.log('Nothing to redo');
            return;
        }
        const command = this.undoneHistory.pop();
        command.execute();
        this.history.push(command);
    }
    
    getHistory() {
        return this.history.map((c, i) => `${i+1}. ${c.constructor.name}(${c.amount})`);
    }
}

// Usage
console.log('\n=== Command Pattern: Undo/Redo ===');
const account = new BankAccount(1000000);
const invoker = new CommandInvoker();

// Eksekusi commands
invoker.execute(new DepositCommand(account, 500000));
invoker.execute(new WithdrawCommand(account, 200000));
invoker.execute(new DepositCommand(account, 100000));

console.log('\nHistory:', invoker.getHistory());
console.log(`Balance: Rp ${account.getBalance()}`); // 1400000

// Undo
console.log('\n--- Undo ---');
invoker.undo(); // undo deposit 100k
invoker.undo(); // undo withdraw 200k
console.log(`Balance: Rp ${account.getBalance()}`); // 1200000

// Redo
console.log('\n--- Redo ---');
invoker.redo(); // redo withdraw 200k
console.log(`Balance: Rp ${account.getBalance()}`); // 1000000

console.log('\nFinal history:', invoker.getHistory());
```

### 4.4 Iterator — Tree Traversal

```javascript
/**
 * Iterator Pattern
 * 
 * ES6 punya Symbol.iterator — tapi kita buat custom untuk tree traversal.
 */

class TreeNode {
    constructor(value, left = null, right = null) {
        this.value = value;
        this.left = left;
        this.right = right;
    }
}

// Iterator untuk In-order traversal
class InOrderIterator {
    constructor(root) {
        this.root = root;
        this.stack = [];
        this._init();
    }
    
    _init() {
        let node = this.root;
        while (node) {
            this.stack.push(node);
            node = node.left;
        }
    }
    
    hasNext() { return this.stack.length > 0; }
    
    next() {
        const node = this.stack.pop();
        let result = node.value;
        
        if (node.right) {
            let right = node.right;
            while (right) {
                this.stack.push(right);
                right = right.left;
            }
        }
        
        return result;
    }
}

// Pre-order, Post-order iterators...

// Tree dengan iterator
class BinarySearchTree {
    constructor() {
        this.root = null;
    }
    
    insert(value) {
        const node = new TreeNode(value);
        if (!this.root) {
            this.root = node;
        } else {
            this._insertNode(this.root, node);
        }
    }
    
    _insertNode(node, newNode) {
        if (newNode.value < node.value) {
            if (node.left === null) node.left = newNode;
            else this._insertNode(node.left, newNode);
        } else {
            if (node.right === null) node.right = newNode;
            else this._insertNode(node.right, newNode);
        }
    }
    
    getInOrderIterator() {
        return new InOrderIterator(this.root);
    }
    
    // ES6 iterator protocol
    [Symbol.iterator]() {
        return this.getInOrderIterator();
    }
}

// Usage
console.log('\n=== Iterator Pattern ===');
const tree = new BinarySearchTree();
[50, 30, 70, 20, 40, 60, 80].forEach(v => tree.insert(v));

console.log('In-order traversal:');
const iterator = tree.getInOrderIterator();
while (iterator.hasNext()) {
    process.stdout.write(iterator.next() + ' ');
}
console.log('\n'); // 20 30 40 50 60 70 80

// ES6 for...of (karena Symbol.iterator)
console.log('Using for...of:');
for (const value of tree) {
    process.stdout.write(value + ' ');
}
console.log();
```

### 4.5 Template Method — Report Generation

```javascript
/**
 * Template Method Pattern
 * 
 * Define skeleton of algorithm di base class,
 * details di subclass (inversion of control).
 */

class ReportGenerator {
    generate() {
        // Template: urutan langkah yang fixed
        const header = this.generateHeader();
        const data = this.generateData();
        const summary = this.generateSummary();
        const footer = this.generateFooter();
        
        return this.format(header, data, summary, footer);
    }
    
    // Abstract methods — subclass override
    generateHeader() { throw new Error('Abstract'); }
    generateData() { throw new Error('Abstract'); }
    generateSummary() { throw new Error('Abstract'); }
    generateFooter() { throw new Error('Abstract'); }
    format(header, data, summary, footer) { throw new Error('Abstract'); }
}

class CSVReportGenerator extends ReportGenerator {
    generateHeader() {
        return 'Tanggal,Penjualan,Profit\n';
    }
    
    generateData() {
        return '2026-01-01,1000000,200000\n2026-01-02,1500000,300000\n';
    }
    
    generateSummary() {
        return '2026,2500000,500000\n';
    }
    
    generateFooter() {
        return 'Generated: ' + new Date().toISOString();
    }
    
    format(header, data, summary, footer) {
        return header + data + summary + footer;
    }
}

class HTMLReportGenerator extends ReportGenerator {
    generateHeader() {
        return '<table><tr><th>Tanggal</th><th>Penjualan</th><th>Profit</th></tr>\n';
    }
    
    generateData() {
        return '<tr><td>2026-01-01</td><td>1.000.000</td><td>200.000</td></tr>\n' +
               '<tr><td>2026-01-02</td><td>1.500.000</td><td>300.000</td></tr>\n';
    }
    
    generateSummary() {
        return '<tr><th>Total</th><th>2.500.000</th><th>500.000</th></tr>\n';
    }
    
    generateFooter() {
        return '</table><p>Generated: ' + new Date().toISOString() + '</p>';
    }
    
    format(h, d, s, f) {
        return `<html><body>${h}${d}${s}${f}</body></html>`;
    }
}

// Usage
console.log('\n=== Template Method Pattern ===');

const csvGen = new CSVReportGenerator();
console.log('CSV Report:');
console.log(csvGen.generate());

console.log('\nHTML Report:');
console.log(HTMLReportGenerator.prototype.generate.call(new HTMLReportGenerator()));
```

## 5. Studi Kasus: GoPay Notification System

**Observer:** State changes (payment successful, refund processed, limit reached) notify semua interested parties (email, SMS, push notif, analytics).

**Strategy:** Notifikasi bisa via email, SMS, push — pilih strategy berdasarkan user preference & channel availability.

**Command:** Transaction history dengan undo (rare, tapi untuk admin reversal).

**Iterator:** Traverse transaction history, wallet history dengan berbagai filters.

**Template Method:** Report generation (daily, weekly, monthly) dengan format berbeda (CSV, PDF, JSON).

## 6. Ringkasan

| Pattern | Kapan | Benefit |
|---------|-------|---------|
| Observer | Banyak objek perlu notify tentang change | Loose coupling |
| Strategy | Multiple algorithms yang interchangeable | Flexibility, isolasi algoritma |
| Command | Encapsulate request as object | Undo/redo, queue, logging |
| Iterator | Traverse kompleks struktur | Decouple traversal dari struktur |
| Template Method | Fixed algorithm skeleton, variable details | Code reuse, inversion of control |

## 7. Referensi

- GoF Design Patterns (Behavioral section)
- Refactoring.guru — Behavioral Patterns
- JavaScript EventEmitter (Node.js docs)
- ES6 Iterators & Generators (MDN)
