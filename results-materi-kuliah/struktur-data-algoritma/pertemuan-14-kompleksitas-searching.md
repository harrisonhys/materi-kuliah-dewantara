# Pertemuan 14: Kompleksitas Algoritma (Big-O, Ω, Θ) & Searching

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu diharapkan mampu:

- Memahami dan membedakan notasi Big-O, Big-Ω, dan Big-Θ secara konseptual
- Menghitung kompleksitas waktu dan ruang dari sebuah blok kode
- Menerapkan aturan drop constants dan drop non-dominant terms
- Mengimplementasikan Sequential Search, Binary Search (iteratif & rekursif), dan Hash Table
- Memilih algoritma pencarian yang tepat berdasarkan konteks dan skala data
- Menganalisis dampak pilihan algoritma di sistem nyata (skala jutaan pengguna)

---

## 📖 Pengantar (Hook)

Bayangkan kamu bekerja sebagai backend engineer di **BCA Digital**. Suatu hari, tim product bilang: *"Fitur transfer kita lambat banget, user nunggu 3 detik buat validasi nomor rekening!"*

Kamu buka kodenya. Ternyata untuk memvalidasi satu nomor rekening, sistem melakukan **loop satu per satu** melewati seluruh database nasabah — 50 juta record. Setiap kali ada user transfer, server harus memeriksa hingga 50 juta data. Tidak heran lambat.

Solusinya bukan beli server yang lebih mahal. Solusinya adalah **memilih algoritma yang tepat**.

Inilah inti dari pertemuan ini: **kompleksitas algoritma** — ilmu untuk mengukur seberapa cepat (atau lambat) sebuah kode berjalan ketika data tumbuh, dan **searching** — teknik-teknik untuk menemukan data secara efisien.

---

## 🧩 Konsep Utama

### 1. Notasi Asimptotik — Tiga Perspektif Kompleksitas

Notasi asimptotik adalah cara matematis untuk mendeskripsikan perilaku sebuah fungsi ketika inputnya mendekati tak hingga (n → ∞). Ada tiga notasi utama:

#### Big-O (O) — Upper Bound / Worst Case
> "Dalam kondisi terburuk, algoritma ini tidak akan lebih lambat dari..."

Big-O adalah batas atas. Kita tanya: **paling buruk, seberapa lama?**

```
f(n) = O(g(n))  →  ada konstanta c dan n₀ sehingga f(n) ≤ c·g(n) untuk semua n ≥ n₀
```

**Contoh:** Mencari nama "Zulaikha" di buku telepon yang disusun acak → harus baca semua halaman → O(n)

#### Big-Ω (Omega) — Lower Bound / Best Case
> "Dalam kondisi terbaik, algoritma ini paling cepat..."

Big-Ω adalah batas bawah. Kita tanya: **paling baik, seberapa cepat?**

```
f(n) = Ω(g(n))  →  ada konstanta c dan n₀ sehingga f(n) ≥ c·g(n) untuk semua n ≥ n₀
```

**Contoh:** Mencari nama "Ahmad" di buku telepon acak → beruntung ada di halaman pertama → Ω(1)

#### Big-Θ (Theta) — Tight Bound / Average Case
> "Rata-rata, algoritma ini berjalan sekitar..."

Big-Θ adalah batas ketat — algoritma secara konsisten berjalan pada kompleksitas ini.

```
f(n) = Θ(g(n))  →  f(n) = O(g(n)) DAN f(n) = Ω(g(n))
```

**Contoh:** Binary search selalu membelah data → Θ(log n)

#### Ringkasan Perbedaan

| Notasi | Perspektif | Pertanyaan | Makna |
|--------|-----------|-----------|-------|
| **O** (Big-O) | Worst case | "Paling buruk seberapa lambat?" | Batas atas |
| **Ω** (Omega) | Best case | "Paling baik seberapa cepat?" | Batas bawah |
| **Θ** (Theta) | Average/Tight | "Rata-rata berapa?" | Batas ketat |

> **Catatan praktis:** Di industri, ketika orang bilang "kompleksitas algoritma ini O(n)", biasanya yang dimaksud adalah worst case behavior. Big-O paling sering dipakai karena kita lebih peduli skenario terburuk.

---

### 2. Cara Menghitung Big-O dari Kode

#### Aturan 1: Drop Constants

Konstanta tidak mempengaruhi pertumbuhan asimptotik.

```
O(2n)    → O(n)
O(500)   → O(1)
O(3n²)   → O(n²)
```

#### Aturan 2: Drop Non-Dominant Terms

Saat n sangat besar, term yang tumbuh lebih cepat mendominasi.

```
O(n² + n)      → O(n²)
O(n + log n)   → O(n)
O(2^n + n³)    → O(2^n)
```

#### Aturan 3: Loop Sederhana → O(n)

```javascript
for (let i = 0; i < n; i++) {
  // operasi O(1)
}
// Total: O(n)
```

#### Aturan 4: Loop Bersarang → Kalikan

```javascript
for (let i = 0; i < n; i++) {       // O(n)
  for (let j = 0; j < n; j++) {     // O(n)
    // operasi O(1)
  }
}
// Total: O(n × n) = O(n²)
```

#### Aturan 5: Operasi Berurutan → Tambahkan, lalu ambil dominan

```javascript
for (let i = 0; i < n; i++) { }     // O(n)
for (let j = 0; j < n; j++) {       // O(n)
  for (let k = 0; k < n; k++) { }   // O(n)
}
// Total: O(n) + O(n²) → O(n²)
```

