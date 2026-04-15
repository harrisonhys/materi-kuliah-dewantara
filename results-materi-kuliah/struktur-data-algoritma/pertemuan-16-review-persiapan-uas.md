# Pertemuan 16: Review & Persiapan UAS — Struktur Data & Algoritma

---

## 🎯 Learning Outcomes

Setelah pertemuan review ini, kamu akan bisa:

* Merangkum semua materi pasca-UTS (Pertemuan 8–14) dalam satu referensi cepat
* Menjawab soal UAS bertipe teori, analisis, dan coding
* Mengintegrasikan semua struktur data dalam satu framework pemilihan
* Mengidentifikasi pola soal yang paling sering keluar di UAS

---

## 📖 Pengantar (Hook)

Di akhir semester ini, kamu sudah mempelajari:
* 6 struktur data utama (Array, LL, Stack, Queue, Tree, Graph)
* 7 algoritma sorting (Bubble sampai Timsort)
* 2 algoritma searching (Linear, Binary)
* Rekursi, memoization, closure, reference

UAS bukan tentang siapa yang paling banyak hafal — tapi siapa yang paling paham **mengapa** dan **kapan** menggunakan setiap konsep. Itu yang kita latih di sini.

---

## 🧩 Ringkasan Materi Pertemuan 8–14

### Pertemuan 8: Linked List (Singly & Doubly)

* **Node** = data + pointer (`next`, dan `prev` untuk doubly)
* Elemen tersebar di Heap, terhubung lewat pointer
* **Singly LL:** traversal forward saja, delete tail O(n)
* **Doubly LL:** traversal dua arah, delete tail O(1) dengan tail pointer
* **Vs Array:** LL unggul di insert/delete head O(1), array unggul di akses random O(1)

| Operasi | Array | Singly LL | Doubly LL |
|---|---|---|---|
| Akses index | **O(1)** | O(n) | O(n) |
| Insert head | O(n) | **O(1)** | **O(1)** |
| Insert tail | O(1)* | O(n) | **O(1)** |
| Delete head | O(n) | **O(1)** | **O(1)** |
| Delete tail | O(1) | O(n) | **O(1)** |

### Pertemuan 9: Circular LL & LRU Cache

* **Circular LL** — tail.next → head, tidak ada null, ideal untuk round-robin/loop
* Traversal gunakan `do...while (current !== head)`, bukan `while (current)`
* **LRU Cache** = Doubly LL (order) + HashMap (O(1) lookup) → O(1) get dan put
* Eviction: node sebelum dummy tail (LRU) dihapus saat cache penuh

### Pertemuan 10: Stack

* **LIFO** — Last In, First Out
* Operasi: `push`, `pop`, `peek`, `isEmpty` — semua O(1)
* Implementasi: array-based atau linked list-based
* **Aplikasi:**
  * Validasi kurung — push buka, pop + cek tutup
  * Undo/Redo — dua stack
  * Evaluasi postfix — push angka, pop saat operator
  * Saga/rollback — stack kompensasi

### Pertemuan 11: Queue

* **FIFO** — First In, First Out
* Operasi: `enqueue`, `dequeue`, `peek`, `isEmpty` — O(1)
* **Jenis Queue:**
  * Simple Queue — FIFO murni
  * Circular Queue — efisien, tanpa shifting
  * Priority Queue — dequeue berdasarkan prioritas
  * Deque — insert/delete di kedua ujung
* **Aplikasi:** antrian transaksi, BFS, event loop JavaScript, rate limiter

### Pertemuan 12: Tree & BST

* **Tree** = hierarkis, satu root, tidak ada siklus
* **BST** = left < node < right, berlaku rekursif
* **Kompleksitas:** O(log n) average, O(n) worst (degenerate)
* **Traversal:**
  * **In-order** (L-Root-R) → SORTED output
  * **Pre-order** (Root-L-R) → copy/serialize
  * **Post-order** (L-R-Root) → delete tree
* Delete 2 anak → ganti dengan in-order successor

### Pertemuan 13: Graph & DFS/BFS

* **Graph** = vertex + edge (directed/undirected, weighted/unweighted)
* **Representasi:** Adjacency List O(V+E) vs Matrix O(V²)
* **DFS** — Stack, depth-first, cycle detection, path finding
* **BFS** — Queue, level-by-level, shortest path (unweighted)
* **Wajib:** visited set untuk mencegah infinite loop

### Pertemuan 14: Kompleksitas & Searching

* **Linear Search** — O(n), tidak butuh data sorted
* **Binary Search** — O(log n), butuh data sorted, divide & conquer
* **Hash Table** — O(1) average untuk search, insert, delete
* **Collision resolution:** chaining (linked list), open addressing (probing)
* **Load factor** → saat > 0.7-0.75, rehash untuk pertahankan O(1)

---

## 🧠 Framework Memilih Struktur Data

