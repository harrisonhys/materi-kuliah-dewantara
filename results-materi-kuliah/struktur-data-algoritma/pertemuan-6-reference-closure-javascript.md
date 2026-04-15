# Pertemuan 6: Reference, Closure & Scope di JavaScript

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Membedakan tipe data primitive vs reference di JavaScript
* Menjelaskan perbedaan pass-by-value dan pass-by-reference
* Memahami cara kerja Heap memory dan Call Stack
* Mengimplementasikan closure dan memahami lexical scope
* Menggunakan module pattern untuk enkapsulasi data
* Menjelaskan konsep garbage collection dan prototype chain

---

## 📖 Pengantar (Hook)

Pernahkah kamu menulis kode seperti ini:

```javascript
const a = { saldo: 100000 };
const b = a;
b.saldo = 999999;
console.log(a.saldo); // ???
```

Hasilnya bukan `100000` — tapi `999999`.

Kamu tidak pernah mengubah `a`, tapi nilainya berubah. Inilah salah satu "gotcha" terbesar di JavaScript — dan memahaminya adalah kunci menulis kode yang tidak punya bug tersembunyi, terutama di sistem backend yang menangani data sensitif seperti saldo rekening.

---

## 🧩 Konsep Utama

### Primitive vs Reference Types

**Primitive Types** — disimpan langsung di Stack. Nilainya disalin saat assignment.
* `number`, `string`, `boolean`, `null`, `undefined`, `symbol`, `bigint`

**Reference Types** — disimpan di Heap. Yang disalin adalah **alamat memori** (pointer), bukan nilainya.
* `object`, `array`, `function`

```javascript
// PRIMITIVE — copy by value
let x = 10;
let y = x;
y = 99;
console.log(x); // 10 — tidak berubah

// REFERENCE — copy by reference
let obj1 = { saldo: 100000 };
let obj2 = obj1; // obj2 menyimpan ALAMAT obj1, bukan copy
obj2.saldo = 999999;
console.log(obj1.saldo); // 999999 — BERUBAH!
```

### Call Stack vs Heap Memory

```
CALL STACK                    HEAP
(variabel lokal, primitive)   (object, array, function)

┌─────────────────┐          ┌──────────────────────────┐
│ x = 10          │          │ { saldo: 100000 }  ←──┐  │
│ y = 99          │          │                       │  │
│ obj1 = [addr1] ─┼──────────┼───────────────────────┘  │
│ obj2 = [addr1] ─┼──────────┘ (addr yang SAMA!)        │
└─────────────────┘          └──────────────────────────┘
```

Karena `obj1` dan `obj2` menunjuk ke alamat yang sama di Heap, mengubah lewat salah satu akan terlihat di keduanya.

### Pass-by-Value vs Pass-by-Reference

```javascript
// Pass-by-VALUE (primitive)
function tambah(n) {
  n = n + 100;
  return n;
}

let angka = 50;
tambah(angka);
console.log(angka); // 50 — tidak berubah, yang dikirim adalah copy

// Pass-by-REFERENCE (object)
function transferSaldo(akun, jumlah) {
  akun.saldo -= jumlah; // MODIFIKASI object asli!
}

const rekening = { id: 'A001', saldo: 500000 };
transferSaldo(rekening, 100000);
console.log(rekening.saldo); // 400000 — berubah!
```

### Deep Copy vs Shallow Copy

```javascript
const original = { user: 'Andi', alamat: { kota: 'Jakarta' } };

// SHALLOW COPY — hanya level pertama yang di-copy
const shallow = { ...original };
shallow.user = 'Budi'; // OK — tidak mengubah original
shallow.alamat.kota = 'Bandung'; // BAHAYA — masih shared reference!
console.log(original.alamat.kota); // 'Bandung' — berubah!

// DEEP COPY — semua level di-copy
const deep = JSON.parse(JSON.stringify(original));
deep.alamat.kota = 'Surabaya';
console.log(original.alamat.kota); // 'Bandung' — tidak berubah
```

### Closure dan Lexical Scope