#### Aturan 6: Divide & Conquer → O(log n)

Jika setiap iterasi membagi masalah menjadi setengahnya:

```javascript
let low = 0, high = n;
while (low <= high) {
  let mid = Math.floor((low + high) / 2);
  // ...
  low = mid + 1;  // atau high = mid - 1
}
// Jumlah iterasi: log₂(n) → O(log n)
```

---

### 3. Hierarki Kompleksitas (Dari Terbaik ke Terburuk)

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2^n) < O(n!)
```

| Kompleksitas | Nama | Contoh | n=10 | n=100 | n=1.000 |
|---|---|---|---|---|---|
| O(1) | Konstan | Akses array by index | 1 | 1 | 1 |
| O(log n) | Logaritmik | Binary search | ~3 | ~7 | ~10 |
| O(n) | Linear | Sequential search | 10 | 100 | 1.000 |
| O(n log n) | Linearitmik | Merge sort | ~33 | ~664 | ~9.965 |
| O(n²) | Kuadratik | Bubble sort | 100 | 10.000 | 1.000.000 |
| O(2^n) | Eksponensial | Fibonacci naif | 1.024 | 10³⁰ | ☠️ |
| O(n!) | Faktorial | Permutasi semua rute | 3.628.800 | ☠️ | ☠️ |

---

### 4. Time Complexity vs Space Complexity

- **Time Complexity:** Berapa banyak *operasi* yang dilakukan seiring bertambahnya n
- **Space Complexity:** Berapa banyak *memori tambahan* yang dibutuhkan seiring bertambahnya n

Keduanya sering kali ada trade-off:

| Teknik | Time Complexity | Space Complexity |
|---|---|---|
| Caching / Memoization | Lebih cepat (O(1) lookup) | Lebih boros (O(n) extra space) |
| In-place sorting | Tidak perlu extra array | Hemat memori O(1) |
| Hash Map | O(1) average | O(n) untuk menyimpan data |

---

### 5. Amortized Analysis

Amortized analysis mengukur kompleksitas **rata-rata per operasi** dari serangkaian operasi, bukan worst case satu operasi.

**Contoh klasik: Dynamic Array (seperti JavaScript Array)**

Ketika array penuh dan kamu menambah elemen baru, JavaScript mengalokasikan array baru 2× ukurannya dan meng-copy semua elemen → satu operasi `push` ini bisa O(n).

Tapi ini jarang terjadi. Jika dihitung rata-rata, setiap `push` tetap **O(1) amortized** karena operasi mahal hanya terjadi setiap 2^k push.

```
Operasi: push, push, push, ..., push (resize!), push, push, ...
Cost:      1,    1,    1,  ...,    n,            1,    1,  ...
Amortized per operasi: O(1)
```

---

## 🧠 Ilustrasi / Analogi

### Analogi Mencari Buku di Perpustakaan

Bayangkan kamu mencari buku dengan nomor ISBN tertentu di perpustakaan:

**Sequential Search (O(n)) — Cara Kasir Baru**
```
📚📚📚📚📚📚📚📚📚📚  ← 10 rak buku
↑ mulai dari sini, cek satu per satu...
```
Kamu mulai dari rak pertama, cek setiap buku satu per satu. Kalau bukunya ada di rak terakhir — kamu harus melewati semua rak. Untuk 1 juta buku → paling buruk 1 juta langkah.

**Binary Search (O(log n)) — Cara Cerdas (buku terurut)**
```
[1..500.000] → cek tengah → [250.001..500.000] → cek tengah → ...
```
Kalau buku disusun berurutan, kamu bisa ke rak tengah, lihat nomornya lebih kecil atau besar, lalu eliminasi setengah rak. Untuk 1 juta buku → maksimal **20 langkah** saja!

**Hash Map (O(1)) — Cara Sistem Komputer**
```
ISBN: 978-3-16-148410-0
       ↓ hash function
   indeks: 42  → langsung ke lokasi fisik!
```
Ada katalog komputer: masukkan ISBN → langsung tahu di rak mana, nomor berapa. Tidak peduli 1 juta atau 50 juta buku — **selalu 1 langkah** (rata-rata).

---

## 💻 Contoh Teknis

### 1. Menganalisis Kompleksitas Kode

```javascript
// ============================================================
// ANALISIS KOMPLEKSITAS ALGORITMA
// ============================================================

// --- O(1) — Konstan ---
function getFirst(arr) {
  return arr[0]; // selalu 1 operasi, tidak peduli panjang arr
}
// Time: O(1) | Space: O(1)

// --- O(n) — Linear ---
function findMax(arr) {
  let max = arr[0];
  for (let i = 1; i < arr.length; i++) { // loop n kali
    if (arr[i] > max) max = arr[i];
  }
  return max;
}
// Time: O(n) | Space: O(1)

// --- O(n²) — Kuadratik ---
function bubbleSort(arr) {
  const a = [...arr];
  for (let i = 0; i < a.length; i++) {        // n kali
    for (let j = 0; j < a.length - i - 1; j++) { // n kali
      if (a[j] > a[j + 1]) {
        [a[j], a[j + 1]] = [a[j + 1], a[j]];
      }
    }
  }
  return a;
}
// Time: O(n²) | Space: O(n) untuk salinan array

