# Pertemuan 9: Circular Linked List & Review Linked List

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan struktur dan karakteristik circular linked list
* Mengimplementasikan circular singly dan doubly linked list
* Membedakan kapan menggunakan singly, doubly, vs circular linked list
* Mengimplementasikan LRU Cache menggunakan doubly linked list + hash map
* Menganalisis trade-off semua varian linked list dalam satu tabel komprehensif

---

## 📖 Pengantar (Hook)

Ada masalah unik di sistem streaming musik seperti Spotify: **playlist loop**.

Kamu punya 10 lagu. Setelah lagu ke-10 selesai, sistem harus kembali ke lagu ke-1 secara otomatis — tanpa kamu melakukan sesuatu. Bagaimana cara terbaik merepresentasikan ini di kode?

Dengan circular linked list, tail node tidak menunjuk ke `null` — tapi kembali ke `head`. Traversal alami menjadi loop tanpa batas. Ini bukan bug, ini fitur.

---

## 🧩 Konsep Utama

### Circular Linked List

**Circular Singly LL** — sama seperti singly LL, tapi tail.next menunjuk kembali ke head.

```
     ┌────────────────────────────┐
     ↓                            │
   [10|●]→[20|●]→[30|●]→[40|●]──┘
    HEAD
```

**Circular Doubly LL** — sama seperti doubly LL, tapi tail.next → head dan head.prev → tail.

```
     ┌─────────────────────────────────┐
     ↓                                 │
   [●|10|●]⟷[●|20|●]⟷[●|30|●]⟷[●|40|●]
     │                                 ↑
     └─────────────────────────────────┘
    HEAD/TAIL terhubung di kedua ujung
```

### Keunikan Circular LL

* **Tidak ada null pointer** — tidak ada "akhir" list, bisa terus loop
* **Akses dari node manapun** — bisa traversal penuh mulai dari node manapun
* **Efisien untuk round-robin** — giliran bergantian tanpa restart manual

### Perbandingan Semua Varian Linked List

| Varian | Traversal | Insert Head | Insert Tail | Delete Head | Delete Tail | Use Case |
|---|---|---|---|---|---|---|
| Singly LL | Forward | O(1) | O(n) | O(1) | O(n) | Stack, sederhana |
| Singly LL + tail | Forward | O(1) | **O(1)** | O(1) | O(n) | Queue |
| Doubly LL | Both | O(1) | O(1)* | O(1) | O(1)* | Deque, LRU Cache |
| Circular Singly | Loop | O(1) | O(1)** | O(1) | O(n) | Round-robin |
| Circular Doubly | Loop (both) | O(1) | O(1) | O(1) | O(1) | OS process scheduling |

*dengan tail pointer | **dengan akses ke last node

---

## 🧠 Ilustrasi / Analogi

| Konsep | Analogi |
|---|---|
| **Circular LL** | Jalur kereta MRT yang melingkar — tidak ada stasiun akhir |
| **Round-robin scheduling** | Distribusi giliran kerja di tim: A→B→C→A→B→C... |
| **LRU Cache** | Rak buku terbatas — buku terbaru ditaruh depan, buku terlama dibuang kalau rak penuh |
| **Sentinel node** | Dummy node di awal/akhir — penyederhanaan logic untuk edge case |

---

## 💻 Contoh Teknis

