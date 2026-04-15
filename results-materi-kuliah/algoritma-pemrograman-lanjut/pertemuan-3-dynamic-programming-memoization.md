# Pertemuan 3: Dynamic Programming — Memoization

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Mengidentifikasi masalah yang memiliki sifat Optimal Substructure dan Overlapping Subproblems
- Mengimplementasikan solusi top-down dengan memoization menggunakan JavaScript Map dan WeakMap
- Menyelesaikan masalah 0/1 Knapsack dengan memoization rekursif
- Membandingkan kompleksitas rekursi naif vs memoization secara eksperimental

## 2. Pengantar: Hook

OVO memiliki sistem "cashback optimization" — untuk setiap pengguna, sistem harus memilih kombinasi transaksi mana yang diikutsertakan dalam program loyalty agar total cashback maksimal, tanpa melebihi batas budget promosi per pengguna.

Ini adalah **0/1 Knapsack Problem** dalam kehidupan nyata. Dengan brute-force: 2^n kombinasi transaksi — untuk 30 transaksi saja, itu **1 miliar kombinasi**. Tidak mungkin real-time.

Dengan **Dynamic Programming + Memoization**: O(n × W) — untuk 30 transaksi dan budget 10.000 poin, cukup 300.000 operasi. Selesai dalam **<1ms**.

Rahasianya? **Jangan hitung ulang yang sudah pernah dihitung.** Sederhana. Tapi mengubah segalanya.

## 3. Konsep Utama

### 3.1 Dua Syarat Masalah DP

**Syarat 1: Optimal Substructure**
Solusi optimal masalah mengandung solusi optimal submasalahnya.

```
Contoh: Jarak terpendek A → C melalui B
  shortestPath(A, C) = shortestPath(A, B) + shortestPath(B, C)
  
Counter-example: Jalan terpanjang (longest path) — TIDAK punya optimal substructure
  longestPath(A, C) ≠ longestPath(A, B) + longestPath(B, C)
  (menggunakan node B dua kali bisa terjadi)
```

**Syarat 2: Overlapping Subproblems**
Submasalah yang sama dihitung berulang kali dalam rekursi.

```
fib(5):
  fib(4) + fib(3)
    fib(3) + fib(2)   fib(2) + fib(1)
      ...               ...
      
fib(3) dihitung 2 kali, fib(2) dihitung 3 kali!
→ Ini overlapping subproblems → DP cocok
```

### 3.2 Top-Down DP = Rekursi + Memoization

**Strategi:**
1. Tulis solusi rekursif seperti biasa (tapi catat state yang mendefinisikan subproblem)
2. Sebelum menghitung, cek cache — sudah pernah dihitung?
3. Setelah menghitung, simpan di cache

```javascript
// Template memoization
function solve(state, memo = new Map()) {
    // 1. Base case
    if (isBaseCase(state)) return baseValue(state);
    
    // 2. Cek cache
    const key = stateToKey(state);
    if (memo.has(key)) return memo.get(key);
    
    // 3. Hitung (rekursi)
    const result = /* kombinasi subproblem */;
    
    // 4. Simpan di cache
    memo.set(key, result);
    return result;
}
```

### 3.3 State Space Analysis

**Kunci DP:** Berapa banyak state unik yang mungkin?
- Jumlah state unik = kompleksitas waktu DP
- Setiap state dihitung tepat SATU kali

```
Fibonacci: state = n → n state unik → O(n) waktu
Knapsack: state = (item_index, remaining_capacity) → n×W state → O(n×W) waktu
LCS: state = (i, j) → m×n state → O(m×n) waktu
```

### 3.4 Memoization dengan Kunci Kompleks

```javascript
// State dengan multiple parameters — serialize ke string key
function dpComplex(i, j, k, memo = new Map()) {
    const key = `${i},${j},${k}`; // composite key
    if (memo.has(key)) return memo.get(key);
    // ...
}

// Alternatif: nested Map
function dpNested(i, j, memo = new Map()) {
    if (!memo.has(i)) memo.set(i, new Map());
    const innerMemo = memo.get(i);
    if (innerMemo.has(j)) return innerMemo.get(j);
    // ...
    innerMemo.set(j, result);
    return result;
}
```

## 4. Ilustrasi dan Analogi

### Analogi: Buku Catatan Ujian

Bayangkan kamu mengerjakan soal matematika yang sama berulang kali di ujian berbeda.

