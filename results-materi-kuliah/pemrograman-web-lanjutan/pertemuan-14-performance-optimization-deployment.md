# Pertemuan 14: Performance Optimization & Deployment

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Mengoptimalkan bundle size dengan code splitting dan lazy loading
- Menggunakan webpack/Vite untuk build optimization
- Mengimplementasikan caching strategies
- Melakukan performance profiling
- Mendeploy ke production (Vercel, Railway, Netlify)
- Menggunakan Docker untuk containerization
- Monitoring dan logging di production

## 2. Pengantar: Hook

Performance adalah feature. Tokopedia homepage 3 detik vs 1 detik = 30% improvement. Slow apps = user abandon. Fast apps = profit.

Optimization terjadi di multiple layers:
- Frontend: bundle size, lazy loading, caching
- Backend: database queries, caching, CDN
- Infrastructure: deployment strategy, scaling

## 3. Konsep Utama

### 3.1 Performance Metrics

```
FCP (First Contentful Paint): 2.5 sec
LCP (Largest Contentful Paint): 2.5 sec
CLS (Cumulative Layout Shift): 0.1
FID (First Input Delay): 100ms (replaced by INP)

Goal: All "green" in Lighthouse
```

### 3.2 Bundle Analysis

```javascript
// Before optimization:
app.js: 2.5 MB
vendor.js: 1.8 MB
Total: 4.3 MB (slow!)

// After:
app.js: 0.5 MB (code split)
vendor.js: 0.8 MB (tree-shake)
chat.async.js: 0.3 MB (lazy load)
Total: 1.6 MB (60% smaller!)
```

## 4. Ilustrasi & Analogi

**Analogi: Delivery Truck**

- **Unoptimized:** Deliver all packages together (heavy truck, slow)
- **Code splitting:** Deliver packages to different areas separately (lighter loads, faster)
- **Lazy loading:** Only deliver when customer requests (on-demand)

## 5. Contoh Teknis

### 5.1 Code Splitting & Lazy Loading

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
    plugins: [vue()],
    build: {
        rollupOptions: {
            output: {
                manualChunks: {
                    // Split vendor libraries
                    vue: ['vue', 'vue-router', 'pinia'],
                    axios: ['axios'],
                    // Charts as separate chunk
                    charts: ['chart.js', 'vue-chartjs']
                }
            }
        }
    }
});

// Router lazy loading
const routes = [
    { path: '/', component: () => import('@/pages/Home.vue') },
    { path: '/products', component: () => import('@/pages/Products.vue') },
    {
        path: '/admin',
        component: () => import('@/pages/admin/Layout.vue'),
        // Lazy load admin sub-routes
        children: [
            {
                path: 'users',
                component: () => import('@/pages/admin/Users.vue')
            },
            {
                path: 'reports',
                component: () => import('@/pages/admin/Reports.vue')
            }
        ]
    }
];

// Component lazy loading
<template>
    <div>
        <button @click="showChart = true">Load Chart</button>
        <Suspense v-if="showChart">
            <template #default>
                <ChartComponent />
            </template>
            <template #fallback>
                <p>Loading chart...</p>
            </template>
        </Suspense>
    </div>
</template>

<script>
import { ref, defineAsyncComponent } from 'vue';

const ChartComponent = defineAsyncComponent(() =>
    import('@/components/Chart.vue')
);

export default {
    components: { ChartComponent },
    setup() {
        const showChart = ref(false);
        return { showChart };
    }
}
</script>
```

### 5.2 Caching Strategies

```javascript
// Browser caching
// In Express.js
app.use(express.static('public', {
    maxAge: '1d', // 1 day
    etag: false
}));

// Specific route caching
app.get('/api/products', (req, res) => {
    res.set('Cache-Control', 'public, max-age=3600'); // 1 hour
    res.json(products);
});

// Service Worker for offline support
// public/service-worker.js
const CACHE_NAME = 'app-v1';
const urlsToCache = [
    '/',
    '/index.html',
    '/app.js',
    '/vendor.js'
];

self.addEventListener('install', event => {
    event.waitUntil(
        caches.open(CACHE_NAME)
            .then(cache => cache.addAll(urlsToCache))
    );
});

self.addEventListener('fetch', event => {
    event.respondWith(
        caches.match(event.request)
            .then(response => {
                if (response) return response;
                
                return fetch(event.request)
                    .then(response => {
                        if (!response || response.status !== 200) {
                            return response;
                        }
                        
                        // Cache successful responses
                        const responseClone = response.clone();
                        caches.open(CACHE_NAME)
                            .then(cache => cache.put(event.request, responseClone));
                        
                        return response;
                    })
                    .catch(() => {
                        // Offline fallback
                        return caches.match('/offline.html');
                    });
            })
    );
});
```

### 5.3 Docker Containerization

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Build app
RUN npm run build

# Expose port
EXPOSE 3000

# Start app
CMD ["npm", "start"]

# docker build -t my-app:1.0 .
# docker run -p 3000:3000 my-app:1.0
```