```javascript
// ============= CIRCULAR SINGLY LINKED LIST =============
class CircularSinglyLL {
  constructor() {
    this.head = null;
    this.tail = null; // tail pointer untuk O(1) append
    this.size = 0;
  }

  append(data) {
    const node = new Node(data);
    if (!this.head) {
      this.head = this.tail = node;
      node.next = this.head; // menunjuk ke dirinya sendiri
    } else {
      node.next = this.head; // node baru menunjuk ke head
      this.tail.next = node; // tail lama menunjuk ke node baru
      this.tail = node; // update tail
    }
    this.size++;
  }

  // Traversal — hati-hati: harus berhenti saat kembali ke head!
  toArray() {
    if (!this.head) return [];
    const result = [];
    let current = this.head;
    do {
      result.push(current.data);
      current = current.next;
    } while (current !== this.head); // berhenti di head, bukan null!
    return result;
  }

  // Round-robin: ambil elemen pertama, rotasi ke belakang
  rotate() {
    if (!this.head) return null;
    const data = this.head.data;
    this.head = this.head.next;
    this.tail = this.tail.next; // tail juga bergeser (karena circular)
    // Tidak perlu update pointer — circular sudah handle ini
    return data;
  }
}

// ============= LRU CACHE (Doubly LL + HashMap) =============
// LRU = Least Recently Used
// Saat cache penuh, buang elemen yang paling lama tidak diakses
class LRUCache {
  constructor(kapasitas) {
    this.kapasitas = kapasitas;
    this.map = new Map(); // key → node (untuk O(1) lookup)

    // Dummy head dan tail untuk sederhanakan edge case
    this.head = { key: null, value: null, prev: null, next: null };
    this.tail = { key: null, value: null, prev: null, next: null };
    this.head.next = this.tail;
    this.tail.prev = this.head;
    this.size = 0;
  }

  // Pindahkan node ke depan (most recently used)
  _moveToFront(node) {
    // Lepas dari posisi saat ini
    node.prev.next = node.next;
    node.next.prev = node.prev;

    // Taruh setelah head
    node.next = this.head.next;
    node.prev = this.head;
    this.head.next.prev = node;
    this.head.next = node;
  }

  // Get — O(1)
  get(key) {
    if (!this.map.has(key)) return -1;
    const node = this.map.get(key);
    this._moveToFront(node); // diakses = pindah ke depan
    return node.value;
  }

  // Put — O(1)
  put(key, value) {
    if (this.map.has(key)) {
      // Update existing
      const node = this.map.get(key);
      node.value = value;
      this._moveToFront(node);
      return;
    }

    // Buat node baru
    const node = { key, value, prev: null, next: null };
    this.map.set(key, node);

    // Taruh di depan (most recently used)
    node.next = this.head.next;
    node.prev = this.head;
    this.head.next.prev = node;
    this.head.next = node;
    this.size++;

    // Jika melebihi kapasitas, hapus yang paling lama (sebelum tail)
    if (this.size > this.kapasitas) {
      const lru = this.tail.prev; // node sebelum dummy tail
      lru.prev.next = this.tail;
      this.tail.prev = lru.prev;
      this.map.delete(lru.key);
      this.size--;
    }
  }

  toArray() {
    const result = [];
    let current = this.head.next;
    while (current !== this.tail) {
      result.push(`${current.key}:${current.value}`);
      current = current.next;
    }
    return result;
  }
}

// ============= TEST LRU CACHE =============
const cache = new LRUCache(3);
cache.put('user:1', { nama: 'Andi', saldo: 500000 });
cache.put('user:2', { nama: 'Budi', saldo: 200000 });
cache.put('user:3', { nama: 'Cici', saldo: 800000 });
console.log(cache.toArray()); // ['user:3:...', 'user:2:...', 'user:1:...']

cache.get('user:1'); // Andi diakses, pindah ke depan
console.log(cache.toArray()); // ['user:1:...', 'user:3:...', 'user:2:...']

cache.put('user:4', { nama: 'Dodi', saldo: 100000 }); // cache penuh, buang LRU
// LRU = user:2 (paling lama tidak diakses)
console.log(cache.toArray()); // ['user:4:...', 'user:1:...', 'user:3:...']
console.log(cache.get('user:2')); // -1 (sudah di-evict)
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### LRU Cache untuk Database Connection Pool

**Skenario:** Backend fintech melakukan ribuan query per menit ke database untuk cek saldo, riwayat transaksi, dan profil merchant. Membuka koneksi DB baru setiap query mahal — ada overhead autentikasi + TCP handshake.

**Solusi:** Connection Pool dengan LRU Cache
* Pool menyimpan koneksi aktif (max 50 koneksi)
* Koneksi yang paling baru dipakai → tetap di pool
* Koneksi yang paling lama idle → dibuang (tutup) saat pool penuh
* Setiap request → cek pool dulu (O(1)), buka baru hanya jika tidak ada

```javascript
class DBConnectionPool {
  constructor(maxConnections) {
    this.pool = new LRUCache(maxConnections);
    this._queryCount = 0;
    this._cacheHits = 0;
  }