**Tanpa DP (rekursi naif):** Setiap kali ujian, kamu hitung ulang dari awal. Jika soal `fib(20)` muncul, kamu butuh ~1 juta perhitungan.

**Dengan Memoization:** Kamu bawa **buku catatan** yang berisi semua jawaban soal yang pernah dikerjakan. Jika soal sudah pernah muncul, buka catatan → jawab langsung. Biaya: hanya O(n) soal berbeda yang perlu dihitung.

**Dengan Tabulation (bottom-up):** Kamu sudah mengisi buku catatan dari awal sebelum ujian — isi dari soal termudah ke tersulit. Tidak ada rekursi sama sekali.

### Analogi Knapsack: Promo OVO

Kamu punya **budget promosi Rp 100.000** dan daftar transaksi pengguna:

| Transaksi | Nilai Cashback | Biaya Promosi |
|-----------|---------------|---------------|
| Pulsa Telkomsel | 5.000 | 20.000 |
| Belanja Alfamart | 8.000 | 30.000 |
| Tagihan PLN | 12.000 | 40.000 |
| GoFood | 10.000 | 35.000 |
| Transfer Bank | 3.000 | 10.000 |

**Tujuan:** Pilih transaksi yang diikutkan cashback agar total cashback maksimal, total biaya ≤ 100.000.

**Optimal Substructure:** Jika kamu tahu solusi terbaik untuk budget 70.000 dengan item 1-4, dan kamu tambahkan item 5 (biaya 30.000), kamu bisa cek apakah itu lebih baik dari solusi terbaik budget 100.000 tanpa item 5.

## 5. Contoh Teknis

### 5.1 Fibonacci: Naif vs Memoization vs Iteratif

```javascript
/**
 * Perbandingan tiga pendekatan Fibonacci
 */

// Approach 1: Naif — O(2^n) waktu, O(n) ruang
function fibNaive(n) {
    if (n <= 1) return n;
    return fibNaive(n - 1) + fibNaive(n - 2);
}

// Approach 2: Memoization — O(n) waktu, O(n) ruang
function fibMemo(n, memo = new Map()) {
    if (n <= 1) return n;
    if (memo.has(n)) return memo.get(n);
    
    const result = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    memo.set(n, result);
    return result;
}

// Approach 3: Iteratif (bottom-up) — O(n) waktu, O(1) ruang
function fibIterative(n) {
    if (n <= 1) return n;
    let prev = 0, curr = 1;
    for (let i = 2; i <= n; i++) {
        [prev, curr] = [curr, prev + curr];
    }
    return curr;
}

// Benchmark
function benchmark(fn, n, label) {
    const start = performance.now();
    const result = fn(n);
    const end = performance.now();
    console.log(`${label}(${n}): ${result}, time: ${(end-start).toFixed(3)}ms`);
}

benchmark(fibNaive, 35, 'Naive');      // ~100ms
benchmark(fibMemo, 35, 'Memo');        // <1ms
benchmark(fibMemo, 1000, 'Memo');      // <1ms
benchmark(fibIterative, 1000000, 'Iterative'); // <10ms

// Visualisasi jumlah pemanggilan
let callCount = 0;
function fibWithCount(n, memo = new Map()) {
    callCount++;
    if (n <= 1) return n;
    if (memo.has(n)) return memo.get(n);
    const result = fibWithCount(n - 1, memo) + fibWithCount(n - 2, memo);
    memo.set(n, result);
    return result;
}

callCount = 0;
fibWithCount(10);
console.log(`fibMemo(10) calls: ${callCount}`);  // 19 calls

function fibNaiveCount(n) {
    callCount++;
    if (n <= 1) return n;
    return fibNaiveCount(n - 1) + fibNaiveCount(n - 2);
}
callCount = 0;
fibNaiveCount(10);
console.log(`fibNaive(10) calls: ${callCount}`); // 177 calls — 9x lebih banyak!
```

### 5.2 0/1 Knapsack dengan Memoization

