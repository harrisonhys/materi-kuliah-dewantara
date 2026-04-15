# Pertemuan 1: Pengantar Struktur Data & ADT

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Menjelaskan hirarki data dari bit hingga database dan mengapa setiap level itu penting
- Mendefinisikan Abstract Data Type (ADT) dan membedakan antara interface vs implementasi
- Membaca notasi Big-O secara intuitif untuk memahami efisiensi algoritma
- Menyiapkan lingkungan pengembangan (VS Code + Node.js) dan menjalankan program JavaScript pertama
- Menjelaskan kenapa pemilihan struktur data yang tepat berdampak langsung ke performa sistem nyata

---

## 📖 Pengantar (Hook)

Bayangkan kamu kerja di tim backend GoPay. Setiap detik, ada ribuan transaksi masuk — top-up, pembayaran, transfer. Tiba-tiba di jam sibuk (08.00 pagi, semua orang bayar ojek), sistem melambat parah. Notifikasi telat. Transaksi pending menumpuk. User marah-marah di Twitter.

Tim investigasi menemukan masalahnya: **struktur data yang dipakai untuk menyimpan antrian transaksi adalah Array biasa** — setiap kali transaksi baru masuk, sistem harus geser semua elemen di array. Dengan 10.000 transaksi, itu berarti 10.000 operasi geser per transaksi baru. Sistem collapse.

Solusinya? Ganti dengan **Queue** berbasis Linked List — transaksi baru masuk di belakang, transaksi diproses dari depan, tanpa perlu geser apapun. Satu perubahan struktur data, masalah selesai.

**Inilah inti mata kuliah ini:** struktur data bukan teori abstrak — ini adalah senjata yang menentukan apakah sistemmu bisa bertahan di production atau tidak.

---

## 🧩 Konsep Utama

### 1. Hirarki Data

Data punya tingkatan, seperti susunan LEGO dari yang paling kecil:

```
Bit → Byte → Field → Record → File → Database
```

| Level | Definisi | Contoh |
|-------|----------|--------|
| **Bit** | Unit terkecil, nilai 0 atau 1 | `1` |
| **Byte** | 8 bit | `01000001` = karakter 'A' |
| **Field** | Kumpulan byte yang bermakna | `nama: "Budi"` |
| **Record** | Kumpulan field yang merepresentasikan satu entitas | `{ id: 1, nama: "Budi", saldo: 50000 }` |
| **File** | Kumpulan record sejenis | `transaksi.json` — semua data transaksi |
| **Database** | Kumpulan file yang terorganisir dan saling berhubungan | MySQL, PostgreSQL, MongoDB |

**Kenapa penting?** Ketika kamu menyimpan transaksi senilai Rp 1.000.000, data itu dimulai dari bit di memori RAM, dikemas jadi byte, disusun jadi field, menjadi record, disimpan ke file/database. Memahami hirarki ini membantumu mendesain sistem yang efisien.

---

### 2. Abstract Data Type (ADT)

**ADT adalah kontrak.** Ia mendefinisikan *apa* yang bisa dilakukan terhadap data (operasi/interface), tanpa menentukan *bagaimana* caranya (implementasi).

Analogi: ketika kamu pakai Google Maps, kamu tahu bisa input tujuan, lihat rute, mulai navigasi (interface). Kamu tidak perlu tahu algoritma Dijkstra yang berjalan di baliknya (implementasi).

**Komponen ADT:**
- **Domain:** Kumpulan nilai yang valid
- **Operasi:** Fungsi-fungsi yang tersedia
- **Aksioma:** Aturan yang selalu berlaku

**Contoh ADT Stack:**
```
Interface Stack:
  - push(item)     → masukkan item ke tumpukan
  - pop()          → keluarkan item teratas
  - peek()         → lihat item teratas tanpa mengeluarkan
  - isEmpty()      → cek apakah tumpukan kosong
  - size()         → jumlah item di tumpukan

Implementasi bisa pakai: Array, Linked List, dll.
```

---

### 3. Interface vs Implementasi

