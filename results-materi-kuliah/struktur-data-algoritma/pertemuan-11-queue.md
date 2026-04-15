# Pertemuan 11: Queue, Circular Queue & Priority Queue

## 🎯 Learning Outcomes

Setelah mempelajari materi ini, kamu diharapkan mampu:

- Menjelaskan konsep FIFO dan membedakannya dengan LIFO (Stack)
- Mengimplementasikan Simple Queue berbasis array dan pointer
- Memahami kelemahan array-based queue dan solusinya dengan Circular Queue
- Mengimplementasikan Priority Queue dengan array terurut maupun min-heap
- Menjelaskan hubungan Queue dengan Event Loop JavaScript
- Menerapkan Queue pada studi kasus sistem antrian pembayaran fintech

---

## 📖 Pengantar (Hook)

Bayangkan kamu lagi antri beli kopi di cafe yang rame. Siapa yang datang duluan, dialah yang dilayani duluan. Simpel banget, kan?

Nah, di dunia software — hal yang sama terjadi ribuan kali per detik. Waktu kamu menekan tombol bayar di aplikasi e-wallet, requestmu masuk ke antrian di server payment gateway. Server memproses satu per satu, sesuai urutan. Tapi ada yang menarik: kalau transaksimu nilainya besar (katakan diatas 50 juta), apakah kamu harus tetap antri di belakang semua transaksi receh?

Di sinilah **Priority Queue** masuk — dan itulah yang akan kita bahas di pertemuan ini.

> "A queue is just a waiting list, but in computing, how you manage that waiting list can be the difference between a system that scales and one that crashes under load."

---

## 🧩 Konsep Utama

### 1. Prinsip FIFO (First In, First Out)

Queue bekerja berdasarkan prinsip **FIFO**: elemen pertama yang masuk adalah elemen pertama yang keluar. Kebalikan dari Stack yang LIFO.

**Operasi Dasar Queue:**

| Operasi | Deskripsi | Kompleksitas |
|---------|-----------|--------------|
| `enqueue(item)` | Tambah elemen di bagian belakang (rear) | O(1) |
| `dequeue()` | Hapus & kembalikan elemen dari depan (front) | O(1)* |
| `peek()` / `front()` | Lihat elemen terdepan tanpa menghapus | O(1) |
| `isEmpty()` | Cek apakah queue kosong | O(1) |
| `size()` | Kembalikan jumlah elemen | O(1) |

> *O(1) untuk pointer-based, O(n) untuk naive array-based (karena `shift()`)

---

### 2. Masalah `shift()` pada Array-Based Queue

Ini jebakan yang sering tidak disadari:

```
// Jangan lakukan ini untuk queue berperforma tinggi!
queue.push("A");   // enqueue → O(1) ✅
queue.shift();     // dequeue → O(n) ❌ — semua elemen digeser
```

Setiap kali `shift()` dipanggil, JavaScript harus menggeser seluruh elemen array ke posisi baru. Untuk antrian dengan 1 juta elemen, ini sangat mahal.

**Solusi:** Gunakan pointer `head` yang menunjuk elemen terdepan tanpa benar-benar menghapus dari array (dengan konsekuensi memory leak pada antrian panjang), atau gunakan implementasi **Linked List** / **Circular Queue**.

---

### 3. Circular Queue (Ring Buffer)

Circular Queue menggunakan fixed-size array dengan dua pointer: `head` (front) dan `tail` (rear). Ketika pointer mencapai ujung array, ia "wrap around" kembali ke awal — seperti jam yang berputar.

**Keunggulan:**
- Enqueue & Dequeue keduanya O(1)
- Fixed memory footprint — cocok untuk embedded system dan buffer jaringan

---

### 4. Deque (Double-Ended Queue)

Deque memungkinkan penambahan dan penghapusan dari **kedua ujung** (depan dan belakang). Ini adalah generalisasi dari Queue dan Stack sekaligus.

Operasi Deque:
- `addFront()`, `addRear()`
- `removeFront()`, `removeRear()`
- `peekFront()`, `peekRear()`

---

### 5. Priority Queue

Dalam Priority Queue, elemen dengan **prioritas tertinggi** keluar lebih dulu — bukan berdasarkan urutan masuk.

**Dua pendekatan implementasi:**

