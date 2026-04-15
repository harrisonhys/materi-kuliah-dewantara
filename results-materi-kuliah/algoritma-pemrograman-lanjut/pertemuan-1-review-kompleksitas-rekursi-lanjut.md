# Pertemuan 1: Review Kompleksitas & Rekursi Lanjut

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Menganalisis kompleksitas waktu dan ruang algoritma menggunakan notasi Big-O, Big-Θ, dan Big-Ω
- Menerapkan Master Theorem untuk menyelesaikan relasi rekurensi algoritma Divide & Conquer
- Mengimplementasikan Tail Call Optimization (TCO) pada JavaScript untuk menghindari stack overflow
- Membandingkan trade-off antara rekursi biasa vs rekursi ekor vs iterasi

## 2. Pengantar: Hook

Bayangkan kamu diminta menghitung `fibonacci(50)` menggunakan rekursi naif. Di laptop modern dengan 8GB RAM, program kamu **crash** setelah beberapa menit karena stack overflow. Tapi kolega kamu dengan kode yang "terlihat sama" berhasil menghitungnya dalam **milidetik** menggunakan teknik yang berbeda.

Apa bedanya? Bukan hardware — tapi **pemahaman mendalam tentang kompleksitas dan rekursi**. Di industri, perbedaan antara O(2^n) dan O(n log n) bukan sekadar angka akademis — ini membedakan sistem yang **bisa scale** dari yang **crash di production** ketika traffic naik 10x.

Di pertemuan ini kita review fondasi yang akan menjadi bahasa bersama selama satu semester.

## 3. Konsep Utama

### 3.1 Hierarki Kompleksitas

```
O(1) < O(log n) < O(√n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2^n) < O(n!)
```

| Notasi | Nama | Contoh | n=10 | n=100 | n=1000 |
|--------|------|--------|------|-------|--------|
| O(1) | Konstan | Array access | 1 | 1 | 1 |
| O(log n) | Logaritmik | Binary search | 3 | 7 | 10 |
| O(n) | Linear | Linear search | 10 | 100 | 1,000 |
| O(n log n) | Linearitmik | Merge sort | 33 | 664 | 9,966 |
| O(n²) | Kuadratik | Bubble sort | 100 | 10,000 | 1,000,000 |
| O(2^n) | Eksponensial | Fibonacci naif | 1,024 | 10^30 | 💥 |

### 3.2 Tiga Notasi Asimptotik

- **Big-O (O)** — batas atas (worst case): "paling lambat segini"
- **Big-Omega (Ω)** — batas bawah (best case): "paling cepat segini"  
- **Big-Theta (Θ)** — batas ketat: "selalu sekitar segini"

```javascript
// Binary search: O(log n) worst, Ω(1) best, Θ(log n) average
function binarySearch(arr, target) {
    let left = 0, right = arr.length - 1;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (arr[mid] === target) return mid; // Ω(1) — lucky!
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1; // O(log n) — unlucky
}
```

### 3.3 Master Theorem

Untuk relasi rekurensi bentuk: **T(n) = aT(n/b) + f(n)**

di mana:
- `a` = jumlah subproblem
- `b` = faktor pembagi ukuran
- `f(n)` = biaya membagi + menggabungkan

**Tiga Kasus:**

| Kondisi | Hasil | Intuisi |
|---------|-------|---------|
| f(n) = O(n^(log_b(a) - ε)) | T(n) = Θ(n^log_b(a)) | Rekursi dominan |
| f(n) = Θ(n^log_b(a)) | T(n) = Θ(n^log_b(a) × log n) | Seimbang |
| f(n) = Ω(n^(log_b(a) + ε)) | T(n) = Θ(f(n)) | Kombinasi dominan |

**Contoh Aplikasi:**

