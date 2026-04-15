# Pertemuan 4: Database Relasional & ORM — Sequelize & Prisma

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Memahami relational database design (normalization, relationships)
- Menggunakan Sequelize ORM untuk CRUD operations dan complex queries
- Memahami dan menghindari N+1 problem
- Mengimplementasikan database migrations dengan Sequelize
- Membandingkan Sequelize vs Prisma dan memilih yang sesuai
- Mengoptimalkan query performance dengan indexing dan eager loading
- Mengelola database relationships (1-to-many, many-to-many)

## 2. Pengantar: Hook

Database adalah "jantung" dari aplikasi. Kalau design jelek, tambah user sedikit saja performa sudah drop. Kalau migration tidak terencana, rollback menjadi mimpi buruk.

Tokopedia mengelola **miliaran product records, transaksi harian**. Kalau query tidak optimal, search feature loading 30 detik. User kabur. Tapi dengan indexing yang tepat dan ORM yang bagus, query instant.

ORM (Object-Relational Mapping) adalah "translator" antara JavaScript objects dan database tables. Sequelize dan Prisma adalah dua pilihan populer. Mana yang lebih baik? Tergantung use case. Sequelize lebih mature dan flexible. Prisma lebih modern dan type-safe.

## 3. Konsep Utama

### 3.1 Relational Database Design

```
Table: Users
┌────┬───────┬───────────┬──────────┐
│ id │ name  │ email     │ role     │
├────┼───────┼───────────┼──────────┤
│ 1  │ Budi  │ b@tok.com │ buyer    │
│ 2  │ Rani  │ r@tok.com │ seller   │
└────┴───────┴───────────┴──────────┘

Table: Orders
┌───┬─────────┬──────┬────────┐
│id │user_id  │total │status  │
├───┼─────────┼──────┼────────┤
│101│    1    │100k  │shipped │
│102│    1    │200k  │pending │
│103│    2    │150k  │pending │
└───┴─────────┴──────┴────────┘

Relationship: User HAS MANY Orders
Foreign Key: Orders.user_id references Users.id
```

### 3.2 ORM Comparison Table

| Aspect | Sequelize | Prisma |
|--------|-----------|--------|
| **Learning Curve** | Medium | Easy |
| **Type Safety** | Basic | Excellent (TypeScript) |
| **Migration** | Manual SQL | Auto (prisma migrate) |
| **Query API** | JavaScript callbacks | Chainable, readable |
| **Performance** | Excellent | Excellent |
| **Community** | Large, mature | Growing, modern |
| **Best For** | Legacy projects, flexibility | New projects, type safety |

### 3.3 N+1 Problem

```javascript
// ❌ N+1 PROBLEM: 1 query for users + 100 queries for each user's orders
const users = await User.findAll(); // 1 query
users.forEach(user => {
    const orders = await Order.findAll({ where: { userId: user.id } }); // 100 queries!
});
// Total: 101 queries!

// ✅ SOLUTION: Use eager loading (1 query + 1 join)
const users = await User.findAll({
    include: [{ association: 'orders' }] // Load orders at once
});
// Total: 1 query with JOIN
```

### 3.4 Indexes

```javascript
// Without index: O(n) full table scan
SELECT * FROM products WHERE sku = 'PROD-123'; // scans all rows

// With index: O(log n) binary search
CREATE INDEX idx_sku ON products(sku); // fast lookup
SELECT * FROM products WHERE sku = 'PROD-123'; // instant

// Multi-column index
CREATE INDEX idx_user_date ON orders(user_id, created_at);
```

## 4. Ilustrasi & Analogi

**Analogi: Library System**

- **Database table:** Shelf of books
- **Primary key:** ISBN (unique identifier)
- **Foreign key:** "See related books on shelf 3" (relationship)
- **Index:** Card catalog (fast lookup)
- **N+1 problem:** Walk to shelf 1, read book. Walk back. Repeat for all 100 books. 101 walks!
- **Eager loading:** Get all books at once with a cart

## 5. Contoh Teknis

