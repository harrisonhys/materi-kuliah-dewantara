# Pertemuan 2: Array & Object JavaScript (Pengganti Struct)

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Mendeklarasikan dan memanipulasi Array 1D, 2D, dan multidimensi di JavaScript
* Menggunakan Object JS sebagai pengganti Struct/Record untuk merepresentasikan data terstruktur
* Membuat dan memanipulasi Array of Objects untuk koleksi data bertipe sama
* Menggunakan metode array penting (push, pop, map, filter, reduce, splice, slice)
* Menggunakan destructuring dan spread operator untuk manipulasi data modern

---

## 📖 Pengantar (Hook)

Bayangkan kamu kerja di backend GoPay. Setiap hari, sistem memproses 10 juta transaksi.

Setiap transaksi punya data: ID, pengirim, penerima, nominal, timestamp, status. Kalau kamu simpan setiap data ini sebagai variabel terpisah — `id1, id2, id3...` — kamu butuh 10 juta variabel hanya untuk satu hari!

Tentu saja tidak ada yang melakukan itu.

Di dunia nyata, data diorganisir dalam **Array** (koleksi banyak item) dan **Object** (data terstruktur dengan atribut). Dua struktur ini adalah tulang punggung hampir setiap program yang pernah kamu gunakan.

---

## 🧩 Konsep Utama

### Array di JavaScript

Array adalah koleksi data yang tersimpan secara berurutan, diakses lewat **indeks** (mulai dari 0).

```javascript
// Deklarasi array
const buah = ['apel', 'mangga', 'jeruk'];

// Akses elemen (indeks dimulai dari 0)
console.log(buah[0]); // 'apel'
console.log(buah[2]); // 'jeruk'

// Panjang array
console.log(buah.length); // 3
```

**Array 2D (Matriks):**
```javascript
const matriks = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

// Akses elemen matriks[baris][kolom]
console.log(matriks[1][2]); // 6 (baris 1, kolom 2)
```

**Metode Array Penting:**

| Metode | Fungsi | Return |
|---|---|---|
| `push(item)` | Tambah di akhir | Panjang baru |
| `pop()` | Hapus dari akhir | Elemen yang dihapus |
| `shift()` | Hapus dari awal | Elemen yang dihapus |
| `unshift(item)` | Tambah di awal | Panjang baru |
| `splice(i, n)` | Hapus n elemen mulai index i | Array elemen terhapus |
| `slice(i, j)` | Ambil sub-array [i, j) | Array baru |
| `indexOf(item)` | Cari posisi elemen | Indeks (-1 jika tidak ada) |
| `includes(item)` | Cek keberadaan elemen | true/false |
| `map(fn)` | Transform setiap elemen | Array baru |
| `filter(fn)` | Filter elemen yang lolos kondisi | Array baru |
| `reduce(fn, init)` | Akumulasikan nilai | Satu nilai |
| `sort(fn)` | Urutkan (modifikasi langsung!) | Array yang sama |
| `find(fn)` | Cari elemen pertama yang cocok | Elemen / undefined |

### Object di JavaScript (Pengganti Struct)

Object adalah kumpulan pasangan **key-value** yang merepresentasikan satu entitas terstruktur.

```javascript
// Deklarasi object (seperti Struct di C/C++)
const transaksi = {
  id: 'TRX-001',
  pengirim: 'Andi',
  penerima: 'Budi',
  nominal: 500000,
  timestamp: '2024-01-15 10:30:00',
  status: 'success'
};

// Akses properti
console.log(transaksi.id);        // 'TRX-001'
console.log(transaksi['nominal']); // 500000

// Modifikasi properti
transaksi.status = 'pending';

// Tambah properti baru
transaksi.fee = 2500;

// Cek properti ada atau tidak
console.log('id' in transaksi); // true
```

### Array of Objects

Ini adalah pattern paling umum di dunia nyata — koleksi banyak entitas bertipe sama:

```javascript
const riwayatTransaksi = [
  { id: 'TRX-001', nominal: 500000, status: 'success' },
  { id: 'TRX-002', nominal: 150000, status: 'failed' },
  { id: 'TRX-003', nominal: 200000, status: 'success' },
];

// Filter hanya transaksi sukses
const sukses = riwayatTransaksi.filter(trx => trx.status === 'success');

// Total nominal semua transaksi sukses
const total = sukses.reduce((acc, trx) => acc + trx.nominal, 0);
console.log(total); // 700000
```

---

## 🧠 Ilustrasi / Analogi