// --- O(log n) — Logaritmik ---
function binarySearch(arr, target) {
  let low = 0, high = arr.length - 1;
  while (low <= high) {           // maksimal log₂(n) iterasi
    const mid = Math.floor((low + high) / 2);
    if (arr[mid] === target) return mid;
    else if (arr[mid] < target) low = mid + 1;
    else high = mid - 1;
  }
  return -1;
}
// Time: O(log n) | Space: O(1)

// --- Contoh dengan multiple loops (O(n²) bukan O(n³)) ---
function example(n) {
  // Loop 1: O(n)
  for (let i = 0; i < n; i++) {
    console.log(i);
  }

  // Loop 2 bersarang: O(n²)
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      console.log(i, j);
    }
  }

  // Total: O(n) + O(n²) = O(n²)  ← ambil yang dominan
}
```

---

### 2. Sequential Search (Linear Search)

```javascript
// ============================================================
// SEQUENTIAL SEARCH — O(n) time, O(1) space
// ============================================================

/**
 * Mencari elemen dalam array secara berurutan
 * @param {Array} arr - Array yang akan dicari (tidak harus terurut)
 * @param {*} target  - Nilai yang dicari
 * @returns {number}  - Index pertama yang ditemukan, -1 jika tidak ada
 */
function sequentialSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) {
      return i; // best case: O(1) jika target di index 0
    }
  }
  return -1; // worst case: harus cek semua → O(n)
}

// Versi dengan statistik pencarian
function sequentialSearchVerbose(arr, target) {
  let comparisons = 0;

  for (let i = 0; i < arr.length; i++) {
    comparisons++;
    if (arr[i] === target) {
      return {
        found: true,
        index: i,
        comparisons,
        message: `Ditemukan di index ${i} setelah ${comparisons} perbandingan`
      };
    }
  }

  return {
    found: false,
    index: -1,
    comparisons,
    message: `Tidak ditemukan setelah ${comparisons} perbandingan`
  };
}

// Mencari semua kemunculan (bukan hanya pertama)
function sequentialSearchAll(arr, target) {
  const results = [];
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) results.push(i);
  }
  return results; // Time: O(n) — selalu scan semua
}

// --- Test ---
const data = [64, 25, 12, 22, 11, 90, 22, 45];

console.log('=== Sequential Search ===');
console.log(sequentialSearch(data, 22));       // 3
console.log(sequentialSearch(data, 99));       // -1
console.log(sequentialSearchVerbose(data, 90));
// { found: true, index: 5, comparisons: 6, message: '...' }
console.log(sequentialSearchAll(data, 22));   // [3, 6]
```

---

### 3. Binary Search — Iteratif & Rekursif

```javascript
// ============================================================
// BINARY SEARCH — O(log n) time, O(1) / O(log n) space
// SYARAT: Array HARUS terurut ascending
// ============================================================

/**
 * Binary Search — Iteratif
 * Space: O(1) — lebih efisien memori
 */
function binarySearchIterative(arr, target) {
  let low = 0;
  let high = arr.length - 1;
  let iterations = 0;

  while (low <= high) {
    iterations++;
    const mid = Math.floor((low + high) / 2);

    if (arr[mid] === target) {
      console.log(`  Ditemukan setelah ${iterations} iterasi (dari ${arr.length} elemen)`);
      return mid;
    } else if (arr[mid] < target) {
      low = mid + 1;  // target ada di kanan
    } else {
      high = mid - 1; // target ada di kiri
    }
  }

  console.log(`  Tidak ditemukan setelah ${iterations} iterasi`);
  return -1;
}

/**
 * Binary Search — Rekursif
 * Space: O(log n) — karena call stack sedalam log n
 */
function binarySearchRecursive(arr, target, low = 0, high = arr.length - 1) {
  // Base case: range tidak valid
  if (low > high) return -1;

  const mid = Math.floor((low + high) / 2);

  if (arr[mid] === target) return mid;
  if (arr[mid] < target) {
    return binarySearchRecursive(arr, target, mid + 1, high); // cari di kanan
  } else {
    return binarySearchRecursive(arr, target, low, mid - 1);  // cari di kiri
  }
}

/**
 * Binary Search untuk mencari batas bawah (lower bound)
 * Berguna ketika ada duplikat — cari index pertama dari target
 */
function binarySearchLowerBound(arr, target) {
  let low = 0, high = arr.length;
  while (low < high) {
    const mid = Math.floor((low + high) / 2);
    if (arr[mid] < target) low = mid + 1;
    else high = mid;
  }
  return low < arr.length && arr[low] === target ? low : -1;
}

// --- Test ---
const sortedArr = [2, 5, 8, 12, 16, 23, 38, 56, 72, 91];

console.log('=== Binary Search ===');
console.log('Array:', sortedArr);
console.log('\nIteratif:');
console.log(binarySearchIterative(sortedArr, 23));  // index 5
console.log(binarySearchIterative(sortedArr, 100)); // -1

console.log('\nRekursif:');
console.log(binarySearchRecursive(sortedArr, 56));  // index 7
console.log(binarySearchRecursive(sortedArr, 1));   // -1

// Perbandingan skala
console.log('\n=== Perbandingan Iterasi: Sequential vs Binary ===');
function compareSearches(n, target) {
  const arr = Array.from({ length: n }, (_, i) => i * 2); // [0, 2, 4, ..., 2n-2]
  
  // Sequential
  let seqCount = 0;
  for (let i = 0; i < arr.length; i++) {
    seqCount++;
    if (arr[i] === target) break;
  }

  // Binary
  let binCount = 0;
  let low = 0, high = arr.length - 1;
  while (low <= high) {
    binCount++;
    const mid = Math.floor((low + high) / 2);
    if (arr[mid] === target) break;
    else if (arr[mid] < target) low = mid + 1;
    else high = mid - 1;
  }

  console.log(`n=${n.toLocaleString()}: Sequential=${seqCount} iterasi, Binary=${binCount} iterasi`);
}