```
┌─────────────────────────────────────────┐
│              ADT (Interface)            │
│  "Apa yang bisa dilakukan"              │
│  push(), pop(), peek(), isEmpty()       │
├─────────────────────────────────────────┤
│           Implementasi A                │  ← Stack pakai Array
│           Implementasi B                │  ← Stack pakai Linked List
│           Implementasi C                │  ← Stack pakai Object
└─────────────────────────────────────────┘
```

User ADT tidak perlu tahu implementasinya. Kamu bisa ganti implementasi tanpa mengubah kode yang menggunakan ADT tersebut.

---

### 4. Pengantar Kompleksitas Algoritma (Big-O Intuitif)

Big-O adalah cara mengukur **seberapa lambat algoritma bertambah** ketika data bertambah besar. Bukan waktu eksak, tapi *pola pertumbuhan*.

| Notasi | Nama | Analogi |
|--------|------|---------|
| O(1) | Konstan | Ambil barang dari loker — selalu 1 langkah |
| O(log n) | Logaritmik | Cari nama di buku telepon yang sudah tersortir — buka tengah, buang setengah |
| O(n) | Linear | Cari barang di toko yang tidak teratur — lihat satu per satu |
| O(n²) | Kuadratik | Bandingkan semua pasangan teman di kelas |
| O(2ⁿ) | Eksponensial | Coba semua kombinasi password |

**Intuisi praktis:** Kalau data 1.000 item:
- O(1) → 1 operasi (sama aja)
- O(log n) → ~10 operasi (sangat cepat)
- O(n) → 1.000 operasi (masih oke)
- O(n²) → 1.000.000 operasi (mulai berat)
- O(2ⁿ) → angka astronomis (jangan pernah)

---

## 🧠 Ilustrasi / Analogi

### ADT sebagai Mesin ATM

Bayangkan mesin ATM sebagai sebuah ADT:

```
┌──────────────────────────────────────────────┐
│                  ATM (ADT)                   │
│                                              │
│  INTERFACE (yang kamu bisa lakukan):         │
│  ┌────────────────────────────────────────┐  │
│  │  • Masukkan kartu                      │  │
│  │  • Input PIN                           │  │
│  │  • Pilih "Tarik Tunai"                 │  │
│  │  • Input nominal                       │  │
│  │  • Ambil uang + struk                  │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  IMPLEMENTASI (yang terjadi di dalam):       │
│  ┌────────────────────────────────────────┐  │
│  │  • Validasi kartu magnetik/chip        │  │
│  │  • Enkripsi PIN via HSM                │  │
│  │  • Koneksi ke host bank via ISO 8583   │  │
│  │  • Debit rekening di core banking      │  │
│  │  • Aktuasi motor dispenser uang        │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  Kamu pakai interface yang sama              │
│  di ATM BRI, BCA, Mandiri — implementasi     │
│  berbeda, interface sama!                    │
└──────────────────────────────────────────────┘
```

### Big-O: Analogi Warung Makan

| Situasi | Kompleksitas | Penjelasan |
|---------|-------------|-----------|
| Ambil pesanan nomor antrian dari display | O(1) | Langsung lihat, selalu 1 langkah |
| Cari nama di daftar reservasi (buku, terurut) | O(log n) | Buka tengah, cari ke kiri/kanan |
| Panggil nama satu per satu sampai ketemu | O(n) | Harus cek semua kemungkinan |
| Cek apakah ada 2 orang yang pesan menu sama | O(n²) | Bandingkan setiap pasang orang |

---

## 💻 Contoh Teknis

### Setup Lingkungan