**Lexical Scope** — fungsi bisa mengakses variabel dari scope di mana fungsi itu **didefinisikan**, bukan di mana fungsi itu **dipanggil**.

**Closure** — fungsi yang "mengingat" environment (variabel) dari scope luar, bahkan setelah scope luar sudah selesai dieksekusi.

```javascript
function buatCounter() {
  let count = 0; // variabel di scope luar

  return {
    increment: function() { count++; },
    decrement: function() { count--; },
    getCount:  function() { return count; }
  };
}

const counter = buatCounter();
counter.increment();
counter.increment();
counter.increment();
console.log(counter.getCount()); // 3
// Variabel 'count' tidak bisa diakses dari luar!
console.log(counter.count); // undefined
```

`count` hidup di Heap, di-"capture" oleh ketiga fungsi inner. Ini adalah **closure** — dan ini kenapa `count` tidak bisa diakses dari luar secara langsung.

### Module Pattern

Closure bisa digunakan untuk membuat "private state" — data yang hanya bisa diakses lewat interface yang kita definisikan:

```javascript
const RekeningModule = (function() {
  // "Private" — tidak bisa diakses dari luar
  let _saldo = 0;
  const _riwayat = [];

  function _validasiJumlah(jumlah) {
    return jumlah > 0 && jumlah <= _saldo;
  }

  // "Public" interface
  return {
    deposit(jumlah) {
      if (jumlah <= 0) throw new Error('Jumlah tidak valid');
      _saldo += jumlah;
      _riwayat.push({ tipe: 'deposit', jumlah, waktu: Date.now() });
    },
    withdraw(jumlah) {
      if (!_validasiJumlah(jumlah)) throw new Error('Saldo tidak cukup');
      _saldo -= jumlah;
      _riwayat.push({ tipe: 'withdraw', jumlah, waktu: Date.now() });
    },
    getSaldo() { return _saldo; },
    getRiwayat() { return [..._riwayat]; } // return copy, bukan reference!
  };
})();

RekeningModule.deposit(500000);
RekeningModule.withdraw(100000);
console.log(RekeningModule.getSaldo()); // 400000
console.log(RekeningModule._saldo); // undefined — tidak bisa diakses!
```

---

## 🧠 Ilustrasi / Analogi

| Konsep | Analogi |
|---|---|
| **Primitive (by value)** | Fotokopi dokumen — mengubah fotokopi tidak mengubah aslinya |
| **Reference (by reference)** | Kunci duplikat — dua kunci untuk satu kamar yang sama |
| **Heap** | Gudang penyimpanan — object disimpan di sini, hanya alamatnya yang diingat |
| **Call Stack** | Tumpukan piring — fungsi yang dipanggil ditaruh di atas, selesai diambil dari atas |
| **Closure** | Ransel yang dibawa keluar kantor — fungsi "membawa" variabel yang dia butuhkan walau sudah keluar dari scope asalnya |
| **Module Pattern** | Brankas dengan interface terbatas — hanya bisa deposit/withdraw, tidak bisa buka langsung |

---

## 💻 Contoh Teknis

