# Pertemuan 4: Sorting Dasar — Bubble, Insertion & Selection Sort

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan cara kerja Bubble Sort, Insertion Sort, dan Selection Sort langkah demi langkah
* Mengimplementasikan ketiga algoritma tersebut dalam JavaScript
* Menghitung kompleksitas waktu (Big-O) untuk best, average, dan worst case
* Membandingkan performa ketiga algoritma menggunakan `performance.now()`
* Menentukan kapan masing-masing algoritma cocok digunakan

---

## 📖 Pengantar (Hook)

Bayangkan kamu punya 1 juta data nasabah yang harus diurutkan berdasarkan saldo untuk laporan akhir tahun.

Kamu pilih algoritma yang salah — yang harusnya selesai dalam 1 detik, butuh 17 menit. Tim menunggu. Server overload. Bos marah.

Pilihan algoritma sorting **bukan hal sepele.** Di sistem yang memproses jutaan record seperti database fintech, perbedaan antara O(n²) dan O(n log n) bisa berarti perbedaan antara **sistem yang berjalan lancar dan sistem yang crash.**

Di pertemuan ini, kita pelajari tiga algoritma sorting "klasik" — bukan karena mereka terbaik, tapi karena memahami mereka adalah fondasi untuk memahami yang lebih canggih.

---

## 🧩 Konsep Utama

### Mengapa Sorting Penting?

* Data yang terurut memungkinkan **Binary Search** (O(log n)) vs Linear Search (O(n))
* Banyak algoritma lain bergantung pada data yang sudah terurut
* Penyajian laporan kepada pengguna hampir selalu butuh data terurut
* Database index bekerja berdasarkan data yang terurut

### Terminologi

* **In-place** — sorting dilakukan di array yang sama, tanpa array tambahan (hemat memori)
* **Stable** — elemen dengan nilai sama tetap pada urutan relatif awal mereka
* **Comparison-based** — membandingkan dua elemen untuk menentukan urutan

### Bubble Sort

**Ide:** Bandingkan dua elemen bersebelahan. Jika yang kiri > yang kanan, tukar. Ulangi sampai tidak ada pertukaran.

**Cara kerja:** Elemen terbesar "menggelembung" ke akhir array di setiap iterasi.

```
Array awal: [64, 34, 25, 12, 22]

Pass 1:
[34, 64, 25, 12, 22] → tukar 64 dan 34
[34, 25, 64, 12, 22] → tukar 64 dan 25
[34, 25, 12, 64, 22] → tukar 64 dan 12
[34, 25, 12, 22, 64] → tukar 64 dan 22 ← 64 sudah di posisi benar

Pass 2:
[25, 34, 12, 22, 64]
[25, 12, 34, 22, 64]
[25, 12, 22, 34, 64] ← 34 sudah di posisi benar

... dan seterusnya
```

**Big-O Bubble Sort:**
| Case | Time | Space |
|---|---|---|
| Best (sudah terurut) | O(n) dengan early exit | O(1) |
| Average | O(n²) | O(1) |
| Worst (terbalik) | O(n²) | O(1) |

### Insertion Sort

**Ide:** Ambil satu elemen, sisipkan ke posisi yang tepat di "bagian yang sudah terurut" (seperti menyusun kartu di tangan).

```
Array awal: [64, 34, 25, 12, 22]

Step 1: [64] terurut
Step 2: ambil 34, sisip sebelum 64 → [34, 64]
Step 3: ambil 25, sisip sebelum 34 → [25, 34, 64]
Step 4: ambil 12, sisip di awal → [12, 25, 34, 64]
Step 5: ambil 22, sisip antara 12 dan 25 → [12, 22, 25, 34, 64]
```

**Big-O Insertion Sort:**
| Case | Time | Space |
|---|---|---|
| Best (sudah terurut) | **O(n)** | O(1) |
| Average | O(n²) | O(1) |
| Worst (terbalik) | O(n²) | O(1) |

### Selection Sort

**Ide:** Cari elemen terkecil, tukar ke posisi pertama. Lakukan lagi untuk posisi berikutnya.