| Pendekatan | Enqueue | Dequeue |
|------------|---------|---------|
| Array terurut | O(n) | O(1) |
| Min/Max Heap | O(log n) | O(log n) |

Untuk production system, heap adalah pilihan standar.

---

### 6. Queue dan JavaScript Event Loop

JavaScript adalah single-threaded, tapi bisa "terasa" asinkron berkat **Event Loop** yang mengelola dua jenis antrian:

| Antrian | Contoh | Prioritas |
|---------|--------|-----------|
| **Microtask Queue** | `Promise.then()`, `queueMicrotask()` | Lebih tinggi — dikosongkan dulu sebelum macrotask berikutnya |
| **Macrotask Queue** | `setTimeout()`, `setInterval()`, I/O events | Lebih rendah |

```
console.log("1 - Sync");

setTimeout(() => console.log("2 - Macrotask"), 0);

Promise.resolve().then(() => console.log("3 - Microtask"));

console.log("4 - Sync");

// Output: 1, 4, 3, 2
```

Ini adalah contoh Priority Queue di level runtime JavaScript sendiri!

---

## 🧠 Ilustrasi / Analogi

### Analogi Kasir Supermarket

```
Antrian Biasa (Simple Queue):
 [Andi] [Budi] [Citra] [Deni]
   ↑ dilayani pertama        ↑ terakhir masuk

Antrian VIP (Priority Queue):
 Normal:  [Andi-100rb] [Budi-50rb]  [Citra-200rb]
                                         ↑
 Setelah priority sort:
 [Citra-200rb] [Andi-100rb] [Budi-50rb]
       ↑ VIP duluan!
```

### Analogi Circular Queue: Conveyor Belt di Bandara

Bayangkan conveyor belt bagasi di bandara — kapasitas terbatas, berputar terus. Koper baru masuk dari satu titik, diambil dari titik lain. Ketika ujung belt tercapai, koper "wrap around" ke awal. Itulah circular queue.

---

## 💻 Contoh Teknis

### Implementasi 1: Simple Queue (Naive Array)

```javascript
// ⚠️ Mudah dipahami, tapi dequeue O(n) karena shift()
class SimpleQueue {
  constructor() {
    this.items = [];
  }

  enqueue(item) {
    this.items.push(item); // O(1)
  }

  dequeue() {
    if (this.isEmpty()) throw new Error("Queue is empty");
    return this.items.shift(); // O(n) — bermasalah untuk queue besar!
  }

  peek() {
    if (this.isEmpty()) throw new Error("Queue is empty");
    return this.items[0];
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }

  toString() {
    return `Queue: [${this.items.join(" -> ")}] (front → rear)`;
  }
}

// Penggunaan
const q = new SimpleQueue();
q.enqueue("Transaksi-A");
q.enqueue("Transaksi-B");
q.enqueue("Transaksi-C");
console.log(q.toString());         // Queue: [Transaksi-A -> Transaksi-B -> Transaksi-C]
console.log(q.dequeue());          // Transaksi-A
console.log(q.peek());             // Transaksi-B
console.log(`Size: ${q.size()}`);  // Size: 2
```

---

### Implementasi 2: Efficient Queue (Pointer-Based)

```javascript
// ✅ Dequeue O(1) menggunakan head pointer
// Trade-off: memory tidak langsung dibebaskan (perlu occasional cleanup)
class EfficientQueue {
  constructor() {
    this.items = {};
    this.head = 0;
    this.tail = 0;
  }

  enqueue(item) {
    this.items[this.tail] = item;
    this.tail++;
  }

  dequeue() {
    if (this.isEmpty()) throw new Error("Queue is empty");
    const item = this.items[this.head];
    delete this.items[this.head]; // bebaskan memory
    this.head++;
    return item;
  }

  peek() {
    if (this.isEmpty()) throw new Error("Queue is empty");
    return this.items[this.head];
  }

  isEmpty() {
    return this.head === this.tail;
  }

  size() {
    return this.tail - this.head;
  }
}

// Big-O Summary:
// enqueue: O(1) amortized
// dequeue: O(1)
// peek:    O(1)
// isEmpty: O(1)
// size:    O(1)

const eq = new EfficientQueue();
eq.enqueue("TX-001");
eq.enqueue("TX-002");
eq.enqueue("TX-003");
console.log(eq.dequeue()); // TX-001 — O(1)!
console.log(eq.size());    // 2
```

