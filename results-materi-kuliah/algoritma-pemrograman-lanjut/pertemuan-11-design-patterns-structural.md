# Pertemuan 11: Design Patterns — Structural

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Mengimplementasikan Adapter, Decorator, Facade, Proxy, dan Composite dalam JavaScript
- Memahami kapan menggunakan pattern composition vs inheritance
- Menerapkan pattern untuk fleksibilitas dan extensibility kode
- Menyelesaikan masalah "object wrapping" yang kompleks

## 2. Pengantar: Hook

Bayangkan Tokopedia perlu mengintegrasikan payment dari berbagai negara dengan API yang berbeda-beda:
- Indonesia: Midtrans (REST, JSON)
- Thailand: Omise (REST, YAML)
- Filipina: PayMaya (SOAP, XML)

Bagaimana membuat kode client itu unified tanpa mengetahui perbedaan API internal? **Adapter Pattern** — bungkus API lain dengan interface yang sama.

Atau: Sebuah order punya harga dasar, tapi bisa dikombinasikan dengan diskon, pajak, asuransi, cicilan. Setiap kombinasi? Itu adalah "decorator" — layer by layer.

Structural Patterns adalah tentang **komposisi objek** — menyusun objek sederhana menjadi struktur kompleks dengan cara yang fleksibel.

## 3. Konsep Utama

### 3.1 Lima Structural Patterns

| Pattern | Masalah | Solusi |
|---------|---------|--------|
| **Adapter** | Interface incompatible | Bungkus dengan adapter |
| **Decorator** | Tambah tanggung jawab dinamis | Wrap untuk extend behavior |
| **Facade** | Subsystem kompleks | Satu pintu simplifikasi |
| **Proxy** | Kontrol akses, lazy loading | Placeholder dengan kontrol |
| **Composite** | Part-whole hierarchy | Tree struktur dengan interface sama |

### 3.2 Composition vs Inheritance

```javascript
// INHERITANCE — rigid, deep hierarchy
class PaymentMethod {}
class CreditCard extends PaymentMethod {}
class DebitCard extends PaymentMethod {}
class CreditCardWithDiscount extends CreditCard {} // ub oh, sudah 3 level

// COMPOSITION — fleksibel, mudah dikombinasi
class PaymentMethod { charge(amount) {} }
class CreditCard extends PaymentMethod {}
class DiscountDecorator {
    constructor(payment, discountRate) {
        this.payment = payment;
        this.discountRate = discountRate;
    }
    charge(amount) {
        const discounted = amount * (1 - this.discountRate);
        return this.payment.charge(discounted);
    }
}

// Pakai: diskoun, pajak, asuransi bisa dikombinasikan bebas!
let payment = new CreditCard();
payment = new DiscountDecorator(payment, 0.1); // 10% discount
payment = new TaxDecorator(payment, 0.1);      // 10% pajak
payment.charge(100000);
```

## 4. Contoh Teknis

### 4.1 Adapter — Payment Gateway Integration

