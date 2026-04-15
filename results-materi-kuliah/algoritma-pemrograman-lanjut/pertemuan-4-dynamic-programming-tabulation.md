# Pertemuan 4: Dynamic Programming — Tabulation

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Mengimplementasikan solusi bottom-up (tabulation) untuk masalah LCS, LIS, dan Edit Distance
- Mengoptimalkan ruang tabel DP dengan teknik rolling array
- Melakukan rekonstruksi solusi (path backtracking) dari tabel DP
- Membandingkan trade-off antara memoization (top-down) dan tabulation (bottom-up)

## 2. Pengantar: Hook

Ketika kamu mengetik kata yang salah di WhatsApp, fitur autocorrect secara instan menyarankan koreksi yang benar. Di balik itu ada algoritma **Edit Distance (Levenshtein)** — yang menghitung berapa langkah minimum untuk mengubah satu kata menjadi kata lain.

Gojek menggunakan algoritma serupa untuk mencocokkan nama merchant yang diketik pengguna dengan database: "Alfmart" → "Alfamart" (1 insert), "McDonals" → "McDonald's" (1 insert). Sistem harus melakukannya untuk **jutaan query per detik** dengan latensi < 5ms.

Rahasianya: bukan memoization top-down (yang masih punya overhead rekursi), tapi **tabulation** — membangun tabel dari bawah ke atas, tanpa rekursi sama sekali, dengan ruang yang bisa dioptimasi menjadi **O(m) saja**.

## 3. Konsep Utama

### 3.1 Tabulation vs Memoization

| Aspek | Memoization (Top-Down) | Tabulation (Bottom-Up) |
|-------|----------------------|----------------------|
| Arah | Dari masalah besar → kecil | Dari masalah kecil → besar |
| Implementasi | Rekursif + cache | Iteratif + tabel |
| Stack frames | O(n) — bisa overflow | O(1) — tidak ada rekursi |
| Subproblem | Hanya yang dibutuhkan | Semua (meski tidak perlu) |
| Cache hit | Hanya state yang diakses | Semua state prefilled |
| Space opt. | Sulit (cache tidak berurutan) | Mudah (rolling array) |

**Aturan praktis:** Gunakan tabulation ketika:
- Semua state pasti akan dihitung
- Perlu optimasi ruang (rolling array)
- Menghindari stack overflow untuk n besar

### 3.2 Longest Common Subsequence (LCS)

**Problem:** Panjang subsequence terpanjang yang dimiliki dua string.

```
s1 = "ABCBDAB"  → A B C B D A B
s2 = "BDCAB"    → B D C A B

LCS = "BCAB" (panjang 4) atau "BDAB" (panjang 4)
```

**Recurrence:**
```
dp[i][j] = LCS dari s1[0..i-1] dan s2[0..j-1]

Jika s1[i-1] == s2[j-1]:
    dp[i][j] = dp[i-1][j-1] + 1  ← karakter cocok, perpanjang LCS

Jika s1[i-1] != s2[j-1]:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])  ← skip salah satu karakter
```

### 3.3 Longest Increasing Subsequence (LIS)

**Problem:** Panjang subsequence terpanjang yang elemen-elemennya menaik secara ketat.

```
arr = [10, 9, 2, 5, 3, 7, 101, 18]
LIS = [2, 5, 7, 101] atau [2, 3, 7, 101] (panjang 4)
```

**Recurrence DP O(n²):**
```
dp[i] = panjang LIS yang berakhir di arr[i]

dp[i] = 1 + max(dp[j]) untuk semua j < i dengan arr[j] < arr[i]
```

**Pendekatan binary search O(n log n):**
- Maintain array `tails` di mana `tails[i]` = elemen terkecil yang bisa mengakhiri IS panjang i+1
- Untuk setiap elemen, binary search posisi di `tails`

### 3.4 Edit Distance (Levenshtein)

**Problem:** Minimum operasi (insert/delete/replace) untuk mengubah string A ke B.

