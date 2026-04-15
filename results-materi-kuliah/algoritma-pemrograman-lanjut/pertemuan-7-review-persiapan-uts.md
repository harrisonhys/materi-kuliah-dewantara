# Pertemuan 7: Review & Persiapan UTS

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Merangkum dan membandingkan paradigma algoritma yang telah dipelajari (D&C, DP, Greedy, Backtracking)
- Memilih algoritma yang tepat berdasarkan karakteristik masalah
- Menganalisis kompleksitas waktu dan ruang berbagai algoritma yang telah dipelajari
- Menyelesaikan soal-soal tipe UTS dalam batas waktu yang ditentukan

## 2. Pengantar: Hook

Seorang engineer di Gojek mendapat tiket bug: "Fitur pencarian driver terdekat kadang mengembalikan hasil yang salah, dan kadang timeout." Dia harus:

1. **Diagnosa:** Apakah ini masalah algoritma O(n²) yang terlalu lambat untuk n besar?
2. **Analisis:** Apakah struktur masalahnya cocok untuk D&C, DP, Greedy, atau Backtracking?
3. **Implementasi:** Pilih algoritma yang tepat dan implementasikan
4. **Verifikasi:** Buktikan kompleksitasnya dan test edge cases

Kemampuan ini — memilih paradigma yang tepat, bukan sekadar menghafal algoritma — adalah yang diuji di UTS dan yang dibutuhkan di industri.

## 3. Peta Paradigma Algoritma

### 3.1 Decision Tree: Memilih Algoritma

```
Apakah masalah bisa dipecah menjadi submasalah INDEPENDEN?
│
├── YA → Apakah ada OVERLAP antar submasalah?
│   │
│   ├── YA → DYNAMIC PROGRAMMING
│   │        (Fibonacci, Knapsack, LCS, Edit Distance)
│   │
│   └── TIDAK → DIVIDE & CONQUER
│                (Merge Sort, Closest Pair, Binary Search)
│
└── TIDAK → Apakah pilihan LOKAL OPTIMAL = GLOBAL OPTIMAL?
    │
    ├── YA → GREEDY
    │        (Activity Selection, Huffman, Fractional Knapsack)
    │
    └── TIDAK → Perlu EXHAUSTIVE SEARCH?
        │
        ├── YA + bisa PRUNE → BACKTRACKING
        │                     (N-Queens, Sudoku, Subset Sum)
        │
        └── TIDAK → DYNAMIC PROGRAMMING dengan state tambahan
```

### 3.2 Tabel Ringkasan P1-P6

| Topik | Paradigma | Key Technique | Complexity | Contoh |
|-------|-----------|--------------|------------|--------|
| Rekursi + TCO | Fundamental | Trampolining | O(n) time, O(1) space | Factorial |
| Closest Pair | D&C | Strip analysis (max 7 pts) | O(n log n) | Geolocation |
| Strassen | D&C | 7 perkalian vs 8 | O(n^2.81) | Matrix multiply |
| Fibonacci Memo | DP Top-down | Memoization + cache | O(n) | Sequence calc |
| 0/1 Knapsack | DP Top-down | State: (item, capacity) | O(nW) | OVO cashback |
| LCS | DP Bottom-up | 2D table, backtrack | O(mn) | Reconciliation |
| LIS | DP Bottom-up | Binary search variant | O(n log n) | Stock analysis |
| Edit Distance | DP Bottom-up | Rolling array opt | O(mn) | Fuzzy search |
| Activity Selection | Greedy | Earliest finish time | O(n log n) | Scheduling |
| Huffman Coding | Greedy | Min-heap, frequency | O(n log n) | Compression |
| N-Queens | Backtracking | Col + diagonal sets | O(n!) | Puzzle |
| Sudoku | Backtracking + MRV | Constraint propagation | Fast in practice | Game solver |

### 3.3 Master Theorem Quick Reference