```javascript
/**
 * Adapter Pattern
 * 
 * Masalah: Tiga payment provider punya API berbeda-beda.
 * Solusi: Bungkus dengan adapter yang unified.
 */

// Target interface — yang diinginkan klien
class PaymentAdapter {
    async processPayment(amount, details) { throw new Error('Abstract'); }
    async refund(transactionId) { throw new Error('Abstract'); }
    async checkStatus(transactionId) { throw new Error('Abstract'); }
}

// Adaptee 1: Midtrans (Indonesia)
class MidtransAPI {
    async charge(params) {
        // API Midtrans yang berbeda
        console.log('[Midtrans] Charging:', params.amount);
        return { transaction_id: `MID-${Date.now()}`, status: 'success' };
    }
    
    async cancelTransaction(txId) {
        console.log('[Midtrans] Canceling:', txId);
        return { status: 'cancelled' };
    }
    
    async getTransactionStatus(txId) {
        return { status: 'settlement' };
    }
}

// Adaptee 2: Omise (Thailand)
class OmiseAPI {
    async createCharge(chargeData) {
        // Omise API yang lagi berbeda
        console.log('[Omise] Creating charge:', chargeData.amount);
        return { id: `OMI-${Date.now()}`, status: 'successful' };
    }
    
    async voidCharge(chargeId) {
        return { status: 'voided' };
    }
    
    async getCharge(chargeId) {
        return { status: 'successful' };
    }
}

// Adapter untuk Midtrans
class MidtransAdapter extends PaymentAdapter {
    constructor(apiKey) {
        super();
        this.api = new MidtransAPI();
        this.apiKey = apiKey;
    }
    
    async processPayment(amount, details) {
        const result = await this.api.charge({
            amount: Math.round(amount),
            items: [{ name: details.description, qty: 1, price: amount }],
            customer_details: { email: details.email },
        });
        
        return {
            transactionId: result.transaction_id,
            status: result.status === 'success' ? 'SUCCESS' : 'FAILED',
        };
    }
    
    async refund(transactionId) {
        const result = await this.api.cancelTransaction(transactionId);
        return { status: result.status === 'cancelled' ? 'REFUNDED' : 'FAILED' };
    }
    
    async checkStatus(transactionId) {
        const result = await this.api.getTransactionStatus(transactionId);
        return { status: result.status };
    }
}

// Adapter untuk Omise
class OmiseAdapter extends PaymentAdapter {
    constructor(apiKey) {
        super();
        this.api = new OmiseAPI();
        this.apiKey = apiKey;
    }
    
    async processPayment(amount, details) {
        const result = await this.api.createCharge({
            amount: Math.round(amount * 100), // Omise pakai cents
            currency: 'THB',
            description: details.description,
            metadata: { email: details.email },
        });
        
        return {
            transactionId: result.id,
            status: result.status === 'successful' ? 'SUCCESS' : 'FAILED',
        };
    }
    
    async refund(transactionId) {
        const result = await this.api.voidCharge(transactionId);
        return { status: result.status === 'voided' ? 'REFUNDED' : 'FAILED' };
    }
    
    async checkStatus(transactionId) {
        const result = await this.api.getCharge(transactionId);
        return { status: result.status };
    }
}

// Adapter Factory
class AdapterFactory {
    static create(provider, apiKey) {
        const adapters = {
            midtrans: MidtransAdapter,
            omise: OmiseAdapter,
        };
        const AdapterClass = adapters[provider.toLowerCase()];
        if (!AdapterClass) throw new Error(`Unknown provider: ${provider}`);
        return new AdapterClass(apiKey);
    }
}

// Klien — tidak tahu provider mana!
async function processPaymentUnified(provider, amount, details) {
    const adapter = AdapterFactory.create(provider, 'api-key-xxx');
    const result = await adapter.processPayment(amount, details);
    console.log(`Unified payment result: ${JSON.stringify(result)}`);
    return result;
}

(async () => {
    console.log('\n=== Adapter Pattern ===');
    await processPaymentUnified('midtrans', 500000, { description: 'Laptop', email: 'user@x.com' });
    await processPaymentUnified('omise', 500, { description: 'Laptop', email: 'user@x.com' });
})();
```

### 4.2 Decorator — Dynamic Behavior