```
Array awal: [64, 34, 25, 12, 22]

Step 1: cari min di [64,34,25,12,22] → 12, tukar dengan 64 → [12, 34, 25, 64, 22]
Step 2: cari min di [34,25,64,22] → 22, tukar dengan 34 → [12, 22, 25, 64, 34]
Step 3: cari min di [25,64,34] → 25, sudah di posisi → [12, 22, 25, 64, 34]
Step 4: cari min di [64,34] → 34, tukar → [12, 22, 25, 34, 64]
```

**Big-O Selection Sort:**
| Case | Time | Space |
|---|---|---|
| Best | O(n²) | O(1) |
| Average | O(n²) | O(1) |
| Worst | O(n²) | O(1) |

Selection Sort **selalu O(n²)** — tidak ada optimasi untuk best case.

---

## 🧠 Ilustrasi / Analogi

| Algoritma | Analogi |
|---|---|
| **Bubble Sort** | Gelembung udara naik ke permukaan air — elemen terbesar "naik" ke akhir tiap putaran |
| **Insertion Sort** | Menyusun kartu di tangan: ambil satu per satu, taruh di posisi yang tepat |
| **Selection Sort** | Pilih orang terpendek dari kerumunan, taruh di barisan pertama, lalu cari terpendek berikutnya |

---

## 💻 Contoh Teknis