```
Merge Sort: T(n) = 2T(n/2) + O(n)
→ a=2, b=2, n^log_b(a) = n^log_2(2) = n^1 = n
→ f(n) = O(n) = Θ(n^log_b(a)) → Kasus 2
→ T(n) = Θ(n log n) ✓

Binary Search: T(n) = T(n/2) + O(1)
→ a=1, b=2, n^log_2(1) = n^0 = 1
→ f(n) = O(1) = Θ(1) → Kasus 2
→ T(n) = Θ(log n) ✓

Strassen: T(n) = 7T(n/2) + O(n²)
→ a=7, b=2, n^log_2(7) ≈ n^2.807
→ f(n) = O(n²) = O(n^(2.807 - ε)) → Kasus 1
→ T(n) = Θ(n^2.807) ✓ (lebih baik dari O(n³) naif!)
```

### 3.4 Tail Call Optimization (TCO)

**Tail call** = pemanggilan rekursif adalah ekspresi TERAKHIR yang dievaluasi fungsi.

```javascript
// Rekursi BIASA — BUKAN tail call (stack menumpuk)
function factorialNormal(n) {
    if (n <= 1) return 1;
    return n * factorialNormal(n - 1); // harus tunggu rekursi selesai, baru kali n
    //     ↑ operasi setelah return rekursif = BUKAN tail call
}

// Rekursi EKOR — tail call (bisa dioptimasi jadi loop)
function factorialTail(n, accumulator = 1) {
    if (n <= 1) return accumulator;
    return factorialTail(n - 1, n * accumulator); // ini ekspresi TERAKHIR
    //     ↑ tidak ada operasi setelah return = TAIL CALL
}
```

**Visualisasi Call Stack:**

```
factorialNormal(5):        factorialTail(5):
5 → 4 → 3 → 2 → 1        5 (acc=1)
     ← ← ← ←             → 4 (acc=5)    [frame lama bisa dibuang!]
  120 ← 24 ← 6 ← 2 ← 1   → 3 (acc=20)
Stack depth: O(n)           → 2 (acc=60)
                            → 1 (acc=120)
                            Stack depth: O(1) dengan TCO
```

**Status TCO di JavaScript:**
- **V8 (Node.js/Chrome)**: TCO hanya di strict mode, support masih parsial
- **Workaround: Trampolining** — teknik manual untuk TCO

```javascript
// Trampolining — TCO manual di JavaScript
function trampoline(fn) {
    return function(...args) {
        let result = fn(...args);
        while (typeof result === 'function') {
            result = result(); // panggil thunk sampai selesai
        }
        return result;
    };
}

function factorialTrampoline(n, acc = 1) {
    if (n <= 1) return acc;
    return () => factorialTrampoline(n - 1, n * acc); // return thunk, bukan rekursi langsung
}

const factorial = trampoline(factorialTrampoline);
console.log(factorial(100000)); // tidak stack overflow!
```

## 4. Ilustrasi dan Analogi

### Analogi Memasak

Bayangkan kamu chef di restoran:

- **O(1)** = ambil piring dari rak (selalu 1 gerakan)
- **O(log n)** = cari bumbu di lemari terorganisir (buka setengah, cek, buka setengah lagi)
- **O(n)** = cek semua bahan di kulkas satu per satu
- **O(n log n)** = sortir semua pesanan sebelum masak (efisien tapi perlu prep)
- **O(n²)** = untuk setiap tamu, periksa semua tamu lain untuk pairing meja
- **O(2^n)** = coba semua kombinasi topping pizza → **tidak mungkin untuk menu besar!**

### Analogi TCO: Relay Race vs Telepon Rusak

**Rekursi biasa** = permainan "Telepon Rusak": pesan diteruskan, tapi **semua orang harus tetap berdiri** menunggu jawaban kembali. Semakin panjang rantai, semakin banyak yang berdiri (stack frame menumpuk).

**Tail recursion** = relay race: setelah kamu lempas tongkat (pass accumulator), kamu **boleh duduk** — pekerjaanmu selesai. Hanya satu orang yang berlari di satu waktu.

## 5. Contoh Teknis

### 5.1 Analisis Kompleksitas Kode JavaScript