```
T(n) = aT(n/b) + O(n^c)

log_b(a) vs c:
  c < log_b(a) → T(n) = Θ(n^log_b(a))   [Kasus 1: rekursi dominan]
  c = log_b(a) → T(n) = Θ(n^c × log n)  [Kasus 2: seimbang]
  c > log_b(a) → T(n) = Θ(n^c)          [Kasus 3: combine dominan]

Hafalan:
  Merge Sort:   2T(n/2) + O(n)   → O(n log n)
  Binary Search: T(n/2) + O(1)   → O(log n)
  Strassen:    7T(n/2) + O(n²)   → O(n^2.807)
```

### 3.4 DP: Memoization vs Tabulation

```
Memoization (Top-down):        Tabulation (Bottom-up):
─────────────────────────      ────────────────────────────
+ Hanya hitung state yg perlu  + Tidak ada overhead rekursi
+ Lebih natural/intuitif       + Mudah space optimization
+ Early termination            + Cache-friendly (sequential)
- Stack overflow (n besar)     - Hitung semua state (meski tidak perlu)
- Cache miss lebih banyak      - Urutan pengisian harus benar
```

## 4. Ilustrasi dan Analogi: Framework "PODCC"

**P** — Problem Type: sequence? grid? graph? set?
**O** — Optimal Substructure? (DP atau Greedy)
**D** — Dimension: berapa variabel yang mendefinisikan state?
**C** — Constraint: apa yang membatasi pilihan?
**C** — Complexity: hasil kompleksitas masuk akal?

Gunakan framework ini sebelum coding untuk menghemat waktu.

## 5. Contoh Teknis: Komparasi Implementasi

### 5.1 Fibonacci — Semua Pendekatan Dibandingkan

```javascript
// Rangkuman semua pendekatan yang sudah dipelajari

// 1. Naif: O(2^n)
const fib1 = n => n <= 1 ? n : fib1(n-1) + fib1(n-2);

// 2. Memo: O(n) time, O(n) space
const fib2 = (n, m = new Map()) => {
    if (n <= 1) return n;
    if (m.has(n)) return m.get(n);
    const r = fib2(n-1, m) + fib2(n-2, m);
    m.set(n, r);
    return r;
};

// 3. Tabulation: O(n) time, O(n) space
const fib3 = n => {
    if (n <= 1) return n;
    const dp = [0, 1];
    for (let i = 2; i <= n; i++) dp[i] = dp[i-1] + dp[i-2];
    return dp[n];
};

// 4. Space optimized: O(n) time, O(1) space
const fib4 = n => {
    if (n <= 1) return n;
    let [a, b] = [0, 1];
    for (let i = 2; i <= n; i++) [a, b] = [b, a+b];
    return b;
};

// 5. Matrix exponentiation: O(log n) time (untuk n sangat besar)
function matMul(A, B) {
    return [
        [A[0][0]*B[0][0] + A[0][1]*B[1][0], A[0][0]*B[0][1] + A[0][1]*B[1][1]],
        [A[1][0]*B[0][0] + A[1][1]*B[1][0], A[1][0]*B[0][1] + A[1][1]*B[1][1]]
    ];
}

function matPow(M, n) {
    if (n === 1) return M;
    if (n % 2 === 0) {
        const half = matPow(M, n/2);
        return matMul(half, half);
    }
    return matMul(M, matPow(M, n-1));
}

const fib5 = n => {
    if (n <= 1) return n;
    const result = matPow([[1,1],[1,0]], n);
    return result[0][1];
};

// Benchmark
[30, 40, 45].forEach(n => {
    // Skip fib1 untuk n besar (terlalu lambat)
    if (n <= 35) {
        console.time(`fib1(${n})`);
        fib1(n);
        console.timeEnd(`fib1(${n})`);
    }
    
    console.time(`fib2(${n})`);
    fib2(n);
    console.timeEnd(`fib2(${n})`);
    
    console.time(`fib5(${n})`);
    fib5(n);
    console.timeEnd(`fib5(${n})`);
});
```

### 5.2 Problem: Coin Change Comparison