```javascript
// ============= BUBBLE SORT =============
function bubbleSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    let swapped = false; // optimasi early exit
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]; // swap ES6
        swapped = true;
      }
    }
    if (!swapped) break; // sudah terurut, hentikan
  }
  return arr;
}

// ============= INSERTION SORT =============
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    const key = arr[i]; // elemen yang akan disisipkan
    let j = i - 1;
    // Geser elemen yang lebih besar ke kanan
    while (j >= 0 && arr[j] > key) {
      arr[j + 1] = arr[j];
      j--;
    }
    arr[j + 1] = key; // sisipkan di posisi yang tepat
  }
  return arr;
}

// ============= SELECTION SORT =============
function selectionSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    let minIdx = i;
    for (let j = i + 1; j < n; j++) {
      if (arr[j] < arr[minIdx]) {
        minIdx = j; // temukan indeks minimum
      }
    }
    if (minIdx !== i) {
      [arr[i], arr[minIdx]] = [arr[minIdx], arr[i]]; // tukar
    }
  }
  return arr;
}

// ============= PERBANDINGAN PERFORMA =============
function generateRandom(n) {
  return Array.from({ length: n }, () => Math.floor(Math.random() * 10000));
}

const sizes = [100, 1000, 5000];
for (const n of sizes) {
  const data = generateRandom(n);

  console.time(`Bubble  n=${n}`);
  bubbleSort([...data]);
  console.timeEnd(`Bubble  n=${n}`);

  console.time(`Insertion n=${n}`);
  insertionSort([...data]);
  console.timeEnd(`Insertion n=${n}`);

  console.time(`Selection n=${n}`);
  selectionSort([...data]);
  console.timeEnd(`Selection n=${n}`);

  console.log('---');
}
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Kenapa Pilihan Sorting Algorithm Bisa Crash Sistem

**Skenario:** Tim backend di startup fintech "PayFast" perlu menampilkan leaderboard 10.000 merchant berdasarkan volume transaksi, diperbarui setiap 5 menit.

**Percobaan pertama dengan Bubble Sort (O(n²)):**
* n = 10.000 → operasi perbandingan: 10.000² = **100 juta operasi**
* Waktu di server: sekitar 2-3 detik per sorting
* Update setiap 5 menit = 12x per jam = server harus kerja keras setiap 5 menit
* Saat traffic tinggi (peak hour), server mulai lag

**Pelajaran dari kasus ini:**
* Untuk n = 10.000, O(n²) masih terasa lambat di real-time system
* Untuk leaderboard yang terus diperbarui, lebih baik gunakan **sorted data structure** (seperti sorted array atau priority queue) yang diperbarui secara incremental
* Atau gunakan O(n log n) algorithms (Quick/Merge Sort) yang jauh lebih cepat

**Pertanyaan interview umum fintech:**
> "Jika kamu punya 1 juta transaksi yang perlu diurutkan, algoritma sorting mana yang kamu pilih dan kenapa?"

Jawaban yang baik: "Tergantung karakteristik data. Jika hampir terurut, Insertion Sort bisa O(n). Untuk data acak, gunakan Merge Sort atau QuickSort (O(n log n)). Tapi dalam praktik, `Array.prototype.sort()` JavaScript menggunakan Timsort yang hybrid — ini yang paling praktis dipakai."

---

## 📊 Visualisasi

### Perbandingan Ketiga Algoritma

```
n=10:     semua cepat, tidak ada perbedaan berarti
n=1000:   terasa sedikit, Insertion Sort terbaik untuk nearly-sorted
n=10000:  perbedaan mulai terasa (~0.1-0.5 detik)
n=100000: Bubble/Selection mulai lambat (beberapa detik)
```

### Tabel Ringkasan

| Algoritma | Best | Average | Worst | Space | Stable? |
|---|---|---|---|---|---|
| **Bubble Sort** | O(n)* | O(n²) | O(n²) | O(1) | ✅ Ya |
| **Insertion Sort** | **O(n)** | O(n²) | O(n²) | O(1) | ✅ Ya |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) | ❌ Tidak |

*dengan early exit optimization

---

## ⚠️ Kesalahan Umum

1. **Lupa copy array saat benchmark** → `bubbleSort(data)` modifikasi data asli. Benchmark berikutnya tidak akurat. Selalu gunakan `bubbleSort([...data])`.

2. **Mengira Bubble Sort selalu O(n²)** → Dengan optimasi early exit, best case adalah O(n) ketika array sudah terurut.

3. **Menggunakan Selection Sort untuk data nearly-sorted** → Selection Sort selalu O(n²), tidak ada manfaat dari data yang hampir terurut. Gunakan Insertion Sort untuk kasus ini.

4. **Confuse antara "jumlah swap" dan "jumlah comparison"** → Selection Sort melakukan sedikit swap (O(n)), tapi comparison-nya tetap O(n²). Ini berguna jika biaya swap sangat mahal (misal: swap data besar di disk).

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Jelaskan mengapa Insertion Sort lebih efisien daripada Selection Sort untuk data yang hampir terurut. Berikan analisis Big-O untuk kedua kasus tersebut.

b) Kapan kamu akan memilih Selection Sort meskipun kompleksitasnya selalu O(n²)?

c) Apa yang dimaksud dengan algoritma sorting yang "stable"? Mengapa ini penting dalam konteks bisnis (misal: mengurutkan transaksi berdasarkan nominal, yang beberapa di antaranya nominalnya sama)?

### Soal 2 — Coding

1. Implementasikan Bubble Sort yang dimodifikasi untuk mengurutkan array of objects berdasarkan properti tertentu:
   ```javascript
   const nasabah = [
     { nama: 'Andi', saldo: 5000000 },
     { nama: 'Budi', saldo: 1200000 },
     { nama: 'Cici', saldo: 8500000 },
   ];
   // Urutkan berdasarkan saldo (ascending)
   ```

2. Bandingkan waktu eksekusi ketiga algoritma untuk:
   * Array random 5000 elemen
   * Array yang sudah terurut 5000 elemen
   * Array yang terurut terbalik 5000 elemen

   Dokumentasikan hasilnya dalam tabel dan buat kesimpulan.

---

## 📌 Ringkasan

* **Bubble Sort:** bandingkan bersebelahan, tukar jika salah urutan — O(n²), bisa O(n) dengan early exit
* **Insertion Sort:** ambil satu, sisipkan ke posisi tepat — O(n²), terbaik O(n) untuk nearly-sorted data
* **Selection Sort:** cari minimum, taruh di depan — selalu O(n²), sedikit swap
* Ketiganya **in-place** (O(1) space) dan berguna secara konseptual
* Untuk data besar di production, gunakan algoritma O(n log n) (Quick Sort, Merge Sort)
* `Array.prototype.sort()` JavaScript menggunakan **Timsort** — hybrid insertion + merge, O(n log n)
* Pahami Big-O: **Best, Average, Worst case** — ketiganya punya makna berbeda

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Wengrow, J. (2020). A Common-Sense Guide to DSA | Visualgo.net*