```javascript
// Soal: Berapa kompleksitas fungsi ini?
function mystery(n) {
    let count = 0;
    for (let i = 0; i < n; i++) {           // O(n)
        for (let j = i; j < n; j++) {       // O(n-i) → rata-rata O(n/2)
            count++;
        }
        // Inner loop: n + (n-1) + (n-2) + ... + 1 = n(n+1)/2
    }
    return count;
}
// Total: O(n²) — walaupun j mulai dari i, masih kuadratik

// Analisis lebih detail:
// Iterasi i=0: j dari 0 ke n → n operasi
// Iterasi i=1: j dari 1 ke n → n-1 operasi
// ...
// Total = n + (n-1) + ... + 1 = n(n+1)/2 = O(n²)
```

### 5.2 Fibonacci dengan Berbagai Pendekatan

```javascript
// 1. Naif — O(2^n) waktu, O(n) ruang (stack)
function fibNaive(n) {
    if (n <= 1) return n;
    return fibNaive(n - 1) + fibNaive(n - 2);
}

// 2. Memoization — O(n) waktu, O(n) ruang
function fibMemo(n, memo = new Map()) {
    if (n <= 1) return n;
    if (memo.has(n)) return memo.get(n);
    const result = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    memo.set(n, result);
    return result;
}

// 3. Tail recursive dengan trampoline — O(n) waktu, O(1) ruang efektif
function fibTailHelper(n, a = 0, b = 1) {
    if (n === 0) return a;
    return () => fibTailHelper(n - 1, b, a + b); // thunk
}
const fibTail = trampoline(fibTailHelper);

// 4. Iteratif — O(n) waktu, O(1) ruang (paling praktis)
function fibIterative(n) {
    if (n <= 1) return n;
    let a = 0, b = 1;
    for (let i = 2; i <= n; i++) {
        [a, b] = [b, a + b]; // destructuring assignment — ES6+
    }
    return b;
}

// Benchmark:
console.time('Naive fib(40)');
console.log(fibNaive(40));    // ~1000ms
console.timeEnd('Naive fib(40)');

console.time('Memo fib(40)');
console.log(fibMemo(40));     // <1ms
console.timeEnd('Memo fib(40)');

console.time('Iterative fib(1000000)');
console.log(fibIterative(1000000)); // <10ms, tapi angkanya Infinity (float overflow)
console.timeEnd('Iterative fib(1000000)');

// Dengan BigInt untuk presisi:
function fibBigInt(n) {
    let a = 0n, b = 1n;
    for (let i = 2; i <= n; i++) {
        [a, b] = [b, a + b];
    }
    return b;
}
console.log(fibBigInt(100)); // 354224848179261915075n — presisi penuh!
```

### 5.3 Implementasi Master Theorem Calculator

```javascript
/**
 * Menghitung kompleksitas menggunakan Master Theorem
 * T(n) = a*T(n/b) + O(n^c * log^k(n))
 */
function masterTheorem(a, b, c, k = 0) {
    const logBA = Math.log(a) / Math.log(b); // log_b(a)
    
    console.log(`T(n) = ${a}T(n/${b}) + O(n^${c} × log^${k}(n))`);
    console.log(`log_${b}(${a}) = ${logBA.toFixed(4)}`);
    
    const epsilon = 1e-9; // toleransi floating point
    
    if (c < logBA - epsilon) {
        // Kasus 1: rekursi dominan
        console.log(`Kasus 1: c=${c} < log_b(a)=${logBA.toFixed(4)}`);
        console.log(`T(n) = Θ(n^${logBA.toFixed(4)})`);
        return `Θ(n^log_${b}(${a}))`;
    } else if (Math.abs(c - logBA) < epsilon) {
        // Kasus 2: seimbang
        const kStr = k > 0 ? ` × log^${k+1}(n)` : ' × log(n)';
        console.log(`Kasus 2: c=${c} = log_b(a)=${logBA.toFixed(4)}`);
        console.log(`T(n) = Θ(n^${c}${kStr})`);
        return `Θ(n^${c} × log^${k+1}(n))`;
    } else {
        // Kasus 3: kombinasi dominan
        console.log(`Kasus 3: c=${c} > log_b(a)=${logBA.toFixed(4)}`);
        const kStr = k > 0 ? ` × log^${k}(n)` : '';
        console.log(`T(n) = Θ(n^${c}${kStr})`);
        return `Θ(n^${c}${kStr})`;
    }
}

// Test cases:
console.log('\n=== Merge Sort ===');
masterTheorem(2, 2, 1); // T(n) = 2T(n/2) + O(n) → Θ(n log n)

console.log('\n=== Binary Search ===');
masterTheorem(1, 2, 0); // T(n) = T(n/2) + O(1) → Θ(log n)

console.log('\n=== Strassen ===');
masterTheorem(7, 2, 2); // T(n) = 7T(n/2) + O(n²) → Θ(n^2.807)

console.log('\n=== Naive Matrix Multiply ===');
masterTheorem(8, 2, 2); // T(n) = 8T(n/2) + O(n²) → Θ(n³)
```