```javascript
/**
 * Decorator Pattern
 * 
 * Masalah: Order punya harga dasar, tapi bisa dikombinasikan dengan:
 * - Diskon hingga 3 tingkat
 * - Pajak (region-specific)
 * - Asuransi pengiriman
 * - Cicilan tanpa bunga
 * 
 * Inheritance: 2^4 = 16 kombinasi class!
 * Decorator: 1 base + 4 decorators = fleksibel
 */

// Component
class Order {
    constructor(items) {
        this.items = items;
        this.basePrice = items.reduce((sum, item) => sum + item.price, 0);
    }
    
    getTotal() { return this.basePrice; }
    getDescription() { return 'Base Order'; }
}

// Decorator base
class OrderDecorator {
    constructor(order) {
        this.order = order;
    }
    
    getTotal() { return this.order.getTotal(); }
    getDescription() { return this.order.getDescription(); }
}

// Concrete Decorators
class DiscountDecorator extends OrderDecorator {
    constructor(order, discountRate, description) {
        super(order);
        this.discountRate = discountRate;
        this.discountName = description;
    }
    
    getTotal() {
        const baseTotal = this.order.getTotal();
        const discount = baseTotal * this.discountRate;
        this.discountAmount = discount;
        return baseTotal - discount;
    }
    
    getDescription() {
        return `${this.order.getDescription()} + ${this.discountName} (-Rp ${this.discountAmount?.toLocaleString()})`;
    }
}

class TaxDecorator extends OrderDecorator {
    constructor(order, taxRate) {
        super(order);
        this.taxRate = taxRate;
    }
    
    getTotal() {
        const baseTotal = this.order.getTotal();
        this.taxAmount = baseTotal * this.taxRate;
        return baseTotal + this.taxAmount;
    }
    
    getDescription() {
        return `${this.order.getDescription()} + Tax (+Rp ${this.taxAmount?.toLocaleString()})`;
    }
}

class InsuranceDecorator extends OrderDecorator {
    constructor(order, insuranceRate) {
        super(order);
        this.insuranceRate = insuranceRate;
    }
    
    getTotal() {
        const baseTotal = this.order.getTotal();
        this.insuranceCost = baseTotal * this.insuranceRate;
        return baseTotal + this.insuranceCost;
    }
    
    getDescription() {
        return `${this.order.getDescription()} + Shipping Insurance (+Rp ${this.insuranceCost?.toLocaleString()})`;
    }
}

class InstallmentDecorator extends OrderDecorator {
    constructor(order, months) {
        super(order);
        this.months = months;
    }
    
    getTotal() {
        // Installment tidak ubah total, tapi presentation
        return this.order.getTotal();
    }
    
    getDescription() {
        const monthly = this.order.getTotal() / this.months;
        return `${this.order.getDescription()} (${this.months}x Rp ${Math.round(monthly).toLocaleString()})`;
    }
}

// Kombinasi bebas!
const items = [
    { name: 'Laptop', price: 10000000 },
    { name: 'Mouse', price: 200000 },
];

let order = new Order(items);
console.log('\n=== Decorator Pattern: Order ===');
console.log(`Basic: ${order.getDescription()}`);
console.log(`Total: Rp ${order.getTotal().toLocaleString()}`);

// Tambah diskon
order = new DiscountDecorator(order, 0.1, 'Tokopedia Member 10%');
console.log(`\nWith discount: ${order.getDescription()}`);
console.log(`Total: Rp ${order.getTotal().toLocaleString()}`);

// Tambah pajak
order = new TaxDecorator(order, 0.1);
console.log(`\nWith tax: ${order.getDescription()}`);
console.log(`Total: Rp ${order.getTotal().toLocaleString()}`);

// Tambah asuransi
order = new InsuranceDecorator(order, 0.05);
console.log(`\nWith insurance: ${order.getDescription()}`);
console.log(`Total: Rp ${order.getTotal().toLocaleString()}`);

// Tambah cicilan
order = new InstallmentDecorator(order, 3);
console.log(`\nWith installment: ${order.getDescription()}`);
```

### 4.3 Facade — Kompleks Subsystem

