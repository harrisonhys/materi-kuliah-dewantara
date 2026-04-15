# Pertemuan 10: Design Patterns — Creational

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Memahami tujuan dan konteks penggunaan Creational Design Patterns dari GoF
- Mengimplementasikan Singleton, Factory Method, Abstract Factory, Builder, dan Prototype dalam JavaScript ES6+
- Membedakan kapan menggunakan setiap pattern berdasarkan masalah yang dihadapi
- Menerapkan pattern dalam konteks aplikasi fintech/e-commerce nyata

## 2. Pengantar: Hook

Bayangkan tim engineering Tokopedia harus membuat sistem payment yang mendukung 5 payment provider berbeda (GoPay, OVO, Dana, Kartu Kredit, Bank Transfer), tapi kode yang memanggil payment tidak boleh tahu provider mana yang digunakan.

Atau: GoPay memiliki object "User Session" yang harus persis SATU instance di seluruh aplikasi — karena dua session berbeda = security disaster.

Ini bukan masalah algoritma — tapi masalah **desain objek**. Creational Patterns adalah jawaban terstruktur yang telah teruji oleh ribuan engineer sebelum kamu.

Gang of Four (GoF) — buku "Design Patterns: Elements of Reusable Object-Oriented Software" (1994) — mendokumentasikan 23 patterns yang masih relevan 30 tahun kemudian.

## 3. Konsep Utama

### 3.1 Mengapa Design Patterns?

**Pattern** = solusi terulang untuk masalah desain yang sering muncul, bukan kode yang bisa copy-paste langsung.

**Tiga keuntungan:**
1. **Vocabulary bersama:** "Gunakan Singleton untuk config" lebih cepat dari menjelaskan implementasinya
2. **Proven solution:** Sudah teruji di ribuan proyek
3. **Maintainability:** Tim baru lebih mudah memahami kode

### 3.2 Creational Patterns — Overview

| Pattern | Masalah | Solusi |
|---------|---------|--------|
| Singleton | Butuh tepat satu instance global | Class yang mengontrol instansiasi dirinya sendiri |
| Factory Method | Buat objek tanpa tahu class-nya | Delegasikan ke subclass/function |
| Abstract Factory | Keluarga objek yang saling terkait | Interface untuk membuat family of objects |
| Builder | Objek kompleks dengan banyak konfigurasi | Proses pembangunan step-by-step |
| Prototype | Kloning objek yang mahal dibuat | Clone dari prototype existing |

## 4. Ilustrasi dan Analogi

### Singleton: Presiden Perusahaan
Sebuah perusahaan hanya punya SATU CEO. Bagaimanapun cara kamu "meminta" CEO, selalu dapatkan orang yang sama. Tidak bisa ada dua CEO secara bersamaan.

### Factory Method: Tukang Kue
"Saya mau kue coklat" — kamu tidak perlu tahu resepnya. Tukang kue (factory) yang tahu cara membuatnya.

### Abstract Factory: Toko Furnitur
IKEA menjual "keluarga" produk dalam gaya yang konsisten (Modern/Scandinavian/Industrial). Kamu pilih gaya, IKEA "factory" menghasilkan kursi, meja, lemari yang semua matching.

### Builder: Barista Kopi
"Satu Americano, ukuran Large, dua shot espresso, tambah susu oat, no sugar" — barista (builder) membangun kopi secara langkah demi langkah berdasarkan spesifikasi kamu.

### Prototype: Fotokopi Dokumen
Daripada menulis ulang dokumen 100 halaman, kamu fotokopi — lalu modifikasi sedikit bagian yang berbeda.

## 5. Contoh Teknis

### 5.1 Singleton — Session Manager GoPay