### 5.4 Mengukur Space Complexity

```javascript
// Mengukur kedalaman call stack
function measureRecursionDepth(fn, n) {
    let maxDepth = 0;
    let currentDepth = 0;
    
    // Versi instrumented
    function instrumentedFib(x) {
        currentDepth++;
        maxDepth = Math.max(maxDepth, currentDepth);
        
        if (x <= 1) {
            currentDepth--;
            return x;
        }
        
        const result = instrumentedFib(x - 1) + instrumentedFib(x - 2);
        currentDepth--;
        return result;
    }
    
    const result = instrumentedFib(n);
    return { result, maxDepth };
}

console.log(measureRecursionDepth(null, 10));
// { result: 55, maxDepth: 10 } — O(n) stack depth
```

## 6. Studi Kasus Nyata: GoTo Financial Recommendation Engine

**Konteks:** Sistem rekomendasi produk keuangan GoPay harus memproses jutaan transaksi pengguna per menit untuk memberikan rekomendasi real-time (produk asuransi, pinjaman, investasi).

**Masalah Awal:**
```javascript
// Versi lama — O(n²) untuk menghitung similarity antara semua user
function computeAllSimilarities(users) {
    const similarities = {};
    for (let i = 0; i < users.length; i++) {      // n iterasi
        for (let j = i + 1; j < users.length; j++) { // n iterasi lagi
            const sim = cosineSimilarity(users[i].vector, users[j].vector);
            similarities[`${i}-${j}`] = sim;
        }
    }
    return similarities;
}
// Dengan 1 juta user: 10^12 operasi = tidak mungkin real-time!
```

**Solusi dengan analisis kompleksitas:**

1. **Approximate Nearest Neighbor (ANN)** dengan LSH (Locality Sensitive Hashing):
   - O(n log n) preprocessing
   - O(log n) query per user
   
2. **Hierarchical clustering** — O(n log n) dengan Master Theorem:
   ```
   T(n) = 2T(n/2) + O(n log n)  [divide users, merge clusters]
   → n^log_2(2) = n
   → f(n) = n log n > n → Kasus 3
   → T(n) = Θ(n log n)
   ```

**Hasil:** Latensi rekomendasi turun dari **2.3 detik** menjadi **<50ms** untuk 10 juta pengguna aktif. Revenue dari produk rekomendasikan naik 23%.

**Lesson:** Analisis kompleksitas bukan teori — ini **keputusan bisnis** yang berdampak langsung pada revenue.

## 7. Visualisasi

### Call Tree Fibonacci(5) — Naif vs Memo

```
fibNaive(5):                    fibMemo(5):
        fib(5)                       fib(5)
       /      \                     /      \
   fib(4)    fib(3)            fib(4)    [cache:3]
   /    \    /    \            /    \
fib(3) fib(2) fib(2) fib(1) fib(3) [cache:2]
/   \  / \   / \           /   \
fib(2) fib(1) ...          fib(2) [cache:1]
                           /   \
                        fib(1) fib(0)

Total nodes: 2^n - 1 ≈ 31    Total nodes: n = 5
(banyak duplikat!)            (tiap nilai hanya dihitung sekali)
```