### 5.4 Deployment to Vercel

```javascript
// vercel.json
{
    "buildCommand": "npm run build",
    "outputDirectory": "dist",
    "env": {
        "VITE_API_URL": "@api_url"
    },
    "routes": [
        {
            "src": "/(.*)",
            "destination": "/index.html",
            "status": 200
        }
    ]
}

// Deploy via git
// 1. git push
// 2. Vercel automatically detects and deploys
// 3. Preview URL generated
// 4. Production URL updated after merge to main
```

### 5.5 Performance Monitoring

```javascript
// src/utils/performance.js
export const reportWebVitals = () => {
    // Largest Contentful Paint
    new PerformanceObserver((list) => {
        list.getEntries().forEach((entry) => {
            console.log('LCP:', entry.startTime);
            // Send to analytics
            fetch('/api/metrics', {
                method: 'POST',
                body: JSON.stringify({
                    metric: 'LCP',
                    value: entry.startTime
                })
            });
        });
    }).observe({ entryTypes: ['largest-contentful-paint'] });
    
    // First Input Delay
    new PerformanceObserver((list) => {
        list.getEntries().forEach((entry) => {
            console.log('FID:', entry.processingDuration);
        });
    }).observe({ entryTypes: ['first-input'] });
};

// src/main.js
import { reportWebVitals } from '@/utils/performance';

if (process.env.NODE_ENV === 'production') {
    reportWebVitals();
}
```

### 5.6 Database Optimization for Production

```javascript
// Add indexes
CREATE INDEX idx_user_id ON orders(user_id);
CREATE INDEX idx_status_date ON orders(status, created_at);

// Connection pooling
const sequelize = new Sequelize(DB, USER, PASS, {
    pool: {
        max: 10,
        min: 2,
        acquire: 30000,
        idle: 10000
    },
    logging: false // disable logs in production
});

// Query optimization
// ❌ N+1 problem
orders.forEach(order => {
    const user = await User.findByPk(order.userId);
});

// ✅ Eager loading
const orders = await Order.findAll({
    include: { model: User }
});

// ✅ Projection (select only needed fields)
const orders = await Order.findAll({
    attributes: ['id', 'amount', 'status'],
    include: {
        model: User,
        attributes: ['id', 'name', 'email']
    }
});
```

## 6. Studi Kasus Nyata: Tokopedia Checkout Performance

Tokopedia checkout page:
- Before: 5MB bundle, 3.5sec load
- After optimization:
  - Code split: 1.5MB initial + 0.8MB lazy
  - Image optimization: 200KB → 50KB
  - Caching strategy: repeat visits <1 sec
  - Result: 60% faster checkout = 15% more conversions!

## 7. Visualisasi: Deployment Pipeline

```
Development
    ↓ (git push)
GitHub
    ↓ (webhook)
CI/CD Pipeline (GitHub Actions)
    ├─ Install dependencies
    ├─ Run tests
    ├─ Build (optimize)
    ├─ Lint check
    └─ Deploy
    ↓
Production (Vercel/Railway)
    ├─ Docker container
    ├─ Auto-scaling
    ├─ CDN caching
    └─ Monitoring
    ↓
User gets fast, reliable app
```

## 8. Kesalahan Umum

### ❌ No code splitting

```javascript
// ❌ WRONG - 5MB single bundle
import { Chart } from 'chart.js';

// ✅ CORRECT - load only when needed
const Chart = () => import('chart.js');
```

### ❌ No caching

```javascript
// ❌ WRONG
res.set('Cache-Control', 'no-cache');

// ✅ CORRECT
res.set('Cache-Control', 'public, max-age=3600');
```

### ❌ Logging in production

```javascript
// ❌ WRONG - logs fill disk
console.log('User activity:', data);

// ✅ CORRECT - selective logging
if (process.env.DEBUG) {
    logger.debug('User activity:', data);
}
```

## 9. Latihan & Studi Kasus

### Latihan 1: Code Splitting
```javascript
// Setup lazy loading for admin routes
```

### Latihan 2: Docker
```javascript
// Create Dockerfile and deploy
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami bundle analysis
- [ ] Bisa implement code splitting
- [ ] Bisa setup lazy loading
- [ ] Bisa implement caching
- [ ] Bisa profile performance
- [ ] Bisa use Lighthouse
- [ ] Bisa create Dockerfile
- [ ] Bisa deploy to Vercel/Railway
- [ ] Bisa setup monitoring
- [ ] Mengerti production best practices

## 11. Referensi

- [Vite Documentation](https://vitejs.dev)
- [Google Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [Web Vitals](https://web.dev/vitals/)
- [Vercel Deployment](https://vercel.com/docs)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)

**Status:** ✅ Pertemuan 14 selesai
