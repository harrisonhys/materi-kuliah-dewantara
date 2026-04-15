# Pertemuan 7: Vue Router & Pinia State Management

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Mengonfigurasi Vue Router untuk multi-page navigation
- Mengimplementasikan nested routes dan lazy loading
- Menggunakan route guards untuk authorization
- Mengelola global state dengan Pinia stores
- Mengimplementasikan mutations, actions, getters di Pinia
- Membuat reusable composables untuk shared logic
- Menghubungkan router dengan state management

## 2. Pengantar: Hook

Aplikasi single-page (SPA) membutuhkan routing yang powerful. Ketika user click link, jangan reload page — update content dengan JavaScript. Vue Router handle ini seamlessly.

Tapi routing alone tidak cukup. State (data global) harus terkelola. Di GoPay, state management critical: user wallet balance, transaction history, selected account. Pinia adalah state management yang modern dan type-safe.

Router + State Management = Production-ready app.

## 3. Konsep Utama

### 3.1 Router Architecture

```
User clicks link
    ↓
Router detects route change
    ↓
Route guards execute (beforeEach)
    ↓
Component unmounts, new component mounts
    ↓
Route transition complete
    ↓
onMounted() in new component
```

### 3.2 Store Architecture (Pinia)

```
Component → Action → Mutation → State
                         ↓
                    Reactive data
                         ↓
                    Derived state (getters)
                         ↓
                    Display in template
```

## 4. Ilustrasi & Analogi

**Analogi: Navigation in Mall**

- **Route:** Different floors in mall (home, products, checkout)
- **Guard:** Security checkpoint (check authentication)
- **State (Pinia):** Shopping cart (shared across all pages)
- **Getters:** Total price calculation
- **Actions:** Checkout process

## 5. Contoh Teknis

### 5.1 Vue Router Setup

```javascript
// src/router/index.js
import { createRouter, createWebHistory } from 'vue-router';

const routes = [
    {
        path: '/',
        name: 'Home',
        component: () => import('@/pages/Home.vue')
    },
    {
        path: '/products',
        name: 'Products',
        component: () => import('@/pages/Products.vue'),
        meta: { requiresAuth: false }
    },
    {
        path: '/checkout',
        name: 'Checkout',
        component: () => import('@/pages/Checkout.vue'),
        meta: { requiresAuth: true } // requires login
    },
    {
        path: '/admin',
        component: () => import('@/layouts/AdminLayout.vue'),
        meta: { requiresAuth: true, role: 'admin' },
        children: [
            {
                path: 'users',
                name: 'AdminUsers',
                component: () => import('@/pages/admin/Users.vue')
            },
            {
                path: 'products',
                name: 'AdminProducts',
                component: () => import('@/pages/admin/Products.vue')
            }
        ]
    },
    {
        path: '/:pathMatch(.*)*',
        name: 'NotFound',
        component: () => import('@/pages/NotFound.vue')
    }
];

const router = createRouter({
    history: createWebHistory(),
    routes
});

// Global route guard
router.beforeEach((to, from, next) => {
    const isLoggedIn = localStorage.getItem('token');
    
    if (to.meta.requiresAuth && !isLoggedIn) {
        next('/login');
    } else if (to.meta.role && !hasRole(to.meta.role)) {
        next('/');
    } else {
        next();
    }
});

export default router;

// src/main.js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';
import pinia from './stores';

createApp(App)
    .use(router)
    .use(pinia)
    .mount('#app');
```

### 5.2 Route Navigation & Links

```javascript
// src/App.vue
<template>
    <nav class="navbar">
        <!-- Using router-link -->
        <router-link to="/">Home</router-link>
        <router-link to="/products">Products</router-link>
        <router-link 
            v-if="isLoggedIn"
            to="/checkout"
            active-class="active"
        >
            Checkout
        </router-link>
    </nav>
    
    <!-- Router outlet: displays routed component -->
    <router-view />
</template>

<script>
import { computed } from 'vue';
import { useRouter } from 'vue-router';

export default {
    setup() {
        const router = useRouter();
        const isLoggedIn = computed(() => 
            !!localStorage.getItem('token')
        );
        
        // Programmatic navigation
        const goToCheckout = () => {
            router.push('/checkout');
            // or: router.push({ name: 'Checkout' });
        };
        
        // Navigation with parameters
        const goToProduct = (productId) => {
            router.push({
                name: 'ProductDetail',
                params: { id: productId }
            });
        };
        
        return { isLoggedIn, goToCheckout, goToProduct };
    }
}
</script>
```

### 5.3 Pinia Store Setup