**Yang perlu diinstall:**
1. [Node.js](https://nodejs.org) — versi LTS (18.x atau 20.x)
2. [VS Code](https://code.visualstudio.com)
3. Ekstensi VS Code yang disarankan:
   - `ESLint` — deteksi error otomatis
   - `Prettier` — format kode otomatis
   - `Code Runner` — jalankan kode langsung dari VS Code
   - `JavaScript (ES6) code snippets` — snippet berguna

**Cek instalasi Node.js:**
```bash
node --version    # Harus muncul: v18.x.x atau lebih baru
npm --version     # Harus muncul: 9.x.x atau lebih baru
```

---

### Program JavaScript Pertama di Node.js

Buat file `hello.js`:

```javascript
// hello.js
// Program pertama: memahami bahwa semua data adalah representasi informasi

// =============================================
// 1. HIRARKI DATA — dari nilai primitif ke objek kompleks
// =============================================

// Bit & Byte (representasi biner)
const angka = 65;
console.log('Nilai:', angka);
console.log('Representasi biner:', angka.toString(2)); // "1000001" = 65 dalam biner
console.log('Sebagai karakter:', String.fromCharCode(angka)); // 'A'

// Field — nilai bermakna tunggal
const namaUser = 'Budi Santoso';
const saldoAwal = 1500000; // dalam rupiah
const isAktif = true;

// Record — kumpulan field yang merepresentasikan satu entitas
const user = {
  id: 'USR-001',
  nama: namaUser,
  saldo: saldoAwal,
  aktif: isAktif,
  createdAt: new Date().toISOString(),
};

console.log('\n--- Record User ---');
console.log(user);

// File — kumpulan record (simulasi dengan Array)
const daftarUser = [
  { id: 'USR-001', nama: 'Budi Santoso', saldo: 1500000 },
  { id: 'USR-002', nama: 'Siti Rahayu', saldo: 250000 },
  { id: 'USR-003', nama: 'Ahmad Fauzi', saldo: 3200000 },
];

console.log('\n--- "File" Daftar User ---');
console.log(`Total record: ${daftarUser.length}`);
daftarUser.forEach((u, index) => {
  console.log(`[${index}] ${u.id} - ${u.nama} - Rp ${u.saldo.toLocaleString('id-ID')}`);
});

// =============================================
// 2. DEMONSTRASI ADT — Stack sederhana
// =============================================

// Interface Stack didefinisikan via class
// Implementasi menggunakan Array di baliknya
class Stack {
  // Implementasi tersembunyi dari user
  #data = []; // private field (ES2022)

  // Interface publik
  push(item) {
    this.#data.push(item);
    console.log(`  push(${item}) → stack: [${this.#data}]`);
  }

  pop() {
    if (this.isEmpty()) {
      throw new Error('Stack underflow! Stack kosong.');
    }
    const item = this.#data.pop();
    console.log(`  pop() → mengeluarkan: ${item}, stack: [${this.#data}]`);
    return item;
  }

  peek() {
    if (this.isEmpty()) return null;
    return this.#data[this.#data.length - 1];
  }

  isEmpty() {
    return this.#data.length === 0;
  }

  size() {
    return this.#data.length;
  }
}

console.log('\n--- Demo ADT Stack ---');
const tumpukanTugas = new Stack();
tumpukanTugas.push('Kerjakan PR Sorting');
tumpukanTugas.push('Review PR teman');
tumpukanTugas.push('Submit tugas rekursi');

console.log(`Tugas teratas: ${tumpukanTugas.peek()}`);
console.log(`Total tugas: ${tumpukanTugas.size()}`);
tumpukanTugas.pop();
tumpukanTugas.pop();

// =============================================
// 3. ILUSTRASI BIG-O — bandingkan dua fungsi pencarian
// =============================================

// Pencarian Linear: O(n) — cek satu per satu
function cariLinear(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) return i; // ketemu di index i
  }
  return -1; // tidak ketemu
}

// Pencarian Binary: O(log n) — butuh array terurut
function cariBinary(arr, target) {
  let kiri = 0;
  let kanan = arr.length - 1;

  while (kiri <= kanan) {
    const tengah = Math.floor((kiri + kanan) / 2);
    if (arr[tengah] === target) return tengah;
    if (arr[tengah] < target) kiri = tengah + 1;
    else kanan = tengah - 1;
  }
  return -1;
}

// Ukur performa keduanya
const dataUrut = Array.from({ length: 10000 }, (_, i) => i * 2); // [0, 2, 4, ..., 19998]
const target = 9998; // elemen dekat akhir — kasus terburuk untuk linear

console.log('\n--- Perbandingan Big-O ---');

const mulaiLinear = performance.now();
const hasilLinear = cariLinear(dataUrut, target);
const selesaiLinear = performance.now();

const mulaiBinary = performance.now();
const hasilBinary = cariBinary(dataUrut, target);
const selesaiBinary = performance.now();

