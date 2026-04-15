# Pertemuan 3: Rekursi — Direct, Indirect & Tail Recursion

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Membedakan rekursi langsung (direct), tidak langsung (indirect), dan infinite recursion
* Mengidentifikasi base case dan recursive case dalam sebuah fungsi rekursif
* Mengimplementasikan rekursi untuk faktorial, Fibonacci, dan Tower of Hanoi di JavaScript
* Membandingkan pendekatan rekursif vs iteratif dari sisi keterbacaan dan performa
* Memahami konsep tail recursion sebagai optimasi rekursi

---

## 📖 Pengantar (Hook)

Pernah buka kamus dan mencari definisi kata, lalu definisi itu menggunakan kata lain yang kamu tidak tahu, lalu kamu cari kata itu, dan definisinya menggunakan kata awal yang tadi kamu cari?

Itu adalah **rekursi dalam kehidupan nyata** — sesuatu yang mendefinisikan dirinya sendiri.

Di pemrograman, rekursi adalah teknik di mana sebuah fungsi memanggil **dirinya sendiri** untuk menyelesaikan masalah. Dan ternyata, banyak masalah di dunia — dari penjelajahan folder komputer hingga algoritma AI — paling elegan diselesaikan dengan rekursi.

---

## 🧩 Konsep Utama

### Struktur Fungsi Rekursif

Setiap fungsi rekursif yang benar HARUS punya dua bagian:

1. **Base Case** — kondisi berhenti. Tanpa ini, rekursi berjalan selamanya → Stack Overflow!
2. **Recursive Case** — fungsi memanggil dirinya sendiri dengan input yang **lebih kecil/sederhana**

```javascript
function rekursi(n) {
  // BASE CASE — WAJIB ADA
  if (n === 0) return 'selesai';
  
  // RECURSIVE CASE — panggil diri sendiri dengan n yang lebih kecil
  return rekursi(n - 1);
}
```

### Jenis Rekursi

**1. Direct Recursion** — fungsi memanggil dirinya sendiri langsung
```javascript
function faktorial(n) {
  if (n === 0) return 1;          // base case
  return n * faktorial(n - 1);   // recursive case
}
```

**2. Indirect Recursion** — fungsi A memanggil fungsi B, fungsi B memanggil A
```javascript
function isEven(n) {
  if (n === 0) return true;
  return isOdd(n - 1);
}

function isOdd(n) {
  if (n === 0) return false;
  return isEven(n - 1);
}
```

**3. Infinite Recursion** — tidak ada base case → Stack Overflow!
```javascript
// ❌ JANGAN LAKUKAN INI
function selamanya(n) {
  return selamanya(n - 1); // tidak ada base case!
}
```

### Call Stack dan Rekursi

Setiap kali fungsi dipanggil, JavaScript menyimpan informasinya di **Call Stack**. Saat rekursi berjalan:

```
faktorial(4)
  faktorial(3)
    faktorial(2)
      faktorial(1)
        faktorial(0) → return 1
      return 1 * 1 = 1
    return 2 * 1 = 2
  return 3 * 2 = 6
return 4 * 6 = 24
```

Stack tumbuh ke atas, lalu "collapse" saat base case tercapai. Jika stack terlalu dalam → **Stack Overflow Error!**

---

## 🧠 Ilustrasi / Analogi

**Rekursi itu seperti cermin yang saling berhadapan:**

Ketika dua cermin saling berhadapan, kamu melihat refleksi dalam refleksi dalam refleksi... sampai tak terhingga (infinite recursion).

Tapi jika ada batas (base case) — seperti tembok — refleksi berhenti pada titik tertentu.

**Analogi lain — Pembuka Kotak Hadiah:**
* Buka kotak → ada kotak lagi di dalamnya (recursive case)
* Buka kotak → ada hadiah (bukan kotak) → selesai! (base case)
* Tiap level kotak = satu level call stack