---

### Implementasi 3: Circular Queue (Fixed-Size Ring Buffer)

```javascript
class CircularQueue {
  constructor(capacity) {
    this.capacity = capacity;
    this.items = new Array(capacity).fill(null);
    this.head = 0;      // pointer ke elemen depan
    this.tail = 0;      // pointer ke slot kosong berikutnya
    this.count = 0;     // jumlah elemen aktual
  }

  enqueue(item) {
    if (this.isFull()) throw new Error("Queue is full");
    this.items[this.tail] = item;
    this.tail = (this.tail + 1) % this.capacity; // wrap around
    this.count++;
  }

  dequeue() {
    if (this.isEmpty()) throw new Error("Queue is empty");
    const item = this.items[this.head];
    this.items[this.head] = null;                 // bersihkan slot
    this.head = (this.head + 1) % this.capacity; // wrap around
    this.count--;
    return item;
  }

  peek() {
    if (this.isEmpty()) throw new Error("Queue is empty");
    return this.items[this.head];
  }

  isFull() {
    return this.count === this.capacity;
  }

  isEmpty() {
    return this.count === 0;
  }

  size() {
    return this.count;
  }

  visualize() {
    const display = [...this.items].map((item, idx) => {
      let label = item === null ? "[ _ ]" : `[${item}]`;
      if (idx === this.head && idx === this.tail && !this.isEmpty()) {
        label += " ← H/T";
      } else if (idx === this.head) {
        label += " ← HEAD";
      } else if (idx === this.tail) {
        label += " ← TAIL";
      }
      return label;
    });
    return display.join("  ");
  }
}

// Demonstrasi Circular Queue
const cq = new CircularQueue(5);
cq.enqueue("TX-A");
cq.enqueue("TX-B");
cq.enqueue("TX-C");
console.log("Setelah 3 enqueue:");
console.log(cq.visualize());
// [TX-A] ← HEAD  [TX-B]  [TX-C]  [ _ ] ← TAIL  [ _ ]

cq.dequeue(); // hapus TX-A
cq.dequeue(); // hapus TX-B
cq.enqueue("TX-D");
cq.enqueue("TX-E");
console.log("\nSetelah 2 dequeue + 2 enqueue (wrap around!):");
console.log(cq.visualize());
// [ _ ]  [ _ ]  [TX-C] ← HEAD  [TX-D]  [TX-E] ← TAIL
// Dengan wrap: TAIL bisa "melingkar" ke awal array
```

---

### Implementasi 4: Priority Queue (Array Terurut + Min-Heap)