compareSearches(1_000, 998);
compareSearches(100_000, 99_998);
compareSearches(50_000_000, 49_999_998);
```

---

### 4. Hash Table dengan Collision Handling

```javascript
// ============================================================
// HASH TABLE — O(1) average, O(n) worst case
// ============================================================

/**
 * Hash Table sederhana dengan Separate Chaining (collision handling)
 * Chaining: setiap bucket menyimpan linked list / array jika ada collision
 */
class HashTable {
  constructor(size = 53) {
    this.table = new Array(size);
    this.size = size;
    this.count = 0;
  }

  // Hash function: polynomial rolling hash
  _hash(key) {
    let hash = 0;
    const PRIME = 31;
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      hash = (hash * PRIME + key.charCodeAt(i)) % this.size;
    }
    return hash;
  }

  // SET — O(1) average
  set(key, value) {
    const index = this._hash(key);
    if (!this.table[index]) {
      this.table[index] = []; // buat bucket baru
    }
    // Cek apakah key sudah ada (update)
    const bucket = this.table[index];
    for (let pair of bucket) {
      if (pair[0] === key) {
        pair[1] = value; // update
        return;
      }
    }
    bucket.push([key, value]); // insert baru
    this.count++;
  }

  // GET — O(1) average
  get(key) {
    const index = this._hash(key);
    const bucket = this.table[index];
    if (!bucket) return undefined;

    for (let [k, v] of bucket) {
      if (k === key) return v;
    }
    return undefined;
  }

  // DELETE — O(1) average
  delete(key) {
    const index = this._hash(key);
    const bucket = this.table[index];
    if (!bucket) return false;

    const idx = bucket.findIndex(([k]) => k === key);
    if (idx === -1) return false;

    bucket.splice(idx, 1);
    this.count--;
    return true;
  }

  // Semua keys
  keys() {
    const result = [];
    for (let bucket of this.table) {
      if (bucket) {
        for (let [key] of bucket) result.push(key);
      }
    }
    return result;
  }

  // Load factor — idealnya < 0.7
  get loadFactor() {
    return (this.count / this.size).toFixed(2);
  }

  // Statistik distribusi bucket
  stats() {
    let used = 0, maxChain = 0, collisions = 0;
    for (let bucket of this.table) {
      if (bucket && bucket.length > 0) {
        used++;
        if (bucket.length > 1) collisions += bucket.length - 1;
        maxChain = Math.max(maxChain, bucket.length);
      }
    }
    return { used, total: this.size, maxChain, collisions, loadFactor: this.loadFactor };
  }
}

// --- Test Hash Table ---
const ht = new HashTable(53);
ht.set('rekening-001', { nama: 'Budi Santoso', saldo: 5_000_000 });
ht.set('rekening-002', { nama: 'Siti Aminah', saldo: 12_500_000 });
ht.set('rekening-003', { nama: 'Ahmad Fauzi', saldo: 2_750_000 });

console.log('=== Hash Table ===');
console.log(ht.get('rekening-001')); // { nama: 'Budi Santoso', saldo: 5000000 }
console.log(ht.get('rekening-999')); // undefined
ht.set('rekening-001', { nama: 'Budi Santoso', saldo: 6_000_000 }); // update
console.log(ht.get('rekening-001').saldo); // 6000000
console.log('Stats:', ht.stats());

// --- Map bawaan JavaScript (lebih direkomendasikan di production) ---
console.log('\n=== JavaScript Built-in Map ===');
const accountMap = new Map();
accountMap.set('1234567890', { nama: 'Dewi Kurnia', saldo: 8_000_000 });
accountMap.set('0987654321', { nama: 'Rizky Pratama', saldo: 15_000_000 });

console.log(accountMap.get('1234567890')); // O(1) average
console.log(accountMap.has('0000000000')); // false — O(1)
console.log('Ukuran:', accountMap.size);    // 2

// --- Set untuk tracking unik ---
const processedTrx = new Set();
processedTrx.add('TRX-001');
processedTrx.add('TRX-002');
processedTrx.add('TRX-001'); // duplikat, diabaikan

console.log('\n=== JavaScript Built-in Set ===');
console.log(processedTrx.size);           // 2
console.log(processedTrx.has('TRX-001')); // true — O(1)
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Sistem Pencarian Rekening Bank — BCA Digital Scale

Bayangkan kamu membangun sistem validasi nomor rekening untuk BCA. Ketika user transfer, sistem harus memverifikasi apakah nomor rekening tujuan valid dalam hitungan milidetik.

**Skenario:** Database nasabah tumbuh dari 10.000 ke 50 juta pengguna.