```javascript
// Masalah yang sama, diselesaikan 3 cara berbeda

const coins = [1, 5, 6, 9];
const amount = 11;

// DP Bottom-up
function coinDP(coins, amount) {
    const dp = Array(amount + 1).fill(Infinity);
    dp[0] = 0;
    
    for (let i = 1; i <= amount; i++) {
        for (const coin of coins) {
            if (coin <= i && dp[i - coin] + 1 < dp[i]) {
                dp[i] = dp[i - coin] + 1;
            }
        }
    }
    
    return dp[amount] === Infinity ? -1 : dp[amount];
}

// Greedy (SALAH untuk denominasi ini)
function coinGreedy(coins, amount) {
    const sorted = [...coins].sort((a, b) => b - a);
    let count = 0, rem = amount;
    for (const c of sorted) while (rem >= c) { rem -= c; count++; }
    return rem === 0 ? count : -1;
}

console.log('DP:', coinDP(coins, amount));       // 2: [5,6] atau [2,9]
console.log('Greedy:', coinGreedy(coins, amount)); // mungkin salah!

// coins=[1,5,6,9], amount=11:
// Greedy: 9+1+1 = 3 koin
// DP optimal: 5+6 = 2 koin ← Greedy SALAH!
```

### 5.3 Algoritma yang Sering Dikacaukan

```javascript
// 1. LCS vs Edit Distance — berbeda!
// LCS: subsequence terpanjang yang sama
// Edit Distance: operasi minimum untuk transform

// Contoh: s1="ABCD", s2="ACBD"
// LCS = "ACD" atau "ABD" → panjang 3
// Edit Distance: ABCD → ACBD (swap B dan C) → bisa 2 operations

// 2. Activity Selection vs Weighted Job Scheduling
// Activity Selection: semua job bernilai sama → GREEDY
// Weighted Job Scheduling: job punya profit berbeda → DP

// 3. Fractional vs 0/1 Knapsack
// Fractional: bisa ambil sebagian → GREEDY (sort by ratio)
// 0/1: ambil semua atau tidak → DP

// 4. D&C vs DP
// D&C: submasalah INDEPENDEN (merge sort bagian kiri tidak tergantung kanan)
// DP: submasalah OVERLAP (fib(3) dipakai oleh fib(4) dan fib(5))
```

## 6. Studi Kasus: Soal Tipe UTS

### Soal 1 — Analisis Kompleksitas (20 poin)

Tentukan kompleksitas waktu dan ruang fungsi berikut:

```javascript
function mystery(n) {
    if (n <= 1) return 1;
    return mystery(Math.floor(n/2)) + mystery(Math.floor(n/3)) + n;
}
```

**Pembahasan:**
```
T(n) = T(n/2) + T(n/3) + n

Ini bukan standard Master Theorem (dua subproblem dengan pembagi berbeda).
Gunakan Akra-Bazzi method atau estimasi:

T(n/2) + T(n/3) < 2T(n/2) → ini ≤ T(n) = 2T(n/2) + n → O(n log n)
T(n/2) + T(n/3) > T(n/2) → ini ≥ T(n) = T(n/2) + n → O(n)

Analisis lebih teliti dengan Akra-Bazzi:
T(n) = Θ(n) [karena overhead combine = n dominan dibanding rekursi]

Jawaban: O(n) waktu, O(log n) ruang (stack depth)
```

### Soal 2 — Master Theorem (15 poin)

Selesaikan rekurensi berikut:
1. `T(n) = 4T(n/2) + O(n²)`
2. `T(n) = 3T(n/3) + O(n)`
3. `T(n) = 2T(n/4) + O(√n)`

**Jawaban:**
```
1. a=4, b=2, log_2(4)=2, f(n)=O(n²) = O(n^(log_b a)) → Kasus 2
   T(n) = Θ(n² log n)

2. a=3, b=3, log_3(3)=1, f(n)=O(n) = O(n^1) → Kasus 2
   T(n) = Θ(n log n)

3. a=2, b=4, log_4(2)=0.5, f(n)=O(n^0.5) = O(n^(log_b a)) → Kasus 2
   T(n) = Θ(√n × log n)
```

### Soal 3 — Implementasi DP (30 poin)

Implementasikan solusi DP untuk "Longest Palindromic Subsequence":

