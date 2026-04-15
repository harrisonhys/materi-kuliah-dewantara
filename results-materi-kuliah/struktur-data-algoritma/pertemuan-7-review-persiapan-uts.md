# Pertemuan 7: Review & Persiapan UTS — Struktur Data & Algoritma

---

## 🎯 Learning Outcomes

Setelah pertemuan review ini, kamu akan bisa:

* Merangkum konsep pertemuan 1–6 dalam satu referensi cepat
* Menjawab soal UTS bertipe teori, analisis, dan coding
* Mengidentifikasi kesalahan konseptual yang paling sering muncul
* Menghubungkan Big-O, struktur data, rekursi, dan sorting dalam satu framework berpikir

---

## 📖 Pengantar (Hook)

UTS bukan tentang hafalan. UTS ini adalah tentang apakah kamu **benar-benar paham** mengapa kita perlu struktur data, mengapa O(n log n) jauh lebih baik dari O(n²), dan mengapa referensi vs nilai bisa membuat saldo rekening berubah tanpa kamu sadari.

Jika kamu bisa menjelaskan konsep-konsep ini dengan analogi yang sederhana, kamu sudah siap.

---

## 🧩 Ringkasan Materi Pertemuan 1–6

### Pertemuan 1: Pengantar Struktur Data & ADT

* **Struktur data** = cara mengorganisir data untuk efisiensi operasi tertentu
* **ADT (Abstract Data Type)** = interface (apa yang bisa dilakukan) tanpa peduli implementasi
* **Big-O Notation** = ukuran pertumbuhan waktu/ruang relatif terhadap input
* **Hierarki kompleksitas:** O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2^n)

| Big-O | Nama | Contoh Operasi |
|---|---|---|
| O(1) | Konstan | Akses array via index |
| O(log n) | Logaritmik | Binary search |
| O(n) | Linear | Loop satu kali |
| O(n log n) | Linearitmik | Merge Sort, Quick Sort |
| O(n²) | Kuadratik | Bubble Sort, nested loop |

### Pertemuan 2: Array & Object JavaScript

* **Array** = koleksi terurut, akses O(1) via index, insert/delete O(n) di tengah
* **Object** = pasangan key-value, pengganti Struct, akses properti O(1)
* **Array of Objects** = pattern paling umum untuk data terstruktur
* **Metode wajib:** `map()` (transform), `filter()` (seleksi), `reduce()` (akumulasi), `sort()`
* **Ingat:** `sort()` modifikasi in-place! Gunakan `[...arr].sort()` untuk tidak merusak aslinya
* **Ingat:** `[10, 9, 2].sort()` → `[10, 2, 9]` (lexicographic)! Selalu beri comparator untuk angka

### Pertemuan 3: Rekursi

* **Dua syarat wajib:** Base case (kondisi berhenti) + Recursive case (panggil diri sendiri lebih kecil)
* **Direct recursion** = fungsi memanggil dirinya langsung
* **Indirect recursion** = A → B → A
* **Call stack** tumbuh setiap level rekursi — terlalu dalam → Stack Overflow
* **Fibonacci naif = O(2^n)** — gunakan memoization → O(n)
* **Tail recursion** = recursive call sebagai operasi terakhir → bisa di-optimize menjadi loop

```
faktorial(4)
  ↳ 4 × faktorial(3)
      ↳ 3 × faktorial(2)
          ↳ 2 × faktorial(1)
              ↳ 1 × faktorial(0) → 1
```

### Pertemuan 4: Sorting Dasar

| Algoritma | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble Sort | O(n)* | O(n²) | O(n²) | O(1) | ✅ |
| Insertion Sort | **O(n)** | O(n²) | O(n²) | O(1) | ✅ |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ |

*dengan early exit optimization

* **Bubble Sort:** bandingkan bersebelahan, tukar jika salah urutan
* **Insertion Sort:** ambil satu elemen, sisipkan ke posisi tepat — TERBAIK untuk nearly-sorted!
* **Selection Sort:** cari minimum, letakkan di depan — sedikit swap, tapi selalu O(n²)