```javascript
/**
 * 0/1 Knapsack Problem
 * 
 * State: (itemIndex, remainingCapacity)
 * Pilihan: ambil item ke-i ATAU skip
 * 
 * Kompleksitas: O(n × W) waktu, O(n × W) ruang
 */

/**
 * @param {Array} items - [{name, value, weight}]
 * @param {number} capacity - maksimum weight
 * @returns {Object} - {maxValue, selectedItems}
 */
function knapsack(items, capacity) {
    const n = items.length;
    const memo = new Map();
    
    function dp(i, remainingCap) {
        // Base case
        if (i === n || remainingCap === 0) return 0;
        
        // Cek cache
        const key = `${i},${remainingCap}`;
        if (memo.has(key)) return memo.get(key);
        
        // Option 1: Skip item ke-i
        const skip = dp(i + 1, remainingCap);
        
        // Option 2: Ambil item ke-i (jika muat)
        let take = 0;
        if (items[i].weight <= remainingCap) {
            take = items[i].value + dp(i + 1, remainingCap - items[i].weight);
        }
        
        const result = Math.max(skip, take);
        memo.set(key, result);
        return result;
    }
    
    const maxValue = dp(0, capacity);
    
    // Backtrack untuk menemukan item yang dipilih
    const selected = [];
    let remCap = capacity;
    for (let i = 0; i < n; i++) {
        // Item i dipilih jika dp(i, remCap) != dp(i+1, remCap)
        const withoutItem = memo.get(`${i+1},${remCap}`) ?? 0;
        const currentVal = memo.get(`${i},${remCap}`) ?? 0;
        
        if (currentVal !== withoutItem && items[i].weight <= remCap) {
            selected.push(items[i]);
            remCap -= items[i].weight;
        }
    }
    
    return { maxValue, selectedItems: selected };
}

// Simulasi OVO Cashback Optimization
const transactions = [
    { name: 'Pulsa Telkomsel', value: 5000, weight: 20000 },   // cashback 5k, biaya 20k
    { name: 'Belanja Alfamart', value: 8000, weight: 30000 },
    { name: 'Tagihan PLN', value: 12000, weight: 40000 },
    { name: 'GoFood Order', value: 10000, weight: 35000 },
    { name: 'Transfer Bank', value: 3000, weight: 10000 },
    { name: 'Grab Ride', value: 7000, weight: 25000 },
    { name: 'Netflix Sub', value: 15000, weight: 50000 },
];

const promotionBudget = 100000;

console.time('Knapsack DP');
const result = knapsack(transactions, promotionBudget);
console.timeEnd('Knapsack DP');

console.log(`\nTotal cashback dimaksimalkan: Rp ${result.maxValue.toLocaleString('id-ID')}`);
console.log('Transaksi yang dipilih:');
result.selectedItems.forEach(item => {
    console.log(`  ✓ ${item.name}: cashback Rp ${item.value.toLocaleString('id-ID')}, biaya Rp ${item.weight.toLocaleString('id-ID')}`);
});

const totalCost = result.selectedItems.reduce((sum, item) => sum + item.weight, 0);
console.log(`Total biaya promosi: Rp ${totalCost.toLocaleString('id-ID')} dari budget Rp ${promotionBudget.toLocaleString('id-ID')}`);
```

### 5.3 Coin Change — Minimum Coins

```javascript
/**
 * Coin Change Problem
 * 
 * Berapa koin minimum untuk membuat amount tertentu?
 * State: amount yang tersisa
 * 
 * Aplikasi: sistem pembayaran, kembalian vending machine
 */
function coinChange(coins, amount) {
    const memo = new Map();
    
    function dp(remaining) {
        if (remaining === 0) return 0;
        if (remaining < 0) return Infinity;
        
        if (memo.has(remaining)) return memo.get(remaining);
        
        let minCoins = Infinity;
        for (const coin of coins) {
            const subResult = dp(remaining - coin);
            if (subResult !== Infinity) {
                minCoins = Math.min(minCoins, 1 + subResult);
            }
        }
        
        memo.set(remaining, minCoins);
        return minCoins;
    }
    
    const result = dp(amount);
    return result === Infinity ? -1 : result;
}

// Versi dengan tracking path
function coinChangeWithPath(coins, amount) {
    const memo = new Map();
    const choice = new Map(); // untuk backtracking
    
    function dp(remaining) {
        if (remaining === 0) return 0;
        if (remaining < 0) return Infinity;
        if (memo.has(remaining)) return memo.get(remaining);
        
        let minCoins = Infinity;
        let bestCoin = -1;
        
        for (const coin of coins) {
            const subResult = dp(remaining - coin);
            if (subResult !== Infinity && 1 + subResult < minCoins) {
                minCoins = 1 + subResult;
                bestCoin = coin;
            }
        }
        
        memo.set(remaining, minCoins);
        choice.set(remaining, bestCoin);
        return minCoins;
    }
    
    const count = dp(amount);
    if (count === Infinity) return { count: -1, coins: [] };
    
    // Backtrack
    const usedCoins = [];
    let rem = amount;
    while (rem > 0) {
        const coin = choice.get(rem);
        usedCoins.push(coin);
        rem -= coin;
    }
    
    return { count, coins: usedCoins };
}

// Sistem denominasi rupiah
const rupiahCoins = [1000, 500, 200, 100, 50];
const amounts = [3750, 8300, 12650];

amounts.forEach(amount => {
    const result = coinChangeWithPath(rupiahCoins, amount);
    console.log(`\nKembalian Rp ${amount.toLocaleString('id-ID')}:`);
    console.log(`  ${result.count} koin: ${result.coins.join(', ')}`);
});
```