| Konsep Rekursi | Analogi |
|---|---|
| Base case | Hadiah di kotak terdalam |
| Recursive case | Kotak di dalam kotak |
| Stack overflow | Kotak tanpa akhir |
| Return value | Kamu membawa hadiah keluar sambil tutup setiap kotak |

---

## 💻 Contoh Teknis

### 1. Faktorial

```javascript
// Rekursif
function faktorialRekursif(n) {
  if (n === 0 || n === 1) return 1;  // base case
  return n * faktorialRekursif(n - 1);
}

// Iteratif (perbandingan)
function faktorialIteratif(n) {
  let hasil = 1;
  for (let i = 2; i <= n; i++) {
    hasil *= i;
  }
  return hasil;
}

console.log(faktorialRekursif(5)); // 120
console.log(faktorialIteratif(5)); // 120
```

**Analisis Big-O:** O(n) — waktu, O(n) — ruang (call stack)

### 2. Fibonacci

```javascript
// Naive rekursif — O(2^n) — SANGAT LAMBAT untuk n besar!
function fibNaive(n) {
  if (n <= 1) return n;
  return fibNaive(n - 1) + fibNaive(n - 2);
}

// Memoized — O(n)
function fibMemo(n, memo = {}) {
  if (n in memo) return memo[n];      // cek cache dulu
  if (n <= 1) return n;
  memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
  return memo[n];
}

console.time('naive'); fibNaive(35); console.timeEnd('naive');   // lambat
console.time('memo');  fibMemo(35);  console.timeEnd('memo');    // cepat
```

### 3. Tower of Hanoi

```javascript
// Pindahkan n disk dari 'dari' ke 'ke' via 'bantu'
function hanoi(n, dari, ke, bantu) {
  if (n === 1) {
    console.log(`Pindah disk 1: ${dari} → ${ke}`);
    return;
  }
  hanoi(n - 1, dari, bantu, ke);    // Pindah n-1 disk ke bantu
  console.log(`Pindah disk ${n}: ${dari} → ${ke}`);
  hanoi(n - 1, bantu, ke, dari);   // Pindah n-1 disk dari bantu ke tujuan
}

hanoi(3, 'A', 'C', 'B');
// Output: 7 langkah untuk 3 disk (2^n - 1 langkah)
```

### 4. Tail Recursion (Optimasi)

```javascript
// Rekursi biasa — tidak bisa di-optimize
function faktorialBiasa(n) {
  if (n === 0) return 1;
  return n * faktorialBiasa(n - 1); // ada operasi SETELAH recursive call
}

// Tail recursion — recursive call adalah operasi TERAKHIR
function faktorialTail(n, akumulator = 1) {
  if (n === 0) return akumulator;
  return faktorialTail(n - 1, n * akumulator); // ini bisa di-optimize oleh engine
}

console.log(faktorialTail(5)); // 120
```

Tail recursion bisa di-optimize menjadi loop oleh JavaScript engine (TCO - Tail Call Optimization), menghemat penggunaan stack.

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Rekursi untuk Memproses Nested JSON dari API

**Skenario:** API transaksi fintech sering mengembalikan data dalam format nested JSON — transaksi induk yang punya sub-transaksi, yang masing-masing bisa punya sub-sub-transaksi (seperti cicilan yang punya breakdown biaya).

```javascript
const transaksiNested = {
  id: 'TRX-001',
  nominal: 1500000,
  detail: {
    pokok: 1000000,
    bunga: {
      perbulan: 50000,
      total: 500000,
      breakdown: {
        bulan1: 50000,
        bulan2: 50000,
        // ...
      }
    }
  }
};

// Rekursi untuk menghitung total semua nilai numerik dalam nested object
function hitungTotal(obj) {
  if (typeof obj === 'number') return obj;  // base case
  if (typeof obj !== 'object') return 0;    // base case (string, dll)
  
  // recursive case — jumlahkan semua nilai dalam object
  return Object.values(obj).reduce((sum, val) => sum + hitungTotal(val), 0);
}
```