```javascript
// === Pendekatan 1: Array Terurut ===
// Enqueue O(n), Dequeue O(1)
class PriorityQueueSorted {
  constructor() {
    this.items = []; // { value, priority } — priority kecil = lebih tinggi
  }

  enqueue(value, priority) {
    const element = { value, priority };
    let added = false;

    for (let i = 0; i < this.items.length; i++) {
      if (priority < this.items[i].priority) {
        this.items.splice(i, 0, element); // O(n)
        added = true;
        break;
      }
    }

    if (!added) this.items.push(element);
  }

  dequeue() {
    if (this.isEmpty()) throw new Error("Priority Queue is empty");
    return this.items.shift().value; // O(1) — selalu ambil dari depan
  }

  peek() {
    if (this.isEmpty()) throw new Error("Priority Queue is empty");
    return this.items[0].value;
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }
}

// === Pendekatan 2: Min-Heap ===
// Enqueue O(log n), Dequeue O(log n) — lebih efisien untuk data besar
class MinHeapPriorityQueue {
  constructor() {
    this.heap = []; // [{ value, priority }]
  }

  // Helper: indeks parent dan children
  _parent(i) { return Math.floor((i - 1) / 2); }
  _left(i)   { return 2 * i + 1; }
  _right(i)  { return 2 * i + 2; }

  _swap(i, j) {
    [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
  }

  // Bubble Up — setelah insert di akhir
  _bubbleUp(idx) {
    while (idx > 0) {
      const parent = this._parent(idx);
      if (this.heap[parent].priority <= this.heap[idx].priority) break;
      this._swap(parent, idx);
      idx = parent;
    }
  }

  // Bubble Down — setelah remove root
  _bubbleDown(idx) {
    const n = this.heap.length;
    while (true) {
      let smallest = idx;
      const left  = this._left(idx);
      const right = this._right(idx);

      if (left < n && this.heap[left].priority < this.heap[smallest].priority) {
        smallest = left;
      }
      if (right < n && this.heap[right].priority < this.heap[smallest].priority) {
        smallest = right;
      }
      if (smallest === idx) break;

      this._swap(idx, smallest);
      idx = smallest;
    }
  }

  enqueue(value, priority) {
    this.heap.push({ value, priority });
    this._bubbleUp(this.heap.length - 1); // O(log n)
  }

  dequeue() {
    if (this.isEmpty()) throw new Error("Heap is empty");
    const min = this.heap[0];
    const last = this.heap.pop();
    if (this.heap.length > 0) {
      this.heap[0] = last;
      this._bubbleDown(0); // O(log n)
    }
    return min.value;
  }

  peek() {
    if (this.isEmpty()) throw new Error("Heap is empty");
    return this.heap[0].value;
  }

  isEmpty() {
    return this.heap.length === 0;
  }

  size() {
    return this.heap.length;
  }
}

// Big-O Comparison:
// ┌─────────────────┬──────────┬──────────┐
// │ Implementasi    │ Enqueue  │ Dequeue  │
// ├─────────────────┼──────────┼──────────┤
// │ Array Terurut   │ O(n)     │ O(1)     │
// │ Min-Heap        │ O(log n) │ O(log n) │
// └─────────────────┴──────────┴──────────┘
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Simulasi Antrian Pembayaran di Payment Gateway

Bayangkan kamu bekerja sebagai backend engineer di perusahaan payment gateway (seperti Midtrans, Xendit, atau DOKU). Setiap detik, sistem menerima ribuan request transaksi dari merchant.

**Aturan bisnis:**
- Transaksi ≥ Rp 50.000.000 → Priority 1 (VIP — proses duluan)
- Transaksi Rp 1.000.000 – 49.999.999 → Priority 2 (Standard)
- Transaksi < Rp 1.000.000 → Priority 3 (Mikro)

**Mengapa perlu Priority Queue?**
- Bank settlement memiliki cut-off time (biasanya pukul 15.00)
- Transaksi VIP nominal besar membawa pendapatan komisi lebih besar
- SLA (Service Level Agreement) berbeda per tier

```javascript
// ============================================================
// SIMULASI PAYMENT GATEWAY DENGAN PRIORITY QUEUE
// ============================================================

class Transaction {
  constructor(id, merchantName, amount, timestamp) {
    this.id = id;
    this.merchantName = merchantName;
    this.amount = amount;          // dalam Rupiah
    this.timestamp = timestamp;    // waktu masuk antrian
    this.priority = this._calculatePriority(amount);
  }

  _calculatePriority(amount) {
    if (amount >= 50_000_000)  return 1; // VIP
    if (amount >= 1_000_000)   return 2; // Standard
    return 3;                            // Mikro
  }

  getPriorityLabel() {
    const labels = { 1: "🔴 VIP", 2: "🟡 Standard", 3: "🟢 Mikro" };
    return labels[this.priority];
  }

  formatAmount() {
    return new Intl.NumberFormat("id-ID", {
      style: "currency", currency: "IDR", minimumFractionDigits: 0
    }).format(this.amount);
  }
}

class PaymentGatewayQueue {
  constructor() {
    this.heap = [];
    this.processedCount = 0;
    this.totalWaitTime = 0;
    this.processingLog = [];
  }