console.log(`Linear Search O(n): ketemu di index ${hasilLinear}, waktu: ${(selesaiLinear - mulaiLinear).toFixed(4)}ms`);
console.log(`Binary Search O(log n): ketemu di index ${hasilBinary}, waktu: ${(selesaiBinary - mulaiBinary).toFixed(4)}ms`);
console.log(`\nDengan 10.000 data:`);
console.log(`  Linear mungkin cek hingga: 10.000 elemen`);
console.log(`  Binary cek maksimal: ~${Math.ceil(Math.log2(10000))} elemen`);
```

**Jalankan:**
```bash
node hello.js
```

**Output yang diharapkan:**
```
Nilai: 65
Representasi biner: 1000001
Sebagai karakter: A

--- Record User ---
{ id: 'USR-001', nama: 'Budi Santoso', saldo: 1500000, aktif: true, ... }

--- "File" Daftar User ---
Total record: 3
[0] USR-001 - Budi Santoso - Rp 1.500.000
[1] USR-002 - Siti Rahayu - Rp 250.000
[2] USR-003 - Ahmad Fauzi - Rp 3.200.000

--- Demo ADT Stack ---
  push(Kerjakan PR Sorting) → stack: [Kerjakan PR Sorting]
  push(Review PR teman) → stack: [Kerjakan PR Sorting,Review PR teman]
  push(Submit tugas rekursi) → stack: [...]
Tugas teratas: Submit tugas rekursi
Total tugas: 3
  pop() → mengeluarkan: Submit tugas rekursi, ...
  pop() → mengeluarkan: Review PR teman, ...

--- Perbandingan Big-O ---
Linear Search O(n): ketemu di index 4999, waktu: 0.2xxx ms
Binary Search O(log n): ketemu di index 4999, waktu: 0.0xxx ms
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Antrian Transaksi GoPay yang Collapse

**Konteks sistem:**
GoPay memproses ~1.500 transaksi per detik pada jam puncak. Setiap transaksi perlu masuk antrian, divalidasi, lalu dieksekusi secara berurutan (FIFO — First In First Out).

**Masalah:**
Tim awal mengimplementasikan antrian transaksi dengan Array JavaScript biasa:

```javascript
// IMPLEMENTASI BURUK — jangan lakukan ini di production!
const antrianTransaksi = [];

// Transaksi baru masuk — tambahkan ke DEPAN array (seperti antrian nyata)
function tambahTransaksi(transaksi) {
  antrianTransaksi.unshift(transaksi); // O(n) — geser semua elemen!
}

// Proses transaksi — ambil dari BELAKANG
function prosesTransaksi() {
  return antrianTransaksi.pop(); // O(1)
}
```

**Dampak:**
- `unshift()` pada Array adalah operasi **O(n)** — harus geser SEMUA elemen ke kanan
- Dengan 10.000 transaksi pending, setiap transaksi baru butuh 10.000 operasi geser
- Di jam puncak: 1.500 transaksi/detik × 10.000 geser = **15 juta operasi geser per detik**
- CPU spike ke 100%, response time melonjak dari 50ms ke 8 detik, transaksi timeout

**Solusi — ADT Queue dengan implementasi yang benar:**

```javascript
// IMPLEMENTASI BENAR — Queue dengan dua pointer
class AntrianTransaksi {
  #data = {};       // Pakai Object, bukan Array
  #head = 0;        // Pointer ke elemen terdepan
  #tail = 0;        // Pointer ke posisi kosong berikutnya

  // Masukkan transaksi baru — O(1)
  enqueue(transaksi) {
    this.#data[this.#tail] = transaksi;
    this.#tail++;
  }

  // Proses transaksi terdepan — O(1)
  dequeue() {
    if (this.isEmpty()) throw new Error('Antrian kosong!');
    const transaksi = this.#data[this.#head];
    delete this.#data[this.#head]; // bebaskan memori
    this.#head++;
    return transaksi;
  }

  peek() {
    return this.#data[this.#head];
  }

  isEmpty() {
    return this.#head === this.#tail;
  }

  size() {
    return this.#tail - this.#head;
  }
}

// Simulasi produksi
const antrian = new AntrianTransaksi();

// Masukkan 5 transaksi
antrian.enqueue({ id: 'TXN-001', nominal: 50000, type: 'transfer' });
antrian.enqueue({ id: 'TXN-002', nominal: 150000, type: 'topup' });
antrian.enqueue({ id: 'TXN-003', nominal: 25000, type: 'payment' });

console.log(`Antrian: ${antrian.size()} transaksi`);
console.log('Proses:', antrian.dequeue()); // TXN-001 diproses pertama
console.log('Proses:', antrian.dequeue()); // TXN-002
console.log(`Sisa antrian: ${antrian.size()}`);
```