**Recurrence:**
```
dp[i][j] = edit distance dari A[0..i-1] ke B[0..j-1]

Base:
    dp[i][0] = i  (hapus i karakter dari A)
    dp[0][j] = j  (tambah j karakter dari kosong ke B)

Transisi:
    Jika A[i-1] == B[j-1]:
        dp[i][j] = dp[i-1][j-1]  (tidak perlu operasi)
    Else:
        dp[i][j] = 1 + min(
            dp[i-1][j],   // delete A[i-1]
            dp[i][j-1],   // insert B[j-1]
            dp[i-1][j-1]  // replace A[i-1] dengan B[j-1]
        )
```

## 4. Ilustrasi dan Analogi

### Analogi LCS: DNA Sequence Alignment

Bioinformatik menggunakan LCS untuk membandingkan urutan DNA. Dua sampel DNA mungkin memiliki subsequence bersama yang menunjukkan hubungan evolusioner.

Dalam konteks fintech: **reconciliation sistem** — dua log transaksi dari sistem berbeda (database master vs backup) dibandingkan LCS-nya untuk menemukan transaksi mana yang sama, mana yang hanya ada di satu sistem.

### Analogi Edit Distance: Git Diff

Ketika Git menampilkan `diff`, ia menggunakan Edit Distance untuk menentukan perubahan minimal antara dua versi file. Setiap baris adalah "karakter" — baris yang diubah = replace, baris baru = insert, baris dihapus = delete.

### Analogi Tabel DP: Spreadsheet

Tabulation = mengisi spreadsheet dari sel A1 ke kanan dan ke bawah, di mana setiap sel bergantung pada sel di kiri dan atasnya. Kamu tidak perlu rekursi — cukup ikuti urutan pengisian yang benar.

## 5. Contoh Teknis

### 5.1 LCS — Implementasi Lengkap dengan Backtracking

```javascript
/**
 * Longest Common Subsequence
 * 
 * @param {string} s1
 * @param {string} s2
 * @returns {Object} - {length, lcs}
 */
function longestCommonSubsequence(s1, s2) {
    const m = s1.length;
    const n = s2.length;
    
    // Buat tabel (m+1) × (n+1) diinisialisasi 0
    const dp = Array.from({length: m + 1}, () => Array(n + 1).fill(0));
    
    // Bottom-up fill
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (s1[i - 1] === s2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    
    // Backtrack untuk mendapatkan LCS aktual
    let i = m, j = n;
    const lcsChars = [];
    
    while (i > 0 && j > 0) {
        if (s1[i - 1] === s2[j - 1]) {
            lcsChars.unshift(s1[i - 1]); // prepend karakter
            i--;
            j--;
        } else if (dp[i - 1][j] > dp[i][j - 1]) {
            i--;
        } else {
            j--;
        }
    }
    
    return {
        length: dp[m][n],
        lcs: lcsChars.join('')
    };
}

// Aplikasi: Transaction Reconciliation
function reconcileTransactions(log1, log2) {
    const ids1 = log1.map(t => t.id);
    const ids2 = log2.map(t => t.id);
    
    // Konversi ke string untuk LCS
    const s1 = ids1.join(',');
    const s2 = ids2.join(',');
    
    // Lebih tepat: LCS pada array of strings
    function lcsArray(arr1, arr2) {
        const m = arr1.length, n = arr2.length;
        const dp = Array.from({length: m + 1}, () => Array(n + 1).fill(0));
        
        for (let i = 1; i <= m; i++) {
            for (let j = 1; j <= n; j++) {
                if (arr1[i-1] === arr2[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
                else dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
        
        // Backtrack
        const common = [];
        let i = m, j = n;
        while (i > 0 && j > 0) {
            if (arr1[i-1] === arr2[j-1]) {
                common.unshift(arr1[i-1]);
                i--; j--;
            } else if (dp[i-1][j] > dp[i][j-1]) i--;
            else j--;
        }
        
        return common;
    }
    
    const commonIds = lcsArray(ids1, ids2);
    const onlyInLog1 = ids1.filter(id => !commonIds.includes(id));
    const onlyInLog2 = ids2.filter(id => !commonIds.includes(id));
    
    return { common: commonIds, onlyInLog1, onlyInLog2 };
}

// Test LCS
const { length, lcs } = longestCommonSubsequence("ABCBDAB", "BDCAB");
console.log(`LCS length: ${length}, LCS: "${lcs}"`);

// Test reconciliation
const masterLog = [
    { id: 'TXN001' }, { id: 'TXN002' }, { id: 'TXN003' }, { id: 'TXN005' }
];
const backupLog = [
    { id: 'TXN001' }, { id: 'TXN003' }, { id: 'TXN004' }, { id: 'TXN005' }
];

const reconciled = reconcileTransactions(masterLog, backupLog);
console.log('Sama di kedua log:', reconciled.common);
console.log('Hanya di master:', reconciled.onlyInLog1);  // TXN002
console.log('Hanya di backup:', reconciled.onlyInLog2);  // TXN004
```