```javascript
// src/stores/cartStore.js
import { defineStore } from 'pinia';
import { ref, computed } from 'vue';

export const useCartStore = defineStore('cart', () => {
    // STATE
    const items = ref([]);
    const discountPercent = ref(0);
    
    // GETTERS (computed properties)
    const subtotal = computed(() =>
        items.value.reduce((sum, item) => 
            sum + (item.price * item.quantity), 0
        )
    );
    
    const discount = computed(() =>
        subtotal.value * (discountPercent.value / 100)
    );
    
    const total = computed(() =>
        subtotal.value - discount.value
    );
    
    const itemCount = computed(() =>
        items.value.reduce((sum, item) => sum + item.quantity, 0)
    );
    
    // ACTIONS (methods)
    const addItem = (product) => {
        const existing = items.value.find(i => i.id === product.id);
        
        if (existing) {
            existing.quantity++;
        } else {
            items.value.push({ ...product, quantity: 1 });
        }
    };
    
    const removeItem = (productId) => {
        items.value = items.value.filter(i => i.id !== productId);
    };
    
    const updateQuantity = (productId, quantity) => {
        const item = items.value.find(i => i.id === productId);
        if (item) {
            item.quantity = Math.max(1, quantity);
        }
    };
    
    const applyDiscount = (percent) => {
        if (percent > 50) throw new Error('Max 50% discount');
        discountPercent.value = percent;
    };
    
    const clearCart = () => {
        items.value = [];
        discountPercent.value = 0;
    };
    
    return {
        // expose state
        items,
        discountPercent,
        // expose getters
        subtotal,
        discount,
        total,
        itemCount,
        // expose actions
        addItem,
        removeItem,
        updateQuantity,
        applyDiscount,
        clearCart
    };
});
```

### 5.4 Using Store in Components

```javascript
// src/pages/Checkout.vue
<template>
    <div class="checkout">
        <h2>Shopping Cart</h2>
        
        <!-- Cart items -->
        <table>
            <tr v-for="item in cart.items" :key="item.id">
                <td>{{ item.name }}</td>
                <td>
                    <input 
                        type="number"
                        :value="item.quantity"
                        @input="cart.updateQuantity(item.id, $event.target.value)"
                    />
                </td>
                <td>Rp{{ item.price * item.quantity }}</td>
                <td>
                    <button @click="cart.removeItem(item.id)">Remove</button>
                </td>
            </tr>
        </table>
        
        <!-- Summary -->
        <div class="summary">
            <p>Subtotal: Rp{{ cart.subtotal }}</p>
            <p>Discount: Rp{{ cart.discount }}</p>
            <p><strong>Total: Rp{{ cart.total }}</strong></p>
        </div>
        
        <!-- Checkout -->
        <button @click="checkout" :disabled="cart.items.length === 0">
            Proceed to Payment
        </button>
    </div>
</template>

<script>
import { useCartStore } from '@/stores/cartStore';
import { useRouter } from 'vue-router';

export default {
    setup() {
        const cart = useCartStore();
        const router = useRouter();
        
        const checkout = async () => {
            // Payment logic
            const response = await fetch('/api/orders', {
                method: 'POST',
                body: JSON.stringify({
                    items: cart.items,
                    total: cart.total
                })
            });
            
            if (response.ok) {
                cart.clearCart();
                router.push('/orders');
            }
        };
        
        return { cart, checkout };
    }
}
</script>
```

### 5.5 Route Parameters & Query

```javascript
// Route definition
{
    path: '/products/:id',
    name: 'ProductDetail',
    component: () => import('@/pages/ProductDetail.vue')
}

// Component using params
// src/pages/ProductDetail.vue
<template>
    <div>
        <h2>{{ product.name }}</h2>
        <p>Price: Rp{{ product.price }}</p>
        <button @click="addToCart">Add to Cart</button>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { useCartStore } from '@/stores/cartStore';

export default {
    setup() {
        const route = useRoute();
        const cart = useCartStore();
        const product = ref(null);
        
        onMounted(async () => {
            const productId = route.params.id;
            const response = await fetch(`/api/products/${productId}`);
            product.value = await response.json();
        });
        
        const addToCart = () => {
            cart.addItem(product.value);
        };
        
        return { product, addToCart };
    }
}
</script>

// Navigate to product
router.push({ name: 'ProductDetail', params: { id: 123 } });

// Query parameters
router.push({
    name: 'Products',
    query: { category: 'electronics', sort: 'price' }
});

// Access query in component
const route = useRoute();
const category = route.query.category; // 'electronics'
```

### 5.6 Route Guards & Auth

```javascript
// src/router/guards.js
export const setupRouterGuards = (router) => {
    router.beforeEach((to, from, next) => {
        const isLoggedIn = !!localStorage.getItem('token');
        
        // Require authentication
        if (to.meta.requiresAuth && !isLoggedIn) {
            next('/login?redirect=' + to.path);
            return;
        }
        
        // Role-based access
        if (to.meta.role) {
            const userRole = localStorage.getItem('userRole');
            if (userRole !== to.meta.role) {
                next('/unauthorized');
                return;
            }
        }
        
        next();
    });
    
    // After navigation
    router.afterEach((to, from) => {
        document.title = to.meta.title || 'GoPay';
    });
};
```

## 6. Studi Kasus Nyata: GoPay Wallet App Navigation

GoPay app harus navigate between:
- Home (balance, quick actions)
- Transfer (find recipient, confirm, receipt)
- History (paginated transactions)
- Settings (personal, security, linked accounts)

