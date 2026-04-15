# Pertemuan 11: Pagination & Large Dataset Handling

## 1. Learning Outcomes

Setelah pertemuan ini, mahasiswa mampu:
- Mengimplementasikan offset-based pagination
- Mengimplementasikan cursor-based pagination (lebih efisien)
- Mengoptimalkan query untuk large datasets
- Menggunakan virtual scrolling untuk performance
- Menghitung pagination state (total pages, current page)
- Menangani filtering + sorting + pagination bersama
- Implementasi infinite scroll patterns

## 2. Pengantar: Hook

GoPay transaction history punya milliaran records. Loading semua ke memory = crash. Pagination membuat aplikasi usable.

Offset-based: `SELECT * FROM transactions WHERE user_id = 123 LIMIT 20 OFFSET 0` simple tapi slow untuk besar datasets (must skip all previous rows).

Cursor-based: `SELECT * FROM transactions WHERE user_id = 123 AND id > 999 LIMIT 20` fast karena use index.

Virtual scrolling: Only render visible rows. 10k items, hanya render 20 items di screen = instant.

## 3. Konsep Utama

### 3.1 Pagination Types

```
Offset-Based:
Pros: Simple, standard, direct "go to page 5"
Cons: Slow for large datasets, inconsistent data if rows added

Cursor-Based:
Pros: Efficient, consistent, handles deletions
Cons: Harder to implement, can't jump to page 5

Infinite Scroll:
Pros: Mobile-friendly, smooth UX
Cons: Can't go back, hard to find specific item
```

### 3.2 Virtual Scrolling

```
Real DOM: 10,000 items = 10,000 DOM nodes (slow)
Virtual: 10,000 items but only 20 visible = 20 DOM nodes (fast)
```

## 4. Ilustrasi & Analogi

**Analogi: Restaurant Menu**

- **Full list:** Print all 500 items = heavy, slow
- **Pagination:** Show 10 items per page = manageable
- **Infinite scroll:** Show more as you scroll = smooth
- **Virtual scroll:** Only show visible items on screen = fast

## 5. Contoh Teknis

### 5.1 Offset-Based Pagination

```javascript
// Backend API
app.get('/transactions', async (req, res) => {
    const page = parseInt(req.query.page) || 1;
    const pageSize = parseInt(req.query.pageSize) || 20;
    const offset = (page - 1) * pageSize;
    
    const { rows: transactions, count } = await Transaction.findAndCountAll({
        where: { userId: req.user.id },
        limit: pageSize,
        offset,
        order: [['createdAt', 'DESC']]
    });
    
    res.json({
        transactions,
        pagination: {
            page,
            pageSize,
            total: count,
            pages: Math.ceil(count / pageSize)
        }
    });
});

// Frontend Component
<template>
    <div>
        <!-- Transactions list -->
        <table>
            <tr v-for="tx in transactions" :key="tx.id">
                <td>{{ tx.date }}</td>
                <td>{{ tx.description }}</td>
                <td>Rp{{ tx.amount }}</td>
            </tr>
        </table>
        
        <!-- Pagination controls -->
        <div class="pagination">
            <button 
                @click="previousPage"
                :disabled="currentPage === 1"
            >
                Previous
            </button>
            
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            
            <button 
                @click="nextPage"
                :disabled="currentPage === totalPages"
            >
                Next
            </button>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';
import apiClient from '@/api/client';

export default {
    setup() {
        const transactions = ref([]);
        const currentPage = ref(1);
        const totalPages = ref(1);
        const pageSize = 20;
        
        const loadTransactions = async (page = 1) => {
            const response = await apiClient.get('/transactions', {
                params: { page, pageSize }
            });
            
            transactions.value = response.data.transactions;
            currentPage.value = response.data.pagination.page;
            totalPages.value = response.data.pagination.pages;
        };
        
        const nextPage = () => {
            if (currentPage.value < totalPages.value) {
                loadTransactions(currentPage.value + 1);
            }
        };
        
        const previousPage = () => {
            if (currentPage.value > 1) {
                loadTransactions(currentPage.value - 1);
            }
        };
        
        onMounted(() => loadTransactions());
        
        return { transactions, currentPage, totalPages, nextPage, previousPage };
    }
}
</script>
```

### 5.2 Cursor-Based Pagination