Diberikan string s, temukan panjang subsequence palindrom terpanjang.
Contoh: s="BBABCBCAB" → LPS="BABCBAB" → panjang 7

```javascript
function longestPalindromeSubseq(s) {
    const n = s.length;
    // dp[i][j] = panjang LPS dari s[i..j]
    const dp = Array.from({length: n}, () => Array(n).fill(0));
    
    // Base: setiap karakter tunggal adalah palindrom panjang 1
    for (let i = 0; i < n; i++) dp[i][i] = 1;
    
    // Isi dari substring pendek ke panjang
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            
            if (s[i] === s[j]) {
                dp[i][j] = (len === 2) ? 2 : dp[i+1][j-1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
            }
        }
    }
    
    return dp[0][n-1];
}

console.log(longestPalindromeSubseq("BBABCBCAB")); // 7
console.log(longestPalindromeSubseq("abcd"));       // 1
console.log(longestPalindromeSubseq("cbbd"));       // 2 ("bb")
```

### Soal 4 — Analisis Greedy (20 poin)

**Soal:** Apakah greedy "pilih aktivitas dengan durasi terpendek" optimal untuk Activity Selection? Buktikan dengan counter-example atau Exchange Argument.

**Jawaban:**
```
TIDAK optimal. Counter-example:

Aktivitas:
A: start=0, end=10 (durasi 10)
B: start=0, end=3 (durasi 3)
C: start=4, end=7 (durasi 3)
D: start=8, end=11 (durasi 3)

Greedy durasi terpendek: pilih B, C, D → 3 aktivitas
Greedy earliest finish: pilih B (end=3), C (end=7), D (end=11) → 3 aktivitas
(kebetulan sama di contoh ini)

Counter-example yang lebih jelas:
A: start=1, end=2 (durasi 1)
B: start=0, end=5 (durasi 5) — tapi jika greedy durasi pilih A, overlap dengan C
C: start=2, end=3 (durasi 1)
D: start=3, end=4 (durasi 1)

Greedy durasi: A(1-2), C(2-3), D(3-4) = 3 aktivitas ✓
(di sini keduanya sama)

Proper counter-example:
A: start=0, end=5
B: start=0, end=2
C: start=3, end=5
D: start=2, end=3

Durasi: A=5, B=2, D=1, C=2
Greedy durasi: D(2-3), B(0-2): conflict! → D, C(3-5) → 2 atau D, B, C → hmm

Sebenarnya greedy durasi BISA optimal untuk beberapa kasus tapi tidak guaranteed.
Satu-satunya yang terbukti optimal: earliest finish time (Exchange Argument).
```

### Soal 5 — Backtracking Analysis (15 poin)

```javascript
// Berapa jumlah node yang dikunjungi algorithm ini untuk input (3, 9)?
function countSubsets(arr, target, idx = 0, current = 0) {
    if (current === target) return 1;
    if (idx === arr.length || current > target) return 0; // pruning!
    
    return countSubsets(arr, target, idx + 1, current + arr[idx]) + // ambil
           countSubsets(arr, target, idx + 1, current);              // skip
}

// Tanpa pruning: 2^n = 2^3 = 8 nodes
// Dengan pruning (current > target): lebih sedikit
// Untuk [1,2,3] dan target=9: hanya {1+2+3=6 < 9} → 0 solusi, tapi explore semua? 

// ANALISIS:
// [1,2,3]: max sum = 6 < 9 → semua path akan explore (pruning tidak trigger)
// Total nodes: 2^3 × 2 - 1 = 15 (2^(n+1) - 1 untuk full binary tree)
```

## 7. Visualisasi: Algorithm Landscape