**Mengapa rekursi?**
Karena kita tidak tahu seberapa dalam level nesting-nya. Dengan rekursi, kita tidak perlu tahu — fungsi akan berjalan sampai menemukan base case (nilai numerik) di level manapun.

**Dampak:** Kode yang sama bisa memproses struktur JSON 2 level dalam atau 10 level dalam — tanpa perubahan.

---

## 📊 Visualisasi

### Rekursi vs Iteratif — Kapan Pilih Mana?

| Aspek | Rekursif | Iteratif |
|---|---|---|
| **Keterbacaan** | Lebih elegan untuk masalah yang rekursif alami | Lebih straightforward untuk loop sederhana |
| **Performa** | Overhead call stack | Biasanya lebih cepat |
| **Risiko** | Stack overflow untuk input besar | Tidak ada risiko stack overflow |
| **Debugging** | Lebih susah di-trace | Lebih mudah |
| **Cocok untuk** | Tree/Graph traversal, divide & conquer | Loop sederhana, agregasi |

### Fibonacci Naive: Kenapa Lambat?

```
fib(5)
├── fib(4)
│   ├── fib(3)
│   │   ├── fib(2)  ← HITUNG 2 KALI
│   │   └── fib(1)
│   └── fib(2)      ← HITUNG 2 KALI (LAGI!)
│       ├── fib(1)
│       └── fib(0)
└── fib(3)          ← HITUNG 2 KALI (LAGI!)
    ├── fib(2)
    └── fib(1)
```

Memoization menyimpan hasil fib(n) agar tidak dihitung ulang → dari O(2^n) ke O(n).

---

## ⚠️ Kesalahan Umum

1. **Lupa base case** → Program crash dengan "Maximum call stack size exceeded"

2. **Base case yang salah** → Rekursi berjalan lebih atau kurang dari yang diinginkan

3. **Fibonacci naive untuk n besar** → fib(50) bisa memakan waktu beberapa menit tanpa memoization

4. **Tidak memahami bahwa rekursi menggunakan memori stack** → Untuk n = 100.000, rekursi bisa stack overflow. Konversi ke iteratif atau gunakan tail recursion.

5. **Mengira rekursi selalu lebih lambat** → Dengan memoization, rekursi bisa sama cepat atau lebih cepat dari iteratif untuk masalah tertentu.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Apa yang dimaksud dengan "base case" dalam rekursi? Apa yang terjadi jika base case tidak ada atau salah?

b) Jelaskan perbedaan antara rekursi dan iterasi. Kapan kamu lebih memilih rekursi?

c) Mengapa Fibonacci rekursif naif memiliki kompleksitas O(2^n)? Bagaimana memoization mengatasinya?

### Soal 2 — Coding

Implementasikan fungsi-fungsi berikut secara rekursif di JavaScript:

1. `sumArray(arr)` — hitung jumlah semua elemen dalam array secara rekursif
2. `countDown(n)` — cetak angka dari n sampai 1, lalu cetak "LAUNCH!"
3. `pangkat(base, exp)` — hitung base^exp tanpa menggunakan operator `**`
4. `isPalindrome(str)` — cek apakah string adalah palindrom (rekursif)

Untuk soal 1 dan 3, bandingkan versi rekursif dan iteratif menggunakan `console.time()`.

---

## 📌 Ringkasan

* **Rekursi** = fungsi yang memanggil dirinya sendiri untuk menyelesaikan sub-masalah
* **WAJIB ada:** Base Case (kondisi berhenti) + Recursive Case (panggil diri sendiri)
* **Direct recursion** = panggil diri sendiri langsung
* **Indirect recursion** = A → B → A
* **Infinite recursion** = tanpa base case → Stack Overflow
* **Call stack** tumbuh setiap level rekursi — ada batasan kedalaman
* **Fibonacci naif = O(2^n)** — lambat. Gunakan **memoization = O(n)**
* **Tail recursion** = recursive call sebagai operasi terakhir — bisa di-optimize
* Rekursi cocok untuk: Tree/Graph traversal, divide & conquer, nested data processing

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | MDN Web Docs: Recursion | javascript.info*