### 5.1 Sequelize Setup & Models

```javascript
// config/database.js
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize(
    process.env.DB_NAME,
    process.env.DB_USER,
    process.env.DB_PASSWORD,
    {
        host: process.env.DB_HOST,
        dialect: 'mysql',
        logging: false, // disable logs in production
        pool: { max: 10, min: 2 }
    }
);

module.exports = sequelize;

// models/User.js
const { DataTypes } = require('sequelize');
const sequelize = require('../config/database');

const User = sequelize.define('User', {
    id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true
    },
    name: {
        type: DataTypes.STRING(100),
        allowNull: false,
        validate: { len: [2, 100] }
    },
    email: {
        type: DataTypes.STRING(100),
        allowNull: false,
        unique: true,
        validate: { isEmail: true }
    },
    phone: {
        type: DataTypes.STRING(15),
        unique: true
    },
    role: {
        type: DataTypes.ENUM('buyer', 'seller', 'admin'),
        defaultValue: 'buyer'
    },
    balance: {
        type: DataTypes.DECIMAL(15, 2),
        defaultValue: 0
    }
}, {
    timestamps: true,
    indexes: [
        { fields: ['email'] },
        { fields: ['phone'] },
        { fields: ['role'] }
    ]
});

module.exports = User;

// models/Order.js
const { DataTypes } = require('sequelize');
const sequelize = require('../config/database');
const User = require('./User');

const Order = sequelize.define('Order', {
    id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true
    },
    userId: {
        type: DataTypes.INTEGER,
        allowNull: false,
        references: { model: User, key: 'id' }
    },
    totalPrice: {
        type: DataTypes.DECIMAL(15, 2),
        allowNull: false
    },
    status: {
        type: DataTypes.ENUM('pending', 'paid', 'shipped', 'delivered', 'cancelled'),
        defaultValue: 'pending'
    },
    shippingAddress: DataTypes.TEXT
}, {
    timestamps: true,
    indexes: [
        { fields: ['userId'] },
        { fields: ['status'] },
        { fields: ['createdAt'] }
    ]
});

module.exports = Order;

// models/index.js - Setup associations
const User = require('./User');
const Order = require('./Order');
const OrderItem = require('./OrderItem');
const Product = require('./Product');

// Associations
User.hasMany(Order, { foreignKey: 'userId', as: 'orders' });
Order.belongsTo(User, { foreignKey: 'userId', as: 'user' });

Order.hasMany(OrderItem, { foreignKey: 'orderId', as: 'items' });
OrderItem.belongsTo(Order, { foreignKey: 'orderId' });

Product.hasMany(OrderItem, { foreignKey: 'productId' });
OrderItem.belongsTo(Product, { foreignKey: 'productId', as: 'product' });

// Many-to-many through OrderItem
Order.belongsToMany(Product, { 
    through: OrderItem, 
    foreignKey: 'orderId',
    as: 'products'
});
Product.belongsToMany(Order, {
    through: OrderItem,
    foreignKey: 'productId'
});

module.exports = { User, Order, OrderItem, Product };
```

### 5.2 CRUD Operations

```javascript
// CREATE
const newUser = await User.create({
    name: 'Budi',
    email: 'budi@tokopedia.com',
    phone: '08123456789',
    role: 'seller'
});

// READ
const user = await User.findByPk(1); // Find by primary key

const userByEmail = await User.findOne({ 
    where: { email: 'budi@tokopedia.com' } 
});

const allUsers = await User.findAll({
    where: { role: 'seller' },
    limit: 10,
    offset: 0,
    order: [['createdAt', 'DESC']]
});

// UPDATE
await user.update({ balance: 5000000 });

// Or bulk update
await User.update(
    { role: 'premium' },
    { where: { balance: { [Op.gt]: 1000000 } } }
);

// DELETE
await user.destroy();

// Or bulk delete
await User.destroy({
    where: { createdAt: { [Op.lt]: new Date('2023-01-01') } }
});
```

### 5.3 Eager Loading & N+1 Prevention