### Pertemuan 5: Sorting Lanjut

| Algoritma | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Quick Sort | O(n log n) | O(n log n) | O(n²)* | O(log n) | ❌ |
| Merge Sort | O(n log n) | O(n log n) | **O(n log n)** | O(n) | ✅ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | **O(1)** | ❌ |
| Timsort (JS) | O(n) | O(n log n) | O(n log n) | O(n) | ✅ |

*Quick Sort worst case dengan pivot tetap. Gunakan random pivot untuk menghindari!

**Panduan memilih:**
* Data kecil (n < 50) → Insertion Sort
* Butuh stable sort → Merge Sort atau Timsort
* Memori terbatas → Heap Sort
* Kasus umum → `Array.sort()` (Timsort)

### Pertemuan 6: Reference, Closure & Scope

* **Primitive** (number, string, bool) → by value, disimpan di Stack
* **Reference** (object, array, function) → by reference/pointer, disimpan di Heap
* `const b = a` (object) — bukan copy! Keduanya menunjuk ke object yang sama
* **Shallow copy:** `{...obj}` atau `[...arr]` — hanya level pertama
* **Deep copy:** `JSON.parse(JSON.stringify(obj))` — semua level
* **Closure** = fungsi yang mengingat variabel dari lexical scope luar
* **Module pattern** = closure untuk membuat private state + public interface
* **Garbage collection** = otomatis, berbasis reachability

---

## 🧠 Peta Konsep Terintegrasi

```
STRUKTUR DATA & ALGORITMA
│
├── KOMPLEKSITAS (mengapa penting?)
│   ├── Big-O → ukur pertumbuhan, bukan waktu absolut
│   └── Best/Average/Worst case → pertimbangkan semua skenario
│
├── DATA STORAGE (bagaimana data disimpan?)
│   ├── Array → index-based, O(1) akses
│   ├── Object → key-value, O(1) lookup
│   └── Reference vs Value → memahami pointer di Heap
│
├── ALGORITMA (bagaimana data diproses?)
│   ├── Rekursi → divide & conquer, butuh base case
│   └── Sorting → pilih berdasarkan data karakteristik + kebutuhan
│
└── PRAKTIK (bagaimana diterapkan?)
    ├── Closure → private state, memoization, module pattern
    └── Fintech case studies → leaderboard, rate limiter, nested JSON
```

---

## 💻 Soal Coding Campuran

```javascript
// SOAL 1: Gabungkan rekursi + array + Big-O
// Implementasikan binary search secara rekursif
// Input: array terurut + target
// Output: index target, atau -1 jika tidak ada
// Kompleksitas: O(log n)
function binarySearch(arr, target, kiri = 0, kanan = arr.length - 1) {
  // Jawaban:
  if (kiri > kanan) return -1;
  const mid = Math.floor((kiri + kanan) / 2);
  if (arr[mid] === target) return mid;
  if (arr[mid] < target) return binarySearch(arr, target, mid + 1, kanan);
  return binarySearch(arr, target, kiri, mid - 1);
}

// SOAL 2: Closure + module pattern
// Buat sistem antrian OTP sederhana
function buatOTPQueue() {
  const _queue = []; // private

  return {
    generate(userId) {
      const otp = Math.floor(100000 + Math.random() * 900000).toString();
      _queue.push({ userId, otp, expiry: Date.now() + 5 * 60 * 1000 });
      return otp;
    },
    validate(userId, otp) {
      const idx = _queue.findIndex(
        item => item.userId === userId && item.otp === otp
      );
      if (idx === -1) return false;
      const item = _queue[idx];
      if (Date.now() > item.expiry) {
        _queue.splice(idx, 1); // hapus yang expired
        return false;
      }
      _queue.splice(idx, 1); // OTP dipakai sekali
      return true;
    },
    size: () => _queue.length
  };
}

// SOAL 3: Sorting + filter + reduce (pipeline data)
const transaksi = [
  { id: 'T1', nominal: 500000, status: 'success', merchant: 'A' },
  { id: 'T2', nominal: 1200000, status: 'failed', merchant: 'B' },
  { id: 'T3', nominal: 350000, status: 'success', merchant: 'A' },
  { id: 'T4', nominal: 800000, status: 'success', merchant: 'C' },
];

// Top 3 merchant berdasarkan total volume transaksi sukses
const top3 = Object.entries(
  transaksi
    .filter(t => t.status === 'success')
    .reduce((acc, t) => {
      acc[t.merchant] = (acc[t.merchant] || 0) + t.nominal;
      return acc;
    }, {})
)
  .sort((a, b) => b[1] - a[1])
  .slice(0, 3);

console.log(top3); // [['A', 850000], ['C', 800000]]
```