```
ALGORITMA PEMROGRAMAN LANJUT — Peta Konsep

Analisis Kompleksitas
├── Big-O, Big-Θ, Big-Ω
├── Master Theorem
└── Tail Call Optimization

Divide & Conquer
├── Closest Pair O(n log n)        → KD-Tree (praktis)
├── Strassen O(n^2.807)            → ML/Graphics
└── Quick Select O(n) avg          → Percentile stats

Dynamic Programming
├── Top-Down (Memoization)
│   ├── Fibonacci: O(n)
│   ├── 0/1 Knapsack: O(nW)
│   └── Coin Change: O(n×coins)
└── Bottom-Up (Tabulation)
    ├── LCS: O(mn)                 → Reconciliation
    ├── LIS: O(n²) atau O(n log n) → Sequence analysis
    └── Edit Distance: O(mn)       → Fuzzy search

Greedy
├── Activity Selection: O(n log n) → Scheduling
├── Huffman Coding: O(n log n)     → Compression
└── Fractional Knapsack: O(n log n)→ Resource allocation

Backtracking
├── N-Queens: O(n!)               → Constraint solving
├── Sudoku + MRV: Fast in practice → Game AI
└── Subset/Permutation: O(k^n)    → Test generation
```

## 8. Kesalahan Umum di UTS

### ❌ 1: Confuse antara O(n log n) dan O(n²)

```javascript
// Ini O(n log n) atau O(n²)?
function foo(arr) {
    for (let i = 0; i < arr.length; i++) {      // n
        // binary search: O(log n)
        binarySearch(arr, arr[i]);
    }
}
// O(n × log n) = O(n log n) ✓
```

### ❌ 2: Lupa Base Case Master Theorem

Master Theorem HANYA berlaku untuk `T(n) = aT(n/b) + f(n)` dengan `n/b` adalah pembagian (bukan pengurangan). `T(n) = T(n-1) + O(1)` BUKAN Master Theorem → solusi manual: O(n).

### ❌ 3: Greedy untuk Masalah 0/1 Knapsack

Selalu gunakan DP untuk 0/1 Knapsack. Greedy hanya untuk fractional.

### ❌ 4: Lupa Return Value di Backtracking

```javascript
// SALAH — tidak return true ketika rekursi berhasil
function solve(state) {
    if (isDone(state)) return true;
    for (const choice of choices) {
        apply(state, choice);
        solve(state); // LUPA return!
        undo(state, choice);
    }
    return false;
}

// BENAR
function solveCorrect(state) {
    if (isDone(state)) return true;
    for (const choice of choices) {
        apply(state, choice);
        if (solveCorrect(state)) return true; // propagate success!
        undo(state, choice);
    }
    return false;
}
```

## 9. Soal Latihan UTS

### Soal A — Teori (30 poin)

1. (10 poin) Jelaskan perbedaan antara Optimal Substructure di D&C dan DP. Berikan satu contoh masalah yang memiliki Optimal Substructure tapi TIDAK memiliki Overlapping Subproblems (sehingga D&C cocok, bukan DP).

2. (10 poin) Selesaikan rekurensi: `T(n) = 2T(n/2) + Θ(n log n)`. Jelaskan kasus Master Theorem yang berlaku.

3. (10 poin) Mengapa greedy "earliest deadline" BUKAN strategi optimal untuk Activity Selection, sedangkan "earliest finish" ADALAH optimal? Berikan Exchange Argument.

### Soal B — Analisis Kasus (35 poin)

Sebuah platform ride-sharing memiliki daftar driver (posisi koordinat x,y) dan customer (koordinat x,y). Sistem perlu memasangkan customer-driver terdekat.

**Pertanyaan:**
1. (10 poin) Jika ada 10.000 driver dan 1.000 customer, berapa operasi yang dibutuhkan dengan brute-force O(n×m)?
2. (15 poin) Usulan algoritma yang lebih efisien. Analisis kompleksitasnya.
3. (10 poin) Jika customer memiliki preferensi (misalnya: hanya mau driver dengan rating ≥ 4.5), bagaimana constraint ini mempengaruhi algoritma?

**Jawaban panduan:**
1. 10.000 × 1.000 = 10 juta operasi — masih feasible tapi bisa lebih baik
2. KD-Tree: Build O(n log n), query O(log n) per customer → O(n log n + m log n) total
3. Pre-filter driver dengan rating ≥ 4.5 sebelum insert ke KD-Tree (offline), atau gunakan ranged query dengan constraint