```javascript
const { Op } = require('sequelize');

// ❌ N+1 PROBLEM
const users = await User.findAll();
for (const user of users) {
    user.orderCount = await Order.count({ where: { userId: user.id } });
    // N queries!
}

// ✅ SOLUTION 1: Eager Loading (include)
const users = await User.findAll({
    include: [{
        association: 'orders',
        attributes: ['id', 'totalPrice', 'status'],
        where: { status: 'delivered' }, // filter included data
        required: false // left join (include even if no orders)
    }]
});

// ✅ SOLUTION 2: Raw query with count
const users = await User.findAll({
    attributes: {
        include: [[sequelize.fn('COUNT', sequelize.col('orders.id')), 'orderCount']]
    },
    include: [{
        model: Order,
        attributes: [],
        required: false
    }],
    group: ['User.id'],
    subQuery: false
});

// ✅ SOLUTION 3: Nested include for deep relationships
const orders = await Order.findAll({
    include: [{
        association: 'user',
        attributes: ['name', 'email']
    }, {
        association: 'items',
        include: [{
            model: Product,
            as: 'product',
            attributes: ['sku', 'name', 'price']
        }]
    }]
});
```

### 5.4 Database Migrations

```javascript
// migrations/001-create-users-table.js
module.exports = {
    async up(queryInterface, DataTypes) {
        await queryInterface.createTable('Users', {
            id: {
                type: DataTypes.INTEGER,
                primaryKey: true,
                autoIncrement: true
            },
            name: {
                type: DataTypes.STRING(100),
                allowNull: false
            },
            email: {
                type: DataTypes.STRING(100),
                allowNull: false,
                unique: true
            },
            createdAt: {
                type: DataTypes.DATE,
                defaultValue: DataTypes.NOW
            },
            updatedAt: {
                type: DataTypes.DATE,
                defaultValue: DataTypes.NOW
            }
        });
        
        await queryInterface.addIndex('Users', ['email']);
    },
    
    async down(queryInterface) {
        await queryInterface.dropTable('Users');
    }
};

// Run migrations
// npx sequelize-cli db:migrate
// npx sequelize-cli db:migrate:undo
```

### 5.5 Prisma Alternative

```javascript
// prisma/schema.prisma
datasource db {
    provider = "mysql"
    url      = env("DATABASE_URL")
}

generator client {
    provider = "prisma-client-js"
}

model User {
    id    Int     @id @default(autoincrement())
    email String  @unique
    name  String
    role  String  @default("buyer")
    orders Order[]
    
    @@index([email])
}

model Order {
    id    Int     @id @default(autoincrement())
    userId Int
    user  User    @relation(fields: [userId], references: [id])
    total Decimal @db.Decimal(15, 2)
    items OrderItem[]
    
    @@index([userId])
}

model OrderItem {
    id      Int   @id @default(autoincrement())
    orderId Int
    order   Order @relation(fields: [orderId], references: [id])
    productId Int
    product Product @relation(fields: [productId], references: [id])
}

model Product {
    id    Int    @id @default(autoincrement())
    sku   String @unique
    name  String
    items OrderItem[]
}

// Usage
const user = await prisma.user.create({
    data: { name: 'Budi', email: 'budi@tok.com' }
});

const userWithOrders = await prisma.user.findUnique({
    where: { id: 1 },
    include: {
        orders: {
            include: { items: { include: { product: true } } }
        }
    }
});

// Type-safe! TypeScript knows all properties
```

## 6. Studi Kasus Nyata: Tokopedia Inventory Management

Tokopedia memiliki jutaan produk dan harus handle:
- Millions of inventory updates per day
- Fast product search by SKU
- Seller's inventory dashboard (paginated)
- Stock alerts (low stock detection)