```javascript
// ============================================================
// SIMULASI SISTEM PENCARIAN REKENING BANK
// Membandingkan Sequential O(n) vs Binary O(log n) vs Hash O(1)
// ============================================================

// Simulasi database nasabah
function generateAccounts(n) {
  const accounts = [];
  for (let i = 0; i < n; i++) {
    const accountNumber = String(1_000_000_000 + i).padStart(10, '0');
    accounts.push({
      accountNumber,
      name: `Nasabah-${i}`,
      balance: Math.floor(Math.random() * 100_000_000)
    });
  }
  return accounts;
}

// --- METODE 1: Sequential Search — O(n) ---
function findAccountSequential(accounts, targetNumber) {
  for (const account of accounts) {
    if (account.accountNumber === targetNumber) return account;
  }
  return null;
}

// --- METODE 2: Binary Search — O(log n) ---
// Syarat: accounts sudah diurutkan berdasarkan accountNumber
function findAccountBinary(sortedAccounts, targetNumber) {
  let low = 0, high = sortedAccounts.length - 1;
  while (low <= high) {
    const mid = Math.floor((low + high) / 2);
    const cmp = sortedAccounts[mid].accountNumber.localeCompare(targetNumber);
    if (cmp === 0) return sortedAccounts[mid];
    else if (cmp < 0) low = mid + 1;
    else high = mid - 1;
  }
  return null;
}

// --- METODE 3: Hash Map — O(1) average ---
function buildAccountHashMap(accounts) {
  const map = new Map();
  for (const account of accounts) {
    map.set(account.accountNumber, account);
  }
  return map;
}

function findAccountHashMap(map, targetNumber) {
  return map.get(targetNumber) ?? null;
}

// --- Benchmark ---
function benchmark(label, fn, iterations = 1000) {
  const start = performance.now();
  for (let i = 0; i < iterations; i++) fn();
  const end = performance.now();
  const avgMs = ((end - start) / iterations).toFixed(4);
  console.log(`  ${label}: rata-rata ${avgMs} ms per pencarian`);
  return parseFloat(avgMs);
}

// Test dengan berbagai ukuran database
const sizes = [10_000, 100_000, 1_000_000];

for (const n of sizes) {
  console.log(`\n📊 Database: ${n.toLocaleString()} nasabah`);
  console.log('  Mempersiapkan data...');

  const accounts = generateAccounts(n);
  const sortedAccounts = [...accounts].sort((a, b) =>
    a.accountNumber.localeCompare(b.accountNumber)
  );
  const hashMap = buildAccountHashMap(accounts);

  // Target: rekening yang ada di posisi akhir (worst case untuk sequential)
  const targetNumber = accounts[Math.floor(n * 0.95)].accountNumber;

  benchmark('Sequential O(n)', () => findAccountSequential(accounts, targetNumber), 100);
  benchmark('Binary    O(log n)', () => findAccountBinary(sortedAccounts, targetNumber), 1000);
  benchmark('Hash Map  O(1)', () => findAccountHashMap(hashMap, targetNumber), 10000);
}
```

**Estimasi Hasil (pada mesin modern):**

| Ukuran Database | Sequential O(n) | Binary O(log n) | Hash Map O(1) |
|---|---|---|---|
| 10.000 nasabah | ~0.05 ms | ~0.001 ms | ~0.0001 ms |
| 100.000 nasabah | ~0.5 ms | ~0.001 ms | ~0.0001 ms |
| 1.000.000 nasabah | ~5 ms | ~0.002 ms | ~0.0001 ms |
| 50.000.000 nasabah | ~250 ms ❌ | ~0.003 ms ✅ | ~0.0001 ms ✅ |

**Analisis Dampak Bisnis:**

```
Skenario: BCA dengan 50 juta nasabah, 100.000 transaksi/detik

Sequential Search:
  - 250 ms per lookup
  - Kapasitas max: 4 transaksi/detik 🔴
  - Butuh server ribuan untuk handle 100k TPS → tidak realistis

Binary Search:
  - 0.003 ms per lookup
  - Kapasitas max: ~333.000 transaksi/detik 🟡
  - Tapi perlu array terurut → overhead saat insert data baru (re-sort)

Hash Map (Redis / in-memory DB):
  - 0.0001 ms per lookup
  - Kapasitas max: jutaan transaksi/detik 🟢
  - Ini yang dipakai BCA, Mandiri, GoPay di production
  - Biasanya menggunakan Redis sebagai cache di depan database
```

```javascript
// Solusi Production: Hybrid Approach
// Redis (Hash Map) sebagai cache, Database (B-Tree Index) sebagai persistent storage

class BankingAccountService {
  constructor() {
    this.redisCache = new Map();     // simulasi Redis — O(1) lookup
    this.dbIndex = new Map();        // simulasi DB dengan index — O(log n)
    this.cacheHits = 0;
    this.cacheMisses = 0;
  }

  // Simulasi query ke DB (lebih lambat)
  async _queryDatabase(accountNumber) {
    // Di production: SELECT * FROM accounts WHERE account_number = ? (indexed)
    await new Promise(resolve => setTimeout(resolve, 1)); // simulasi network latency
    return this.dbIndex.get(accountNumber) ?? null;
  }

  // Cache-aside pattern: cek cache dulu, baru DB
  async findAccount(accountNumber) {
    // 1. Cek Redis cache — O(1)
    if (this.redisCache.has(accountNumber)) {
      this.cacheHits++;
      return { source: 'cache', data: this.redisCache.get(accountNumber) };
    }

    // 2. Cache miss → query database
    this.cacheMisses++;
    const account = await this._queryDatabase(accountNumber);

    if (account) {
      // 3. Simpan ke cache dengan TTL (simulasi: kita skip TTL untuk simplisitas)
      this.redisCache.set(accountNumber, account);
    }

    return { source: 'database', data: account };
  }

  get cacheHitRate() {
    const total = this.cacheHits + this.cacheMisses;
    return total === 0 ? '0%' : `${((this.cacheHits / total) * 100).toFixed(1)}%`;
  }

  addAccount(account) {
    this.dbIndex.set(account.accountNumber, account);
  }
}

// Demo penggunaan
async function demoHybridSearch() {
  const service = new BankingAccountService();

  // Setup data
  service.addAccount({ accountNumber: '1234567890', name: 'Budi', balance: 5_000_000 });
  service.addAccount({ accountNumber: '0987654321', name: 'Siti', balance: 12_000_000 });

  console.log('\n=== Hybrid Cache + DB Search ===');

  // Pertama kali: cache miss → ke DB
  let result = await service.findAccount('1234567890');
  console.log(`[${result.source}] Budi:`, result.data?.name);

  // Kedua kali: cache hit
  result = await service.findAccount('1234567890');
  console.log(`[${result.source}] Budi:`, result.data?.name);

  console.log('Cache hit rate:', service.cacheHitRate);
}

demoHybridSearch();
```