---

## 📊 Cheat Sheet Kompleksitas

```
OPERASI ARRAY:
  akses via index        → O(1)
  pencarian linear       → O(n)
  insert/delete di akhir → O(1) amortized
  insert/delete di awal  → O(n)

SORTING:
  Bubble/Insertion/Selection → O(n²) avg
  Quick/Merge/Heap            → O(n log n) avg
  Timsort (Array.sort)        → O(n) best, O(n log n) avg
  Insertion Sort nearly sorted→ O(n)!

REKURSI:
  Fibonacci naif  → O(2^n)
  Fibonacci memo  → O(n)
  Faktorial       → O(n)
  Binary Search   → O(log n)
  Merge Sort      → O(n log n)
```

---

## 🏦 Studi Kasus Terintegrasi

#### Sistem Rekonsiliasi Transaksi Harian

**Skenario:** Sistem fintech perlu melakukan rekonsiliasi — mencocokkan transaksi internal dengan data bank. Proses ini harus:
1. Load 100.000+ record transaksi
2. Sort berdasarkan ID
3. Cari transaksi yang tidak match (ada di internal tapi tidak di bank)
4. Generate laporan dengan total per kategori

**Pilihan algoritma:**
* Sorting → Timsort (via `Array.sort`) — stable, O(n log n)
* Pencarian mismatch → bisa pakai binary search setelah sorted — O(log n) per lookup
* Aggregasi → reduce — O(n)
* State laporan → module pattern dengan closure

```javascript
function buatSistemRekonsiliasi() {
  let _totalDiproses = 0;
  let _totalMismatch = 0;

  return {
    rekonsiliasi(transaksiInternal, transaksiBank) {
      // Sort kedua array berdasarkan ID — O(n log n)
      const internal = [...transaksiInternal].sort((a, b) =>
        a.id.localeCompare(b.id));
      const bank = [...transaksiBank].sort((a, b) =>
        a.id.localeCompare(b.id));

      const mismatch = internal.filter(trx => {
        // Binary search di array bank yang sudah sorted — O(log n)
        return binarySearch(bank.map(b => b.id), trx.id) === -1;
      });

      _totalDiproses += internal.length;
      _totalMismatch += mismatch.length;

      return {
        mismatch,
        totalVolume: mismatch.reduce((sum, t) => sum + t.nominal, 0)
      };
    },
    getStatistik: () => ({
      totalDiproses: _totalDiproses,
      totalMismatch: _totalMismatch,
      persentaseMismatch: ((_totalMismatch / _totalDiproses) * 100).toFixed(2) + '%'
    })
  };
}
```

---

## ⚠️ Kesalahan Paling Sering di UTS

1. **Tidak bedakan Best/Average/Worst case** → Insertion Sort O(n) adalah BEST case, bukan average!

2. **Lupa bahwa `sort()` memodifikasi array asli** → Gunakan `[...arr].sort()` di benchmark.

3. **Mengira rekursi selalu lebih lambat** → Dengan memoization, rekursi bisa setara iteratif. Fibonacci memo O(n) vs iteratif O(n) — hampir sama.