**Hasil:**
- `enqueue()` dan `dequeue()` keduanya O(1) — konstan, tidak peduli berapa banyak transaksi
- Throughput naik dari 150 TPS (transactions per second) ke 1.500+ TPS
- Response time kembali normal: 45ms rata-rata

**Pelajaran:** Memilih ADT yang tepat (Queue) dan implementasi yang benar (Object pointer vs Array unshift) mengubah sistem dari kolaps menjadi production-ready. Ini bukan optimasi kecil — ini perbedaan antara sistem yang hidup dan mati.

---

## 📊 Visualisasi

### Hirarki Data — dari Bit ke Database

```
BIT
 │  0 atau 1
 ▼
BYTE (8 bit)
 │  01000010 = 'B'
 ▼
FIELD (kumpulan byte bermakna)
 │  nama = "Budi"
 │  saldo = 1500000
 ▼
RECORD (kumpulan field = satu entitas)
 │  { id: "USR-001", nama: "Budi", saldo: 1500000 }
 ▼
FILE (kumpulan record sejenis)
 │  users.json → [record1, record2, record3, ...]
 ▼
DATABASE (kumpulan file terorganisir)
    PostgreSQL: tabel users, tabel transaksi, tabel merchants
    MongoDB: koleksi users, koleksi transactions
```

### Visualisasi Big-O Growth

```
Jumlah
Operasi
  │
  │                                              ◄ O(2ⁿ) Eksponensial
1M│                                   *
  │                              *
  │                         *
100K│                    *
  │               *
10K │          *          ◄ O(n²) Kuadratik
  │       *      *  *  *  *  *  *  *
1K │   *    * * *
  │  *  * *              ◄ O(n) Linear: * * * * *
100│ * *
  │**                    ◄ O(log n): ** *
 10│*                    ◄ O(1): ─────────────────
  └──────────────────────────────────────────────►
  1  10  100  1K   10K  100K  1M         Ukuran Data (n)
```

### ADT Stack — LIFO (Last In, First Out)

```
Operasi: push('A'), push('B'), push('C'), pop()

  push('A')    push('B')    push('C')    pop()
  ┌───────┐    ┌───────┐    ┌───────┐    ┌───────┐
  │       │    │       │    │  'C'  │◄───│       │ ← 'C' keluar
  ├───────┤    ├───────┤    ├───────┤    ├───────┤
  │       │    │  'B'  │    │  'B'  │    │  'B'  │
  ├───────┤    ├───────┤    ├───────┤    ├───────┤
  │  'A'  │    │  'A'  │    │  'A'  │    │  'A'  │
  └───────┘    └───────┘    └───────┘    └───────┘
   TOP='A'      TOP='B'      TOP='C'      TOP='B'
```

---

## ⚠️ Kesalahan Umum

### 1. Menyamakan ADT dengan Implementasi
```javascript
// SALAH — "Saya pakai Array, jadi ini bukan Stack"
// BENAR — Stack adalah ADT. Implementasinya BISA pakai Array.
// Yang membuat sesuatu jadi Stack adalah BEHAVIOR-nya (LIFO),
// bukan struktur internalnya.

const stack1 = []; // Stack yang diimplementasikan dengan Array — valid!
stack1.push('A');
stack1.push('B');
stack1.pop(); // 'B' keluar — ini Stack yang sah
```

### 2. Bingung O(n) dengan Waktu Eksak
```javascript
// SALAH — "O(n) berarti n milidetik"
// BENAR — O(n) berarti waktu BERTUMBUH SEIRING n
// Di mesin yang lambat, O(1) bisa lebih lama dari O(n) untuk n kecil!
// Big-O bicara tentang PERTUMBUHAN, bukan nilai absolut.

// Contoh: ini O(1) tapi mungkin butuh 1 detik karena I/O
function ambilKonfigurasi() {
  // Baca dari database — lambat, tapi kompleksitasnya O(1)
  return database.query('SELECT * FROM config WHERE id = 1');
}
```