```javascript
/**
 * Facade Pattern
 * 
 * Masalah: Checkout process melibatkan 5+ subsystem:
 * - Inventory (check stok)
 * - Payment (process pembayaran)
 * - Shipping (hitung ongkos, pilih kurir)
 * - Order (buat order DB)
 * - Notification (kirim email/SMS)
 * 
 * Client tidak perlu tahu semua itu. Facade adalah "satu tombol".
 */

class InventoryService {
    async checkStock(itemId, quantity) {
        console.log(`[Inventory] Checking stock ${itemId}: ${quantity} units`);
        return { available: true, stock: 100 };
    }
    
    async reserveItems(itemIds) {
        console.log(`[Inventory] Reserving items:`, itemIds);
        return { reserved: true, reservationId: 'RES-001' };
    }
}

class PaymentService {
    async processPayment(amount, method) {
        console.log(`[Payment] Processing Rp ${amount} via ${method}`);
        return { transactionId: 'TXN-001', status: 'SUCCESS' };
    }
}

class ShippingService {
    async calculateShipping(address, items) {
        console.log(`[Shipping] Calculating for ${address}`);
        return { cost: 50000, estimatedDays: 3, provider: 'JNE' };
    }
    
    async createShipment(orderId, address) {
        console.log(`[Shipping] Creating shipment for order ${orderId}`);
        return { trackingNumber: 'JNE-123456789', provider: 'JNE' };
    }
}

class OrderService {
    async createOrder(details) {
        console.log(`[Order] Creating order with items:`, details.items.length);
        return { orderId: 'ORD-001', status: 'CREATED' };
    }
    
    async updateOrderStatus(orderId, status) {
        console.log(`[Order] Updated ${orderId} to ${status}`);
    }
}

class NotificationService {
    async sendConfirmationEmail(email, orderId) {
        console.log(`[Notification] Email sent to ${email} for order ${orderId}`);
    }
    
    async sendSMS(phone, message) {
        console.log(`[Notification] SMS sent to ${phone}`);
    }
}

// FACADE — "satu pintu" untuk checkout
class CheckoutFacade {
    constructor() {
        this.inventory = new InventoryService();
        this.payment = new PaymentService();
        this.shipping = new ShippingService();
        this.order = new OrderService();
        this.notification = new NotificationService();
    }
    
    async checkout(cartDetails, paymentMethod, shippingAddress, customerEmail) {
        console.log('\n=== FACADE: Unified Checkout ===');
        
        try {
            // 1. Check stock
            const stockCheck = await this.inventory.checkStock(cartDetails.items[0].id, 1);
            if (!stockCheck.available) throw new Error('Out of stock');
            
            // 2. Reserve items
            const itemIds = cartDetails.items.map(i => i.id);
            const reservation = await this.inventory.reserveItems(itemIds);
            
            // 3. Calculate shipping
            const shippingInfo = await this.shipping.calculateShipping(shippingAddress, cartDetails.items);
            
            // 4. Process payment
            const totalAmount = cartDetails.subtotal + shippingInfo.cost;
            const payment = await this.payment.processPayment(totalAmount, paymentMethod);
            
            if (payment.status !== 'SUCCESS') throw new Error('Payment failed');
            
            // 5. Create order
            const order = await this.order.createOrder({
                items: cartDetails.items,
                shipping: shippingInfo,
                totalAmount,
            });
            
            // 6. Create shipment
            const shipment = await this.shipping.createShipment(order.orderId, shippingAddress);
            
            // 7. Send notification
            await this.notification.sendConfirmationEmail(customerEmail, order.orderId);
            await this.notification.sendSMS(cartDetails.phone, `Order ${order.orderId} confirmed!`);
            
            console.log('\n✅ Checkout successful!');
            return {
                orderId: order.orderId,
                transactionId: payment.transactionId,
                trackingNumber: shipment.trackingNumber,
                totalAmount,
                estimatedDelivery: shippingInfo.estimatedDays,
            };
            
        } catch (error) {
            console.error(`❌ Checkout failed: ${error.message}`);
            throw error;
        }
    }
}

// Client — hanya panggil facade!
(async () => {
    const facade = new CheckoutFacade();
    
    const result = await facade.checkout(
        { items: [{id: 'PROD-001', price: 100000}], subtotal: 100000, phone: '08123456789' },
        'gopay',
        'Jakarta, Jl. Sudirman 123',
        'customer@email.com'
    );
    
    console.log('\nFinal Result:', result);
})();
```

### 4.4 Proxy — Lazy Loading & Access Control

```javascript
/**
 * Proxy Pattern
 * 
 * Masalah: User profile besar (foto, history order, wishlist) — 
 * tapi sering hanya butuh nama dan email. Jangan load semua!
 * 
 * Proxy: placeholder yang lazy-load data real hanya saat diakses.
 */

class RealUserProfile {
    constructor(userId) {
        console.log(`[RealUserProfile] Loading complete profile for ${userId}...`);
        this.userId = userId;
        this.name = 'Budi Santoso';
        this.email = 'budi@email.com';
        this.phone = '08123456789';
        this.address = 'Jakarta, Jl. Sudirman 123';
        this.orderHistory = [/* 1000 orders */];
        this.wishlist = [/* 500 items */];
        this.preferences = { /* kompleks */ };
        this.lastLoadTime = new Date();
    }
    
    getBasicInfo() {
        return { name: this.name, email: this.email };
    }
    
    getFullProfile() {
        return {
            name: this.name,
            email: this.email,
            phone: this.phone,
            address: this.address,
            orderHistory: this.orderHistory.length + ' orders',
            wishlistItems: this.wishlist.length + ' items',
        };
    }
    
    updatePreferences(prefs) {
        Object.assign(this.preferences, prefs);
    }
}

// PROXY — lazy load & access control
class UserProfileProxy {
    constructor(userId) {
        this.userId = userId;
        this.realProfile = null; // load nanti!
        this.accessLog = [];
    }
    
    #ensureLoaded() {
        if (!this.realProfile) {
            console.log(`[Proxy] Lazy loading real profile...`);
            this.realProfile = new RealUserProfile(this.userId);
        }
    }
    
    #logAccess(method) {
        this.accessLog.push({ method, time: new Date() });
    }
    
    // BASIC info — tanpa load full profile
    getBasicInfo() {
        console.log(`[Proxy] Returning basic info (without loading full profile)`);
        this.#logAccess('getBasicInfo');
        return { name: 'Budi Santoso', email: 'budi@email.com' };
    }
    
    // FULL profile — trigger lazy load
    getFullProfile() {
        this.#ensureLoaded();
        this.#logAccess('getFullProfile');
        return this.realProfile.getFullProfile();
    }
    
    updatePreferences(prefs) {
        this.#ensureLoaded();
        this.#logAccess('updatePreferences');
        return this.realProfile.updatePreferences(prefs);
    }
    
    getAccessLog() {
        return this.accessLog;
    }
}

console.log('\n=== Proxy Pattern: Lazy Loading ===');
const profile = new UserProfileProxy('USER-123');

// Akses basic info — tidak load full profile!
console.log('Basic info:', profile.getBasicInfo());
console.log('Profile loaded?', profile.realProfile ? 'Yes' : 'No'); // No!

// Akses full profile — trigger load
console.log('\nFull profile:', profile.getFullProfile());
console.log('Profile loaded?', profile.realProfile ? 'Yes' : 'No'); // Yes!

// Lihat access log
console.log('\nAccess history:', profile.getAccessLog());
```