State management:
- User wallet data
- Selected account
- Recent transfers
- Settings preferences

```javascript
// src/stores/walletStore.js
import { defineStore } from 'pinia';
import { ref, computed } from 'vue';

export const useWalletStore = defineStore('wallet', () => {
    const wallet = ref(null);
    const selectedAccountId = ref(null);
    const recentTransfers = ref([]);
    const isLoading = ref(false);
    
    const selectedAccount = computed(() =>
        wallet.value?.accounts?.find(a => a.id === selectedAccountId.value)
    );
    
    const balance = computed(() =>
        selectedAccount.value?.balance || 0
    );
    
    const fetchWallet = async () => {
        isLoading.value = true;
        try {
            const response = await fetch('/api/wallet');
            wallet.value = await response.json();
            if (wallet.value.accounts.length > 0) {
                selectedAccountId.value = wallet.value.accounts[0].id;
            }
        } finally {
            isLoading.value = false;
        }
    };
    
    const switchAccount = (accountId) => {
        selectedAccountId.value = accountId;
    };
    
    const transfer = async (recipientPhone, amount) => {
        const response = await fetch('/api/transfer', {
            method: 'POST',
            body: JSON.stringify({
                accountId: selectedAccountId.value,
                recipientPhone,
                amount
            })
        });
        
        const result = await response.json();
        
        // Add to recent transfers
        recentTransfers.value.unshift({
            id: result.id,
            recipientPhone,
            amount,
            timestamp: new Date()
        });
        
        // Refresh balance
        await fetchWallet();
        
        return result;
    };
    
    return {
        wallet,
        selectedAccountId,
        recentTransfers,
        isLoading,
        selectedAccount,
        balance,
        fetchWallet,
        switchAccount,
        transfer
    };
});

// src/router/index.js
const routes = [
    {
        path: '/wallet',
        component: () => import('@/layouts/WalletLayout.vue'),
        children: [
            {
                path: '',
                name: 'WalletHome',
                component: () => import('@/pages/wallet/Home.vue')
            },
            {
                path: 'transfer',
                name: 'Transfer',
                component: () => import('@/pages/wallet/Transfer.vue'),
                meta: { requiresAuth: true }
            },
            {
                path: 'history',
                name: 'History',
                component: () => import('@/pages/wallet/History.vue'),
                meta: { requiresAuth: true }
            },
            {
                path: 'settings',
                name: 'Settings',
                component: () => import('@/pages/wallet/Settings.vue'),
                meta: { requiresAuth: true }
            }
        ]
    }
];
```

## 7. Visualisasi: State Flow with Router

```
User clicks link
    ↓
router.push() / router-link
    ↓
Route changes
    ↓
New component mounts
    ↓
setup() executed
    ↓
const store = useStore()
    ↓
Component reads store.data
    ↓
Template renders
    ↓
User interacts
    ↓
store.action() called
    ↓
State mutations
    ↓
All components using store reactive-updated
```

## 8. Kesalahan Umum

### ❌ Modifying store state directly

```javascript
// ❌ WRONG
const store = useStore();
store.items.push(newItem); // direct mutation

// ✅ CORRECT
const store = useStore();
store.addItem(newItem); // use action
```

### ❌ Not protecting routes

```javascript
// ❌ WRONG - anyone can access /admin
{
    path: '/admin',
    component: () => import('@/pages/Admin.vue')
}

// ✅ CORRECT - require auth and admin role
{
    path: '/admin',
    component: () => import('@/pages/Admin.vue'),
    meta: { requiresAuth: true, role: 'admin' }
}
```

### ❌ Memory leak in route change

```javascript
// ❌ WRONG - timer not cleaned in old component
onMounted(() => {
    setInterval(() => fetchData(), 1000);
});

// ✅ CORRECT - clean up on unmount
const timerId = ref(null);
onMounted(() => {
    timerId.value = setInterval(() => fetchData(), 1000);
});
onUnmounted(() => {
    clearInterval(timerId.value);
});
```

## 9. Latihan & Studi Kasus

### Latihan 1: Basic Router
```javascript
// Task: Create 3 routes: /, /about, /contact
// Use router-link to navigate
// Display different component for each
```

### Latihan 2: Pinia Store
```javascript
// Task: Create counter store
// - state: count
// - getter: isEven
// - action: increment, decrement
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami router architecture
- [ ] Bisa define routes dengan nested routes
- [ ] Bisa implement route guards
- [ ] Bisa use route parameters dan query
- [ ] Bisa create Pinia stores
- [ ] Bisa implement mutations dan actions
- [ ] Bisa use getters untuk computed state
- [ ] Bisa navigate programmatically
- [ ] Bisa manage global state across components
- [ ] Bisa handle route transitions

## 11. Referensi

- [Vue Router Documentation](https://router.vuejs.org)
- [Pinia Documentation](https://pinia.vuejs.org)
- [Route Guards](https://router.vuejs.org/guide/advanced/navigation-guards.html)

**Status:** ✅ Pertemuan 7 selesai