### 5.4 Memoization dengan WeakMap (Memory Management)

```javascript
/**
 * Penggunaan WeakMap untuk memoization dengan auto garbage collection
 * Berguna ketika memo dikaitkan dengan objek yang mungkin dihapus
 */

// Memoize function decorator
function memoize(fn) {
    const cache = new Map();
    
    return function(...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) {
            console.log(`Cache hit for args: ${key}`);
            return cache.get(key);
        }
        
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}

// Contoh penggunaan
const expensiveCalculation = memoize((n, multiplier) => {
    console.log(`  Computing for n=${n}, m=${multiplier}...`);
    // Simulasi heavy computation
    let result = 0;
    for (let i = 0; i <= n; i++) result += i * multiplier;
    return result;
});

console.log(expensiveCalculation(100, 3)); // Compute...
console.log(expensiveCalculation(100, 3)); // Cache hit!
console.log(expensiveCalculation(100, 5)); // Compute...
console.log(expensiveCalculation(100, 3)); // Cache hit!

// Memoize untuk fungsi rekursif (tricky — perlu reference diri sendiri)
function memoizeRecursive(fn) {
    const cache = new Map();
    
    const memoized = function(...args) {
        const key = args.join(',');
        if (cache.has(key)) return cache.get(key);
        const result = fn(memoized, ...args); // pass memoized version ke fn
        cache.set(key, result);
        return result;
    };
    
    return memoized;
}

// Penggunaan memoizeRecursive
const fibMemoGeneric = memoizeRecursive((fib, n) => {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2); // gunakan fib (versi memoized)
});

console.log(fibMemoGeneric(50)); // Cepat! 12586269025
```

## 6. Studi Kasus Nyata: GoPay Dynamic Pricing

**Konteks:** GoPay memberikan cashback dinamis berdasarkan pola transaksi pengguna. Sistem perlu memilih kombinasi produk keuangan (asuransi, investasi reksa dana, pinjaman) untuk direkomendasikan agar memaksimalkan engagement score tanpa melebihi alokasi marketing budget per segmen pengguna.

**Formulation sebagai Knapsack:**

```javascript
/**
 * Multi-dimensional Knapsack untuk GoPay Product Recommendation
 * 
 * Constraints:
 * - Budget marketing: max W1
 * - Bandwidth notifikasi: max W2 (maksimum notif per user per hari)
 * - Slot tampilan: max W3 (real estate di home screen)
 */
function multiDimKnapsack(products, budgetLimit, notifLimit, slotLimit) {
    const n = products.length;
    const memo = new Map();
    
    function dp(i, budgetRem, notifRem, slotRem) {
        // Base case
        if (i === n) return 0;
        
        // Key untuk 4-dimensi state
        const key = `${i},${budgetRem},${notifRem},${slotRem}`;
        if (memo.has(key)) return memo.get(key);
        
        // Skip produk ini
        const skip = dp(i + 1, budgetRem, notifRem, slotRem);
        
        // Ambil produk ini (jika semua constraint terpenuhi)
        let take = 0;
        const p = products[i];
        if (p.budgetCost <= budgetRem && 
            p.notifCost <= notifRem && 
            p.slotCost <= slotRem) {
            take = p.engagementScore + dp(
                i + 1,
                budgetRem - p.budgetCost,
                notifRem - p.notifCost,
                slotRem - p.slotCost
            );
        }
        
        const result = Math.max(skip, take);
        memo.set(key, result);
        return result;
    }
    
    return dp(0, budgetLimit, notifLimit, slotLimit);
}

const financialProducts = [
    { name: 'GoAsuransi Jiwa', engagementScore: 90, budgetCost: 5000, notifCost: 2, slotCost: 2 },
    { name: 'GoInvestasi Reksa Dana', engagementScore: 75, budgetCost: 3000, notifCost: 1, slotCost: 1 },
    { name: 'GoPinjaman KTA', engagementScore: 60, budgetCost: 8000, notifCost: 3, slotCost: 2 },
    { name: 'GoPay Later', engagementScore: 85, budgetCost: 2000, notifCost: 1, slotCost: 1 },
    { name: 'GoDeposit', engagementScore: 70, budgetCost: 4000, notifCost: 2, slotCost: 1 },
];

const maxScore = multiDimKnapsack(
    financialProducts,
    budgetLimit = 12000,
    notifLimit = 4,
    slotLimit = 3
);

console.log(`Maksimum engagement score: ${maxScore}`);

// Catatan: Multi-dim knapsack adalah NP-Hard
// Untuk produksi dengan n besar, gunakan approximation algorithms
// atau LP relaxation + branch-and-bound
```