### 3. Salah Memilih Struktur Data karena Tidak Tahu Alternatifnya
```javascript
// MASALAH: perlu sering tambah elemen di TENGAH array
const arr = [1, 2, 3, 4, 5];
arr.splice(2, 0, 99); // O(n) — geser semua elemen setelah index 2

// SOLUSI LEBIH BAIK untuk operasi insert di tengah yang sering:
// Gunakan Linked List — insert di tengah bisa O(1) jika punya pointer ke node-nya
// (akan dipelajari di pertemuan berikutnya)
```

### 4. Lupa bahwa `const` pada Object/Array tidak berarti immutable
```javascript
// Ini TIDAK error — object bisa dimodifikasi meski const
const user = { nama: 'Budi', saldo: 0 };
user.saldo = 1000000; // OK!
user.nama = 'Siti';   // OK!

// Yang tidak bisa: reassign variabelnya
// user = {}; // ERROR: Assignment to constant variable
```

---

## 🧪 Latihan / Studi Kasus

### Soal Konsep

**1.** Jelaskan perbedaan antara ADT dan struktur data dengan analogi yang kamu pilih sendiri (bukan dari materi ini). Berikan contoh nyata di aplikasi yang sering kamu pakai.

**2.** Sebuah aplikasi e-commerce menyimpan riwayat pencarian user (untuk fitur "baru saja kamu lihat"). Operasi yang sering dilakukan:
- Tambah pencarian baru ke daftar (sering)
- Tampilkan 5 pencarian terbaru (sering)
- Hapus pencarian terlama saat daftar penuh (sering)

Berdasarkan pola operasi di atas, ADT apa yang paling cocok? Jelaskan alasanmu.

---

### Soal Coding

**Implementasikan ADT Queue sederhana** menggunakan JavaScript dengan spesifikasi berikut:

```javascript
// Lengkapi implementasi Queue ini:
class Queue {
  // TODO: inisialisasi penyimpanan internal

  // Tambahkan elemen ke belakang antrian
  enqueue(item) {
    // TODO
  }

  // Keluarkan elemen dari depan antrian
  dequeue() {
    // TODO: throw Error jika antrian kosong
  }

  // Lihat elemen terdepan tanpa mengeluarkan
  front() {
    // TODO
  }

  isEmpty() {
    // TODO
  }

  size() {
    // TODO
  }

  // Tampilkan antrian sebagai string: "front → [A, B, C] ← rear"
  toString() {
    // TODO
  }
}

// Test case yang harus lulus:
const q = new Queue();
q.enqueue('TXN-001');
q.enqueue('TXN-002');
q.enqueue('TXN-003');
console.log(q.toString()); // front → [TXN-001, TXN-002, TXN-003] ← rear
console.log(q.size());     // 3
console.log(q.front());    // TXN-001
q.dequeue();
console.log(q.toString()); // front → [TXN-002, TXN-003] ← rear
console.log(q.size());     // 2
```

**Bonus:** Ukur perbedaan performa antara implementasi Queue kamu vs menggunakan `Array.unshift()` untuk enqueue, dengan 100.000 operasi. Gunakan `performance.now()`.

---

## 📌 Ringkasan

- **Hirarki data:** Bit → Byte → Field → Record → File → Database — setiap level membangun yang di atasnya
- **ADT** adalah kontrak antara pengguna dan implementor: mendefinisikan *apa* (interface), bukan *bagaimana* (implementasi)
- **Interface vs Implementasi:** Stack bisa diimplementasikan dengan Array, Linked List, dll — yang penting behavior LIFO-nya
- **Big-O** mengukur *pola pertumbuhan* kompleksitas, bukan waktu absolut
- **Ranking efisiensi:** O(1) > O(log n) > O(n) > O(n log n) > O(n²) > O(2ⁿ)
- **Pemilihan struktur data yang tepat** bisa membedakan sistem yang survive di production vs yang collapse
- **Setup:** Node.js + VS Code + ESLint adalah combo standar untuk JavaScript development
- **Kunci:** Sebelum code, selalu tanya — "Operasi apa yang paling sering? Apa kompleksitas idealnya?"