### 5.2 LIS — Dua Implementasi

```javascript
/**
 * Longest Increasing Subsequence
 */

// Approach 1: DP O(n²) dengan backtracking
function lisDP(arr) {
    const n = arr.length;
    const dp = Array(n).fill(1);
    const prev = Array(n).fill(-1);
    
    let maxLen = 1;
    let maxIdx = 0;
    
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (arr[j] < arr[i] && dp[j] + 1 > dp[i]) {
                dp[i] = dp[j] + 1;
                prev[i] = j;
            }
        }
        if (dp[i] > maxLen) {
            maxLen = dp[i];
            maxIdx = i;
        }
    }
    
    // Backtrack
    const lis = [];
    let idx = maxIdx;
    while (idx !== -1) {
        lis.unshift(arr[idx]);
        idx = prev[idx];
    }
    
    return { length: maxLen, sequence: lis };
}

// Approach 2: Binary Search O(n log n) — lebih efisien
function lisBinarySearch(arr) {
    const tails = []; // tails[i] = smallest tail element of IS of length i+1
    const tailIdx = []; // untuk backtracking
    const prev = Array(arr.length).fill(-1);
    const posToIdx = []; // mapping posisi di tails ke index di arr
    
    function binarySearchPos(val) {
        let lo = 0, hi = tails.length;
        while (lo < hi) {
            const mid = (lo + hi) >> 1;
            if (tails[mid] < val) lo = mid + 1;
            else hi = mid;
        }
        return lo;
    }
    
    for (let i = 0; i < arr.length; i++) {
        const pos = binarySearchPos(arr[i]);
        tails[pos] = arr[i];
        posToIdx[pos] = i;
        
        if (pos > 0) prev[i] = posToIdx[pos - 1];
    }
    
    // Backtrack dari posisi terakhir
    const lis = [];
    let idx = posToIdx[tails.length - 1];
    while (idx !== -1) {
        lis.unshift(arr[idx]);
        idx = prev[idx];
    }
    
    return { length: tails.length, sequence: lis };
}

// Test
const arr = [10, 9, 2, 5, 3, 7, 101, 18];
console.log('Array:', arr);

const resultDP = lisDP(arr);
console.log(`LIS (DP O(n²)): length=${resultDP.length}, sequence=${resultDP.sequence}`);

const resultBS = lisBinarySearch(arr);
console.log(`LIS (BSearch O(n log n)): length=${resultBS.length}, sequence=${resultBS.sequence}`);

// Aplikasi: Stock Price Analysis — LIS = urutan hari terpanjang dengan harga naik
const stockPrices = [45, 48, 42, 55, 67, 60, 75, 80, 70, 90];
const stockDays = ['Sen', 'Sel', 'Rab', 'Kam', 'Jum', 'Sab', 'Min', 'Sen2', 'Sel2', 'Rab2'];

const stockLIS = lisDP(stockPrices);
console.log('\nStock Price LIS Analysis:');
console.log('Prices:', stockPrices);
console.log('Longest consecutive rising days:', stockLIS.length);
console.log('Prices sequence:', stockLIS.sequence);
```