4. **Bingung stable vs unstable sort** → Stable = elemen dengan nilai sama tetap urutan relatifnya. Quick Sort dan Heap Sort TIDAK stable.

5. **Salah konsep reference** → `const b = {...a}` dan `const b = a` hasilnya BERBEDA. Yang pertama shallow copy, yang kedua adalah alias (menunjuk ke object yang sama).

6. **Lupa base case saat coding rekursi** → Langsung ke recursive case. Selalu tulis base case dulu!

---

## 🧪 Soal Latihan UTS

### Bagian A: Teori (25 poin)

**1.** (5 poin) Urutkan algoritma berikut dari kompleksitas terbaik ke terburuk:
O(n²), O(1), O(n log n), O(2^n), O(log n), O(n)

**2.** (5 poin) Jelaskan perbedaan antara **stable** dan **unstable** sort. Berikan satu contoh algoritma untuk masing-masing dan skenario di mana stabilitas sorting menjadi krusial.

**3.** (5 poin) Apa yang terjadi pada memory ketika fungsi rekursif `faktorial(1000)` dipanggil? Jelaskan dengan menyebut istilah: call stack, stack frame, stack overflow.

**4.** (5 poin) Bedakan antara **shallow copy** dan **deep copy** dalam JavaScript. Berikan contoh kode di mana shallow copy bisa menyebabkan bug.

**5.** (5 poin) Mengapa `[10, 9, 2].sort()` menghasilkan `[10, 2, 9]`? Bagaimana cara memperbaikinya?

### Bagian B: Analisis Algoritma (35 poin)

**6.** (10 poin) Diberikan array: `[5, 2, 8, 1, 9, 3]`

Trace (tulis langkah-langkah) Insertion Sort pada array tersebut. Berapa total operasi perbandingan yang dilakukan?

**7.** (10 poin) Analisis kode berikut — identifikasi kompleksitasnya dan apakah ada cara untuk mengoptimalkannya:

```javascript
function cariPasangan(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] + arr[j] === target) return [arr[i], arr[j]];
    }
  }
  return null;
}
```

**8.** (15 poin) Kamu diminta memilih algoritma sorting untuk dua skenario berikut. Jelaskan pilihan kamu dan alasannya:

a) Mengurutkan 1 juta record transaksi berdasarkan timestamp. Data bersifat random.

b) Memperbarui ranking leaderboard 50.000 merchant setiap menit. Jika dua merchant memiliki volume sama, yang bergabung lebih awal harus di posisi lebih tinggi.

### Bagian C: Coding (40 poin)

**9.** (20 poin) Implementasikan fungsi `grupkanTransaksi(arr)` yang:
* Menerima array transaksi dengan properti `{ id, nominal, merchant, status }`
* Mengembalikan object dengan merchant sebagai key, value berupa total nominal transaksi SUKSES
* Diurutkan dari total tertinggi ke terendah
* Big-O solusi kamu harus O(n log n) atau lebih baik

**10.** (20 poin) Buat `RateLimiterModule` menggunakan module pattern (closure) dengan:
* Method `coba(userId)` — return `true` jika diizinkan, `false` jika terlalu banyak request
* Batas: maksimal 5 request per user per 60 detik
* Method `getStatistik(userId)` — return berapa request tersisa dalam window aktif

---

## 📌 Tips Mengerjakan UTS

**Persiapan:**
* Pahami Big-O secara intuitif — bukan hafal, tapi paham mengapa
* Latih tracing algoritma sorting manual di kertas
* Review perbedaan reference vs value — ini sering jebakan

**Saat ujian:**
* Untuk soal analisis Big-O: hitung berapa kali loop berjalan relatif terhadap n
* Untuk soal coding: tulis base case / validasi dulu sebelum logic utama
* Untuk soal pilihan algoritma: selalu sebutkan trade-off, bukan hanya pilihan

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Sedgewick & Wayne (2011). Algorithms | Kyle Simpson. You Don't Know JS: Scope & Closures*