---

## 📊 Visualisasi

### ASCII Diagram: Proses Binary Search

```
Array terurut: [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
Index:          0  1  2   3   4   5   6   7   8   9
Target: 23

Iterasi 1:
┌──┬──┬──┬───┬───┬───┬───┬───┬───┬───┐
│ 2│ 5│ 8│ 12│ 16│ 23│ 38│ 56│ 72│ 91│
└──┴──┴──┴───┴───┴───┴───┴───┴───┴───┘
 ↑ low=0              ↑ mid=4          ↑ high=9
arr[4]=16 < 23 → geser low ke mid+1=5

Iterasi 2:
┌──┬──┬──┬───┬───┬───┬───┬───┬───┬───┐
│ 2│ 5│ 8│ 12│ 16│ 23│ 38│ 56│ 72│ 91│
└──┴──┴──┴───┴───┴───┴───┴───┴───┴───┘
                    ↑ low=5  ↑ mid=7  ↑ high=9
arr[7]=56 > 23 → geser high ke mid-1=6

Iterasi 3:
┌──┬──┬──┬───┬───┬───┬───┬───┬───┬───┐
│ 2│ 5│ 8│ 12│ 16│ 23│ 38│ 56│ 72│ 91│
└──┴──┴──┴───┴───┴───┴───┴───┴───┴───┘
                    ↑↑ low=high=5, mid=5
arr[5]=23 === 23 → DITEMUKAN! ✓

Total: 3 iterasi untuk 10 elemen (log₂10 ≈ 3.32)
```

### ASCII Diagram: Hash Table dengan Chaining

```
Hash Table (size=7)

Index 0: → [ ]
Index 1: → ["rekening-001": Budi] → ["rekening-008": Rina]  ← collision! chained
Index 2: → ["rekening-002": Siti]
Index 3: → [ ]
Index 4: → ["rekening-003": Ahmad] → ["rekening-010": Dewi] ← collision!
Index 5: → ["rekening-004": Rizky]
Index 6: → ["rekening-005": Farida]

hash("rekening-001") = 1
hash("rekening-008") = 1  ← sama! → chain ke bucket 1
```

### Tabel Big-O Komprehensif: Semua Struktur Data

| Struktur Data | Access | Search | Insert | Delete | Space |
|---|---|---|---|---|---|
| **Array** | O(1) | O(n) | O(n) | O(n) | O(n) |
| **Dynamic Array** | O(1) | O(n) | O(1) amortized | O(n) | O(n) |
| **Singly Linked List** | O(n) | O(n) | O(1) head | O(1) head | O(n) |
| **Doubly Linked List** | O(n) | O(n) | O(1) head/tail | O(1) known node | O(n) |
| **Stack** | O(n) | O(n) | O(1) push | O(1) pop | O(n) |
| **Queue** | O(n) | O(n) | O(1) enqueue | O(1) dequeue | O(n) |
| **Hash Table** | N/A | O(1) avg / O(n) worst | O(1) avg / O(n) worst | O(1) avg / O(n) worst | O(n) |
| **BST (balanced)** | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| **BST (unbalanced)** | O(n) | O(n) | O(n) | O(n) | O(n) |
| **AVL Tree / Red-Black** | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| **Min/Max Heap** | O(1) top | O(n) | O(log n) | O(log n) | O(n) |
| **Trie** | O(k) | O(k) | O(k) | O(k) | O(n·k) |
| **Graph (Adj. Matrix)** | O(1) | O(V²) | O(1) edge | O(1) edge | O(V²) |
| **Graph (Adj. List)** | O(V+E) | O(V+E) | O(1) | O(E) | O(V+E) |

*k = panjang key, V = vertices, E = edges*

### Tabel Perbandingan Algoritma Searching

| Algoritma | Best | Average | Worst | Space | Syarat |
|---|---|---|---|---|---|
| **Sequential Search** | O(1) | O(n/2) = O(n) | O(n) | O(1) | Tidak ada |
| **Binary Search** | O(1) | O(log n) | O(log n) | O(1) iter / O(log n) rek | Array terurut |
| **Hash Table lookup** | O(1) | O(1) | O(n) | O(n) | Hash function & extra space |
| **BST Search** | O(1) | O(log n) | O(n) | O(1) | BST valid |
| **Jump Search** | O(1) | O(√n) | O(√n) | O(1) | Array terurut |
| **Interpolation Search** | O(1) | O(log log n) | O(n) | O(1) | Array terurut & distribusi merata |

### Grafik Pertumbuhan (Jumlah Operasi)