### 4.5 Composite — Tree Structure

```javascript
/**
 * Composite Pattern
 * 
 * Masalah: Struktur organisasi hirarkis
 * - CEO
 *   - VP Engineering
 *     - Team Lead Backend
 *       - Dev 1, Dev 2
 *     - Team Lead Frontend
 *   - VP Sales
 *     - Regional Manager
 * 
 * Bisa calculate gaji, generate report, dll dengan interface yang sama!
 */

// Component
class Employee {
    constructor(name, salary) {
        this.name = name;
        this.salary = salary;
    }
    
    getSalary() { return this.salary; }
    getName() { return this.name; }
    getTeamSize() { return 1; } // leaf = 1 orang
    printOrg(prefix = '') { console.log(prefix + this.name); }
    getReport() { return { name: this.name, salary: this.salary, reports: [] }; }
}

// Composite
class Manager extends Employee {
    constructor(name, salary) {
        super(name, salary);
        this.subordinates = [];
    }
    
    addSubordinate(employee) {
        this.subordinates.push(employee);
        return this;
    }
    
    removeSubordinate(employee) {
        this.subordinates = this.subordinates.filter(e => e !== employee);
    }
    
    getSalary() {
        return this.salary + this.subordinates.reduce((sum, e) => sum + e.getSalary(), 0);
    }
    
    getTeamSize() {
        return 1 + this.subordinates.reduce((sum, e) => sum + e.getTeamSize(), 0);
    }
    
    printOrg(prefix = '') {
        console.log(prefix + this.name + ' (Manager)');
        this.subordinates.forEach((e, i) => {
            const isLast = i === this.subordinates.length - 1;
            e.printOrg(prefix + (isLast ? '└── ' : '├── '));
        });
    }
    
    getReport() {
        return {
            name: this.name,
            salary: this.salary,
            teamSize: this.getTeamSize(),
            totalCompensation: this.getSalary(),
            reports: this.subordinates.map(e => e.getReport()),
        };
    }
}

console.log('\n=== Composite Pattern: Organization ===');

const ceo = new Manager('Chairma', 50000000);

const vpEng = new Manager('VP Engineering', 30000000);
vpEng.addSubordinate(new Employee('Backend Lead', 10000000))
     .addSubordinate(new Employee('Frontend Lead', 10000000));

const vpSales = new Manager('VP Sales', 25000000);
vpSales.addSubordinate(new Employee('Regional Mgr 1', 8000000))
       .addSubordinate(new Employee('Regional Mgr 2', 8000000));

ceo.addSubordinate(vpEng).addSubordinate(vpSales);

ceo.printOrg();

const report = ceo.getReport();
console.log('\nOrganization Report:');
console.log(`Total salary cost: Rp ${report.totalCompensation.toLocaleString('id-ID')}`);
console.log(`Total team size: ${report.teamSize} people`);
```