| Konsep JS | Analogi Dunia Nyata |
|---|---|
| **Array** | Rak buku bernomor — buku di posisi 1, 2, 3... |
| **Object** | Formulir data nasabah — setiap field punya nama |
| **Array of Objects** | File kabinet berisi ratusan formulir nasabah |
| **Indeks Array** | Nomor laci di rak |
| **Key Object** | Label di setiap kolom formulir |
| **map()** | Fotokopi semua formulir dengan satu perubahan |
| **filter()** | Ambil hanya formulir nasabah dengan saldo > 1 juta |
| **reduce()** | Hitung total saldo dari semua formulir |

---

## 💻 Contoh Teknis

### Pipeline Data Transaksi ala Backend Fintech

```javascript
// Simulasi data transaksi harian
const transaksiHarian = [
  { id: 'T001', user: 'Andi', nominal: 500000, type: 'transfer', status: 'success' },
  { id: 'T002', user: 'Budi', nominal: 75000,  type: 'topup',    status: 'success' },
  { id: 'T003', user: 'Cici', nominal: 1200000, type: 'transfer', status: 'failed' },
  { id: 'T004', user: 'Dodi', nominal: 250000, type: 'payment',  status: 'success' },
  { id: 'T005', user: 'Evi',  nominal: 800000, type: 'transfer', status: 'success' },
];

// 1. Filter hanya transaksi sukses
const sukses = transaksiHarian.filter(t => t.status === 'success');
console.log('Transaksi sukses:', sukses.length); // 4

// 2. Filter transaksi besar (> 500rb) yang perlu diverifikasi
const besar = transaksiHarian.filter(t => t.nominal > 500000);
console.log('Transaksi besar:', besar.map(t => t.id)); // ['T003', 'T005']

// 3. Hitung total volume transaksi sukses
const totalVolume = sukses.reduce((acc, t) => acc + t.nominal, 0);
console.log('Total volume:', totalVolume.toLocaleString('id-ID')); // 1.625.000

// 4. Tambahkan fee (biaya admin 0.5%) ke setiap transaksi transfer
const denganFee = transaksiHarian.map(t => ({
  ...t,
  fee: t.type === 'transfer' ? t.nominal * 0.005 : 0
}));

// 5. Cari transaksi dengan ID tertentu
const cari = transaksiHarian.find(t => t.id === 'T003');
console.log('Ditemukan:', cari?.user); // Cici

// 6. Urutkan berdasarkan nominal terbesar
const terurut = [...transaksiHarian].sort((a, b) => b.nominal - a.nominal);
console.log('Terbesar:', terurut[0].id, '-', terurut[0].nominal); // T003 - 1200000
```

### Destructuring & Spread Operator

```javascript
// Array destructuring
const [pertama, kedua, ...sisanya] = transaksiHarian;
console.log(pertama.id); // T001

// Object destructuring
const { id, user, nominal } = transaksiHarian[0];
console.log(id, user, nominal); // T001 Andi 500000

// Spread operator — copy array tanpa mutasi
const salinan = [...transaksiHarian];
salinan.push({ id: 'T006', user: 'Fani', nominal: 300000, type: 'topup', status: 'success' });

console.log(transaksiHarian.length); // 5 (tidak berubah!)
console.log(salinan.length); // 6
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Pipeline Data Processing: Laporan Harian GoPay

**Skenario:** Setiap tengah malam, sistem GoPay menjalankan job otomatis untuk menghasilkan laporan transaksi harian. Job ini harus:

1. Memuat 500.000+ record transaksi dari database ke memory
2. Filter transaksi yang perlu eskalasi (nominal > Rp 50 juta)
3. Hitung total volume per jenis transaksi
4. Flag transaksi yang gagal untuk retry
5. Hasilkan summary report

**Masalah jika tanpa struktur data yang baik:**
* Data tersebar di variabel terpisah → mustahil dikelola
* Tidak ada cara efisien untuk filter, sort, dan aggregate
* Kode menjadi sangat panjang dan tidak maintainable

**Solusi dengan Array of Objects + metode fungsional:**
```javascript
// Contoh sederhana pipeline laporan
function generateLaporan(transaksi) {
  const sukses = transaksi.filter(t => t.status === 'success');
  const gagal  = transaksi.filter(t => t.status === 'failed');
  const eskalasi = transaksi.filter(t => t.nominal > 50_000_000);

  const totalVolume = sukses.reduce((sum, t) => sum + t.nominal, 0);

  return {
    tanggal: new Date().toISOString().split('T')[0],
    totalTransaksi: transaksi.length,
    transaksiSukses: sukses.length,
    transaksGagal: gagal.length,
    perluEskalasi: eskalasi.length,
    totalVolume,
  };
}
```

**Dampak bisnis:** Job yang dulu memakan 3 jam bisa selesai dalam 8 menit dengan pendekatan fungsional yang efisien.

---

## 📊 Visualisasi

### Pemetaan Array ke Memori

```
Index:  [  0  ][  1  ][  2  ][  3  ][  4  ]
Data:   ['T01']['T02']['T03']['T04']['T05']
Alamat: [1000] [1008] [1016] [1024] [1032]
        ↑ setiap elemen bersebelahan di memori