  _parent(i) { return Math.floor((i - 1) / 2); }
  _left(i)   { return 2 * i + 1; }
  _right(i)  { return 2 * i + 2; }
  _swap(i, j) {
    [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
  }

  // Comparator: priority lebih kecil = lebih penting
  // Jika priority sama, yang masuk lebih dulu (FIFO) diproses duluan
  _isHigherPriority(a, b) {
    if (a.priority !== b.priority) return a.priority < b.priority;
    return a.timestamp < b.timestamp; // FIFO dalam tier yang sama
  }

  _bubbleUp(idx) {
    while (idx > 0) {
      const parent = this._parent(idx);
      if (!this._isHigherPriority(this.heap[idx], this.heap[parent])) break;
      this._swap(parent, idx);
      idx = parent;
    }
  }

  _bubbleDown(idx) {
    const n = this.heap.length;
    while (true) {
      let best = idx;
      const left  = this._left(idx);
      const right = this._right(idx);

      if (left  < n && this._isHigherPriority(this.heap[left], this.heap[best]))  best = left;
      if (right < n && this._isHigherPriority(this.heap[right], this.heap[best])) best = right;
      if (best === idx) break;

      this._swap(idx, best);
      idx = best;
    }
  }

  enqueue(transaction) {
    this.heap.push(transaction);
    this._bubbleUp(this.heap.length - 1);
    console.log(
      `  [ENQUEUE] ${transaction.id} | ${transaction.getPriorityLabel()} | ` +
      `${transaction.formatAmount()} | Posisi antrian: ${this.heap.length}`
    );
  }

  dequeue() {
    if (this.heap.length === 0) return null;
    const top = this.heap[0];
    const last = this.heap.pop();
    if (this.heap.length > 0) {
      this.heap[0] = last;
      this._bubbleDown(0);
    }
    return top;
  }

  processNext(currentTime) {
    const tx = this.dequeue();
    if (!tx) return null;

    const waitTime = currentTime - tx.timestamp; // dalam ms (simulasi)
    this.totalWaitTime += waitTime;
    this.processedCount++;

    const logEntry = {
      order: this.processedCount,
      id: tx.id,
      merchant: tx.merchantName,
      amount: tx.formatAmount(),
      priority: tx.getPriorityLabel(),
      waitTime: `${waitTime}ms`,
    };
    this.processingLog.push(logEntry);
    return logEntry;
  }

  getAverageWaitTime() {
    if (this.processedCount === 0) return 0;
    return (this.totalWaitTime / this.processedCount).toFixed(2);
  }

  size() {
    return this.heap.length;
  }

  printReport() {
    console.log("\n" + "=".repeat(70));
    console.log("LAPORAN PEMROSESAN PAYMENT GATEWAY");
    console.log("=".repeat(70));
    console.log(`${"No".<3} | ${"ID".<12} | ${"Merchant".<20} | ${"Nominal".<18} | ${"Prioritas".<12} | ${"Tunggu"}`);
    console.log("-".repeat(70));
    this.processingLog.forEach(log => {
      console.log(
        `${String(log.order).padStart(2)}  | ${log.id.padEnd(12)} | ` +
        `${log.merchant.padEnd(20)} | ${log.amount.padEnd(18)} | ` +
        `${log.priority.padEnd(12)} | ${log.waitTime}`
      );
    });
    console.log("-".repeat(70));
    console.log(`Total transaksi diproses : ${this.processedCount}`);
    console.log(`Rata-rata waktu tunggu   : ${this.getAverageWaitTime()}ms`);
    console.log("=".repeat(70));
  }
}

// ============================================================
// SIMULASI: Transaksi masuk dalam urutan acak
// ============================================================

console.log("=== PAYMENT GATEWAY SIMULATION ===\n");
console.log("--- Transaksi Masuk (urutan kedatangan) ---");

const gateway = new PaymentGatewayQueue();
let time = 0;

// Transaksi masuk dalam urutan acak (seperti kondisi real)
const incomingTransactions = [
  new Transaction("TX-001", "Warung Pak Budi",        85_000,        time += 10),  // Mikro
  new Transaction("TX-002", "Toko Elektronik Maju",   2_500_000,     time += 10),  // Standard
  new Transaction("TX-003", "PT Infrastruktur Nusa",  150_000_000,   time += 10),  // VIP
  new Transaction("TX-004", "Kafe Kopi Kenangan",     125_000,       time += 10),  // Mikro
  new Transaction("TX-005", "CV Supplier Tekstil",    75_000_000,    time += 10),  // VIP
  new Transaction("TX-006", "Apotek Sehat Jaya",      3_200_000,     time += 10),  // Standard
  new Transaction("TX-007", "PT Logistik Nusantara",  200_000_000,   time += 10),  // VIP
  new Transaction("TX-008", "Restoran Seafood Bahari", 450_000,      time += 10),  // Mikro
];

incomingTransactions.forEach(tx => gateway.enqueue(tx));

console.log("\n--- Proses Antrian (berdasarkan Priority) ---\n");

const processTime = time + 5;
while (gateway.size() > 0) {
  const result = gateway.processNext(processTime);
  if (result) {
    console.log(
      `  [PROSES #${result.order}] ${result.id} | ${result.priority} | ` +
      `${result.amount} (tunggu: ${result.waitTime})`
    );
  }
}

gateway.printReport();

/*
OUTPUT yang diharapkan (urutan proses):
  [PROSES #1] TX-007 | 🔴 VIP     | Rp200.000.000 (VIP pertama masuk)
  [PROSES #2] TX-003 | 🔴 VIP     | Rp150.000.000
  [PROSES #3] TX-005 | 🔴 VIP     | Rp75.000.000
  [PROSES #4] TX-002 | 🟡 Standard | Rp2.500.000
  [PROSES #5] TX-006 | 🟡 Standard | Rp3.200.000
  [PROSES #6] TX-001 | 🟢 Mikro   | Rp85.000
  [PROSES #7] TX-004 | 🟢 Mikro   | Rp125.000
  [PROSES #8] TX-008 | 🟢 Mikro   | Rp450.000

Insight:
- VIP transaksi (3 buah, total Rp425jt) diproses PERTAMA terlepas kapan masuknya
- Rata-rata waktu tunggu VIP jauh lebih rendah daripada mikro
- Komisi yang dikumpulkan dari 3 VIP transaksi jauh melebihi 5 mikro transaksi
*/
```

**Analisis Dampak Bisnis:**

| Metrik | Tanpa Priority Queue | Dengan Priority Queue |
|--------|---------------------|----------------------|
| VIP wait time | ~4 slot (acak) | Selalu ≤ jumlah VIP |
| Komisi per detik | Tidak teroptimasi | Maksimal dari transaksi besar |
| Settlement risk | VIP bisa miss cut-off | VIP selalu diproses pertama |
| Kompleksitas | O(1) enqueue | O(log n) enqueue/dequeue |

---

## 📊 Visualisasi

### 1. Enqueue & Dequeue pada Simple Queue

```
ENQUEUE TX-A, TX-B, TX-C, TX-D:

Step 1: Enqueue TX-A
  FRONT                REAR
    ↓                    ↓
  [TX-A]

Step 2: Enqueue TX-B
  FRONT                REAR
    ↓                    ↓
  [TX-A] → [TX-B]

Step 3: Enqueue TX-C, TX-D
  FRONT                REAR
    ↓                    ↓
  [TX-A] → [TX-B] → [TX-C] → [TX-D]

Step 4: Dequeue (ambil TX-A)
         FRONT         REAR
           ↓              ↓
  [TX-B] → [TX-C] → [TX-D]
  ← TX-A keluar
```

---

### 2. Circular Queue dengan Head & Tail Pointer (capacity = 5)

```
Kondisi Awal (kosong):
  Index:  [0]   [1]   [2]   [3]   [4]
          [ _ ] [ _ ] [ _ ] [ _ ] [ _ ]
           ↑H/T

Enqueue A, B, C:
  Index:  [0]   [1]   [2]   [3]   [4]
          [ A ] [ B ] [ C ] [ _ ] [ _ ]
           ↑H                ↑T

Dequeue A, Dequeue B:
  Index:  [0]   [1]   [2]   [3]   [4]
          [ _ ] [ _ ] [ C ] [ _ ] [ _ ]
                        ↑H    ↑T

Enqueue D, E, F (F wrap around ke index 0!):
  Index:  [0]   [1]   [2]   [3]   [4]
          [ F ] [ _ ] [ C ] [ D ] [ E ]
                        ↑H          ↑T
                                       ↓ (tail wrap ke 0, F masuk index 0)
Setelah F masuk:
  Index:  [0]   [1]   [2]   [3]   [4]
          [ F ] [ _ ] [ C ] [ D ] [ E ]
           ↑T           ↑H