```javascript
// ============= DEMONSTRASI REFERENCE =============
function demoReference() {
  // Array juga reference type!
  const arr1 = [1, 2, 3];
  const arr2 = arr1;
  arr2.push(4);
  console.log(arr1); // [1, 2, 3, 4] — berubah!

  // Cara aman: copy array
  const arr3 = [...arr1]; // atau arr1.slice()
  arr3.push(5);
  console.log(arr1); // [1, 2, 3, 4] — tidak berubah

  // Cara aman: copy object
  const obj = { a: 1, b: { c: 2 } };
  const safeShallow = { ...obj }; // shallow copy
  const safeDeep = JSON.parse(JSON.stringify(obj)); // deep copy
}

// ============= CLOSURE UNTUK MEMOIZATION =============
function buatMemoize(fn) {
  const cache = {}; // closure: cache "hidup" selama fungsi memo ada

  return function(...args) {
    const key = JSON.stringify(args);
    if (key in cache) {
      console.log(`Cache hit: ${key}`);
      return cache[key];
    }
    cache[key] = fn(...args);
    return cache[key];
  };
}

function hitungFibonacci(n) {
  if (n <= 1) return n;
  return hitungFibonacci(n - 1) + hitungFibonacci(n - 2);
}

const fibMemo = buatMemoize(hitungFibonacci);
console.log(fibMemo(10)); // dihitung
console.log(fibMemo(10)); // Cache hit!

// ============= CLOSURE UNTUK PRIVATE COUNTER =============
function buatIdGenerator(prefix) {
  let _counter = 0; // private via closure

  return function() {
    _counter++;
    return `${prefix}-${String(_counter).padStart(6, '0')}`;
  };
}

const generateTrxId = buatIdGenerator('TRX');
const generateUserId = buatIdGenerator('USR');

console.log(generateTrxId()); // TRX-000001
console.log(generateTrxId()); // TRX-000002
console.log(generateUserId()); // USR-000001 (counter terpisah!)
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Closure untuk Mencegah Race Condition pada Shared State

**Skenario:** Backend payment gateway memiliki fungsi pemrosesan transaksi. Setiap transaksi harus melalui tiga tahap: validasi → debit → credit. Masalah timbul ketika dua request masuk bersamaan dan keduanya membaca saldo yang sama sebelum salah satu sempat meng-update-nya.

**Masalah dengan global state yang terbuka:**
```javascript
// ❌ BERBAHAYA — state terbuka, mudah dimodifikasi dari mana saja
let saldoGlobal = 1000000;

async function prosesPembayaran(jumlah) {
  if (saldoGlobal >= jumlah) { // Request A cek: 1jt >= 900rb ✓
    // Request B juga cek di sini sebelum A selesai: 1jt >= 900rb ✓
    await delay(100); // simulasi network latency
    saldoGlobal -= jumlah; // A: 1jt - 900rb = 100rb
    // B: 100rb - 900rb = -800rb ← OVERDRAFT!
  }
}
```

**Solusi dengan Closure + Module Pattern:**
```javascript
// ✅ Closure untuk enkapsulasi state + transaksi atomik
function buatAkunPembayaran(saldoAwal) {
  let _saldo = saldoAwal;
  let _isProcessing = false; // lock sederhana

  return {
    async bayar(jumlah, idTransaksi) {
      if (_isProcessing) {
        throw new Error(`Transaksi ${idTransaksi}: Akun sedang diproses`);
      }

      _isProcessing = true;
      try {
        if (_saldo < jumlah) {
          throw new Error(`Saldo tidak cukup: ${_saldo} < ${jumlah}`);
        }
        await delay(100); // simulasi DB write
        _saldo -= jumlah;
        console.log(`✅ ${idTransaksi}: Berhasil. Sisa saldo: ${_saldo}`);
        return true;
      } finally {
        _isProcessing = false; // selalu unlock, bahkan jika error
      }
    },
    getSaldo: () => _saldo
  };
}

const akun = buatAkunPembayaran(1000000);
// Dua request bersamaan:
akun.bayar(900000, 'TRX-001'); // Berhasil
akun.bayar(900000, 'TRX-002'); // Error: Akun sedang diproses
```

**Pelajaran:** Closure menjaga `_saldo` dan `_isProcessing` tetap private. Tidak ada kode dari luar yang bisa mengubahnya secara langsung — hanya lewat method `bayar()` yang memiliki logic validasi.

---

## 📊 Visualisasi: Bagaimana Closure Bekerja di Memory

```
Saat buatCounter() dipanggil:
┌─────────────────────────────────────────┐
│ buatCounter() — Execution Context       │
│   count = 0 ← di Heap, bukan di Stack  │
│   returns { increment, decrement, get } │
└─────────────────────────────────────────┘

Setelah buatCounter() selesai:
Stack frame buatCounter() DIHAPUS.
Tapi 'count' di Heap MASIH ADA
karena increment/decrement/get masih
mereference-nya → Garbage Collector
tidak menghapusnya.

const counter = buatCounter()
  counter.increment → "ingat" count di Heap
  counter.decrement → "ingat" count di Heap
  counter.getCount  → "ingat" count di Heap