### Grafik Pertumbuhan Kompleksitas

```
Waktu
  |
  |                                    O(2^n) ●
  |                                   ●
  |                                  ●
  |                         O(n²)   ●
  |                       ●●●●●●●●●
  |             O(n log n) ●●●●●●
  |          O(n) ●●●●●●●●
  |     O(log n) ●●●●●
  | O(1) ●
  +---------------------------------> n
    1  10  100  1K  10K  100K
```

### Stack Frame Visualization

```
factorialNormal(4):           factorialTail(4, acc):
┌─────────────────┐           ┌─────────────────┐
│ factorial(4)    │           │ factorial(3,4)   │  ← frame baru
│ menunggu...     │           └─────────────────┘    (frame lama dibuang)
├─────────────────┤
│ factorial(3)    │
│ menunggu...     │
├─────────────────┤
│ factorial(2)    │
│ menunggu...     │
├─────────────────┤
│ factorial(1)    │
│ return 1        │
└─────────────────┘

4 frames aktif bersamaan      Selalu 1 frame aktif
```

## 8. Kesalahan Umum

### ❌ Kesalahan 1: Mengabaikan Konstanta di Konteks Nyata

```javascript
// "Keduanya O(n), sama saja" — SALAH dalam praktik!
function processA(arr) {
    // O(n) dengan konstanta kecil
    return arr.reduce((sum, x) => sum + x, 0);
}

function processB(arr) {
    // O(n) tapi konstanta besar (banyak operasi per elemen)
    return arr.reduce((sum, x) => {
        const encrypted = heavyEncryption(x); // 1000 operasi
        const validated = heavyValidation(encrypted); // 500 operasi
        return sum + validated;
    }, 0);
}

// Untuk n=1000: processA ~1000 ops, processB ~1.5M ops
// Konstanta PENTING di dunia nyata!
```

### ❌ Kesalahan 2: Lupa Space Complexity

```javascript
// Waktu O(n log n) — bagus! Tapi ruang?
function mergeSortInplace_MYTH(arr) {
    // Merge sort SELALU butuh O(n) ruang ekstra
    // Tidak ada "in-place merge sort" yang efisien secara waktu
}

// Untuk array 1GB di Node.js:
// merge sort → butuh 2GB RAM → OutOfMemoryError!
// Solusi: gunakan heapsort (O(n log n) waktu, O(1) ruang)
```

### ❌ Kesalahan 3: Stack Overflow pada Input Besar

```javascript
// SALAH untuk n > 10000 di Node.js (default stack ~10MB)
function sumRecursive(n) {
    if (n === 0) return 0;
    return n + sumRecursive(n - 1); // RangeError: Maximum call stack size exceeded
}

// BENAR — gunakan trampoline atau iterasi
function sumSafe(n) {
    let total = 0;
    for (let i = 1; i <= n; i++) total += i;
    return total; // O(1) ruang
    // Atau: return n * (n + 1) / 2; // O(1) waktu DAN ruang!
}
```

### ❌ Kesalahan 4: Salah Menerapkan Master Theorem

```javascript
// T(n) = T(n-1) + O(1) — BUKAN Master Theorem!
// Master Theorem hanya untuk T(n) = aT(n/b) + f(n)
// Pembaginya harus PERKALIAN (n/b), bukan PENGURANGAN (n-1)

// T(n) = T(n-1) + O(1) → T(n) = O(n) [dari substitusi langsung]
// T(n) = T(n-1) + O(n) → T(n) = O(n²) [dari arithmetic series]
```

## 9. Latihan dan Studi Kasus

### Latihan 1 — Analisis Kompleksitas

Tentukan kompleksitas waktu dan ruang:

```javascript
// Fungsi A
function findPair(arr, target) {
    const seen = new Set();
    for (const num of arr) {
        if (seen.has(target - num)) return true;
        seen.add(num);
    }
    return false;
}
// Waktu: ___  Ruang: ___

// Fungsi B
function matrixPower(matrix, n) {
    if (n === 1) return matrix;
    if (n % 2 === 0) {
        const half = matrixPower(matrix, n / 2);
        return multiply(half, half);
    }
    return multiply(matrix, matrixPower(matrix, n - 1));
}
// Waktu: ___  Ruang (stack): ___
```