  async getConnection(dbUrl) {
    let conn = this.pool.get(dbUrl);

    if (conn === -1) {
      // Cache miss — buka koneksi baru
      conn = await openConnection(dbUrl); // simulasi O(100ms)
      this.pool.put(dbUrl, conn);
    } else {
      this._cacheHits++;
    }

    this._queryCount++;
    return conn;
  }

  getHitRate() {
    return `${((this._cacheHits / this._queryCount) * 100).toFixed(1)}%`;
  }
}
```

**Hasil:** Dengan connection pool LRU, 95%+ query menggunakan koneksi existing → latensi turun dari ~150ms ke ~5ms. Ini adalah optimasi nyata di sistem high-traffic.

---

## 📊 Visualisasi: LRU Cache Operasi

```
LRU Cache kapasitas 3:
Urutan: [MRU ... LRU] (kiri = paling baru diakses)

PUT('A', 1):  [A]
PUT('B', 2):  [B, A]
PUT('C', 3):  [C, B, A]
GET('A'):     [A, C, B]  ← A dipindah ke depan
PUT('D', 4):  [D, A, C]  ← B di-evict (LRU), D masuk
GET('C'):     [C, D, A]  ← C dipindah ke depan
GET('B'):     -1          ← B sudah di-evict
```

---

## ⚠️ Kesalahan Umum

1. **Infinite loop saat traversal circular LL** → Jangan gunakan `while (current)` — `current` tidak pernah null di circular LL. Gunakan `do...while (current !== head)`.

2. **Tidak update tail saat delete head di circular LL** → Jika head dihapus, `tail.next` harus diupdate ke head baru.

3. **LRU Cache: lupa delete dari Map saat evict** → Node di doubly LL dihapus, tapi entri di HashMap masih ada → memory leak! Selalu hapus dari kedua struktur.

4. **Singleton pointer di circular LL (size = 1)** → Node tunggal: `node.next = node` (menunjuk ke dirinya). Operasi delete harus handle kasus ini secara eksplisit.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Mengapa traversal circular linked list membutuhkan `do...while` bukan `while`? Apa yang terjadi jika menggunakan `while (current !== null)`?

b) Jelaskan mengapa LRU Cache membutuhkan DUA struktur data (doubly linked list + hash map). Apa yang terjadi jika hanya menggunakan salah satu?

c) Sebutkan dua use case nyata (selain yang sudah disebutkan) di mana circular linked list adalah pilihan paling tepat.

### Soal 2 — Coding

1. Implementasikan **Round-Robin Load Balancer** menggunakan circular linked list:
   ```javascript
   const balancer = new RoundRobinLB(['server-1', 'server-2', 'server-3']);
   balancer.next(); // 'server-1'
   balancer.next(); // 'server-2'
   balancer.next(); // 'server-3'
   balancer.next(); // 'server-1' (kembali ke awal)
   balancer.addServer('server-4'); // tambah server baru
   balancer.removeServer('server-2'); // hapus server yang down
   ```

2. Implementasikan `isPalindrome(linkedList)` menggunakan doubly linked list — cek apakah list adalah palindrom dari depan dan belakang sekaligus (gunakan head dan tail pointer).

---

## 📌 Ringkasan

* **Circular LL** — tail menunjuk ke head, tidak ada null pointer, ideal untuk loop/rotation
* **Circular singly LL** — tambahkan tail pointer untuk O(1) append + O(1) rotate
* **Circular doubly LL** — traversal dua arah + loop, dipakai di OS process scheduling
* **LRU Cache** = Doubly LL (urutan akses) + HashMap (O(1) lookup) — O(1) get dan put
* **LRU eviction** — saat cache penuh, node sebelum dummy tail (LRU) dihapus
* Gunakan **sentinel/dummy nodes** untuk sederhanakan logic di edge case (head/tail kosong)
* **Linked list keseluruhan:**
  * Singly LL → Stack implementation
  * Singly LL + tail → Queue implementation
  * Doubly LL → Deque, browser history, LRU Cache
  * Circular LL → Playlist, round-robin scheduler

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | LeetCode #146 LRU Cache | GeeksforGeeks — Circular Linked List*