### 5.3 Edit Distance dengan Space Optimization

```javascript
/**
 * Edit Distance (Levenshtein)
 * 
 * Full table: O(m×n) ruang
 * Optimized: O(min(m,n)) ruang dengan rolling array
 */

// Versi O(m×n) dengan backtracking
function editDistanceFull(A, B) {
    const m = A.length, n = B.length;
    const dp = Array.from({length: m + 1}, () => Array(n + 1).fill(0));
    
    // Base cases
    for (let i = 0; i <= m; i++) dp[i][0] = i;
    for (let j = 0; j <= n; j++) dp[0][j] = j;
    
    // Fill table
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (A[i-1] === B[j-1]) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i-1][j],   // delete A[i-1]
                    dp[i][j-1],   // insert B[j-1]
                    dp[i-1][j-1]  // replace
                );
            }
        }
    }
    
    // Backtrack untuk mendapatkan operasi
    const operations = [];
    let i = m, j = n;
    while (i > 0 || j > 0) {
        if (i > 0 && j > 0 && A[i-1] === B[j-1]) {
            operations.unshift({ op: 'match', char: A[i-1] });
            i--; j--;
        } else if (i > 0 && j > 0 && dp[i][j] === dp[i-1][j-1] + 1) {
            operations.unshift({ op: 'replace', from: A[i-1], to: B[j-1], pos: i-1 });
            i--; j--;
        } else if (i > 0 && dp[i][j] === dp[i-1][j] + 1) {
            operations.unshift({ op: 'delete', char: A[i-1], pos: i-1 });
            i--;
        } else {
            operations.unshift({ op: 'insert', char: B[j-1], pos: j-1 });
            j--;
        }
    }
    
    return { distance: dp[m][n], operations };
}

// Versi O(n) ruang — tanpa backtracking
function editDistanceOptimized(A, B) {
    // Pastikan B adalah string yang lebih pendek (optimasi)
    if (A.length < B.length) [A, B] = [B, A];
    
    const m = A.length, n = B.length;
    let prev = Array.from({length: n + 1}, (_, j) => j);
    let curr = Array(n + 1);
    
    for (let i = 1; i <= m; i++) {
        curr[0] = i;
        for (let j = 1; j <= n; j++) {
            if (A[i-1] === B[j-1]) {
                curr[j] = prev[j-1];
            } else {
                curr[j] = 1 + Math.min(
                    prev[j],   // delete
                    curr[j-1], // insert
                    prev[j-1]  // replace
                );
            }
        }
        [prev, curr] = [curr, prev]; // swap rows
    }
    
    return prev[n];
}

// Test edit distance
const pairs = [
    ['kitten', 'sitting'],
    ['Alfmart', 'Alfamart'],
    ['McDonals', "McDonald's"],
    ['GoPay', 'GoPay'],
    ['', 'hello']
];

pairs.forEach(([a, b]) => {
    const { distance, operations } = editDistanceFull(a, b);
    console.log(`\n"${a}" → "${b}": distance=${distance}`);
    const nonMatch = operations.filter(op => op.op !== 'match');
    nonMatch.forEach(op => {
        if (op.op === 'replace') console.log(`  Replace '${op.from}' with '${op.to}' at pos ${op.pos}`);
        else if (op.op === 'delete') console.log(`  Delete '${op.char}' at pos ${op.pos}`);
        else console.log(`  Insert '${op.char}'`);
    });
});
```

### 5.4 Aplikasi: Fuzzy Search Engine

