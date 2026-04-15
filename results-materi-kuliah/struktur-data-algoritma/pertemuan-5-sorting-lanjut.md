# Pertemuan 5: Sorting Lanjut — Quick Sort, Merge Sort & Heap Sort

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan cara kerja Quick Sort, Merge Sort, dan Heap Sort
* Mengimplementasikan ketiga algoritma dalam JavaScript
* Menganalisis kompleksitas best/average/worst case dan space complexity
* Membandingkan semua algoritma sorting dalam satu tabel komprehensif
* Menentukan kapan menggunakan algoritma mana berdasarkan karakteristik data

---

## 📖 Pengantar (Hook)

Bubble Sort untuk 1 juta data? Butuh **11 hari** (teoritis).

Merge Sort untuk 1 juta data? Butuh **1 detik**.

Itu bukan exaggerasi — itu matematik. Dan itulah mengapa Quick Sort dan Merge Sort yang ada di balik `Array.sort()` hampir semua bahasa pemrograman modern, termasuk JavaScript.

---

## 🧩 Konsep Utama

### Quick Sort

**Ide:** Pilih satu elemen sebagai "pivot." Letakkan semua elemen lebih kecil di kiri pivot, lebih besar di kanan. Rekursif pada kedua sisi.

```
Array: [3, 6, 8, 10, 1, 2, 1]
Pivot: 3 (elemen pertama)

Partisi:
Kiri (< 3):  [1, 2, 1]
Pivot:       [3]
Kanan (> 3): [6, 8, 10]

Rekursif pada kiri dan kanan...
```

**Big-O Quick Sort:**
| Case | Time | Space (call stack) |
|---|---|---|
| Best | O(n log n) | O(log n) |
| Average | O(n log n) | O(log n) |
| Worst (pivot selalu min/max) | O(n²) | O(n) |

**Cara hindari worst case:** Pilih pivot secara acak (random pivot) atau gunakan "median of three."

### Merge Sort

**Ide:** Divide & Conquer — bagi array menjadi dua bagian, sort masing-masing secara rekursif, lalu merge (gabungkan) dua bagian yang sudah terurut.

```
[38, 27, 43, 3, 9, 82, 10]
         ↓ bagi dua
[38, 27, 43, 3]    [9, 82, 10]
      ↓                  ↓
[38, 27]  [43, 3]  [9, 82]  [10]
   ↓          ↓      ↓        ↓
[38][27]  [43][3]  [9][82]  [10]
     ↓ merge         ↓ merge
[27, 38]  [3, 43]  [9, 82]  [10]
         ↓ merge          ↓ merge
   [3, 27, 38, 43]   [9, 10, 82]
              ↓ merge final
       [3, 9, 10, 27, 38, 43, 82]
```

**Big-O Merge Sort:**
| Case | Time | Space |
|---|---|---|
| Best | O(n log n) | O(n) |
| Average | O(n log n) | O(n) |
| Worst | O(n log n) | O(n) |

Merge Sort **selalu O(n log n)** — konsisten, tapi butuh O(n) memori tambahan.

### Heap Sort

**Ide:** Bangun Max-Heap dari array, lalu ekstrak elemen maksimum berulang kali.

**Big-O Heap Sort:**
| Case | Time | Space |
|---|---|---|
| Semua case | O(n log n) | O(1) — in-place! |

---

## 🧠 Ilustrasi / Analogi

| Algoritma | Analogi |
|---|---|
| **Quick Sort** | Divide and conquer di militer — komandan (pivot) membagi pasukan jadi dua kelompok, masing-masing diselesaikan secara independen |
| **Merge Sort** | Tim proyek yang dibagi jadi tim kecil, masing-masing kerjakan bagian sendiri, lalu hasilnya digabungkan |
| **Heap Sort** | Turnamen — pemenang (max) diambil dulu, lalu turnamen diulang dari sisa peserta |

---

## 💻 Contoh Teknis