```javascript
/**
 * Singleton Pattern
 * 
 * Masalah: UserSession harus tepat SATU instance di seluruh app.
 * Dua session = data inconsistency = security risk.
 * 
 * JavaScript: module system sudah natural Singleton!
 * Tapi perlu handle kasus class yang diinstansiasi berkali-kali.
 */

class UserSession {
    static #instance = null;  // private static field (ES2022)
    
    #userId;
    #token;
    #cart;
    #loginTime;
    
    constructor() {
        if (UserSession.#instance) {
            throw new Error('UserSession: Gunakan UserSession.getInstance()');
        }
        
        this.#userId = null;
        this.#token = null;
        this.#cart = [];
        this.#loginTime = null;
    }
    
    static getInstance() {
        if (!UserSession.#instance) {
            UserSession.#instance = new UserSession();
        }
        return UserSession.#instance;
    }
    
    // Reset untuk testing
    static resetInstance() {
        UserSession.#instance = null;
    }
    
    login(userId, token) {
        this.#userId = userId;
        this.#token = token;
        this.#loginTime = new Date();
        console.log(`User ${userId} logged in at ${this.#loginTime.toISOString()}`);
    }
    
    addToCart(item) {
        this.#cart.push(item);
    }
    
    getCart() { return [...this.#cart]; }
    
    isLoggedIn() { return this.#token !== null; }
    
    getUserId() { return this.#userId; }
    
    logout() {
        this.#userId = null;
        this.#token = null;
        this.#cart = [];
        this.#loginTime = null;
        console.log('User logged out');
    }
    
    getSessionInfo() {
        return {
            userId: this.#userId,
            cartItems: this.#cart.length,
            loginDuration: this.#loginTime 
                ? Math.round((new Date() - this.#loginTime) / 1000) + 's'
                : null,
        };
    }
}

// Test
const session1 = UserSession.getInstance();
const session2 = UserSession.getInstance();

console.log('Same instance:', session1 === session2); // true

session1.login('user123', 'jwt-token-abc');
session2.addToCart({ name: 'Laptop', price: 8000000 }); // session1 dan session2 SAMA!

console.log('Session info:', session1.getSessionInfo());
console.log('Cart from session2:', session2.getCart()); // [Laptop]

// Lebih idiomatic JavaScript — Module Singleton
// config.js (module singleton — otomatis singleton karena Node.js caches modules)
const AppConfig = (() => {
    let instance = null;
    
    class Config {
        constructor() {
            this.environment = process.env.NODE_ENV || 'development';
            this.apiBaseUrl = process.env.API_URL || 'http://localhost:3000';
            this.paymentGatewayKey = process.env.PAYMENT_KEY || 'sandbox_key';
            this.maxRetries = 3;
        }
        
        get isDevelopment() { return this.environment === 'development'; }
        get isProduction() { return this.environment === 'production'; }
    }
    
    return {
        getInstance: () => {
            if (!instance) instance = new Config();
            return instance;
        }
    };
})();

const config = AppConfig.getInstance();
console.log('\nApp Config:', config.environment);
console.log('Is Production:', config.isProduction);
```

### 5.2 Factory Method — Payment Gateway

```javascript
/**
 * Factory Method Pattern
 * 
 * Masalah: Kode payment harus mendukung GoPay, OVO, Dana, Kartu Kredit
 * tanpa if-else yang panjang, dan mudah ditambah provider baru.
 * 
 * Factory Method: definisikan interface, setiap concrete class buat payment-nya sendiri
 */

// Product Interface (via duck typing / base class)
class PaymentProvider {
    constructor(config) {
        if (new.target === PaymentProvider) {
            throw new Error('PaymentProvider adalah abstract class');
        }
        this.config = config;
    }
    
    async pay(amount, details) {
        throw new Error('pay() harus diimplementasikan');
    }
    
    async refund(transactionId, amount) {
        throw new Error('refund() harus diimplementasikan');
    }
    
    async getStatus(transactionId) {
        throw new Error('getStatus() harus diimplementasikan');
    }
    
    // Template method — sama untuk semua provider
    async processPayment(amount, details) {
        console.log(`\n[${this.constructor.name}] Processing payment...`);
        this.validateAmount(amount);
        
        const result = await this.pay(amount, details);
        
        console.log(`[${this.constructor.name}] Transaction ID: ${result.transactionId}`);
        return result;
    }
    
    validateAmount(amount) {
        if (amount <= 0) throw new Error('Amount must be positive');
        if (amount > 50000000) throw new Error('Amount exceeds limit');
    }
}

// Concrete Products
class GoPayProvider extends PaymentProvider {
    async pay(amount, details) {
        // Simulasi GoPay API call
        console.log(`  GoPay: calling api.gopay.com/v1/charge`);
        await new Promise(r => setTimeout(r, 100)); // simulasi network
        return {
            transactionId: `GPY-${Date.now()}`,
            status: 'SUCCESS',
            amount,
            provider: 'GoPay',
            details: { phone: details.phone, message: 'GoPay payment success' }
        };
    }
    
    async refund(transactionId, amount) {
        console.log(`GoPay: Refunding ${amount} for ${transactionId}`);
        return { status: 'REFUNDED', amount };
    }
    
    async getStatus(transactionId) {
        return { transactionId, status: 'SUCCESS' };
    }
}

class OVOProvider extends PaymentProvider {
    async pay(amount, details) {
        console.log(`  OVO: calling api.ovo.id/payment`);
        await new Promise(r => setTimeout(r, 150));
        return {
            transactionId: `OVO-${Date.now()}`,
            status: 'SUCCESS',
            amount,
            provider: 'OVO',
            details: { phone: details.phone, cashback: amount * 0.05 }
        };
    }
    
    async refund(transactionId, amount) {
        return { status: 'REFUNDED', amount, timeline: '3-5 hari kerja' };
    }
    
    async getStatus(transactionId) {
        return { transactionId, status: 'SUCCESS' };
    }
}

class CreditCardProvider extends PaymentProvider {
    async pay(amount, details) {
        console.log(`  Credit Card: calling gateway.midtrans.com/v2/charge`);
        await new Promise(r => setTimeout(r, 200));
        return {
            transactionId: `CC-${Date.now()}`,
            status: 'SUCCESS',
            amount,
            provider: 'CreditCard',
            details: { 
                masked_card: `****${details.cardNumber.slice(-4)}`,
                bank: details.bank 
            }
        };
    }
    
    async refund(transactionId, amount) {
        return { status: 'REFUNDED', amount, timeline: '7-14 hari kerja' };
    }
    
    async getStatus(transactionId) {
        return { transactionId, status: 'SUCCESS' };
    }
}

// Factory — Factory Method
class PaymentFactory {
    static create(providerType, config = {}) {
        const providers = {
            'gopay': GoPayProvider,
            'ovo': OVOProvider,
            'credit_card': CreditCardProvider,
        };
        
        const ProviderClass = providers[providerType.toLowerCase()];
        
        if (!ProviderClass) {
            throw new Error(`Unknown payment provider: ${providerType}. Available: ${Object.keys(providers).join(', ')}`);
        }
        
        return new ProviderClass(config);
    }
    
    static getAvailableProviders() {
        return ['gopay', 'ovo', 'credit_card'];
    }
}

// Usage — kode ini tidak tahu provider apa yang dipakai!
async function processOrder(order) {
    const provider = PaymentFactory.create(order.paymentMethod, {
        merchantId: 'tokopedia-123'
    });
    
    const result = await provider.processPayment(order.amount, order.paymentDetails);
    
    // Simpan ke database...
    console.log(`Order ${order.id} paid: ${JSON.stringify(result, null, 2)}`);
    return result;
}

// Test
(async () => {
    const orders = [
        {
            id: 'ORD001', amount: 150000, paymentMethod: 'gopay',
            paymentDetails: { phone: '08123456789' }
        },
        {
            id: 'ORD002', amount: 500000, paymentMethod: 'credit_card',
            paymentDetails: { cardNumber: '1234567890123456', bank: 'BCA' }
        },
        {
            id: 'ORD003', amount: 75000, paymentMethod: 'ovo',
            paymentDetails: { phone: '081234567' }
        },
    ];
    
    for (const order of orders) {
        await processOrder(order);
    }
})();
```

### 5.3 Abstract Factory — UI Theme

```javascript
/**
 * Abstract Factory Pattern
 * 
 * Masalah: Tokopedia punya mode Buyer dan Seller.
 * Setiap mode punya UI components yang berbeda tapi saling terkait:
 * - Buyer mode: ProductCard, CartButton, SearchBar (buyer-specific)
 * - Seller mode: ProductCard, AddProductButton, AnalyticsDashboard
 * 
 * Abstract Factory menghasilkan "keluarga" komponen yang konsisten
 */

// Abstract Factory Interface
class UIFactory {
    createProductCard() { throw new Error('Abstract'); }
    createActionButton() { throw new Error('Abstract'); }
    createSearchBar() { throw new Error('Abstract'); }
    
    // Template untuk render full page
    renderHomePage() {
        const card = this.createProductCard();
        const button = this.createActionButton();
        const search = this.createSearchBar();
        
        return {
            searchBar: search.render(),
            featuredProduct: card.render({ name: 'Laptop Gaming', price: 10000000 }),
            mainAction: button.render(),
        };
    }
}

// Buyer UI Factory
class BuyerUIFactory extends UIFactory {
    createProductCard() {
        return {
            render: (product) => ({
                type: 'BuyerProductCard',
                content: `${product.name} - Rp ${product.price.toLocaleString('id-ID')}`,
                actions: ['Add to Cart', 'Add to Wishlist', 'Compare'],
                showRating: true,
                showReviews: true,
            })
        };
    }
    
    createActionButton() {
        return {
            render: () => ({
                type: 'CartButton',
                label: 'Beli Sekarang',
                icon: '🛒',
                action: 'openCart',
            })
        };
    }
    
    createSearchBar() {
        return {
            render: () => ({
                type: 'BuyerSearchBar',
                placeholder: 'Cari produk, toko, atau brand...',
                filters: ['Kategori', 'Harga', 'Rating', 'Lokasi'],
                suggestions: true,
            })
        };
    }
}

// Seller UI Factory
class SellerUIFactory extends UIFactory {
    createProductCard() {
        return {
            render: (product) => ({
                type: 'SellerProductCard',
                content: `${product.name} - Stock: 50 | Sold: 200`,
                actions: ['Edit', 'Delete', 'Boost Product', 'View Analytics'],
                showAnalytics: true,
                showStock: true,
            })
        };
    }
    
    createActionButton() {
        return {
            render: () => ({
                type: 'AddProductButton',
                label: '+ Tambah Produk',
                icon: '📦',
                action: 'openProductForm',
            })
        };
    }
    
    createSearchBar() {
        return {
            render: () => ({
                type: 'SellerSearchBar',
                placeholder: 'Cari produk di toko kamu...',
                filters: ['Status', 'Kategori', 'Stok'],
                suggestions: false,
            })
        };
    }
}

// Client code — tidak tahu Buyer atau Seller factory!
function renderApp(factory) {
    console.log('=== UI Rendering ===');
    const page = factory.renderHomePage();
    console.log('Search Bar:', page.searchBar);
    console.log('Featured Product:', page.featuredProduct);
    console.log('Main Action:', page.mainAction);
}

// Router yang memilih factory berdasarkan user role
function getUIFactory(userRole) {
    switch (userRole) {
        case 'buyer': return new BuyerUIFactory();
        case 'seller': return new SellerUIFactory();
        default: throw new Error(`Unknown role: ${userRole}`);
    }
}

console.log('\n--- BUYER UI ---');
renderApp(getUIFactory('buyer'));

console.log('\n--- SELLER UI ---');
renderApp(getUIFactory('seller'));
```

### 5.4 Builder — Transaksi Kompleks

```javascript
/**
 * Builder Pattern
 * 
 * Masalah: Membuat objek Transaction yang punya banyak fields opsional.
 * Constructor dengan 15 parameter = developer nightmare.
 * 
 * Builder: membangun objek step-by-step dengan method chaining.
 */

class Transaction {
    constructor(builder) {
        // Required
        this.id = builder.id;
        this.amount = builder.amount;
        this.currency = builder.currency;
        this.fromAccount = builder.fromAccount;
        this.toAccount = builder.toAccount;
        
        // Optional
        this.description = builder.description;
        this.reference = builder.reference;
        this.metadata = builder.metadata;
        this.retryCount = builder.retryCount;
        this.priority = builder.priority;
        this.scheduledAt = builder.scheduledAt;
        this.expiresAt = builder.expiresAt;
        this.notifyRecipient = builder.notifyRecipient;
        this.idempotencyKey = builder.idempotencyKey;
        
        // Auto-generated
        this.createdAt = new Date();
        this.status = 'PENDING';
    }
    
    toString() {
        return `Transaction(${this.id}: ${this.amount} ${this.currency} from ${this.fromAccount} to ${this.toAccount})`;
    }
}

class TransactionBuilder {
    constructor(id, amount, currency) {
        // Required params di constructor
        this.id = id;
        this.amount = amount;
        this.currency = currency;
        
        // Default values
        this.retryCount = 3;
        this.priority = 'NORMAL';
        this.notifyRecipient = true;
        this.metadata = {};
    }
    
    from(accountId) { this.fromAccount = accountId; return this; }
    to(accountId) { this.toAccount = accountId; return this; }
    withDescription(desc) { this.description = desc; return this; }
    withReference(ref) { this.reference = ref; return this; }
    withMetadata(meta) { this.metadata = { ...this.metadata, ...meta }; return this; }
    withRetryCount(n) { this.retryCount = n; return this; }
    withPriority(p) { this.priority = p; return this; }
    scheduleAt(date) { this.scheduledAt = date; return this; }
    expiresAt(date) { this.expiresAt = date; return this; }
    silent() { this.notifyRecipient = false; return this; }
    withIdempotencyKey(key) { this.idempotencyKey = key; return this; }
    
    build() {
        // Validasi
        if (!this.fromAccount) throw new Error('Transaction: fromAccount is required');
        if (!this.toAccount) throw new Error('Transaction: toAccount is required');
        if (this.amount <= 0) throw new Error('Transaction: amount must be positive');
        
        // Idempotency key default
        if (!this.idempotencyKey) {
            this.idempotencyKey = `${this.fromAccount}-${this.toAccount}-${this.amount}-${Date.now()}`;
        }
        
        return new Transaction(this);
    }
}

// Usage — sangat readable!
const transfer = new TransactionBuilder('TXN-001', 500000, 'IDR')
    .from('ACC-123')
    .to('ACC-456')
    .withDescription('Pembayaran sewa kost Januari 2026')
    .withReference('RENT-JAN-2026')
    .withMetadata({ category: 'rent', landlordName: 'Pak Budi' })
    .withPriority('HIGH')
    .withIdempotencyKey('idempotent-key-abc-123')
    .build();

console.log('\n=== Builder: Transaction ===');
console.log(transfer.toString());
console.log('Idempotency key:', transfer.idempotencyKey);
console.log('Metadata:', transfer.metadata);

// Cashback transaction (lebih sederhana)
const cashback = new TransactionBuilder('CASHBACK-001', 10000, 'IDR')
    .from('SYSTEM')
    .to('ACC-123')
    .withDescription('Cashback pembelian Alfamart')
    .silent() // tidak perlu notifikasi
    .withRetryCount(1)
    .build();

console.log('\nCashback:', cashback.toString());
console.log('Notify recipient:', cashback.notifyRecipient); // false
```

### 5.5 Prototype — User Profile

```javascript
/**
 * Prototype Pattern
 * 
 * Masalah: Membuat user profile "standard" lalu kloning untuk variasi.
 * Pembuatan objek mahal (API calls, DB queries) → clone yang sudah ada.
 * 
 * JavaScript: Object.create(), spread operator, dan structuredClone() adalah natural prototype.
 */

class UserProfile {
    constructor(data) {
        this.id = data.id;
        this.name = data.name;
        this.email = data.email;
        this.preferences = { ...data.preferences }; // shallow copy sudah cukup
        this.permissions = [...data.permissions];
        this.subscription = { ...data.subscription };
        this.createdAt = data.createdAt || new Date();
    }
    
    // Prototype method — clone dan modifikasi
    clone() {
        return new UserProfile({
            ...this,
            id: `copy-${this.id}-${Date.now()}`, // baru ID
            preferences: { ...this.preferences }, // deep copy untuk object
            permissions: [...this.permissions],    // deep copy untuk array
            subscription: { ...this.subscription },
            createdAt: new Date(), // reset created time
        });
    }
    
    // Clone dengan override
    cloneWith(overrides) {
        const cloned = this.clone();
        Object.assign(cloned, overrides);
        
        if (overrides.preferences) {
            cloned.preferences = { ...this.preferences, ...overrides.preferences };
        }
        if (overrides.permissions) {
            cloned.permissions = [...new Set([...this.permissions, ...overrides.permissions])];
        }
        
        return cloned;
    }
    
    toString() {
        return `UserProfile(${this.id}: ${this.name}, tier: ${this.subscription.tier})`;
    }
}

// Template profiles untuk berbagai tipe pengguna Tokopedia
const standardBuyerTemplate = new UserProfile({
    id: 'template-buyer',
    name: 'Template Buyer',
    email: null,
    preferences: {
        notifications: { push: true, email: true, sms: false },
        language: 'id',
        currency: 'IDR',
        darkMode: false,
    },
    permissions: ['read:products', 'create:order', 'read:order'],
    subscription: { tier: 'FREE', monthlyLimit: 1000000, cashbackRate: 0 },
});

const premiumBuyerTemplate = standardBuyerTemplate.cloneWith({
    preferences: { notifications: { push: true, email: true, sms: true } },
    permissions: ['read:analytics', 'priority:support'],
    subscription: { tier: 'PREMIUM', monthlyLimit: 50000000, cashbackRate: 0.05 },
});

const sellerTemplate = standardBuyerTemplate.cloneWith({
    permissions: ['create:product', 'read:analytics', 'manage:store'],
    subscription: { tier: 'SELLER', monthlyLimit: 0, storeFee: 0.01 },
});

// Registrasi user baru — kloning template
function registerUser(name, email, role = 'buyer') {
    const template = {
        'buyer': standardBuyerTemplate,
        'premium': premiumBuyerTemplate,
        'seller': sellerTemplate,
    }[role];
    
    if (!template) throw new Error(`Unknown role: ${role}`);
    
    return template.cloneWith({
        id: `user-${Date.now()}`,
        name,
        email,
    });
}

const buyer1 = registerUser('Budi Santoso', 'budi@email.com', 'buyer');
const buyer2 = registerUser('Sari Dewi', 'sari@email.com', 'premium');
const seller1 = registerUser('Ahmad Toko', 'ahmad@toko.com', 'seller');

console.log('\n=== Prototype: User Registration ===');
console.log(buyer1.toString());
console.log('Buyer1 cashback rate:', buyer1.subscription.cashbackRate); // 0

console.log(buyer2.toString());
console.log('Buyer2 cashback rate:', buyer2.subscription.cashbackRate); // 0.05

console.log(seller1.toString());
console.log('Seller permissions:', seller1.permissions);

// Verifikasi deep copy
buyer1.preferences.darkMode = true;
console.log('\nBuyer1 darkMode:', buyer1.preferences.darkMode); // true
console.log('Template darkMode:', standardBuyerTemplate.preferences.darkMode); // false (tidak terpengaruh!)
```

## 6. Studi Kasus Nyata: Tokopedia Payment Architecture

**Singleton:** `PaymentConfig` — satu instance berisi API keys, timeout, retry policy untuk semua payment operations.

**Factory Method:** `PaymentProviderFactory.create(type)` — sistem routing yang memilih GoPay/OVO/DANA/Kartu berdasarkan metode yang dipilih user.

**Abstract Factory:** `UIComponentFactory` — dua factory berbeda (MobileFactory untuk app, WebFactory untuk browser) menghasilkan set komponen yang konsisten.

**Builder:** `OrderBuilder` — membangun Order object yang kompleks (produk, alamat, voucher, payment method, catatan, asuransi, dll.) step-by-step.

**Prototype:** Template untuk tipe merchant (Official Store vs Power Merchant vs Regular) — setiap merchant baru di-clone dari template yang sesuai.

## 7. Visualisasi

### Pattern Relationships

```
Creational Patterns — Tujuan Utama:

SINGLETON         → 1 instance saja
  Use when: Config, Logger, Session, Cache

FACTORY METHOD    → Delegasikan pembuatan ke subclass
  Use when: Tidak tahu tipe object saat runtime

ABSTRACT FACTORY → Keluarga object yang konsisten
  Use when: Perlu set komponen yang saling terkait (UI themes, DB drivers)

BUILDER          → Object kompleks step-by-step
  Use when: Banyak field opsional, urutan konstruksi penting

PROTOTYPE        → Clone dari existing
  Use when: Pembuatan mahal, banyak variasi dari base object
```

### Builder vs Constructor

```javascript
// Constructor dengan banyak parameter — 😰
const tx = new Transaction(
    'TXN-001', 500000, 'IDR', 'ACC-123', 'ACC-456',
    'Sewa kost', 'REF-001', { category: 'rent' }, 
    3, 'HIGH', null, null, true, 'idem-key-xyz'
);

// Builder — 😊
const tx = new TransactionBuilder('TXN-001', 500000, 'IDR')
    .from('ACC-123')
    .to('ACC-456')
    .withDescription('Sewa kost')
    .withPriority('HIGH')
    .build();
```

## 8. Kesalahan Umum

### ❌ Singleton yang Tidak Thread-Safe (Node.js Cluster Mode)

```javascript
// MASALAH: Singleton tidak aman di multi-process environments!
// Node.js child_process.fork() → SETIAP child dapat instance SENDIRI
// → "Singleton" tidak lagi singleton di level OS

// SOLUSI: Gunakan Redis/external store untuk state yang benar-benar shared
// Singleton di Node.js = singleton per PROCESS, bukan per server
```

### ❌ Factory yang Terlalu Banyak Switch/If

```javascript
// SALAH — O(n) untuk setiap provider baru, violasi Open/Closed Principle
class PaymentFactory {
    static create(type) {
        if (type === 'gopay') return new GoPayProvider();
        if (type === 'ovo') return new OVOProvider();
        if (type === 'dana') return new DanaProvider();
        // Tambah provider baru = ubah kode di sini → OCP violation!
    }
}

// BENAR — registry pattern, Open for extension, Closed for modification
class PaymentFactory {
    static #registry = {};
    
    static register(type, ProviderClass) {
        this.#registry[type] = ProviderClass;
    }
    
    static create(type, config) {
        const Provider = this.#registry[type];
        if (!Provider) throw new Error(`Unknown provider: ${type}`);
        return new Provider(config);
    }
}

// Register sekali saat startup:
PaymentFactory.register('gopay', GoPayProvider);
PaymentFactory.register('ovo', OVOProvider);
// Tambah provider baru TANPA modifikasi Factory!
PaymentFactory.register('dana', DanaProvider);
```

### ❌ Builder Tanpa Validasi di build()

```javascript
// SALAH — object bisa invalid
class TransactionBuilderBad {
    build() {
        return new Transaction(this); // tidak ada validasi!
    }
}

// BENAR — validasi semua invariants di build()
class TransactionBuilderGood {
    build() {
        if (!this.fromAccount) throw new Error('fromAccount required');
        if (!this.toAccount) throw new Error('toAccount required');
        if (this.amount <= 0) throw new Error('amount must be positive');
        if (this.fromAccount === this.toAccount) throw new Error('Cannot transfer to same account');
        
        return new Transaction(this); // guaranteed valid
    }
}
```

### ❌ Prototype yang Shallow Copy Object Bersarang

```javascript
// MASALAH: nested objects masih shared reference!
class UserProfileBroken {
    clone() {
        return new UserProfileBroken({
            ...this,
            // preferences adalah object → spread hanya shallow!
        });
    }
}

const original = new UserProfileBroken({ preferences: { darkMode: false, nested: { value: 1 } } });
const cloned = original.clone();

cloned.preferences.nested.value = 999;
console.log(original.preferences.nested.value); // 999 — IKUT BERUBAH! Bug!

// BENAR — deep clone untuk nested objects
class UserProfileGood {
    clone() {
        return new UserProfileGood({
            ...this,
            preferences: JSON.parse(JSON.stringify(this.preferences)), // deep clone
            // atau structuredClone(this.preferences) di Node.js 17+
        });
    }
}
```

## 9. Latihan dan Studi Kasus

### Latihan 1 — Identifikasi Pattern

Untuk setiap situasi, identifikasi pattern yang paling tepat:

1. Logger yang mencatat semua aktivitas, harus satu instance di seluruh aplikasi
2. Membuat report (PDF/Excel/CSV) tanpa tahu format apa yang akan digunakan saat runtime
3. Membuat User object dengan 20+ field opsional
4. Sistem tema UI: Dark mode dan Light mode, masing-masing punya Button, Input, Card yang konsisten
5. Membuat 1000 "enemy" di game dengan atribut yang hampir sama

**Jawaban:** 1=Singleton, 2=Factory Method, 3=Builder, 4=Abstract Factory, 5=Prototype

### Latihan 2 — Implementasi Logger Singleton

```javascript
class Logger {
    static #instance = null;
    #logs = [];
    
    static getInstance() {
        if (!Logger.#instance) Logger.#instance = new Logger();
        return Logger.#instance;
    }
    
    log(level, message, context = {}) {
        const entry = {
            timestamp: new Date().toISOString(),
            level,
            message,
            context,
        };
        this.#logs.push(entry);
        const color = { INFO: '\x1b[34m', WARN: '\x1b[33m', ERROR: '\x1b[31m' }[level] || '';
        console.log(`${color}[${level}] ${entry.timestamp}: ${message}\x1b[0m`);
        return this;
    }
    
    info(msg, ctx) { return this.log('INFO', msg, ctx); }
    warn(msg, ctx) { return this.log('WARN', msg, ctx); }
    error(msg, ctx) { return this.log('ERROR', msg, ctx); }
    
    getLogs(level) {
        return level ? this.#logs.filter(l => l.level === level) : [...this.#logs];
    }
    
    clear() { this.#logs = []; }
}

// Usage
const log = Logger.getInstance();
log.info('Payment started', { orderId: 'ORD-001', amount: 50000 })
   .warn('Slow response from GoPay API', { latency: '2.3s' })
   .error('Payment failed', { reason: 'Insufficient balance' });

console.log('Error count:', Logger.getInstance().getLogs('ERROR').length); // 1
```

### Latihan 3 — HTTP Request Builder

Implementasikan `RequestBuilder` yang membangun HTTP request:

```javascript
class RequestBuilder {
    constructor(method, url) {
        this.method = method;
        this.url = url;
        this.headers = { 'Content-Type': 'application/json' };
        this.queryParams = {};
        this.body = null;
        this.timeout = 5000;
    }
    
    static GET(url) { return new RequestBuilder('GET', url); }
    static POST(url) { return new RequestBuilder('POST', url); }
    
    withHeader(key, value) { this.headers[key] = value; return this; }
    withAuth(token) { return this.withHeader('Authorization', `Bearer ${token}`); }
    withParam(key, value) { this.queryParams[key] = value; return this; }
    withBody(data) { this.body = data; return this; }
    withTimeout(ms) { this.timeout = ms; return this; }
    
    build() {
        const queryString = Object.entries(this.queryParams)
            .map(([k, v]) => `${k}=${encodeURIComponent(v)}`)
            .join('&');
        
        return {
            method: this.method,
            url: queryString ? `${this.url}?${queryString}` : this.url,
            headers: this.headers,
            body: this.body ? JSON.stringify(this.body) : undefined,
            timeout: this.timeout,
        };
    }
}

// Usage:
const request = RequestBuilder.POST('https://api.tokopedia.com/v2/orders')
    .withAuth('jwt-token-abc')
    .withHeader('X-Request-ID', 'req-123')
    .withBody({ productId: 'PROD-001', quantity: 2 })
    .withTimeout(10000)
    .build();

console.log(request);
```

## 10. Ringkasan

| Pattern | Intent | Key Indicator | JS Idiom |
|---------|--------|--------------|---------|
| Singleton | 1 instance global | "satu config/session/logger" | module exports, private static field |
| Factory Method | Delegasikan pembuatan | "tidak tahu tipe saat runtime" | switch/registry function |
| Abstract Factory | Keluarga yang konsisten | "set komponen yang matching" | class hierarchy |
| Builder | Object kompleks bertahap | "banyak field opsional" | method chaining |
| Prototype | Clone dan modifikasi | "banyak variasi dari base" | spread operator, structuredClone |

**Pilih pattern berdasarkan MASALAH, bukan pattern favorit.**

## 11. Referensi

- Gamma, E. et al. — *Design Patterns: Elements of Reusable Object-Oriented Software* (GoF), 1994
- Osmani, A. — *Learning JavaScript Design Patterns* (tersedia gratis online di addyosmani.com)
- Refactoring.guru — [Creational Patterns](https://refactoring.guru/design-patterns/creational-patterns) — visualisasi dan contoh sangat bagus
- MDN Web Docs — [JavaScript Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- Fowler, M. — *Patterns of Enterprise Application Architecture* — extended patterns untuk enterprise
- Netflix Tech Blog — "Design Patterns in Production" (tersedia online)
- LeetCode — Design problems: #155 (Min Stack = Singleton-like), #706 (Design HashMap)