```javascript
// models/Product.js
const { DataTypes } = require('sequelize');
const sequelize = require('../config/database');

const Product = sequelize.define('Product', {
    sku: {
        type: DataTypes.STRING(50),
        unique: true,
        allowNull: false
    },
    name: DataTypes.STRING(200),
    sellerId: DataTypes.INTEGER,
    price: DataTypes.DECIMAL(15, 2),
    stock: {
        type: DataTypes.INTEGER,
        defaultValue: 0
    },
    status: {
        type: DataTypes.ENUM('active', 'inactive', 'archived'),
        defaultValue: 'active'
    }
}, {
    timestamps: true,
    indexes: [
        { fields: ['sku'] },
        { fields: ['sellerId'] },
        { fields: ['status'] },
        { fields: ['stock'] }
    ]
});

// services/InventoryService.js
class InventoryService {
    async getSellerInventory(sellerId, page = 1, pageSize = 20) {
        const offset = (page - 1) * pageSize;
        
        const { rows: products, count } = await Product.findAndCountAll({
            where: { 
                sellerId,
                status: { [Op.ne]: 'archived' }
            },
            offset,
            limit: pageSize,
            order: [['stock', 'ASC']], // low stock first
            attributes: ['id', 'sku', 'name', 'price', 'stock', 'status'],
            raw: true // faster, returns plain objects
        });
        
        return {
            products,
            total: count,
            page,
            pages: Math.ceil(count / pageSize)
        };
    }
    
    async searchBySku(sku) {
        // Index on SKU makes this instant
        return await Product.findOne({
            where: { sku },
            attributes: ['id', 'name', 'price', 'stock', 'sellerId']
        });
    }
    
    async getLowStockProducts(sellerId, threshold = 10) {
        return await Product.findAll({
            where: {
                sellerId,
                stock: { [Op.lte]: threshold },
                status: 'active'
            },
            order: [['stock', 'ASC']]
        });
    }
    
    async updateStock(productId, quantityChange) {
        // Atomic update (database handles concurrency)
        const product = await Product.findByPk(productId);
        const newStock = Math.max(0, product.stock + quantityChange);
        
        await product.update({ stock: newStock });
        
        // Trigger low stock alert if needed
        if (newStock <= 10 && product.stock > 10) {
            await this.notifyLowStock(product);
        }
        
        return newStock;
    }
}

module.exports = InventoryService;

// routes/inventory.js
router.get('/seller/:sellerId/inventory', async (req, res, next) => {
    try {
        const { page = 1 } = req.query;
        const inventory = await inventoryService.getSellerInventory(
            req.params.sellerId,
            parseInt(page)
        );
        res.json(inventory);
    } catch (err) {
        next(err);
    }
});

router.get('/search/sku/:sku', async (req, res, next) => {
    try {
        const product = await inventoryService.searchBySku(req.params.sku);
        res.json(product);
    } catch (err) {
        next(err);
    }
});
```

## 7. Visualisasi: Query Execution & Relationships

```
EAGER LOADING (1 query with JOIN):
SELECT users.*, orders.* 
FROM users 
LEFT JOIN orders ON users.id = orders.user_id
WHERE users.role = 'seller'

Result: User {id, name, orders: [Order, Order]}

---

PAGINATION:
SELECT * FROM products
WHERE seller_id = 123 AND status != 'archived'
ORDER BY stock ASC
LIMIT 20 OFFSET 0

Uses index on (seller_id, stock) for fast lookup
```

## 8. Kesalahan Umum

### ❌ N+1 Queries

```javascript
// ❌ WRONG
const users = await User.findAll();
for (const user of users) {
    const orders = await Order.findAll({ where: { userId: user.id } });
    // Each loop: 1 query
}

// ✅ RIGHT
const users = await User.findAll({
    include: { association: 'orders' }
});
```

### ❌ Missing indexes

```javascript
// ❌ SLOW - no index, full table scan
SELECT * FROM users WHERE email = 'x@tok.com'; // O(n)

// ✅ FAST - with index
CREATE INDEX idx_email ON users(email);
SELECT * FROM users WHERE email = 'x@tok.com'; // O(log n)
```

### ❌ Over-eager loading