  Urutan logis: C → D → E → F (head ke tail mengikuti lingkaran)

Formula wrap around:
  tail = (tail + 1) % capacity   →   (4+1) % 5 = 0 ✅
  head = (head + 1) % capacity
```

---

### 3. Priority Queue — Min-Heap Visualisasi

```
Insert: (TX-003, priority=1), (TX-001, priority=3), (TX-005, priority=1), (TX-002, priority=2)

Array representation: [priority]

Setelah insert TX-003 (p=1):
        [1:TX-003]
        (index 0)

Setelah insert TX-001 (p=3):
        [1:TX-003]
           /
      [3:TX-001]

Setelah insert TX-005 (p=1) — bubble up karena p=1 = root, tidak ada swap:
        [1:TX-003]
           /    \
      [3:TX-001] [1:TX-005]

Setelah insert TX-002 (p=2) — bubble up: parent TX-001(p=3) > TX-002(p=2), swap!
        [1:TX-003]
           /    \
      [2:TX-002] [1:TX-005]
         /
    [3:TX-001]

Dequeue → ambil root [1:TX-003], last node naik ke root, lalu bubble down:
Array sebelum dequeue: [TX-003(1), TX-002(2), TX-005(1), TX-001(3)]
Setelah dequeue TX-003:
  - TX-001(p=3) naik ke root
  - Bubble down: child kiri TX-002(p=2) dan kanan TX-005(p=1)
  - TX-005 terkecil → swap dengan root