```

### Garbage Collection

JavaScript menggunakan **Mark-and-Sweep**: GC menelusuri semua referensi yang bisa dicapai dari root (global scope, call stack). Semua yang tidak bisa dicapai → dihapus.

```javascript
// count TIDAK dihapus selama counter masih ada
let counter = buatCounter(); // count hidup

counter = null; // sekarang tidak ada referensi ke closure
// count sekarang unreachable → GC akan hapus di siklus berikutnya
```

---

## ⚠️ Kesalahan Umum

1. **Mengira spread operator selalu cukup untuk "copy"** → `{...obj}` hanya shallow copy. Nested objects masih shared reference. Gunakan deep copy untuk object yang punya nested data penting.

2. **Closure di dalam loop tanpa `let`** → Masalah klasik:
   ```javascript
   // ❌ Semua akan print '5'
   for (var i = 0; i < 5; i++) {
     setTimeout(() => console.log(i), 100);
   }
   // ✅ Setiap closure dapat 'i' sendiri karena let = block scope
   for (let i = 0; i < 5; i++) {
     setTimeout(() => console.log(i), 100);
   }
   ```

3. **Memory leak akibat closure yang memegang reference besar** → Jika closure menangkap reference ke object besar (DOM element, large array), object tersebut tidak akan di-GC selama closure masih ada.

4. **Mutasi parameter object dalam fungsi** → Bisa menyebabkan side effect yang tidak terduga. Jika tidak sengaja ingin mutasi, buat copy dulu: `const data = { ...inputObj }`.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Apa perbedaan antara variabel yang disimpan di Call Stack vs Heap? Berikan contoh tipe data untuk masing-masing.

b) Jelaskan mengapa hasil `console.log(a.saldo)` bisa berubah meskipun kamu tidak pernah mengubah `a` secara langsung (berikan ilustrasi dengan kode).

c) Apa yang dimaksud dengan "lexical scope"? Mengapa ini penting untuk memahami closure?

### Soal 2 — Coding

1. Buat fungsi `buatRateLimiter(batas, durasi)` menggunakan closure yang:
   * Menghitung berapa kali fungsi dipanggil dalam window waktu `durasi` milliseconds
   * Menolak (throw Error) jika jumlah pemanggilan melebihi `batas`
   * Mereset counter setelah `durasi` berlalu

   Contoh penggunaan:
   ```javascript
   const limiter = buatRateLimiter(3, 1000); // max 3 kali per detik
   limiter('req-1'); // OK
   limiter('req-2'); // OK
   limiter('req-3'); // OK
   limiter('req-4'); // Error: Rate limit exceeded
   ```

2. Implementasikan deep copy function tanpa menggunakan `JSON.stringify` (supaya bisa handle `Date`, `undefined`, fungsi):
   ```javascript
   function deepCopy(obj) {
     // implementasikan secara rekursif
   }
   ```

---

## 📌 Ringkasan

* **Primitive types** (number, string, bool) — disimpan di Stack, di-copy by value
* **Reference types** (object, array, function) — disimpan di Heap, yang di-copy adalah pointer
* Mengubah object lewat referensi apapun akan **mengubah object yang sama**
* **Shallow copy** (`{...obj}`, `[...arr]`) — hanya copy level pertama, nested masih shared
* **Deep copy** (`JSON.parse/stringify`) — copy semua level, tapi tidak handle Date/function
* **Closure** — fungsi yang "mengingat" variabel dari scope luar bahkan setelah scope luar selesai
* **Lexical scope** — scope ditentukan oleh lokasi kode ditulis, bukan saat dieksekusi
* **Module Pattern** — menggunakan closure untuk membuat private state + public interface
* **Garbage Collection** — otomatis, berbasis reachability. Closure bisa menyebabkan memory leak jika tidak hati-hati
* Closure adalah fondasi dari banyak pattern modern: memoization, rate limiting, event handlers, module system

---

*📚 Referensi: Kyle Simpson (2014). You Don't Know JS: Scope & Closures | MDN Web Docs — Closures | javascript.info — Variable scope, closure*