```javascript
// Backend
app.get('/transactions-cursor', async (req, res) => {
    const pageSize = parseInt(req.query.pageSize) || 20;
    const cursor = req.query.cursor; // last transaction ID
    
    let query = Transaction.findAll({
        where: { userId: req.user.id },
        limit: pageSize + 1, // +1 to check if more exist
        order: [['id', 'DESC']]
    });
    
    if (cursor) {
        query = query.where({ id: { [Op.lt]: cursor } }); // after cursor
    }
    
    const transactions = await query;
    const hasMore = transactions.length > pageSize;
    
    if (hasMore) transactions.pop(); // remove extra
    
    res.json({
        transactions,
        nextCursor: transactions[transactions.length - 1]?.id,
        hasMore
    });
});

// Frontend
<template>
    <div>
        <table>
            <tr v-for="tx in transactions" :key="tx.id">
                <td>{{ tx.description }}</td>
            </tr>
        </table>
        
        <button 
            v-if="hasMore"
            @click="loadMore"
            :disabled="isLoading"
        >
            Load More
        </button>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    setup() {
        const transactions = ref([]);
        const cursor = ref(null);
        const hasMore = ref(false);
        const isLoading = ref(false);
        
        const loadMore = async () => {
            isLoading.value = true;
            const params = { pageSize: 20 };
            if (cursor.value) params.cursor = cursor.value;
            
            const response = await apiClient.get('/transactions-cursor', { params });
            
            transactions.value.push(...response.data.transactions);
            cursor.value = response.data.nextCursor;
            hasMore.value = response.data.hasMore;
            isLoading.value = false;
        };
        
        onMounted(() => loadMore());
        
        return { transactions, hasMore, isLoading, loadMore };
    }
}
</script>
```

### 5.3 Virtual Scrolling with vue-virtual-scroller

```javascript
// npm install vue-virtual-scroller

<template>
    <virtual-scroller
        :items="items"
        :item-size="50"
        class="scroller"
    >
        <template #default="{ item }">
            <div class="item">
                {{ item.name }}
            </div>
        </template>
    </virtual-scroller>
</template>

<script>
import { VirtualScroller } from 'vue-virtual-scroller';

export default {
    components: { VirtualScroller },
    setup() {
        const items = ref(generateMillionItems());
        return { items };
    }
}
</script>

<style scoped>
.scroller { height: 600px; }
.item { height: 50px; padding: 10px; border-bottom: 1px solid #ddd; }
</style>
```

### 5.4 Pagination with Filtering & Sorting

```javascript
// Backend API
app.get('/products', async (req, res) => {
    const {
        page = 1,
        pageSize = 20,
        category,
        sort = 'createdAt',
        order = 'DESC'
    } = req.query;
    
    let where = { status: 'active' };
    if (category) where.category = category;
    
    const { rows: products, count } = await Product.findAndCountAll({
        where,
        limit: pageSize,
        offset: (page - 1) * pageSize,
        order: [[sort, order]],
        include: [{ model: Category, attributes: ['name'] }]
    });
    
    res.json({
        products,
        pagination: {
            page,
            pages: Math.ceil(count / pageSize),
            total: count
        }
    });
});

// Frontend
<template>
    <div>
        <!-- Filters & Sort -->
        <div class="controls">
            <select v-model="filters.category" @change="resetPage">
                <option value="">All Categories</option>
                <option value="electronics">Electronics</option>
                <option value="books">Books</option>
            </select>
            
            <select v-model="sortBy" @change="resetPage">
                <option value="createdAt">Newest</option>
                <option value="price">Price: Low to High</option>
                <option value="rating">Rating</option>
            </select>
        </div>
        
        <!-- Products -->
        <div class="products">
            <div v-for="product in products" :key="product.id" class="card">
                {{ product.name }}
            </div>
        </div>
        
        <!-- Pagination -->
        <div class="pagination">
            <button @click="page--" :disabled="page === 1">Prev</button>
            <span>{{ page }} / {{ totalPages }}</span>
            <button @click="page++" :disabled="page === totalPages">Next</button>
        </div>
    </div>
</template>

<script>
import { ref, computed, watch } from 'vue';

export default {
    setup() {
        const products = ref([]);
        const page = ref(1);
        const totalPages = ref(1);
        const filters = reactive({ category: '' });
        const sortBy = ref('createdAt');
        
        const loadProducts = async () => {
            const response = await apiClient.get('/products', {
                params: {
                    page: page.value,
                    pageSize: 20,
                    category: filters.category,
                    sort: sortBy.value
                }
            });
            
            products.value = response.data.products;
            totalPages.value = response.data.pagination.pages;
        };
        
        const resetPage = () => {
            page.value = 1;
        };
        
        watch([page, () => filters.category, sortBy], () => {
            loadProducts();
        });
        
        onMounted(() => loadProducts());
        
        return {
            products,
            page,
            totalPages,
            filters,
            sortBy,
            resetPage
        };
    }
}
</script>
```