```
1. Operasi dominan apa yang kamu butuhkan?

   Banyak LOOKUP by key?
   → HashMap / Object

   Perlu ORDER (first/last)?
   → Array (access by index)
   → Linked List (insert/delete at ends)

   Perlu SORTED data + range query?
   → BST atau sorted array

   Perlu RELATIONSHIPS antar entitas?
   → Graph

   Perlu LIFO (undo, backtracking)?
   → Stack

   Perlu FIFO (antrian, BFS)?
   → Queue

   Perlu HIERARCHY?
   → Tree

2. Pertimbangkan trade-off:
   - O(1) vs O(log n) vs O(n)
   - Space: O(1) vs O(n) vs O(n²)
   - In-place vs extra memory
   - Stable vs unstable (untuk sorting)

3. Di production, sering kombinasi:
   HashMap + BST → database index
   HashMap + DLL → LRU Cache
   Graph + BFS → social network
   Stack + Stack → undo/redo
```

---

## 📊 Tabel Kompleksitas Komprehensif

### Struktur Data

| Struktur | Access | Search | Insert | Delete | Space |
|---|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| Stack | O(n) | O(n) | O(1) | O(1) | O(n) |
| Queue | O(n) | O(n) | O(1) | O(1) | O(n) |
| BST | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| Hash Table | O(1) avg | O(1) avg | O(1) avg | O(1) avg | O(n) |
| Graph | — | O(V+E) | O(1) | O(V+E) | O(V+E) |

*dengan pointer ke posisi yang tepat

### Sorting

| Algoritma | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ |
| Insertion Sort | **O(n)** | O(n²) | O(n²) | O(1) | ✅ |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ |
| Quick Sort | O(n log n) | O(n log n) | O(n²)** | O(log n) | ❌ |
| Merge Sort | O(n log n) | O(n log n) | **O(n log n)** | O(n) | ✅ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | **O(1)** | ❌ |
| Timsort | O(n) | O(n log n) | O(n log n) | O(n) | ✅ |

**dengan pivot tetap, gunakan random pivot!

---

## 💻 Quick Reference: Pola Soal Coding

```javascript
// POLA 1: Validasi / parsing — pakai Stack
function isBalanced(str) {
  const stack = [];
  const map = { ')': '(', ']': '[', '}': '{' };
  for (const c of str) {
    if ('([{'.includes(c)) stack.push(c);
    else if (stack.pop() !== map[c]) return false;
  }
  return stack.length === 0;
}

// POLA 2: Level traversal / shortest path — pakai BFS + Queue
function shortestPath(graph, start, end) {
  const queue = [{ node: start, path: [start] }];
  const visited = new Set([start]);
  while (queue.length) {
    const { node, path } = queue.shift();
    if (node === end) return path;
    for (const neighbor of graph[node] || []) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push({ node: neighbor, path: [...path, neighbor] });
      }
    }
  }
  return null;
}

// POLA 3: Sorted data / range query — pakai BST atau binary search
function binarySearch(arr, target) {
  let lo = 0, hi = arr.length - 1;
  while (lo <= hi) {
    const mid = (lo + hi) >> 1;
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) lo = mid + 1;
    else hi = mid - 1;
  }
  return -1;
}

// POLA 4: Memoization — pakai HashMap/closure
function memoize(fn) {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (!cache.has(key)) cache.set(key, fn(...args));
    return cache.get(key);
  };
}

// POLA 5: Top-K / Min element — pakai sorting atau partial sort
function topK(arr, k) {
  return [...arr].sort((a, b) => b - a).slice(0, k);
}

// POLA 6: Group by — pakai HashMap reduce
function groupBy(arr, key) {
  return arr.reduce((acc, item) => {
    const k = item[key];
    if (!acc[k]) acc[k] = [];
    acc[k].push(item);
    return acc;
  }, {});
}
```

---

## 🏦 Studi Kasus Integratif: Sistem Rekonsiliasi

```javascript
// Gabungan: Queue + BST + HashMap + Sorting
// Use case: rekonsiliasi transaksi harian payment gateway

class SistemRekonsiliasi {
  constructor() {
    this.transaksiInternal = {}; // HashMap: O(1) lookup
    this.bstByNominal = new TransaksiBST(); // BST: range query
    this.antrianProses = new Queue(); // Queue: FIFO processing
  }

  // Ingest: masukkan transaksi ke antrian
  ingest(trx) {
    this.antrianProses.enqueue(trx);
  }

  // Proses: dari antrian ke storage
  prosesAntrian() {
    while (!this.antrianProses.isEmpty()) {
      const trx = this.antrianProses.dequeue();
      this.transaksiInternal[trx.id] = trx; // O(1)
      this.bstByNominal.insert(trx); // O(log n)
    }
  }

  // Rekonsiliasi: cari yang tidak ada di bank
  rekonsiliasi(dataBank) {
    const bankMap = {};
    dataBank.forEach(t => bankMap[t.id] = t); // O(n) build

    const mismatch = Object.values(this.transaksiInternal)
      .filter(t => !bankMap[t.id]); // O(n)

    return mismatch.sort((a, b) => b.nominal - a.nominal); // O(m log m)
  }

  // Deteksi structuring: transaksi besar mendekati batas pelaporan
  deteksiStructuring(batas) {
    // Transaksi antara 90% - 100% batas pelaporan
    return this.bstByNominal.rangeSearch(batas * 0.9, batas); // O(log n + k)
  }
}
```