```

### Kompleksitas Operasi Array

| Operasi | Kompleksitas | Keterangan |
|---|---|---|
| Akses elemen (index) | O(1) | Langsung via alamat memori |
| Pencarian (linear) | O(n) | Harus cek satu per satu |
| Insert di akhir (push) | O(1) amortized | Tambah di belakang |
| Insert di awal (unshift) | O(n) | Semua elemen geser kanan |
| Delete di awal (shift) | O(n) | Semua elemen geser kiri |
| Delete di akhir (pop) | O(1) | Hapus elemen terakhir |

---

## ⚠️ Kesalahan Umum

1. **Mengira array JS fixed size seperti di C/C++**
   → Array JS bersifat dinamis — bisa tumbuh dan menyusut kapan saja. `push()` dan `pop()` tidak butuh ukuran awal.

2. **Mutasi array saat iterasi dengan `forEach`**
   → Jangan modifikasi array yang sedang di-loop. Gunakan `map()` untuk buat array baru.

3. **Lupa bahwa `sort()` memodifikasi array asli**
   → `arr.sort()` mengubah arr itu sendiri! Jika ingin mempertahankan urutan asli, sort salinan: `[...arr].sort()`.

4. **Membandingkan object dengan `===`**
   → `{a:1} === {a:1}` hasilnya `false` — object dibandingkan by reference, bukan by value. Gunakan `JSON.stringify()` atau bandingkan properti satu per satu.

5. **Shallow copy vs deep copy**
   → `const b = [...a]` hanya shallow copy. Jika array berisi object, object di dalamnya masih shared. Gunakan `JSON.parse(JSON.stringify(a))` untuk deep copy.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Apa perbedaan antara `splice()` dan `slice()`? Berikan contoh penggunaan masing-masing.

b) Jelaskan apa yang dimaksud dengan "array is passed by reference" di JavaScript. Apa implikasinya ketika kamu melempar array ke dalam fungsi?

c) Kapan kamu akan menggunakan `reduce()` daripada `forEach()` atau `for` loop biasa?

### Soal 2 — Coding

Diberikan array data mahasiswa berikut:

```javascript
const mahasiswa = [
  { nama: 'Andi', nilai: 85, jurusan: 'STI' },
  { nama: 'Budi', nilai: 62, jurusan: 'TI' },
  { nama: 'Cici', nilai: 91, jurusan: 'STI' },
  { nama: 'Dodi', nilai: 74, jurusan: 'TI' },
  { nama: 'Evi',  nilai: 88, jurusan: 'STI' },
];
```

Buat program JavaScript yang:
1. Filter mahasiswa STI saja
2. Tambahkan properti `grade` ke setiap mahasiswa (A: ≥80, B: ≥70, C: ≥60, D: <60)
3. Hitung rata-rata nilai mahasiswa STI
4. Tampilkan nama mahasiswa dengan nilai tertinggi
5. Urutkan semua mahasiswa berdasarkan nilai (tertinggi ke terendah)

---

## 📌 Ringkasan

* **Array** = koleksi data terurut, diakses via indeks (mulai 0), dinamis di JS
* **Object** = pasangan key-value, pengganti Struct, akses via `obj.key` atau `obj['key']`
* **Array of Objects** = pattern paling umum untuk koleksi data terstruktur
* **Metode wajib tahu:** push/pop, map, filter, reduce, sort, find, splice, slice
* **map()** = transform, **filter()** = seleksi, **reduce()** = akumulasi
* **sort()** memodifikasi array asli — gunakan `[...arr].sort()` untuk tidak merusak aslinya
* **Destructuring** = cara ringkas mengekstrak nilai dari array/object
* **Spread operator (...)** = copy array/object atau gabungkan keduanya
* Kompleksitas akses array: O(1) via index, O(n) via pencarian linear

---

*📚 Referensi: MDN Web Docs — Array | Bhargava, A.Y. (2016). Grokking Algorithms | javascript.info*
