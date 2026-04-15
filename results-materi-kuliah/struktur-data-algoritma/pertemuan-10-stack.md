# Pertemuan 10: Stack — LIFO, Implementasi & Aplikasi

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan prinsip LIFO dan cara kerja Stack
* Mengimplementasikan Stack menggunakan array dan linked list di JavaScript
* Menggunakan Stack untuk validasi tanda kurung, undo/redo, dan evaluasi ekspresi
* Menganalisis kompleksitas operasi Stack
* Mengenali pola masalah yang cocok diselesaikan dengan Stack

---

## 📖 Pengantar (Hook)

Setiap kali kamu menekan `Ctrl+Z` di editor kode, ada sebuah Stack yang bekerja di balik layar.

Setiap kali browser-mu menekan tombol "Back", Stack lagi.

Bahkan ketika JavaScript menjalankan kode rekursif, Call Stack yang sudah kita bahas di Pertemuan 3 — itu adalah Stack.

Stack adalah salah satu struktur data paling fundamental, dan setelah memahaminya, kamu akan mulai melihatnya di mana-mana.

---

## 🧩 Konsep Utama

### Apa itu Stack?

Stack adalah struktur data **LIFO** — **Last In, First Out**. Elemen yang terakhir masuk adalah yang pertama keluar.

Bayangkan tumpukan piring: piring yang ditaruh terakhir ada di posisi paling atas, dan itu yang diambil pertama.

```
PUSH → masukkan elemen ke atas
POP  → ambil elemen dari atas
PEEK → lihat elemen paling atas (tanpa mengambil)

    PUSH 40
    ┌───┐
    │ 40│ ← top
    │ 30│
    │ 20│
    │ 10│
    └───┘
    
    POP → returns 40
    ┌───┐
    │ 30│ ← top
    │ 20│
    │ 10│
    └───┘
```

### Operasi Stack

| Operasi | Deskripsi | Kompleksitas |
|---|---|---|
| `push(item)` | Tambah elemen di top | O(1) |
| `pop()` | Hapus dan return elemen top | O(1) |
| `peek()` / `top()` | Lihat elemen top tanpa hapus | O(1) |
| `isEmpty()` | Cek apakah stack kosong | O(1) |
| `size()` | Jumlah elemen | O(1) |

Semua operasi utama **O(1)** — ini membuat Stack sangat efisien.

---

## 🧠 Ilustrasi / Analogi

| Konsep | Analogi |
|---|---|
| **Stack** | Tumpukan piring — ambil dari atas, taruh ke atas |
| **Push** | Taruh piring baru di atas tumpukan |
| **Pop** | Ambil piring dari atas tumpukan |
| **Peek** | Lihat piring teratas tanpa mengambilnya |
| **Stack overflow** | Tumpukan piring terlalu tinggi — jatuh! |
| **Empty stack** | Tumpukan kosong — tidak bisa ambil apapun |
| **Call Stack** | Tumpukan "pekerjaan yang sedang dilakukan" oleh JavaScript engine |

---

## 💻 Contoh Teknis