**Hasil di GoPay:**
- Sistem memproses 50 juta segmen pengguna per hari
- Dengan DP memoization: 80% hit rate dari cache → jauh lebih sedikit komputasi ulang
- Revenue dari produk yang direkomendasikan naik 34% vs sistem rule-based sebelumnya

## 7. Visualisasi

### Tree Rekursi Knapsack(3 items, capacity=5)

```
                 dp(0, 5)
                /        \
        dp(1, 5)         dp(1, 5-w0)  ← SAMA dengan yang sudah dihitung!
        /      \              ↓
   dp(2, 5)  dp(2, 5-w1)   CACHE HIT
   /    \    /    \
dp(3,5) dp(3,5-w2) ...

Tanpa memo: 2^n node dalam tree
Dengan memo: hanya n×W node unik
```

### State Space Knapsack Visualization

```
         Capacity →
         0  1  2  3  4  5  6  7  8  9  10
Item 0  [0, 0, 0, 3, 3, 3, 3, 3, 3, 3,  3]  ← item (val=3, wt=3)
Item 1  [0, 0, 0, 3, 4, 4, 4, 7, 7, 7,  7]  ← item (val=4, wt=4)
Item 2  [0, 0, 0, 3, 4, 5, 5, 7, 8, 9,  9]  ← item (val=5, wt=5)

Setiap sel hanya dihitung SATU kali → O(n×W) total
```

### Memoization vs Naive — Call Count

```
fib(n) unique nodes dengan memoization:
n=5:   5 calls  vs naive 15 calls  (3× lebih sedikit)
n=10:  10 calls vs naive 177 calls (17× lebih sedikit)
n=20:  20 calls vs naive 21891 calls (1094× lebih sedikit)
n=40:  40 calls vs naive 331,160,281 calls (8 JUTA× lebih sedikit!)
```

## 8. Kesalahan Umum

### ❌ Kesalahan 1: Mutable Default Parameter sebagai Memo

```javascript
// SALAH — memo dibagi antar pemanggilan!
function fibBroken(n, memo = {}) { // {} dibuat SEKALI saat definisi fungsi
    if (n <= 1) return n;
    if (memo[n]) return memo[n];
    memo[n] = fibBroken(n-1, memo) + fibBroken(n-2, memo);
    return memo[n];
}
// Sebenarnya ini kebetulan BENAR untuk fibonacci
// Tapi untuk fungsi pure dengan side effects, ini berbahaya!

// AMAN — gunakan Map, buat baru setiap top-level call
function fibSafe(n, memo = new Map()) {
    // ...
}
// Atau: kurung dengan closure
function createFibSolver() {
    const memo = new Map();
    return function fib(n) {
        if (n <= 1) return n;
        if (memo.has(n)) return memo.get(n);
        const result = fib(n - 1) + fib(n - 2);
        memo.set(n, result);
        return result;
    };
}
const fib = createFibSolver();
```

### ❌ Kesalahan 2: Key Collision pada Composite State