        [1:TX-005]
           /    \
      [2:TX-002] [3:TX-001]

Min-Heap property terjaga! ✅
```

---

### 4. Event Loop Queue — Microtask vs Macrotask

```
JavaScript Runtime:

  Call Stack            Microtask Queue        Macrotask Queue
  ┌──────────┐          ┌──────────────┐       ┌──────────────┐
  │ console  │          │ Promise.then │       │ setTimeout   │
  │ .log("1")│          │ ()           │       │ cb()         │
  └──────────┘          └──────────────┘       └──────────────┘

Alur:
1. Call Stack jalankan semua sync code ("1" dan "4")
2. Call Stack kosong → Event Loop cek Microtask Queue
3. Jalankan SEMUA microtask ("3") hingga kosong
4. Baru ambil SATU macrotask ("2")
5. Ulangi dari langkah 2

Output: 1 → 4 → 3 → 2
```

---

## ⚠️ Kesalahan Umum

### 1. Menggunakan `shift()` untuk Queue Besar

```javascript
// ❌ BURUK — O(n) per dequeue
const queue = [];
queue.push("item");
queue.shift(); // Re-index seluruh array!

// ✅ BAIK — O(1) per dequeue
const queue2 = new EfficientQueue();
queue2.enqueue("item");
queue2.dequeue();
```

### 2. Lupa Handle Edge Case Queue Penuh di Circular Queue

```javascript
// ❌ Tidak cek isFull() sebelum enqueue
cq.items[cq.tail] = item; // Overwrite data yang belum diproses!

// ✅ Selalu cek capacity
if (cq.isFull()) throw new Error("Buffer penuh — drop atau tunggu");
```

### 3. Salah Memahami Priority (angka besar vs kecil)

```javascript
// ❌ Asumsi angka besar = prioritas tinggi, tapi implementasinya min-heap
pq.enqueue("VIP", 10);    // harusnya priority 1, tapi input 10
pq.enqueue("Mikro", 1);   // ini yang keluar duluan! bug!

// ✅ Dokumentasikan konvensinya:
// priority 1 = tertinggi (VIP), priority 3 = terendah (Mikro)
// Min-heap → angka KECIL keluar duluan = sesuai konvensi ✅
```

### 4. Tidak Mempertimbangkan Thread Safety di Production

Di Node.js single-threaded ini bukan masalah, tapi kalau pakai Worker Threads:

```javascript
// ❌ Tanpa mutex, dua worker bisa dequeue elemen yang sama
const item = pq.peek();
// ... context switch ...
const item2 = pq.dequeue(); // bisa dapat item berbeda!

// ✅ Di production, gunakan Redis Queue (Bull, BullMQ) yang atomic:
// const queue = new Queue('payments', { redis: redisConfig });
```

### 5. Memory Leak pada Pointer-Based Queue

```javascript
// ❌ Tidak delete item yang sudah di-dequeue
dequeue() {
  const item = this.items[this.head];
  this.head++; // head maju, tapi items[0..head-1] tetap di memory!
}

// ✅ Delete item setelah dequeue
dequeue() {
  const item = this.items[this.head];
  delete this.items[this.head]; // bebaskan reference
  this.head++;
  return item;
}
```

---

## 🧪 Latihan / Studi Kasus

### Latihan 1 — Dasar (Wajib)

Implementasikan class `BankQueue` yang mensimulasikan antrian teller bank:
- Minimal 3 teller tersedia
- Setiap teller punya queue sendiri
- Nasabah baru masuk ke teller dengan antrian terpendek
- Tampilkan distribusi nasabah per teller

### Latihan 2 — Menengah

Implementasikan **Deque** (Double-Ended Queue) lengkap dengan operasi:
- `addFront(item)`, `addRear(item)`
- `removeFront()`, `removeRear()`
- `peekFront()`, `peekRear()`

Kemudian gunakan Deque untuk mengecek apakah sebuah string adalah **palindrom** (misalnya "kasur rusak", "level", "racecar").

### Latihan 3 — Advanced (Studi Kasus)

**Sistem Rate Limiting Payment Gateway**

Buat `RateLimiter` menggunakan Circular Queue untuk membatasi jumlah request:
- Max 10 transaksi per 60 detik per merchant
- Jika melebihi limit, transaksi ditolak dengan error `RATE_LIMIT_EXCEEDED`
- Circular Queue menyimpan timestamp 10 transaksi terakhir
- Jika transaksi tertua sudah > 60 detik, slot bisa dipakai ulang

```javascript
class SlidingWindowRateLimiter {
  constructor(maxRequests = 10, windowMs = 60_000) {
    this.maxRequests = maxRequests;
    this.windowMs = windowMs;
    this.merchantWindows = new Map(); // merchantId → CircularQueue of timestamps
  }