```javascript
/**
 * Fuzzy Search menggunakan Edit Distance
 * Digunakan di search bar Tokopedia/Shopee untuk koreksi typo
 */
class FuzzySearchEngine {
    constructor(dictionary) {
        this.dictionary = dictionary;
    }
    
    /**
     * Cari kata paling mirip dengan query
     * @param {string} query - kata yang dicari (mungkin ada typo)
     * @param {number} maxDistance - toleransi maksimum
     * @param {number} topK - jumlah hasil
     */
    search(query, maxDistance = 2, topK = 5) {
        const results = [];
        
        for (const word of this.dictionary) {
            const dist = editDistanceOptimized(query.toLowerCase(), word.toLowerCase());
            if (dist <= maxDistance) {
                results.push({ word, distance: dist });
            }
        }
        
        // Sort by distance, then alphabetically
        results.sort((a, b) => {
            if (a.distance !== b.distance) return a.distance - b.distance;
            return a.word.localeCompare(b.word);
        });
        
        return results.slice(0, topK);
    }
    
    /**
     * Autocomplete dengan prefix matching + fuzzy fallback
     */
    autocomplete(prefix, query) {
        // Exact prefix matches first
        const exactPrefix = this.dictionary.filter(
            w => w.toLowerCase().startsWith(prefix.toLowerCase())
        );
        
        if (exactPrefix.length >= 3) return exactPrefix.slice(0, 5);
        
        // Fallback: fuzzy search
        return this.search(query, 3, 5).map(r => r.word);
    }
}

// Simulasi database merchant Tokopedia
const merchantNames = [
    'Alfamart', 'Indomaret', 'Minimart', 'Superindo', 'Giant',
    'McDonald\'s', 'KFC Indonesia', 'Burger King', 'Pizza Hut', 'Domino\'s',
    'Tokopedia', 'Shopee', 'Lazada', 'Blibli', 'Bukalapak',
    'GoPay', 'OVO', 'Dana', 'LinkAja', 'ShopeePay'
];

const engine = new FuzzySearchEngine(merchantNames);

// Test typo corrections
const queries = ['Alfmart', 'McDonalds', 'Tokopedya', 'Bukalap', 'GoPau'];

queries.forEach(q => {
    const results = engine.search(q, 3, 3);
    console.log(`\nQuery: "${q}"`);
    console.log('Suggestions:', results.map(r => `${r.word}(${r.distance})`).join(', '));
});
```

## 6. Studi Kasus Nyata: Gojek Search & Reconciliation

### Case 1: Driver-Order Matching dengan LCS

**Konteks:** Gojek perlu merekonsiliasi dua log — log dari driver app dan log dari server — untuk menemukan trip yang tercatat di kedua sistem (untuk billing) vs yang hanya di satu sistem (kemungkinan bug/fraud).

```javascript
// Driver log (offline-first, mungkin ada yang tidak tersync)
const driverLog = ['TRIP001', 'TRIP002', 'TRIP004', 'TRIP005', 'TRIP007'];
// Server log (mungkin ada yang gagal dicatat dari driver)  
const serverLog = ['TRIP001', 'TRIP003', 'TRIP004', 'TRIP005', 'TRIP006', 'TRIP007'];

function reconcileWithLCS(log1, log2) {
    const m = log1.length, n = log2.length;
    const dp = Array.from({length: m + 1}, () => Array(n + 1).fill(0));
    
    for (let i = 1; i <= m; i++)
        for (let j = 1; j <= n; j++)
            if (log1[i-1] === log2[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
            else dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
    
    // Backtrack untuk common
    const common = [];
    let i = m, j = n;
    while (i > 0 && j > 0) {
        if (log1[i-1] === log2[j-1]) { common.unshift(log1[i-1]); i--; j--; }
        else if (dp[i-1][j] > dp[i][j-1]) i--;
        else j--;
    }
    
    const onlyInDriver = log1.filter(t => !common.includes(t));
    const onlyInServer = log2.filter(t => !common.includes(t));
    
    return { 
        synced: common,
        driverOnly: onlyInDriver,   // perlu sync ke server
        serverOnly: onlyInServer    // investigasi — server ada, driver tidak?
    };
}

const report = reconcileWithLCS(driverLog, serverLog);
console.log('Trip tersinkronisasi:', report.synced);
console.log('Hanya di driver (perlu upload):', report.driverOnly);  // TRIP002
console.log('Hanya di server (investigasi):', report.serverOnly);   // TRIP003, TRIP006
```