```
Operasi
  ↑
1M│                                              ╭── O(n²)
  │                                         ╭───╯
  │                                    ╭────╯
500K│                               ╭───╯
  │                            ╭────╯
  │                       ╭────╯
100K│                  ╭───╯
  │             ╭──────╯ ← O(n log n)
  │        ╭────╯
 10K│  ╭───╯ ← O(n)
  │╭─╯
 1K│─────────────────────────────── O(log n)
  │═════════════════════════════════ O(1)
  └───────────────────────────────────────→ n
        100  1K   10K  100K  1M
```

---

## ⚠️ Kesalahan Umum

### 1. Binary Search pada Array Tidak Terurut

```javascript
// ❌ SALAH — array tidak terurut, hasil tidak bisa diprediksi
const unsorted = [64, 25, 12, 22, 11];
console.log(binarySearchIterative(unsorted, 22)); // Mungkin -1 padahal ada!

// ✅ BENAR — sort dulu
const sorted = [...unsorted].sort((a, b) => a - b);
console.log(binarySearchIterative(sorted, 22)); // 2 ✓

// ⚠️ Ingat: sort sendiri O(n log n)!
// Kalau kamu sort dulu baru binary search, total tetap O(n log n)
// Binary search hanya worth it kalau array SUDAH terurut atau sering dicari
```

### 2. Integer Overflow pada Binary Search

```javascript
// ❌ SALAH di bahasa dengan fixed integer (bukan masalah di JS, tapi penting diketahui)
const mid = Math.floor((low + high) / 2);
// Jika low=1.5M dan high=1.5M → low+high=3M → aman di JS (Number aman hingga 2^53)

// ✅ Pattern aman (best practice dari Java/C++)
const mid = Math.floor(low + (high - low) / 2);
```

### 3. Salah Paham Worst Case Hash Table

```javascript
// ❌ Asumsi: Hash table SELALU O(1)
// Hash table O(n) di worst case jika semua key collision ke bucket yang sama

// Hash function buruk:
function badHash(key) {
  return 0; // semua key ke bucket 0 — O(n) untuk semua operasi!
}

// ✅ Gunakan hash function yang mendistribusikan data merata
// Dalam praktik: JavaScript Map sudah menggunakan implementasi yang baik
// Gunakan Map bawaan JS daripada implement sendiri kecuali ada kebutuhan khusus
```

### 4. Lupa Space Complexity Rekursif

```javascript
// ❌ Lupa bahwa rekursi memakai call stack
function binarySearchRecursive(arr, target, low = 0, high = arr.length - 1) {
  // Setiap rekursi = satu frame di call stack
  // Untuk array 1 juta elemen: kedalaman rekursi ≈ log₂(1M) ≈ 20 level
  // Space: O(log n) — biasanya aman, tapi ada overhead
}

// ✅ Untuk data sangat besar, gunakan versi iteratif: Space O(1)
function binarySearchIterative(arr, target) {
  // Tidak ada rekursi = tidak ada overhead call stack
}
```

### 5. Salah Menghitung Big-O Loop Bersarang dengan Batas Berbeda

```javascript
// ❌ Asumsi semua loop bersarang = O(n²)
function example(arr) {
  for (let i = 0; i < arr.length; i++) {        // O(n)
    for (let j = i + 1; j < arr.length; j++) {  // O(n-i) ≈ O(n/2) rata-rata
      // ...
    }
  }
}
// Total: O(n * n/2) = O(n²/2) = O(n²) — tetap O(n²) setelah drop constants

// ✅ Yang benar-benar O(n) meskipun ada 2 loop:
function twoPointers(arr) {
  let left = 0, right = arr.length - 1;
  while (left < right) {  // Dua pointer, bukan nested loop
    left++;
    right--;
  }
}
// Total langkah: n/2 → O(n)
```

---

## 🧪 Latihan / Studi Kasus

### Latihan 1: Analisis Kompleksitas (Teori)

Hitung Big-O time dan space complexity dari kode berikut:

```javascript
// Soal A
function mystery(n) {
  let result = 0;
  for (let i = 1; i <= n; i *= 2) {  // ← perhatikan: i *= 2
    for (let j = 0; j < n; j++) {
      result += i + j;
    }
  }
  return result;
}
// Jawaban: Time O(n log n), Space O(1)
// Penjelasan: loop luar berjalan log n kali (i *=2), loop dalam n kali

// Soal B
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}
// Jawaban: Time O(2^n), Space O(n) (kedalaman call stack)
// Penjelasan: setiap call membuat 2 call baru → pohon biner dengan kedalaman n
```

### Latihan 2: Implementasi Binary Search Variasi

```javascript
/**
 * Implementasikan fungsi berikut menggunakan Binary Search:
 *
 * searchInsertPosition(arr, target):
 * - Jika target ada, return indexnya
 * - Jika tidak ada, return index dimana seharusnya target disisipkan
 *   agar array tetap terurut
 *
 * Contoh:
 * searchInsertPosition([1, 3, 5, 6], 5) → 2
 * searchInsertPosition([1, 3, 5, 6], 2) → 1
 * searchInsertPosition([1, 3, 5, 6], 7) → 4
 * searchInsertPosition([1, 3, 5, 6], 0) → 0
 *
 * Time: O(log n) | Space: O(1)
 */

// Scaffold:
function searchInsertPosition(arr, target) {
  let low = 0, high = arr.length - 1;
  // TODO: implementasikan binary search
  // Hint: ketika loop selesai, 'low' adalah posisi insert yang benar
}

// Solusi:
function searchInsertPositionSolution(arr, target) {
  let low = 0, high = arr.length - 1;
  while (low <= high) {
    const mid = Math.floor((low + high) / 2);
    if (arr[mid] === target) return mid;
    else if (arr[mid] < target) low = mid + 1;
    else high = mid - 1;
  }
  return low; // posisi insert yang tepat
}

console.log(searchInsertPositionSolution([1, 3, 5, 6], 5)); // 2
console.log(searchInsertPositionSolution([1, 3, 5, 6], 2)); // 1
console.log(searchInsertPositionSolution([1, 3, 5, 6], 7)); // 4
console.log(searchInsertPositionSolution([1, 3, 5, 6], 0)); // 0
```