  isAllowed(merchantId, currentTime = Date.now()) {
    // TODO: Implementasikan sliding window rate limiting menggunakan Circular Queue
    // Hint: hapus timestamp yang sudah di luar window, lalu cek apakah masih ada slot
  }
}

// Test case:
const limiter = new SlidingWindowRateLimiter(3, 10_000); // max 3 req / 10 detik
console.log(limiter.isAllowed("merchant-123", 1000));  // true
console.log(limiter.isAllowed("merchant-123", 2000));  // true
console.log(limiter.isAllowed("merchant-123", 3000));  // true
console.log(limiter.isAllowed("merchant-123", 4000));  // false — RATE_LIMIT_EXCEEDED
console.log(limiter.isAllowed("merchant-123", 12000)); // true — window geser, slot bebas
```

### Latihan 4 — Diskusi

Sebuah startup fintech ingin membangun sistem antrian untuk memproses 100.000 transaksi/menit. Mereka berdebat antara:
- **Opsi A**: In-memory Priority Queue di Node.js
- **Opsi B**: Redis Queue dengan library BullMQ
- **Opsi C**: Apache Kafka dengan consumer group

Analisis trade-off ketiga opsi dari sisi: throughput, durability (data tidak hilang kalau server crash), kompleksitas implementasi, dan biaya infrastruktur.

---

## 📌 Ringkasan

| Konsep | Prinsip | Kompleksitas Kunci |
|--------|---------|-------------------|
| Simple Queue (array) | FIFO | Enqueue O(1), Dequeue O(n) — hindari! |
| Efficient Queue (pointer) | FIFO | Enqueue O(1), Dequeue O(1) |
| Circular Queue | FIFO + fixed-size | Enqueue O(1), Dequeue O(1) |
| Priority Queue (sorted array) | Prioritas | Enqueue O(n), Dequeue O(1) |
| Priority Queue (min-heap) | Prioritas | Enqueue O(log n), Dequeue O(log n) |
| Deque | FIFO + LIFO | Semua operasi O(1) |

**Kapan pakai apa?**

- **Simple Queue** → Pemula, antrian kecil, kode sederhana
- **Efficient Queue** → Antrian dinamis, tidak ada batas kapasitas
- **Circular Queue** → Buffer jaringan, audio streaming, rate limiting — kapasitas fixed
- **Priority Queue** → Task scheduler, antrian pembayaran VIP, algoritma Dijkstra
- **Deque** → Undo/redo history, sliding window algorithm

**Key Insight:**
> Queue bukan hanya struktur data — ini adalah cara berpikir tentang urutan pemrosesan. Pilihan implementasi Queue yang salah bisa membuat sistem fintech kamu crash di peak hour. Pilih sesuai kebutuhan throughput, memory, dan business priority.