### Soal C — Implementasi (35 poin)

Implementasikan fungsi `maxProfit(prices)` yang mengembalikan profit maksimum dari pembelian dan penjualan saham, dengan constraint:
- Boleh beli-jual berkali-kali
- Setelah jual, ada "cooldown" 1 hari (tidak bisa langsung beli)

```javascript
// Input: [1, 2, 3, 0, 2]
// Output: 3
// Penjelasan: beli hari 0 (harga 1), jual hari 2 (harga 3) = profit 2
//             cooldown hari 3, beli hari 4 (harga 2) — TIDAK bisa profit
//             Atau: beli hari 0, jual hari 2, cooldown hari 3, tidak beli hari 4
//             Total: 2

// Hint: State = (held, cooldown, free)
// held = punya saham
// cooldown = habis jual, tidak bisa beli besok
// free = bisa beli atau tidak melakukan apa-apa
```

**Solusi:**
```javascript
function maxProfit(prices) {
    if (prices.length === 0) return 0;
    
    let held = -prices[0]; // state: punya saham (beli hari ini)
    let cooldown = 0;       // state: habis jual (tidak bisa beli besok)
    let free = 0;           // state: bebas (tidak punya saham, tidak cooldown)
    
    for (let i = 1; i < prices.length; i++) {
        const prevHeld = held;
        const prevCooldown = cooldown;
        const prevFree = free;
        
        // Hari ini punya saham = (tadi punya, tidak melakukan apa) 
        //                     ATAU (tadi bebas, beli sekarang)
        held = Math.max(prevHeld, prevFree - prices[i]);
        
        // Hari ini cooldown = tadi punya saham, jual sekarang
        cooldown = prevHeld + prices[i];
        
        // Hari ini bebas = (tadi cooldown) ATAU (tadi bebas)
        free = Math.max(prevCooldown, prevFree);
    }
    
    return Math.max(cooldown, free); // tidak bisa return dengan saham di tangan
}

console.log(maxProfit([1, 2, 3, 0, 2])); // 3
console.log(maxProfit([1]));              // 0
console.log(maxProfit([1, 2]));           // 1
```

## 10. Ringkasan Akhir

### Cheat Sheet untuk UTS

```
PILIH PARADIGMA:
─────────────────────────────────────────────────────
Masalah memiliki OPTIMAL SUBSTRUCTURE + OVERLAPPING SUBPROBLEMS
→ DYNAMIC PROGRAMMING (memoization atau tabulation)

Masalah memiliki OPTIMAL SUBSTRUCTURE, TIDAK overlapping
→ DIVIDE & CONQUER

Greedy choice property (lokal = global optimal, terbukti)
→ GREEDY (selalu lebih cepat dari DP jika valid)

Perlu semua solusi, bisa prune dead-ends
→ BACKTRACKING

─────────────────────────────────────────────────────
KOMPLEKSITAS YANG HARUS DIHAFAL:
Binary Search:    O(log n)
Merge Sort:       O(n log n)
Heap Sort:        O(n log n)
Quick Sort:       O(n log n) avg, O(n²) worst
Matrix Multiply:  O(n³) naive, O(n^2.807) Strassen
Closest Pair D&C: O(n log n)
Fibonacci DP:     O(n)
LCS / Edit Dist:  O(mn)
Dijkstra:         O((V+E) log V)  ← dibahas P8
```

## 11. Referensi

- Cormen, T.H. et al. — *Introduction to Algorithms (CLRS)*, 4th Ed., Bab 1-16
- Bhargava, A. — *Grokking Algorithms* — Review visual semua topik P1-P6
- Skiena, S. — *The Algorithm Design Manual*, 3rd Ed. — Bab 1-11
- LeetCode — Study Plan: "Dynamic Programming", "Divide and Conquer"
- Visualgo.net — Visualisasi semua algoritma yang dipelajari
- Big-O Cheat Sheet: [bigocheatsheet.com](https://bigocheatsheet.com)
- CS50 Harvard — Algorithm Analysis lecture notes (tersedia online gratis)
