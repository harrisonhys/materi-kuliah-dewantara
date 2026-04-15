# Pertemuan 15: Proyek Mini — Sistem Manajemen Data dengan Struktur Data

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Mengintegrasikan minimal tiga struktur data dalam satu sistem nyata
* Memilih struktur data yang tepat berdasarkan kebutuhan operasi
* Mengimplementasikan sistem end-to-end dengan justifikasi arsitektur
* Mendokumentasikan keputusan desain dan trade-off yang diambil

---

## 📖 Pengantar (Hook)

Selama 14 pertemuan, kita sudah belajar struktur data satu per satu. BST, Graph, Stack, Queue, Linked List — masing-masing punya kekuatan dan trade-off.

Tapi di dunia nyata, tidak ada sistem yang hanya menggunakan satu struktur data.

Di pertemuan ini, kita akan **membangun** — menggabungkan semua yang sudah dipelajari menjadi satu sistem yang fungsional. Ini adalah latihan paling penting sebelum UAS: membuktikan bahwa kamu bisa bukan hanya memahami setiap struktur data secara terpisah, tapi tahu **kapan dan mengapa** memilih masing-masing dalam konteks sistem nyata.

---

## 🧩 Deskripsi Proyek

### Pilihan Proyek (Pilih Salah Satu)

---

#### Opsi A: Mini Payment Gateway System

Sistem pembayaran sederhana yang mendukung:
1. **Registrasi merchant dan pengguna**
2. **Transfer antar akun** dengan queue pemrosesan
3. **Riwayat transaksi** yang bisa di-sort dan di-search
4. **Deteksi anomali sederhana** (transaksi besar, frekuensi tinggi)
5. **Undo transaksi terakhir** (dalam window 1 menit)

**Struktur data yang digunakan:**
* **HashMap/Object** → penyimpanan akun (O(1) lookup by ID)
* **Queue** → antrian pemrosesan transaksi
* **Stack** → undo history
* **BST** → index transaksi by nominal untuk range query
* **Graph** (optional/bonus) → deteksi jaringan transfer mencurigakan

---

#### Opsi B: Task Management System (Mirip Trello/Asana)

Sistem manajemen tugas dengan:
1. **Board** dengan multiple columns (Backlog, In Progress, Done)
2. **Task** dengan prioritas, deadline, dan assignee
3. **Task queue** berdasarkan prioritas
4. **Dependency tracking** (task B tidak bisa dimulai sebelum task A selesai)
5. **Activity log** dengan undo/redo

**Struktur data yang digunakan:**
* **Doubly Linked List** → setiap column (card bisa dipindah O(1) jika pointer diketahui)
* **Priority Queue / Min-Heap** → antrian tugas by prioritas dan deadline
* **Graph (DAG)** → dependency antar task, topological sort untuk execution order
* **Stack** → undo/redo history

---

#### Opsi C: Rekomendasi Produk Sederhana

Sistem rekomendasi untuk e-commerce/fintech product:
1. **Product catalog** dengan search dan filter
2. **User profile** dengan riwayat interaksi
3. **Rekomendasi** berbasis "pengguna serupa" (collaborative filtering sederhana)
4. **Cache rekomendasi** untuk menghindari komputasi ulang

**Struktur data yang digunakan:**
* **BST** → product catalog dengan range search (harga, rating)
* **Graph** → jaringan user-product interaction
* **BFS** → finding "users like you" dalam jarak 2 hop
* **LRU Cache** → cache hasil rekomendasi per user

---

## 💻 Contoh Implementasi: Opsi A — Mini Payment Gateway