### Case 2: Autocorrect Nama Jalan Gojek

**Konteks:** Driver mengetik nama jalan tujuan yang mungkin typo. Gojek perlu menyarankan koreksi menggunakan Edit Distance.

```javascript
// Hasilnya menunjukkan:
// "Jl. Sudirma" → "Jl. Sudirman" (edit distance 1, insert 'n')
// "Jl Kuning Baru" → "Jl. Kuningan Baru" (edit distance 2)
// Latency: <2ms untuk database 10.000 nama jalan (dengan pruning)
```

**Optimasi di produksi:**
1. **BK-Tree (Burkhard-Keller Tree)** — struktur data metric tree untuk edit distance queries
2. **Threshold pruning** — stop BFS jika current distance sudah > max threshold
3. **Character frequency filter** — skip kandidat dengan distribusi karakter sangat berbeda sebelum hitung edit distance penuh

## 7. Visualisasi

### Tabel DP untuk LCS("ABCB", "BCB")

```
      ""  B   C   B
  ""  [0  0   0   0]
  A   [0  0   0   0]
  B   [0  1   1   1]
  C   [0  1   2   2]
  B   [0  1   2   3]  ← LCS panjang 3 = "BCB"

Arah backtrack:
  dp[4][3]=3: B=B → ambil 'B', pindah ke dp[3][2]
  dp[3][2]=2: C=C → ambil 'C', pindah ke dp[2][1]
  dp[2][1]=1: B=B → ambil 'B', pindah ke dp[1][0]=0 → selesai
  LCS = "BCB" (dibaca terbalik dari backtrack)
```

### Tabel Edit Distance("SUNDAY", "SATURDAY")

```
        ""  S  A  T  U  R  D  A  Y
     ""  0  1  2  3  4  5  6  7  8
      S  1  0  1  2  3  4  5  6  7
      U  2  1  1  2  2  3  4  5  6
      N  3  2  2  2  3  3  4  5  6
      D  4  3  3  3  3  4  3  4  5
      A  5  4  3  4  4  4  4  3  4
      Y  6  5  4  4  5  5  5  4  3 ← Edit distance = 3

Operasi: Insert 'A', Insert 'T', Replace 'N'→'R'
```

### Rolling Array Optimization

```
Full table (LCS m×n):
Row 0: [0, 0, 0, ..., 0]  ← tidak dibutuhkan setelah row 2 diisi
Row 1: [...]
Row 2: [...]  ← hanya butuh row sebelumnya!
...

Optimized (dua array):
prev = [0, 1, 1, 2, 2, ...]  ← row i-1
curr = [0, 0, 1, 1, 2, ...]  ← row i (sedang diisi)
→ swap setelah setiap row: O(n) ruang
```

## 8. Kesalahan Umum

### ❌ Kesalahan 1: Off-by-One dalam Indeks

```javascript
// SALAH — karakter s1[i] vs s1[i-1]
for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
        if (s1[i] === s2[j]) {  // SALAH! harus s1[i-1] dan s2[j-1]
            dp[i][j] = dp[i-1][j-1] + 1;
        }
    }
}

// BENAR — dp[i][j] merepresentasikan s1[0..i-1] dan s2[0..j-1]
for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
        if (s1[i-1] === s2[j-1]) {  // karakter ke-i adalah s1[i-1]
            dp[i][j] = dp[i-1][j-1] + 1;
        }
    }
}
```

### ❌ Kesalahan 2: Arah Backtracking LCS yang Salah