**Jawaban:**
- Fungsi A: Waktu O(n), Ruang O(n) — HashSet menyimpan elemen
- Fungsi B: Waktu O(log n × M³) di mana M = ukuran matriks, Ruang O(log n)

### Latihan 2 — Master Theorem

Tentukan kompleksitas:
1. `T(n) = 3T(n/3) + O(n)` → ?
2. `T(n) = 4T(n/2) + O(n²)` → ?
3. `T(n) = 9T(n/3) + O(n²)` → ?

**Jawaban:**
1. log_3(3) = 1, f(n) = O(n) = Θ(n^1) → Kasus 2 → **Θ(n log n)**
2. log_2(4) = 2, f(n) = O(n²) = Θ(n^2) → Kasus 2 → **Θ(n² log n)**
3. log_3(9) = 2, f(n) = O(n²) = Θ(n^2) → Kasus 2 → **Θ(n² log n)**

### Latihan 3 — Implementasi TCO

Konversi ke tail-recursive form menggunakan trampoline:

```javascript
// TODO: Konversi fungsi ini ke tail-recursive
function sumOfSquares(n) {
    if (n === 0) return 0;
    return n * n + sumOfSquares(n - 1);
}

// Petunjuk: gunakan accumulator
function sumOfSquaresTail(n, acc = 0) {
    // Isi di sini
}
const safeSum = trampoline(sumOfSquaresTail);

// Verifikasi: safeSum(100000) tidak boleh throw RangeError
```

**Solusi:**
```javascript
function sumOfSquaresTail(n, acc = 0) {
    if (n === 0) return acc;
    return () => sumOfSquaresTail(n - 1, acc + n * n);
}
const safeSum = trampoline(sumOfSquaresTail);
console.log(safeSum(100)); // 338350
```

## 10. Ringkasan

| Konsep | Inti | Kapan Digunakan |
|--------|------|-----------------|
| Big-O | Worst case complexity | Membandingkan algoritma |
| Master Theorem | Solusi rekurensi D&C | Analisis divide & conquer |
| Tail Call | Return rekursi adalah ekspresi terakhir | Optimasi stack |
| Trampolining | Konversi rekursi → iterasi manual | TCO workaround di JS |
| Space Complexity | Memori ekstra yang dibutuhkan | Sistem dengan RAM terbatas |

**Trade-offs yang harus selalu dipertimbangkan:**

```
Rekursi naif    → Kode bersih, tapi O(2^n) dan stack overflow
Memoization     → O(n) waktu, O(n) ruang, kode masih elegan
Tail recursion  → O(n) waktu, O(1) ruang efektif (dengan TCO)
Iterasi         → O(n) waktu, O(1) ruang, portabel ke semua runtime
```

**Golden Rule:** Pilih algoritma berdasarkan **constraint nyata** — bukan "yang paling keren". Untuk sistem payment processing dengan SLA 99.99%, O(n log n) yang stabil lebih baik dari O(n) yang tidak predictable.

## 11. Referensi

- Cormen, T.H. et al. — *Introduction to Algorithms (CLRS)*, 4th Ed., Bab 3-4 (Growth of Functions & Divide and Conquer)
- Sedgewick, R. & Wayne, K. — *Algorithms*, 4th Ed., Bab 1.4 (Analysis of Algorithms)
- MDN Web Docs — [JavaScript Recursion and Stack](https://developer.mozilla.org/en-US/docs/Glossary/Recursion)
- Wingo, A. — "Tail calls in JavaScript" — V8 Blog
- Kiniry, J. — "Trampolines in JavaScript and CoffeeScript" — *fun.js*
- Khan Academy — [Asymptotic Notation](https://www.khanacademy.org/computing/computer-science/algorithms)
- Bhargava, A. — *Grokking Algorithms*, Bab 1 (Introduction to Algorithms)