```javascript
// ❌ WRONG - loads too much data
const user = await User.findByPk(1, {
    include: [
        { association: 'orders', include: [{ association: 'items' }] },
        { association: 'reviews' },
        { association: 'addresses' }
    ]
});

// ✅ CORRECT - load only what needed
const user = await User.findByPk(1, {
    attributes: ['id', 'name', 'email']
});
```

### ❌ Transaction not handled

```javascript
// ❌ WRONG - if second query fails, first committed
const user = await User.create({ name: 'Budi' });
const wallet = await Wallet.create({ userId: user.id }); // fails!
// user exists but wallet doesn't

// ✅ CORRECT - use transaction
const t = await sequelize.transaction();
try {
    const user = await User.create({ name: 'Budi' }, { transaction: t });
    const wallet = await Wallet.create({ userId: user.id }, { transaction: t });
    await t.commit();
} catch (err) {
    await t.rollback();
    throw err;
}
```

## 9. Latihan & Studi Kasus

### Latihan 1: Model Definition
```javascript
// Task: Create Product model with:
// - id (primary key, auto increment)
// - sku (unique, required)
// - name (string, max 200 chars)
// - price (decimal, required)
// - stock (integer, default 0)
// - status (enum: active, inactive, archived)
// - timestamps

// Also add indexes on sku and status
```

**Solusi:**
```javascript
const Product = sequelize.define('Product', {
    id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
    sku: { type: DataTypes.STRING(50), unique: true, allowNull: false },
    name: { type: DataTypes.STRING(200) },
    price: { type: DataTypes.DECIMAL(15, 2), allowNull: false },
    stock: { type: DataTypes.INTEGER, defaultValue: 0 },
    status: { type: DataTypes.ENUM('active', 'inactive', 'archived'), defaultValue: 'active' }
}, {
    timestamps: true,
    indexes: [{ fields: ['sku'] }, { fields: ['status'] }]
});
```

### Latihan 2: Eager Loading Query
```javascript
// Task: Fetch all orders for user ID 1
// Include user info and order items with product details
// Don't fetch products that are archived
```

**Solusi:**
```javascript
const orders = await Order.findAll({
    where: { userId: 1 },
    include: [{
        association: 'user',
        attributes: ['name', 'email']
    }, {
        association: 'items',
        include: [{
            model: Product,
            as: 'product',
            attributes: ['sku', 'name', 'price'],
            where: { status: { [Op.ne]: 'archived' } },
            required: false
        }]
    }]
});
```

### Latihan 3: Pagination with Index
```javascript
// Task: Paginate products by seller
// - Use indexes for fast lookup
// - Return total count
// - Order by latest created first
```

**Solusi:**
```javascript
async function getSellerProducts(sellerId, page = 1, pageSize = 20) {
    const { rows, count } = await Product.findAndCountAll({
        where: { sellerId },
        limit: pageSize,
        offset: (page - 1) * pageSize,
        order: [['createdAt', 'DESC']],
        subQuery: false
    });
    return { products: rows, total: count, page, pages: Math.ceil(count / pageSize) };
}
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami normalization dan relationships
- [ ] Bisa define Sequelize models dengan validation
- [ ] Bisa perform CRUD operations
- [ ] Mengerti N+1 problem dan solusinya (eager loading)
- [ ] Bisa implement pagination dengan indexes
- [ ] Bisa setup database associations (1-to-many, many-to-many)
- [ ] Bisa write migrations
- [ ] Bisa optimize queries dengan explain plan
- [ ] Mengerti transaction untuk data consistency
- [ ] Bisa membandingkan Sequelize vs Prisma

## 11. Referensi

- [Sequelize Documentation](https://sequelize.org)
- [Prisma Documentation](https://www.prisma.io/docs)
- [MySQL Indexes](https://dev.mysql.com/doc/refman/8.0/en/optimization-indexes.html)
- [SQL Query Optimization](https://use-the-index-luke.com/)
- [Sequelize vs Prisma Comparison](https://www.prisma.io/docs/more/comparisons/prisma-and-sequelize)

**Status:** ✅ Pertemuan 4 selesai