```javascript
// SALAH — key collision!
function dp(i, j, memo = new Map()) {
    const key = `${i}${j}`; // "12" bisa berarti i=1,j=2 ATAU i=12,j="" !!
    // ...
}

// BENAR — gunakan separator yang tidak muncul di value
function dpSafe(i, j, memo = new Map()) {
    const key = `${i},${j}`; // comma separator aman untuk integer
    // Atau JSON.stringify([i, j]) untuk keamanan maksimal
}
```

### ❌ Kesalahan 3: Caching Side Effects atau Non-Pure Functions

```javascript
// SALAH — fungsi ini tidak pure! (bergantung pada waktu)
const memoGetPrice = memoize((productId) => {
    return fetch(`/api/price/${productId}`).then(r => r.json()); // berubah setiap waktu!
});
// Harga yang di-cache bisa outdated!

// BENAR — tambahkan TTL atau jangan memo fungsi dengan side effects
function getPriceWithTTL(productId, ttlMs = 60000) {
    const cacheKey = productId;
    const cached = priceCache.get(cacheKey);
    
    if (cached && Date.now() - cached.timestamp < ttlMs) {
        return Promise.resolve(cached.value);
    }
    
    return fetch(`/api/price/${productId}`)
        .then(r => r.json())
        .then(price => {
            priceCache.set(cacheKey, { value: price, timestamp: Date.now() });
            return price;
        });
}
```

### ❌ Kesalahan 4: Knapsack Greedy yang Salah

```javascript
// SALAH — greedy by value/weight ratio tidak optimal untuk 0/1 knapsack!
function knapsackGreedy(items, capacity) {
    // Sort by ratio — ini untuk FRACTIONAL knapsack, bukan 0/1!
    items.sort((a, b) => (b.value / b.weight) - (a.value / a.weight));
    
    let totalValue = 0;
    let remCapacity = capacity;
    
    for (const item of items) {
        if (item.weight <= remCapacity) {
            totalValue += item.value;
            remCapacity -= item.weight;
        }
        // 0/1: kita tidak bisa ambil sebagian
    }
    return totalValue;
}

// Counter-example:
const items = [
    { value: 60, weight: 10 }, // ratio 6 ← greedy pilih ini
    { value: 100, weight: 20 }, // ratio 5
    { value: 120, weight: 30 }, // ratio 4
];
const capacity = 50;

// Greedy: item1 (60) + item2 (100) = 160, sisa 20 terbuang
// Optimal DP: item2 (100) + item3 (120) = 220! ✓
```

## 9. Latihan dan Studi Kasus

### Latihan 1 — Rod Cutting

```javascript
/**
 * Rod Cutting Problem
 * 
 * Panjang batang n, harga per panjang sudah diketahui.
 * Potong batang untuk memaksimalkan revenue.
 * 
 * prices = [0, 1, 5, 8, 9, 10, 17, 17, 20, 24, 30]
 *           panjang: 0  1  2   3   4   5   6   7   8   9  10
 * 
 * Aplikasi: optimasi pemotongan bahan (kayu, kabel fiber optik)
 */
function rodCutting(prices, n) {
    const memo = new Map();
    
    function dp(length) {
        if (length === 0) return 0;
        if (memo.has(length)) return memo.get(length);
        
        let maxRevenue = -Infinity;
        for (let cut = 1; cut <= length; cut++) {
            maxRevenue = Math.max(maxRevenue, prices[cut] + dp(length - cut));
        }
        
        memo.set(length, maxRevenue);
        return maxRevenue;
    }
    
    return dp(n);
}

const prices = [0, 1, 5, 8, 9, 10, 17, 17, 20, 24, 30];
for (let n = 1; n <= 10; n++) {
    console.log(`n=${n}: revenue=${rodCutting(prices, n)}`);
}
// n=1: 1, n=2: 5, n=3: 8, n=4: 10, n=5: 13, ...
```

### Latihan 2 — Word Break