---

## 🧪 Soal Latihan UAS

### Bagian A: Teori (25 poin)

**1.** Jelaskan perbedaan Singly, Doubly, dan Circular Linked List. Berikan satu contoh use case yang paling cocok untuk masing-masing.

**2.** Mengapa LRU Cache membutuhkan DUA struktur data? Apa yang terjadi jika hanya menggunakan linked list saja atau HashMap saja?

**3.** Jelaskan perbedaan DFS dan BFS. Untuk masing-masing, berikan satu masalah yang paling tepat diselesaikan dengan algoritma tersebut.

**4.** Apa yang dimaksud dengan "degenerate BST"? Apa dampaknya pada kompleksitas, dan bagaimana cara mencegahnya?

**5.** Sebutkan perbedaan Stack dan Queue. Berikan masing-masing dua contoh aplikasi nyata.

### Bagian B: Analisis (35 poin)

**6.** (10 poin) Diberikan kode berikut, analisis kompleksitasnya:
```javascript
function prosesData(arr) {
  const result = {};
  for (const item of arr) {          // O(?)
    if (!result[item.id]) {
      result[item.id] = item;         // O(?)
    }
    const found = arr.find(x => x.relatedId === item.id); // O(?)
    if (found) result[item.id].related = found;
  }
  return Object.values(result).sort((a, b) => a.id - b.id); // O(?)
}
// Total kompleksitas: O(?)
// Bagaimana cara mengoptimalkannya ke O(n log n)?
```

**7.** (10 poin) Sistem antrian tiket konser perlu mendukung:
* Pengguna VIP masuk antrian di posisi terdepan
* Pengguna reguler masuk di belakang
* Tampilkan 10 antrian terdepan
* Batalkan tiket pengguna tertentu by ID

Rekomendasikan struktur data terbaik untuk setiap operasi dan justifikasi.

**8.** (15 poin) Kamu diminta merancang sistem cache untuk API endpoint yang sering dipanggil. API menerima `userId` dan mengembalikan profil user. Database call membutuhkan 100ms, tapi jika di-cache hanya 1ms. Cache bisa menyimpan maksimal 1000 entri.

a) Struktur data apa yang kamu gunakan? Jelaskan arsitekturnya.
b) Apa yang terjadi saat cache penuh?
c) Apa kompleksitas get dan put?
d) Bagaimana kamu menghandle cache invalidation ketika data user berubah?

### Bagian C: Coding (40 poin)

**9.** (20 poin) Implementasikan fungsi `serializeBST(root)` dan `deserializeBST(data)` yang:
* `serializeBST` mengubah BST menjadi string (untuk disimpan/dikirim)
* `deserializeBST` mengubah string kembali menjadi BST yang bisa di-query
* BST yang di-deserialize harus menghasilkan in-order traversal yang sama dengan aslinya

**10.** (20 poin) Implementasikan sistem deteksi fraud sederhana:
* Input: array transaksi `{ id, userId, nominal, timestamp }`
* Deteksi "velocity fraud": jika satu user melakukan lebih dari 5 transaksi dalam 10 menit, flag semua transaksinya
* Return: array userId yang di-flag
* Kompleksitas target: O(n log n) atau lebih baik

---

## 📌 Tips Mengerjakan UAS

**Strategi soal teori:**
* Jawab dengan terminologi yang tepat, bukan bahasa sehari-hari
* Sertakan kompleksitas dalam setiap penjelasan struktur data
* Untuk perbandingan, gunakan tabel — lebih jelas dan terstruktur

**Strategi soal analisis:**
* Hitung kompleksitas baris per baris sebelum kesimpulan
* Sebutkan trade-off — tidak ada jawaban "selalu terbaik"
* Untuk desain sistem: mulai dari operasi yang dibutuhkan, baru pilih struktur data

**Strategi soal coding:**
* Tulis base case / validasi dulu
* Gunakan nama variabel yang deskriptif
* Tambahkan komentar Big-O di setiap fungsi penting

## Checklist Pre-UAS ✅

- [ ] Hafal kompleksitas semua operasi 6 struktur data utama
- [ ] Bisa implementasi Stack dan Queue dari scratch (tanpa library)
- [ ] Paham DFS vs BFS dan kapan menggunakan masing-masing
- [ ] Bisa trace BST insert/delete/traversal secara manual
- [ ] Paham LRU Cache: struktur, operasi, dan alasan butuh dua DS
- [ ] Hafalkan 7 sorting algorithm dan kompleksitasnya
- [ ] Paham closure, reference vs value, dan module pattern
- [ ] Sudah latihan soal-soal di atas minimal sekali

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Sedgewick & Wayne (2011). Algorithms | Cormen et al. (2009). Introduction to Algorithms | Kyle Simpson. You Don't Know JS*