```javascript
// ============= MINI PAYMENT GATEWAY =============
// Demonstrasi integrasi: HashMap + Queue + Stack + BST

// ---- DATA STRUCTURES ----
class Queue {
  constructor() { this._items = []; }
  enqueue(item) { this._items.push(item); }
  dequeue() { return this._items.shift(); }
  peek() { return this._items[0]; }
  isEmpty() { return this._items.length === 0; }
  size() { return this._items.length; }
}

class Stack {
  constructor() { this._items = []; }
  push(item) { this._items.push(item); }
  pop() { return this._items.pop(); }
  peek() { return this._items[this._items.length - 1]; }
  isEmpty() { return this._items.length === 0; }
}

// BST untuk index transaksi by nominal
class TransaksiBST {
  constructor() { this.root = null; }

  insert(transaksi) {
    this.root = this._insert(this.root, transaksi);
  }

  _insert(node, trx) {
    if (!node) return { data: trx, left: null, right: null };
    if (trx.nominal < node.data.nominal) node.left = this._insert(node.left, trx);
    else if (trx.nominal > node.data.nominal) node.right = this._insert(node.right, trx);
    else {
      // Nominal sama — simpan sebagai list di node
      if (!node.data.duplicates) node.data.duplicates = [];
      node.data.duplicates.push(trx);
    }
    return node;
  }

  rangeSearch(min, max, node = this.root, result = []) {
    if (!node) return result;
    if (node.data.nominal > min) this.rangeSearch(min, max, node.left, result);
    if (node.data.nominal >= min && node.data.nominal <= max) {
      result.push(node.data);
      if (node.data.duplicates) result.push(...node.data.duplicates);
    }
    if (node.data.nominal < max) this.rangeSearch(min, max, node.right, result);
    return result;
  }
}

// ---- CORE SYSTEM ----
class MiniPaymentGateway {
  constructor() {
    this._akun = {}; // HashMap: akunId → { id, nama, saldo }
    this._antrian = new Queue(); // Queue: antrian pemrosesan
    this._undoStack = new Stack(); // Stack: undo history (1 menit)
    this._trxIndex = new TransaksiBST(); // BST: index by nominal
    this._trxLog = []; // Array: semua transaksi (untuk audit)
    this._idCounter = 1;
  }

  // REGISTRASI — O(1)
  daftarAkun(nama, saldoAwal = 0) {
    const id = `ACC-${String(this._idCounter++).padStart(4, '0')}`;
    this._akun[id] = { id, nama, saldo: saldoAwal };
    console.log(`✅ Akun dibuat: ${id} (${nama})`);
    return id;
  }

  // DEPOSIT — O(1) lookup + O(log n) index insert
  deposit(akunId, jumlah) {
    const akun = this._akun[akunId];
    if (!akun) throw new Error(`Akun ${akunId} tidak ditemukan`);
    if (jumlah <= 0) throw new Error('Jumlah harus positif');

    akun.saldo += jumlah;
    const trx = this._buatTransaksi('DEPOSIT', null, akunId, jumlah);
    this._trxIndex.insert(trx);
    this._trxLog.push(trx);

    console.log(`💰 Deposit ${jumlah.toLocaleString('id-ID')} ke ${akun.nama}`);
    return trx;
  }

  // TRANSFER — masuk ke queue dulu, diproses secara async
  queueTransfer(dariId, keId, jumlah) {
    if (!this._akun[dariId]) throw new Error(`Akun pengirim ${dariId} tidak ditemukan`);
    if (!this._akun[keId]) throw new Error(`Akun penerima ${keId} tidak ditemukan`);
    if (jumlah <= 0) throw new Error('Jumlah harus positif');

    const request = {
      id: `REQ-${Date.now()}`,
      dariId, keId, jumlah,
      timestamp: Date.now()
    };

    this._antrian.enqueue(request);
    console.log(`📥 Transfer ${jumlah.toLocaleString('id-ID')} antri (queue size: ${this._antrian.size()})`);
    return request.id;
  }

  // PROSES SATU TRANSFER DARI QUEUE — O(1) dequeue + O(1) saldo update + O(log n) index
  prosesTransfer() {
    if (this._antrian.isEmpty()) {
      console.log('Antrian kosong');
      return null;
    }

    const req = this._antrian.dequeue();
    const pengirim = this._akun[req.dariId];
    const penerima = this._akun[req.keId];

    if (pengirim.saldo < req.jumlah) {
      console.log(`❌ Gagal: saldo ${pengirim.nama} tidak cukup`);
      return null;
    }

    // Eksekusi transfer
    pengirim.saldo -= req.jumlah;
    penerima.saldo += req.jumlah;

    const trx = this._buatTransaksi('TRANSFER', req.dariId, req.keId, req.jumlah);
    this._trxIndex.insert(trx);
    this._trxLog.push(trx);

    // Simpan ke undo stack dengan kompensasi dan expiry (1 menit)
    this._undoStack.push({
      trx,
      expiry: Date.now() + 60000,
      compensate: () => {
        pengirim.saldo += req.jumlah;
        penerima.saldo -= req.jumlah;
        console.log(`↩️ Transfer di-undo: ${req.jumlah.toLocaleString('id-ID')}`);
      }
    });

    console.log(`✅ Transfer ${req.jumlah.toLocaleString('id-ID')}: ${pengirim.nama} → ${penerima.nama}`);
    return trx;
  }

  // UNDO TRANSAKSI TERAKHIR — O(1) pop
  undo() {
    if (this._undoStack.isEmpty()) {
      console.log('Tidak ada transaksi yang bisa di-undo');
      return;
    }

    const item = this._undoStack.peek();

    // Cek apakah masih dalam window waktu
    if (Date.now() > item.expiry) {
      this._undoStack.pop(); // expired, buang
      console.log('Transaksi sudah melewati batas waktu undo (1 menit)');
      return;
    }

    this._undoStack.pop();
    item.compensate();
  }

  // DETEKSI ANOMALI: transaksi besar dalam range tertentu — O(log n + k)
  deteksiAnomaLi(minNominal, maxNominal) {
    const mencurigakan = this._trxIndex.rangeSearch(minNominal, maxNominal);
    if (mencurigakan.length > 0) {
      console.log(`⚠️ ${mencurigakan.length} transaksi dalam range ${minNominal.toLocaleString('id-ID')} - ${maxNominal.toLocaleString('id-ID')}:`);
      mencurigakan.forEach(t => console.log(`  ${t.id}: ${t.nominal.toLocaleString('id-ID')}`));
    }
    return mencurigakan;
  }

  getSaldo(akunId) {
    return this._akun[akunId]?.saldo ?? null;
  }

  _buatTransaksi(tipe, dariId, keId, nominal) {
    return {
      id: `TRX-${Date.now()}-${Math.random().toString(36).substr(2, 4)}`,
      tipe, dariId, keId, nominal,
      timestamp: new Date().toISOString()
    };
  }
}

// ============= DEMO =============
const pg = new MiniPaymentGateway();

const andi = pg.daftarAkun('Andi', 1000000);
const budi = pg.daftarAkun('Budi', 500000);
const cici = pg.daftarAkun('Cici', 200000);

pg.queueTransfer(andi, budi, 300000);
pg.queueTransfer(andi, cici, 150000);
pg.queueTransfer(budi, cici, 100000);

// Proses semua antrian
while (!pg._antrian.isEmpty()) {
  pg.prosesTransfer();
}

console.log('\n--- Saldo Akhir ---');
[andi, budi, cici].forEach(id => {
  const akun = pg._akun[id];
  console.log(`${akun.nama}: Rp ${akun.saldo.toLocaleString('id-ID')}`);
});

pg.undo(); // undo transfer terakhir (budi → cici)

// Deteksi transaksi antara 100rb - 350rb
pg.deteksiAnomaLi(100000, 350000);
```