```javascript
// ============= QUICK SORT =============
function quickSort(arr) {
  if (arr.length <= 1) return arr;  // base case

  // Random pivot untuk hindari worst case
  const pivotIdx = Math.floor(Math.random() * arr.length);
  const pivot = arr[pivotIdx];

  const kiri    = arr.filter((x, i) => i !== pivotIdx && x <= pivot);
  const kanan   = arr.filter((x, i) => i !== pivotIdx && x > pivot);

  return [...quickSort(kiri), pivot, ...quickSort(kanan)];
}

// ============= MERGE SORT =============
function mergeSort(arr) {
  if (arr.length <= 1) return arr;  // base case

  const mid = Math.floor(arr.length / 2);
  const kiri  = mergeSort(arr.slice(0, mid));
  const kanan = mergeSort(arr.slice(mid));

  return merge(kiri, kanan);
}

function merge(kiri, kanan) {
  const hasil = [];
  let i = 0, j = 0;

  while (i < kiri.length && j < kanan.length) {
    if (kiri[i] <= kanan[j]) {
      hasil.push(kiri[i++]);
    } else {
      hasil.push(kanan[j++]);
    }
  }

  return [...hasil, ...kiri.slice(i), ...kanan.slice(j)];
}

// ============= PERBANDINGAN SEMUA ALGORITMA =============
const n = 10000;
const data = Array.from({ length: n }, () => Math.floor(Math.random() * 100000));

const algos = {
  'Quick Sort':  (arr) => quickSort([...arr]),
  'Merge Sort':  (arr) => mergeSort([...arr]),
  'Array.sort':  (arr) => [...arr].sort((a, b) => a - b),
};

for (const [nama, fn] of Object.entries(algos)) {
  const start = performance.now();
  fn(data);
  const end = performance.now();
  console.log(`${nama}: ${(end - start).toFixed(2)}ms`);
}
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Leaderboard Merchant Real-Time: Mengapa Merge Sort Dipilih

**Skenario:** Platform pembayaran B2B perlu menampilkan leaderboard 50.000 merchant berdasarkan volume transaksi. Leaderboard diperbarui setiap menit. **Jika dua merchant punya volume sama, urutan mereka dari sebelumnya harus dipertahankan** (karena dipakai untuk tiebreaker hadiah bulanan).

**Mengapa Quick Sort kurang tepat di sini?**
* Quick Sort **tidak stable** — elemen dengan nilai sama bisa bertukar urutan
* Dengan random pivot, hasilnya tidak deterministic

**Mengapa Merge Sort dipilih?**
* Merge Sort **stable** — elemen dengan nilai sama tetap pada urutan relatif mereka
* Selalu O(n log n) — tidak ada worst case seperti Quick Sort
* Hasil sorting **deterministic** dan **reproducible**
* Trade-off: butuh O(n) memori tambahan — untuk 50.000 merchant, ini acceptable

**Implementasi di production:**
```javascript
// Merge Sort yang stable untuk object
const leaderboard = merchants.sort((a, b) => {
  if (b.volume !== a.volume) return b.volume - a.volume; // sort by volume DESC
  return a.joinDate - b.joinDate; // tiebreaker: yang lebih lama join lebih tinggi
});
// JavaScript Array.sort() menggunakan Timsort yang STABLE
```

---

## 📊 Visualisasi: Tabel Perbandingan Komprehensif

| Algoritma | Best | Average | Worst | Space | Stable | Cocok untuk |
|---|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | Data kecil, nearly sorted |
| Insertion Sort | **O(n)** | O(n²) | O(n²) | O(1) | ✅ | Data kecil, nearly sorted |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ | Minimisasi swap |
| **Quick Sort** | O(n log n) | **O(n log n)** | O(n²) | O(log n) | ❌ | General purpose, in-place |
| **Merge Sort** | O(n log n) | O(n log n) | **O(n log n)** | O(n) | ✅ | Butuh stable, linked list |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | **O(1)** | ❌ | Memori terbatas |
| **Timsort (JS)** | O(n) | O(n log n) | O(n log n) | O(n) | ✅ | Default pilihan |

### Panduan Memilih Algoritma

```
Apakah data kecil (n < 50)?
  → Insertion Sort (overhead minimum)

Apakah data nearly sorted?
  → Insertion Sort atau Timsort

Apakah memori sangat terbatas?
  → Heap Sort (O(1) space)

Apakah butuh stable sort?
  → Merge Sort atau Timsort (Array.sort() JS)

Kasus umum (data acak, memori cukup)?
  → Quick Sort dengan random pivot, atau Array.sort()
```

---

## ⚠️ Kesalahan Umum

1. **Quick Sort dengan pivot tetap di index 0** → Worst case O(n²) untuk data yang sudah terurut. Selalu gunakan random pivot.

2. **Mengira `Array.sort()` selalu benar** → `[10, 9, 2].sort()` → `[10, 2, 9]`! Default sort adalah lexicographic (string). Selalu berikan comparator: `.sort((a, b) => a - b)`.

3. **Merge Sort untuk linked list vs array** → Merge Sort lebih efisien untuk linked list (merge O(n) tanpa perlu extra array), sementara Quick Sort lebih efisien untuk array (akses random O(1)).

4. **Menggunakan Heap Sort di production tanpa alasan** → Meskipun O(n log n) dan in-place, Heap Sort lebih lambat dari Quick Sort dalam praktik karena cache-unfriendly access pattern.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Mengapa Quick Sort memiliki worst case O(n²)? Berikan contoh input yang akan menghasilkan worst case jika pivot selalu dipilih dari elemen pertama.

b) Mengapa Merge Sort memerlukan O(n) extra space? Apakah ada versi yang bisa mengurangi kebutuhan memori ini?

c) Jelaskan perbedaan antara **stable** dan **unstable** sorting. Berikan satu skenario bisnis nyata di mana stabilitas sorting sangat penting.

### Soal 2 — Coding

1. Implementasikan Merge Sort untuk mengurutkan array of objects berdasarkan properti `timestamp` (tipe string ISO 8601)

2. Lakukan benchmark perbandingan Quick Sort, Merge Sort, dan `Array.sort()` untuk:
   * n = 1.000 (random)
   * n = 10.000 (random)
   * n = 10.000 (sudah terurut)
   * n = 10.000 (terurut terbalik)

   Catat hasil dalam tabel dan buat analisis.

---

## 📌 Ringkasan

* **Quick Sort:** Divide & conquer dengan pivot — O(n log n) avg, O(n²) worst — gunakan random pivot!
* **Merge Sort:** Bagi dua, sort, merge — **selalu O(n log n)**, stable, butuh O(n) extra space
* **Heap Sort:** Build heap, ekstrak max — O(n log n), in-place O(1), tapi tidak stable dan lebih lambat dalam praktik
* **Timsort (Array.sort() JS):** Hybrid insertion + merge, stable, O(n log n) — gunakan ini di production!
* **`[1,2,10].sort()`** → SALAH! Selalu berikan `(a, b) => a - b` untuk angka
* Stable sort penting ketika urutan elemen dengan nilai sama perlu dipertahankan
* Untuk production: gunakan `Array.sort()` kecuali ada alasan spesifik untuk implement sendiri

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Sedgewick & Wayne (2011). Algorithms | Visualgo.net/sorting*