```javascript
// SALAH — backtrack harus dimulai dari dp[m][n], bukan dp[0][0]!
function lcsWrong(s1, s2, dp) {
    let i = 0, j = 0; // SALAH — mulai dari pojok kiri atas
    const lcs = [];
    while (i < dp.length && j < dp[0].length) {
        // logika ini tidak benar...
    }
}

// BENAR — mulai dari pojok kanan bawah, jalan ke kiri atas
function lcsCorrect(s1, s2, dp) {
    let i = s1.length, j = s2.length; // pojok kanan bawah
    const lcs = [];
    while (i > 0 && j > 0) {
        if (s1[i-1] === s2[j-1]) {
            lcs.unshift(s1[i-1]);
            i--; j--;
        } else if (dp[i-1][j] > dp[i][j-1]) {
            i--;
        } else {
            j--;
        }
    }
    return lcs.join('');
}
```

### ❌ Kesalahan 3: Menggunakan LCS untuk Edit Distance (dua hal berbeda!)

```javascript
// LCS: panjang subsequence BERSAMA (order dipertahankan, tidak perlu kontigu)
// Edit Distance: minimum operasi insert/delete/replace

// "ABCDE" dan "AECDB":
// LCS = "ACD" (panjang 3) — huruf yang sama dalam urutan yang sama
// Edit Distance = 4 — berbeda dari 5 - LCS!

// Hubungan LCS dan Edit Distance (tanpa replace):
// editDistance = m + n - 2*lcs (hanya insert dan delete, no replace)
// Tetapi Levenshtein TERMASUK replace → berbeda!
```

### ❌ Kesalahan 4: Tidak Handle String Kosong

```javascript
// CRASH — tidak handle empty string
function editDistanceBroken(A, B) {
    const m = A.length, n = B.length;
    const dp = Array.from({length: m}, () => Array(n).fill(0)); // SALAH! harus m+1, n+1
    // dp[0][0] harusnya = 0, tapi base case dp[i][0] = i tidak diset!
}

// BENAR — inisialisasi dengan base case yang benar
function editDistanceFixed(A, B) {
    const m = A.length, n = B.length;
    const dp = Array.from({length: m + 1}, () => Array(n + 1).fill(0));
    
    for (let i = 0; i <= m; i++) dp[i][0] = i; // delete semua dari A
    for (let j = 0; j <= n; j++) dp[0][j] = j; // insert semua ke B
    // ... lanjut fill
}
```

## 9. Latihan dan Studi Kasus

### Latihan 1 — Shortest Common Supersequence

**Problem:** Panjang string terpendek yang mengandung s1 dan s2 sebagai subsequence.

```
s1 = "AGGTAB", s2 = "GXTXAYB"
SCS = "AGXGTXAYB" (panjang 9)
```

**Petunjuk:** SCS(m,n) = m + n - LCS(m,n)

```javascript
function shortestCommonSupersequence(s1, s2) {
    const { length: lcsLen } = longestCommonSubsequence(s1, s2);
    return s1.length + s2.length - lcsLen;
}

console.log(shortestCommonSupersequence("AGGTAB", "GXTXAYB")); // 9
```

### Latihan 2 — Palindrome Partitioning (Min Cuts)

```javascript
/**
 * Minimum cuts untuk mempartisi string menjadi palindrom-palindrom
 * 
 * Contoh: "aab" → ["aa", "b"] → 1 cut (optimal)
 * 
 * Kompleksitas: O(n²) dengan DP
 */
function minPalindromeCuts(s) {
    const n = s.length;
    
    // Precompute: apakah s[i..j] palindrom?
    const isPalin = Array.from({length: n}, () => Array(n).fill(false));
    for (let i = 0; i < n; i++) isPalin[i][i] = true;
    
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            if (len === 2) {
                isPalin[i][j] = (s[i] === s[j]);
            } else {
                isPalin[i][j] = (s[i] === s[j]) && isPalin[i+1][j-1];
            }
        }
    }
    
    // dp[i] = min cuts untuk s[0..i]
    const dp = Array(n).fill(Infinity);
    for (let i = 0; i < n; i++) {
        if (isPalin[0][i]) {
            dp[i] = 0; // s[0..i] sudah palindrom, tidak perlu cut
        } else {
            for (let j = 1; j <= i; j++) {
                if (isPalin[j][i]) {
                    dp[i] = Math.min(dp[i], dp[j-1] + 1);
                }
            }
        }
    }
    
    return dp[n-1];
}

console.log(minPalindromeCuts("aab"));     // 1: ["aa","b"]
console.log(minPalindromeCuts("abcba"));   // 0: sudah palindrom
console.log(minPalindromeCuts("abacaba")); // 0: sudah palindrom
console.log(minPalindromeCuts("abcdef"));  // 5: semua char terpisah
```