## 5. Studi Kasus: Order Processing Architecture Tokopedia

**Adapter:** Integrasi dengan payment gateway berbeda (Midtrans, GoPay, OVO)
**Decorator:** Order dengan optional services (asuransi, gift wrapping, expedited shipping)
**Facade:** Checkout process yang kompleks disederhanakan
**Proxy:** Lazy-load detailed seller profile, buyer history
**Composite:** Nested order (main order + sub-orders untuk multi-seller)

## 6. Visualisasi

```
ADAPTER: API Wrapping
─────────────────────
Client → [Target Interface]
         ↓
      Adapter → [Adaptee API]

DECORATOR: Layering Behavior
───────────────────────────
Component
  ↓
OrderDecorator(Order) + Discount
  ↓
OrderDecorator(OrderDecorator) + Tax
  ↓
OrderDecorator(OrderDecorator) + Insurance

FACADE: Simplification
──────────────────────────
Client → [Checkout Facade]
         ├→ InventoryService
         ├→ PaymentService
         ├→ ShippingService
         ├→ OrderService
         └→ NotificationService

PROXY: Controlled Access
───────────────────────
Client → UserProfileProxy
         ├→ getBasicInfo() → direct, no load
         └→ getFullProfile() → lazy-load real

COMPOSITE: Tree Structure
─────────────────────────
          CEO
         /   \
      VP1     VP2
     /  \    /
  TL1  TL2 RM1
 / |
D1 D2
```

## 7. Kesalahan Umum

### ❌ Decorator vs Wrapper yang tidak jelas

```javascript
// BENAR — Decorator wrap dan delegate dengan tambahan behavior
class Decorator {
    constructor(component) {
        this.component = component;
    }
    
    operation() {
        console.log('Before');
        const result = this.component.operation(); // delegate
        console.log('After');
        return result;
    }
}

// SALAH — hanya wrap tanpa delegasi (bukan decorator, hanya wrapper)
class BadDecorator {
    constructor(component) {
        this.component = component;
    }
    
    operation() {
        console.log('Before'); // tapi tidak panggil component!
        return 'fixed result'; // SALAH!
    }
}
```

### ❌ Composite yang lupa leaf vs composite

```javascript
// BENAR — interface sama, behavior beda
class Node {
    getSize() { return 1; } // leaf default
}

class LeafNode extends Node {}

class CompositeNode extends Node {
    constructor() { super(); this.children = []; }
    getSize() {
        return 1 + this.children.reduce((sum, c) => sum + c.getSize(), 0); // aggregate
    }
}

// SALAH — menambahkan children ke leaf
class Node {
    constructor() {
        this.children = []; // leaf tidak perlu children!
    }
    getSize() {
        return this.children.length; // berantakan
    }
}
```

## 8. Latihan

### Latihan 1 — Adapter untuk Database

Buat adapter untuk 2 database library yang berbeda API.

### Latihan 2 — Composite File System

Implementasi directory dan file dalam struktur composite.

```javascript
class FileSystemItem {
    constructor(name) { this.name = name; }
    getSize() { throw new Error('Abstract'); }
    delete() { throw new Error('Abstract'); }
}

class File extends FileSystemItem {
    constructor(name, size) { super(name); this.size = size; }
    getSize() { return this.size; }
    delete() { console.log(`Deleting file: ${this.name}`); }
}

class Directory extends FileSystemItem {
    constructor(name) { super(name); this.items = []; }
    add(item) { this.items.push(item); return this; }
    getSize() { return this.items.reduce((sum, item) => sum + item.getSize(), 0); }
    delete() { this.items.forEach(i => i.delete()); }
}
```

## 9. Ringkasan

| Pattern | Saat Digunakan | Benefit |
|---------|--------|---------|
| Adapter | API incompatible | Integration tanpa modifikasi kode asli |
| Decorator | Tambah behavior dinamis | Fleksibel, komposisi bebas |
| Facade | Simplify kompleks | Client tidak perlu tahu details |
| Proxy | Lazy load, access control | Resource efficient, security |
| Composite | Tree structure | Interface sama untuk leaf dan composite |

**Golden Rule:** Gunakan composition (Structural Patterns) lebih dari inheritance.

## 10. Referensi

- GoF Design Patterns (Bab Structural)
- Refactoring.guru — [Structural Patterns](https://refactoring.guru)
- JavaScript Patterns — Advanced Techniques