```javascript
// ============= IMPLEMENTASI STACK (Array-based) =============
class Stack {
  constructor() {
    this._data = [];
  }

  push(item) {
    this._data.push(item);
  }

  pop() {
    if (this.isEmpty()) throw new Error('Stack underflow');
    return this._data.pop();
  }

  peek() {
    if (this.isEmpty()) throw new Error('Stack kosong');
    return this._data[this._data.length - 1];
  }

  isEmpty() {
    return this._data.length === 0;
  }

  size() {
    return this._data.length;
  }

  toArray() {
    return [...this._data].reverse(); // tampilkan dari top ke bottom
  }
}

// ============= IMPLEMENTASI STACK (Linked List-based) =============
class StackLL {
  constructor() {
    this._head = null;
    this._size = 0;
  }

  push(data) {
    const node = new Node(data);
    node.next = this._head;
    this._head = node;
    this._size++;
  }

  pop() {
    if (!this._head) throw new Error('Stack underflow');
    const data = this._head.data;
    this._head = this._head.next;
    this._size--;
    return data;
  }

  peek() {
    if (!this._head) throw new Error('Stack kosong');
    return this._head.data;
  }

  isEmpty() { return this._head === null; }
  size()    { return this._size; }
}

// ============= APLIKASI 1: VALIDASI TANDA KURUNG =============
function isValidBrackets(str) {
  const stack = new Stack();
  const pairs = { ')': '(', ']': '[', '}': '{' };

  for (const char of str) {
    if ('([{'.includes(char)) {
      stack.push(char); // buka kurung → push
    } else if (')]}'.includes(char)) {
      if (stack.isEmpty()) return false; // tidak ada pasangan
      if (stack.pop() !== pairs[char]) return false; // pasangan salah
    }
  }

  return stack.isEmpty(); // harus kosong di akhir
}

console.log(isValidBrackets('({[]})')); // true
console.log(isValidBrackets('({[}])')); // false
console.log(isValidBrackets('((('    )); // false

// ============= APLIKASI 2: UNDO/REDO SYSTEM =============
class UndoRedoManager {
  constructor() {
    this._undoStack = new Stack();
    this._redoStack = new Stack();
  }

  execute(aksi) {
    this._undoStack.push(aksi);
    this._redoStack = new Stack(); // redo stack clear setelah aksi baru
    aksi.do();
    console.log(`✅ Execute: ${aksi.deskripsi}`);
  }

  undo() {
    if (this._undoStack.isEmpty()) {
      console.log('Tidak ada yang bisa di-undo');
      return;
    }
    const aksi = this._undoStack.pop();
    this._redoStack.push(aksi);
    aksi.undo();
    console.log(`↩️ Undo: ${aksi.deskripsi}`);
  }

  redo() {
    if (this._redoStack.isEmpty()) {
      console.log('Tidak ada yang bisa di-redo');
      return;
    }
    const aksi = this._redoStack.pop();
    this._undoStack.push(aksi);
    aksi.do();
    console.log(`↪️ Redo: ${aksi.deskripsi}`);
  }
}

// ============= APLIKASI 3: EVALUASI EKSPRESI POSTFIX =============
// Postfix/RPN (Reverse Polish Notation): "3 4 + 2 *" = (3+4)*2 = 14
function evaluasiPostfix(ekspresi) {
  const stack = new Stack();
  const tokens = ekspresi.split(' ');

  for (const token of tokens) {
    if (!isNaN(token)) {
      stack.push(parseFloat(token)); // angka → push
    } else {
      // operator → pop dua operand, hitung, push hasilnya
      const b = stack.pop();
      const a = stack.pop();
      switch (token) {
        case '+': stack.push(a + b); break;
        case '-': stack.push(a - b); break;
        case '*': stack.push(a * b); break;
        case '/': stack.push(a / b); break;
        default: throw new Error(`Operator tidak dikenal: ${token}`);
      }
    }
  }

  return stack.pop();
}

console.log(evaluasiPostfix('3 4 + 2 *')); // 14
console.log(evaluasiPostfix('5 1 2 + 4 * + 3 -')); // 14
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Audit Trail dengan Rollback: Stack untuk Transaksi Kompleks

**Skenario:** Sistem pembayaran B2B perlu menjalankan "saga" transaksi — serangkaian operasi yang harus berhasil semua atau dibatalkan semua (atomik). Contoh: transfer merchant payout yang melibatkan:
1. Debit saldo pengirim
2. Catat biaya admin
3. Credit saldo penerima
4. Kirim notifikasi

Jika langkah 3 gagal (misalnya rekening penerima diblokir), semua langkah sebelumnya harus di-rollback.

```javascript
class SagaTransaction {
  constructor(idTransaksi) {
    this.id = idTransaksi;
    this._langkahDieksekusi = new Stack(); // stack kompensasi
  }

  async jalankanLangkah(langkah) {
    try {
      await langkah.execute();
      this._langkahDieksekusi.push(langkah); // simpan kompensasi
      console.log(`✅ ${langkah.nama} berhasil`);
    } catch (error) {
      console.error(`❌ ${langkah.nama} gagal: ${error.message}`);
      await this._rollback();
      throw error;
    }
  }

  async _rollback() {
    console.log(`🔄 Rollback transaksi ${this.id}...`);
    while (!this._langkahDieksekusi.isEmpty()) {
      const langkah = this._langkahDieksekusi.pop();
      await langkah.compensate(); // jalankan aksi kompensasi secara terbalik
      console.log(`↩️ Rollback: ${langkah.nama}`);
    }
    console.log(`✅ Rollback selesai`);
  }
}

// Contoh penggunaan:
const saga = new SagaTransaction('TRX-PAYOUT-001');

await saga.jalankanLangkah({
  nama: 'Debit pengirim',
  execute: () => debitSaldo('merchant-A', 5000000),
  compensate: () => creditSaldo('merchant-A', 5000000), // batalkan
});