### 5.5 Infinite Scroll

```javascript
<template>
    <div class="feed" @scroll="handleScroll" ref="feedElement">
        <article v-for="post in posts" :key="post.id" class="post">
            {{ post.content }}
        </article>
        
        <div v-if="isLoading" class="loading">Loading...</div>
        <div v-if="!hasMore" class="end">No more posts</div>
    </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue';

export default {
    setup() {
        const posts = ref([]);
        const cursor = ref(null);
        const hasMore = ref(true);
        const isLoading = ref(false);
        const feedElement = ref(null);
        
        const loadMore = async () => {
            if (isLoading.value || !hasMore.value) return;
            isLoading.value = true;
            
            const params = { pageSize: 10 };
            if (cursor.value) params.cursor = cursor.value;
            
            const response = await apiClient.get('/posts', { params });
            posts.value.push(...response.data.posts);
            cursor.value = response.data.nextCursor;
            hasMore.value = response.data.hasMore;
            isLoading.value = false;
        };
        
        const handleScroll = () => {
            const el = feedElement.value;
            if (el.scrollTop + el.clientHeight >= el.scrollHeight - 200) {
                loadMore();
            }
        };
        
        onMounted(() => {
            loadMore();
            feedElement.value.addEventListener('scroll', handleScroll);
        });
        
        onUnmounted(() => {
            feedElement.value?.removeEventListener('scroll', handleScroll);
        });
        
        return { posts, isLoading, hasMore, feedElement };
    }
}
</script>

<style scoped>
.feed { height: 600px; overflow-y: auto; border: 1px solid #ddd; }
.post { padding: 20px; border-bottom: 1px solid #eee; }
</style>
```

## 6. Studi Kasus Nyata: GoPay Transaction History

GoPay harus handle billions of transactions efficiently:

```javascript
// Use cursor-based for performance
// Combine with filters (date range, type)
// Virtual scroll for smooth interaction
```

## 7. Visualisasi: Query Performance

```
Offset-Based (slow for large offsets):
SELECT * FROM transactions LIMIT 20 OFFSET 1000000
Result: scans 1,000,020 rows, returns 20

Cursor-Based (fast with index):
SELECT * FROM transactions WHERE id > 999999 LIMIT 20
Result: uses index, scans ~40 rows, returns 20
```

## 8. Kesalahan Umum

### ❌ Loading entire dataset

```javascript
// ❌ WRONG - load all
const transactions = await Transaction.findAll({ where: { userId } });

// ✅ CORRECT - paginate
const transactions = await Transaction.findAll({
    where: { userId },
    limit: 20,
    offset: 0
});
```

### ❌ Not handling edge cases

```javascript
// ❌ WRONG
page++; // What if page === maxPages?

// ✅ CORRECT
if (page < totalPages) page++;
```

## 9. Latihan & Studi Kasus

### Latihan 1: Offset Pagination
```javascript
// Implement offset-based with next/previous buttons
```

### Latihan 2: Cursor-Based
```javascript
// Implement cursor-based with "Load More" button
```

## 10. Ringkasan

**Checklist Penguasaan:**
- [ ] Memahami offset vs cursor pagination
- [ ] Bisa implement pagination UI
- [ ] Bisa calculate total pages
- [ ] Bisa combine filtering + pagination
- [ ] Bisa implement infinite scroll
- [ ] Bisa use virtual scrolling
- [ ] Mengerti query optimization
- [ ] Tahu kapan pakai mana approach
- [ ] Bisa handle loading states
- [ ] Bisa handle edge cases

## 11. Referensi

- [Pagination Best Practices](https://www.moesif.com/blog/api-guide/rest-api-pagination/)
- [vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller)
- [MySQL Pagination Performance](https://use-the-index-luke.com/sql/partial-results/fetch-next-page)

**Status:** ✅ Pertemuan 11 selesai