### Latihan 3: Studi Kasus — Sistem Pencarian Produk E-commerce

```javascript
/**
 * Kamu diminta merancang sistem pencarian produk untuk toko online
 * dengan 500.000 produk.
 *
 * Sistem harus mendukung:
 * 1. Pencarian produk berdasarkan ID (exact match)
 * 2. Pencarian produk berdasarkan nama (partial match)
 * 3. Filter produk berdasarkan rentang harga
 *
 * Pertanyaan:
 * a) Struktur data apa yang kamu pilih untuk masing-masing fitur?
 * b) Berapa kompleksitasnya?
 * c) Apa trade-off yang ada?
 *
 * Implementasikan prototype sistem tersebut!
 */

class ProductSearchSystem {
  constructor() {
    this.byId = new Map();           // O(1) lookup by ID
    this.sortedByPrice = [];         // O(log n) range search dengan binary search
    this.nameIndex = new Map();      // inverted index untuk partial name search
  }

  addProduct(product) {
    // 1. Hash map untuk ID lookup
    this.byId.set(product.id, product);

    // 2. Insert ke sorted array (untuk range query harga)
    const insertPos = this._findInsertPos(product.price);
    this.sortedByPrice.splice(insertPos, 0, product);

    // 3. Inverted index untuk nama
    // Tokenize nama → daftarkan setiap kata
    const words = product.name.toLowerCase().split(' ');
    for (const word of words) {
      if (!this.nameIndex.has(word)) this.nameIndex.set(word, new Set());
      this.nameIndex.get(word).add(product.id);
    }
  }

  // O(1)
  findById(id) {
    return this.byId.get(id) ?? null;
  }

  // O(k) dimana k = jumlah produk dengan kata tersebut
  findByName(keyword) {
    const ids = this.nameIndex.get(keyword.toLowerCase()) ?? new Set();
    return [...ids].map(id => this.byId.get(id));
  }

  // O(log n + m) dimana m = jumlah produk dalam range
  findByPriceRange(minPrice, maxPrice) {
    const start = this._findInsertPos(minPrice);
    const result = [];
    for (let i = start; i < this.sortedByPrice.length; i++) {
      if (this.sortedByPrice[i].price > maxPrice) break;
      result.push(this.sortedByPrice[i]);
    }
    return result;
  }

  _findInsertPos(price) {
    let low = 0, high = this.sortedByPrice.length;
    while (low < high) {
      const mid = Math.floor((low + high) / 2);
      if (this.sortedByPrice[mid].price < price) low = mid + 1;
      else high = mid;
    }
    return low;
  }
}

// Demo
const search = new ProductSearchSystem();
search.addProduct({ id: 'P001', name: 'Laptop Gaming Asus', price: 15_000_000 });
search.addProduct({ id: 'P002', name: 'Laptop Kerja Lenovo', price: 8_000_000 });
search.addProduct({ id: 'P003', name: 'Mouse Gaming Logitech', price: 500_000 });

console.log('\n=== Product Search System ===');
console.log(search.findById('P001').name);              // Laptop Gaming Asus
console.log(search.findByName('gaming').length);        // 2 (Asus + Logitech)
console.log(search.findByPriceRange(1_000_000, 10_000_000).map(p => p.name));
// ['Laptop Kerja Lenovo']
```

---

## 📌 Ringkasan

| Konsep | Poin Kunci |
|---|---|
| **Big-O** | Worst case — batas atas pertumbuhan |
| **Big-Ω** | Best case — batas bawah pertumbuhan |
| **Big-Θ** | Average/tight case — pertumbuhan eksak |
| **Drop Constants** | O(2n) → O(n), O(500) → O(1) |
| **Drop Non-Dominant** | O(n² + n) → O(n²) |
| **Sequential Search** | O(n) — tidak perlu terurut, selalu scan semua |
| **Binary Search** | O(log n) — wajib terurut, sangat efisien |
| **Hash Table** | O(1) avg — paling cepat, butuh extra space |
| **Time vs Space** | Sering ada trade-off, sesuaikan konteks |
| **Amortized** | Rata-rata per operasi dari banyak operasi |

**Hierarki yang wajib dihapal:**
```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2^n) < O(n!)
```

**Kapan pilih apa?**

```
Mau cari data?
├── Data tidak terurut → Sequential Search O(n)
├── Data terurut & sering dicari → Binary Search O(log n)
└── Butuh lookup super cepat & ada extra memori → Hash Map O(1)

Skala jutaan record seperti BCA/Mandiri?
→ Hash Map (Redis) sebagai cache + B-Tree Index di database
```

> 💡 **Takeaway utama:** Pilihan struktur data dan algoritma bisa membuat perbedaan antara sistem yang merespons dalam 0.1ms vs 250ms. Di skala 50 juta pengguna, ini bukan perbedaan kecil — ini perbedaan antara produk yang sukses dan produk yang ditinggalkan user.