### Latihan 3 — Challenge: Interleaving Strings

Diberikan s1, s2, s3 — apakah s3 adalah interleaving dari s1 dan s2?

```javascript
// s1 = "aab", s2 = "axy", s3 = "aaxaby" → true
// ("a" dari s1) + ("a" dari s2) + ("x" dari s2) + ("a" dari s1) + ("b" dari s1) + ("y" dari s2)

function isInterleaving(s1, s2, s3) {
    const m = s1.length, n = s2.length;
    if (m + n !== s3.length) return false;
    
    // dp[i][j] = bisakah s3[0..i+j-1] dibentuk dari s1[0..i-1] dan s2[0..j-1]
    const dp = Array.from({length: m + 1}, () => Array(n + 1).fill(false));
    dp[0][0] = true;
    
    for (let i = 1; i <= m; i++) dp[i][0] = dp[i-1][0] && s1[i-1] === s3[i-1];
    for (let j = 1; j <= n; j++) dp[0][j] = dp[0][j-1] && s2[j-1] === s3[j-1];
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            dp[i][j] = (dp[i-1][j] && s1[i-1] === s3[i+j-1]) ||
                       (dp[i][j-1] && s2[j-1] === s3[i+j-1]);
        }
    }
    
    return dp[m][n];
}

console.log(isInterleaving("aab", "axy", "aaxaby")); // true
console.log(isInterleaving("aab", "axy", "abaaxy")); // false
```

## 10. Ringkasan

| Algoritma | Recurrence | Waktu | Ruang | Space-Opt? |
|-----------|-----------|-------|-------|-----------|
| LCS | dp[i][j] = dp[i-1][j-1]+1 atau max(dp[i-1][j], dp[i][j-1]) | O(mn) | O(mn) | O(n) |
| LIS O(n²) | dp[i] = 1 + max(dp[j] \| j<i, arr[j]<arr[i]) | O(n²) | O(n) | Tidak |
| LIS O(n log n) | Binary search di tails array | O(n log n) | O(n) | Tidak |
| Edit Distance | dp[i][j] = dp[i-1][j-1] atau 1+min(3 options) | O(mn) | O(mn) | O(n) |

**Pola umum tabulation:**
1. Definisikan arti `dp[i][j]` secara jelas
2. Tentukan base case (row 0 dan column 0)
3. Tulis recurrence
4. Isi tabel dari atas-kiri ke kanan-bawah
5. (Opsional) Backtrack dari dp[m][n] untuk merekonstruksi solusi
6. (Opsional) Optimasi ruang dengan rolling array

## 11. Referensi

- Cormen, T.H. et al. — *Introduction to Algorithms (CLRS)*, 4th Ed., Bab 14.4 (LCS)
- Levenshtein, V.I. — "Binary codes capable of correcting deletions, insertions, and reversals", 1966 (makalah asli Edit Distance)
- Bhargava, A. — *Grokking Algorithms*, Bab 9 (DP — LCS explanation sangat visual)
- Skiena, S. — *The Algorithm Design Manual*, Bab 10.2-10.3 (String DP)
- LeetCode — #1143 (LCS), #300 (LIS), #72 (Edit Distance), #1035 (Uncrossed Lines)
- Visualgo.net — Edit Distance visualization
- Refactoring.guru — DP patterns
- Stanford CS161 — Lecture slides on String DP (tersedia online)