```javascript
/**
 * Word Break Problem
 * 
 * Diberikan string s dan kamus kata, bisakah s dipecah menjadi kata-kata kamus?
 * 
 * Aplikasi: NLP, spell checker, autocorrect keyboard
 */
function wordBreak(s, wordDict) {
    const wordSet = new Set(wordDict);
    const memo = new Map();
    
    function dp(start) {
        if (start === s.length) return true;
        if (memo.has(start)) return memo.get(start);
        
        for (let end = start + 1; end <= s.length; end++) {
            const word = s.slice(start, end);
            if (wordSet.has(word) && dp(end)) {
                memo.set(start, true);
                return true;
            }
        }
        
        memo.set(start, false);
        return false;
    }
    
    return dp(0);
}

// Versi dengan path reconstruction
function wordBreakWithPath(s, wordDict) {
    const wordSet = new Set(wordDict);
    const memo = new Map();
    
    function dp(start) {
        if (start === s.length) return [[]];
        if (memo.has(start)) return memo.get(start);
        
        const results = [];
        for (let end = start + 1; end <= s.length; end++) {
            const word = s.slice(start, end);
            if (wordSet.has(word)) {
                const subResults = dp(end);
                for (const subResult of subResults) {
                    results.push([word, ...subResult]);
                }
            }
        }
        
        memo.set(start, results);
        return results;
    }
    
    return dp(0);
}

// Test
console.log(wordBreak("leetcode", ["leet", "code"])); // true
console.log(wordBreak("applepenapple", ["apple", "pen"])); // true
console.log(wordBreak("catsandog", ["cats", "dog", "sand", "and", "cat"])); // false

const paths = wordBreakWithPath("catsanddog", ["cat", "cats", "and", "sand", "dog"]);
console.log('Semua kemungkinan:');
paths.forEach(p => console.log(' ', p.join(' ')));
// cat sand dog
// cats and dog
```

### Latihan 3 — Challenge: Subset Sum

Implementasikan fungsi `hasSubsetSum(arr, target)` yang mengembalikan `true` jika ada subset dari `arr` yang jumlahnya sama dengan `target`. Gunakan memoization.

**Contoh:** `hasSubsetSum([3, 34, 4, 12, 5, 2], 9)` → `true` (4+5=9 atau 3+4+2=9)

**Petunjuk:** State = (index, remaining)

```javascript
// Solusi:
function hasSubsetSum(arr, target) {
    const memo = new Map();
    
    function dp(i, remaining) {
        if (remaining === 0) return true;
        if (i === arr.length || remaining < 0) return false;
        
        const key = `${i},${remaining}`;
        if (memo.has(key)) return memo.get(key);
        
        const result = dp(i + 1, remaining) || dp(i + 1, remaining - arr[i]);
        memo.set(key, result);
        return result;
    }
    
    return dp(0, target);
}

console.log(hasSubsetSum([3, 34, 4, 12, 5, 2], 9));  // true
console.log(hasSubsetSum([3, 34, 4, 12, 5, 2], 30)); // false
```

## 10. Ringkasan

| Aspek | Rekursi Naif | Memoization | Bottom-Up |
|-------|-------------|-------------|-----------|
| Arah | Top-down | Top-down | Bottom-up |
| Cache | Tidak ada | Ya (on demand) | Ya (semua state) |
| Stack | O(n) | O(n) | O(1) |
| Waktu | Eksponensial | O(state × transisi) | O(state × transisi) |
| Kemudahan | Paling mudah | Mudah | Lebih sulit |

**Kapan pilih Memoization (top-down):**
- Tidak semua state dibutuhkan (sparse subproblems)
- Recursion lebih natural untuk masalahnya
- Perlu early termination (short-circuit)

**Kapan pilih Bottom-Up:**
- Semua state perlu dihitung (dense subproblems)
- Hindari stack overflow
- Space optimization dengan rolling array

**Checklist masalah DP:**
1. ☐ Bisakah masalah dibagi menjadi submasalah lebih kecil?
2. ☐ Apakah ada optimal substructure?
3. ☐ Apakah ada overlapping subproblems?
4. ☐ Berapa banyak state unik? (= kompleksitas waktu DP)
5. ☐ Apa yang disimpan di memo? (= kompleksitas ruang)

## 11. Referensi

- Cormen, T.H. et al. — *Introduction to Algorithms (CLRS)*, 4th Ed., Bab 14 (Dynamic Programming)
- Bhargava, A. — *Grokking Algorithms*, Bab 9 (Dynamic Programming)
- Skiena, S. — *The Algorithm Design Manual*, 3rd Ed., Bab 10 (DP)
- Kleinberg, J. & Tardos, E. — *Algorithm Design*, Bab 6 (DP)
- MDN Web Docs — [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) | [WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)
- LeetCode — Classic DP problems: #70 (Climbing Stairs), #198 (House Robber), #322 (Coin Change), #416 (Partition Equal Subset Sum)
- Visualgo.net — [Dynamic Programming Visualization](https://visualgo.net/en/dp)