await saga.jalankanLangkah({
  nama: 'Catat biaya admin',
  execute: () => catatBiaya('TRX-PAYOUT-001', 25000),
  compensate: () => hapusBiaya('TRX-PAYOUT-001'),
});

await saga.jalankanLangkah({
  nama: 'Credit penerima',
  execute: () => creditSaldo('bank-B-account', 4975000), // ini yang gagal
  compensate: () => debitSaldo('bank-B-account', 4975000),
});
// Jika credit penerima gagal → rollback otomatis (pop stack):
// 1. Hapus biaya admin
// 2. Credit kembali ke pengirim
```

**Mengapa Stack?** Rollback harus dalam urutan terbalik dari eksekusi. Stack LIFO secara natural memberikan urutan terbalik — langkah terakhir di-rollback pertama.

---

## 📊 Visualisasi: Call Stack JavaScript

```
Saat kode ini berjalan:
function c() { return 1; }
function b() { return c() + 1; }
function a() { return b() + 1; }
a();

CALL STACK (tumbuh ke atas):
┌─────────┐
│   c()   │ ← top (sedang dijalankan)
│   b()   │
│   a()   │
│ <main>  │ ← bottom
└─────────┘

c() selesai → pop
┌─────────┐
│   b()   │ ← top
│   a()   │
│ <main>  │
└─────────┘

b() selesai → pop
┌─────────┐
│   a()   │ ← top
│ <main>  │
└─────────┘
```

---

## ⚠️ Kesalahan Umum

1. **Pop dari empty stack** → Selalu cek `isEmpty()` sebelum `pop()`, atau gunakan try-catch. Stack underflow adalah error umum.

2. **Mengira Stack dan Queue sama** → Stack = LIFO (tumpukan piring). Queue = FIFO (antrian kasir). Keduanya sangat berbeda!

3. **Lupa bahwa `Array.pop()` dan `Array.push()` bisa langsung jadi Stack** → Tidak selalu perlu buat class baru. Untuk kasus sederhana, array sudah cukup.

4. **Menggunakan Stack untuk masalah yang butuh random access** → Stack hanya efisien di top. Jika butuh akses elemen di tengah, gunakan array biasa.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Jelaskan perbedaan antara Stack berbasis array vs Stack berbasis linked list. Kapan kamu memilih satu vs yang lain?

b) Mengapa Call Stack JavaScript menggunakan prinsip LIFO? Apa yang terjadi jika menggunakan FIFO?

c) Selain validasi kurung dan undo/redo, sebutkan dua use case Stack lainnya dalam aplikasi nyata.

### Soal 2 — Coding

1. Implementasikan fungsi `konversiInfixKePostfix(ekspresi)` yang mengonversi ekspresi infix (`3 + 4 * 2`) ke postfix (`3 4 2 * +`). Gunakan Stack untuk menyimpan operator dan perhatikan prioritas operator (`*` dan `/` lebih tinggi dari `+` dan `-`).

2. Implementasikan `minStack` — Stack yang mendukung operasi `getMin()` yang mengembalikan nilai minimum dalam Stack secara O(1):
   ```javascript
   const ms = new MinStack();
   ms.push(5);
   ms.push(3);
   ms.push(7);
   ms.getMin(); // 3
   ms.pop();    // 7
   ms.getMin(); // 3
   ms.pop();    // 3
   ms.getMin(); // 5
   ```
   Hint: gunakan dua stack — satu untuk data, satu untuk tracking minimum.

---

## 📌 Ringkasan

* **Stack** = LIFO (Last In, First Out) — piring ditumpuk, diambil dari atas
* **Operasi utama:** `push`, `pop`, `peek`, `isEmpty` — semua O(1)
* **Implementasi:** bisa dengan Array (`push/pop`) atau Linked List (insert/delete di head)
* **Aplikasi Stack:**
  * Validasi tanda kurung: push buka kurung, pop + cek tutup kurung
  * Undo/Redo: dua stack (undo stack + redo stack)
  * Evaluasi postfix: push angka, pop saat ketemu operator
  * Call Stack JavaScript: setiap fungsi push/pop frame
  * Saga/rollback transaksi: stack kompensasi yang di-pop saat rollback
* **Ingat:** Stack TIDAK cocok untuk akses random atau FIFO — gunakan Queue untuk itu

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Sedgewick & Wayne (2011). Algorithms | MDN Web Docs — Call Stack*