---

## 🏦 Justifikasi Arsitektur

| Kebutuhan | Struktur Data | Alasan |
|---|---|---|
| Lookup akun cepat | HashMap/Object | O(1) akses by ID |
| Antrian pemrosesan | Queue (FIFO) | Transaksi diproses sesuai urutan masuk |
| Undo transaksi | Stack (LIFO) | Undo = batalkan yang terakhir dieksekusi |
| Range query nominal | BST | O(log n) untuk cari transaksi dalam range |
| Audit log lengkap | Array | Append O(1), dibutuhkan untuk compliance |

---

## 📋 Panduan Pengerjaan Proyek

### Deliverable

1. **Kode** — implementasi lengkap yang bisa dijalankan
2. **Justifikasi Arsitektur** — tabel seperti di atas, jelaskan mengapa memilih setiap struktur data
3. **Analisis Kompleksitas** — Big-O setiap operasi utama
4. **Test Cases** — minimal 5 skenario yang menguji edge case
5. **Demo Output** — screenshot atau output dari console yang menunjukkan sistem berjalan

### Kriteria Penilaian

| Kriteria | Bobot |
|---|---|
| Fungsionalitas (sistem berjalan, tidak ada bug) | 30% |
| Integrasi minimum 3 struktur data | 25% |
| Justifikasi pemilihan struktur data | 20% |
| Analisis Big-O yang benar | 15% |
| Kode yang bersih dan terdokumentasi | 10% |

### Tips

* Mulai dengan **data model** — apa saja entitas yang perlu disimpan?
* Tentukan **operasi kritis** — operasi apa yang paling sering dilakukan?
* Pilih struktur data berdasarkan operasi, bukan familiaritas
* Buat test untuk edge case: akun tidak ditemukan, saldo tidak cukup, undo saat stack kosong
* **Jangan over-engineer** — gunakan struktur data sederhana jika memenuhi kebutuhan

---

## ⚠️ Kesalahan Umum di Proyek

1. **Menggunakan satu struktur data untuk segalanya** → Array untuk semua = O(n) di semua operasi. Pilih yang tepat.

2. **Tidak ada error handling** → Sistem nyata harus handle: akun tidak ada, saldo tidak cukup, input invalid.

3. **Justifikasi yang terlalu generik** → "Saya pakai HashMap karena cepat" kurang baik. "Saya pakai HashMap karena lookup akun by ID dilakukan O(1000) kali per detik dan harus O(1)" jauh lebih kuat.

4. **Tidak ada Big-O analysis** → Setiap operasi utama harus ada analisis kompleksitasnya.

---

## 📌 Ringkasan

* Proyek mini adalah kesempatan mengintegrasikan semua konsep semester ini
* **Pilih struktur data berdasarkan operasi dominan**, bukan kebiasaan
* **Justifikasi arsitektur** sama pentingnya dengan implementasi — ini yang ditanya di interview
* Sistem nyata selalu menggunakan **kombinasi** struktur data, bukan satu saja
* Deadline dan kriteria penilaian ada di Panduan Pengerjaan di atas

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Sedgewick & Wayne (2011). Algorithms | Clean Code by Robert C. Martin*
