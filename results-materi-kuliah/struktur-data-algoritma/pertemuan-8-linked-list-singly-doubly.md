# Pertemuan 8: Linked List — Singly & Doubly

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan struktur node dan pointer dalam linked list
* Membedakan singly linked list vs doubly linked list
* Mengimplementasikan operasi insert, delete, dan search pada linked list
* Menganalisis kompleksitas operasi linked list vs array
* Menentukan kapan linked list lebih tepat digunakan daripada array

---

## 📖 Pengantar (Hook)

Array sangat efisien untuk akses random — kamu bisa ambil elemen ke-1000 dalam O(1).

Tapi bagaimana kalau kamu perlu sering menyisipkan elemen di tengah-tengah data? Setiap `insert` di tengah array membutuhkan pergeseran semua elemen di belakangnya — O(n) operasi.

Bayangkan sistem antrian transaksi di payment gateway. Transaksi masuk dan keluar terus-menerus dari berbagai posisi. Dengan linked list, insert dan delete di posisi yang sudah diketahui menjadi O(1). Inilah mengapa linked list ada.

---

## 🧩 Konsep Utama

### Apa itu Linked List?

Linked list adalah kumpulan **node** yang terhubung lewat **pointer**. Setiap node menyimpan:
1. **Data** — nilai yang disimpan
2. **Pointer** — alamat node berikutnya (dan sebelumnya untuk doubly)

Tidak seperti array yang menyimpan elemen secara berurutan di memori, node-node linked list bisa tersebar di mana saja di Heap.

```
ARRAY (berurutan di memori):
[100][200][300][400][500]
  ↑ semua bersebelahan

LINKED LIST (tersebar di memori):
[100|●]→[200|●]→[300|●]→[400|●]→[500|null]
  Addr A   Addr X   Addr Q   Addr M
```

### Singly Linked List

Setiap node hanya punya pointer ke node **berikutnya** (`next`). Traversal hanya bisa ke depan.

```
HEAD
  ↓
[10|●]→[20|●]→[30|●]→[40|null]
```

### Doubly Linked List

Setiap node punya pointer ke node **berikutnya** (`next`) DAN **sebelumnya** (`prev`). Traversal bisa ke depan dan ke belakang.

```
HEAD                            TAIL
  ↓                               ↓
[null|10|●]⟷[●|20|●]⟷[●|30|●]⟷[●|40|null]
```

### Kompleksitas Operasi

| Operasi | Array | Singly LL | Doubly LL |
|---|---|---|---|
| Akses by index | **O(1)** | O(n) | O(n) |
| Search | O(n) | O(n) | O(n) |
| Insert di awal | O(n) | **O(1)** | **O(1)** |
| Insert di akhir | O(1)* | O(n) / O(1)** | **O(1)** |
| Insert di tengah | O(n) | O(n) | O(n) |
| Delete di awal | O(n) | **O(1)** | **O(1)** |
| Delete di akhir | O(1) | O(n) | **O(1)** |
| Delete di tengah | O(n) | O(n) | O(n) |

*amortized | **O(1) jika ada tail pointer

---

## 🧠 Ilustrasi / Analogi

| Konsep | Analogi |
|---|---|
| **Node** | Gerbong kereta — berisi penumpang (data) + kait ke gerbong berikutnya |
| **Pointer (next)** | Kait/penghubung antar gerbong |
| **Head** | Lokomotif — titik awal traversal |
| **Tail** | Gerbong terakhir — titik akhir |
| **Singly LL** | Kereta yang hanya bisa bergerak maju |
| **Doubly LL** | Kereta yang bisa maju dan mundur |
| **null pointer** | Kait yang menggantung — menandai akhir list |

---

## 💻 Contoh Teknis

```javascript
// ============= NODE CLASS =============
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}

// ============= SINGLY LINKED LIST =============
class SinglyLinkedList {
  constructor() {
    this.head = null;
    this.size = 0;
  }

  // Insert di awal — O(1)
  prepend(data) {
    const node = new Node(data);
    node.next = this.head;
    this.head = node;
    this.size++;
  }

  // Insert di akhir — O(n)
  append(data) {
    const node = new Node(data);
    if (!this.head) {
      this.head = node;
    } else {
      let current = this.head;
      while (current.next) {
        current = current.next;
      }
      current.next = node;
    }
    this.size++;
  }

  // Insert di posisi tertentu — O(n)
  insertAt(data, index) {
    if (index < 0 || index > this.size) throw new Error('Index out of bounds');
    if (index === 0) return this.prepend(data);

    const node = new Node(data);
    let current = this.head;
    for (let i = 0; i < index - 1; i++) {
      current = current.next;
    }
    node.next = current.next;
    current.next = node;
    this.size++;
  }

  // Delete di awal — O(1)
  deleteFirst() {
    if (!this.head) return null;
    const deleted = this.head;
    this.head = this.head.next;
    this.size--;
    return deleted.data;
  }

  // Delete node dengan nilai tertentu — O(n)
  delete(data) {
    if (!this.head) return false;

    // Hapus head jika match
    if (this.head.data === data) {
      this.head = this.head.next;
      this.size--;
      return true;
    }

    let current = this.head;
    while (current.next) {
      if (current.next.data === data) {
        current.next = current.next.next; // bypass node yang dihapus
        this.size--;
        return true;
      }
      current = current.next;
    }
    return false;
  }

  // Search — O(n)
  search(data) {
    let current = this.head;
    let index = 0;
    while (current) {
      if (current.data === data) return index;
      current = current.next;
      index++;
    }
    return -1;
  }

  // Traversal — O(n)
  toArray() {
    const result = [];
    let current = this.head;
    while (current) {
      result.push(current.data);
      current = current.next;
    }
    return result;
  }

  // Reverse list — O(n)
  reverse() {
    let prev = null;
    let current = this.head;
    while (current) {
      const next = current.next;
      current.next = prev;
      prev = current;
      current = next;
    }
    this.head = prev;
  }
}

// ============= DOUBLY LINKED LIST =============
class DoublyNode {
  constructor(data) {
    this.data = data;
    this.prev = null;
    this.next = null;
  }
}

class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  // Insert di akhir — O(1) karena ada tail pointer!
  append(data) {
    const node = new DoublyNode(data);
    if (!this.head) {
      this.head = this.tail = node;
    } else {
      node.prev = this.tail;
      this.tail.next = node;
      this.tail = node;
    }
    this.size++;
  }

  // Delete di akhir — O(1) karena ada tail pointer!
  deleteLast() {
    if (!this.tail) return null;
    const deleted = this.tail.data;
    if (this.head === this.tail) {
      this.head = this.tail = null;
    } else {
      this.tail = this.tail.prev;
      this.tail.next = null;
    }
    this.size--;
    return deleted;
  }

  // Traversal mundur (hanya mungkin di doubly)
  toArrayReverse() {
    const result = [];
    let current = this.tail;
    while (current) {
      result.push(current.data);
      current = current.prev;
    }
    return result;
  }
}

// ============= TEST =============
const ll = new SinglyLinkedList();
ll.append(10);
ll.append(20);
ll.append(30);
ll.prepend(5);
console.log(ll.toArray()); // [5, 10, 20, 30]

ll.delete(20);
console.log(ll.toArray()); // [5, 10, 30]

ll.reverse();
console.log(ll.toArray()); // [30, 10, 5]

console.log('Search 10:', ll.search(10)); // 1
console.log('Size:', ll.size); // 3
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Sistem Antrian Transaksi dengan Prioritas di Payment Gateway

**Skenario:** Payment gateway memproses tiga jenis transaksi dengan prioritas berbeda:
* **HIGH** — pembayaran merchant besar (> Rp 10 juta), harus diproses dalam 1 detik
* **MEDIUM** — transfer reguler
* **LOW** — top-up kecil, proses dalam batch

Dengan doubly linked list, sistem bisa dengan cepat:
* Insert transaksi HIGH di awal (O(1) prepend)
* Insert transaksi LOW di akhir (O(1) append dengan tail pointer)
* Hapus transaksi yang sudah diproses dari awal (O(1))
* Hapus transaksi expired dari akhir (O(1))

```javascript
class TransaksiAntrian {
  constructor() {
    this.dll = new DoublyLinkedList();
    this._processed = 0;
  }

  masuk(transaksi) {
    if (transaksi.prioritas === 'HIGH') {
      // Sisipkan di awal — O(1)
      const node = new DoublyNode(transaksi);
      if (!this.dll.head) {
        this.dll.head = this.dll.tail = node;
      } else {
        node.next = this.dll.head;
        this.dll.head.prev = node;
        this.dll.head = node;
      }
      this.dll.size++;
    } else {
      this.dll.append(transaksi); // LOW/MEDIUM di akhir — O(1)
    }
    console.log(`➕ Masuk antrian [${transaksi.prioritas}]: ${transaksi.id}`);
  }

  proses() {
    const trx = this.dll.deleteLast(); // ambil dari depan (yang paling prioritas)
    if (trx) {
      this._processed++;
      console.log(`✅ Diproses: ${trx.id} | Total: ${this._processed}`);
    }
    return trx;
  }

  panjangAntrian() { return this.dll.size; }
}

const antrian = new TransaksiAntrian();
antrian.masuk({ id: 'TRX-001', prioritas: 'LOW', nominal: 50000 });
antrian.masuk({ id: 'TRX-002', prioritas: 'MEDIUM', nominal: 500000 });
antrian.masuk({ id: 'TRX-003', prioritas: 'HIGH', nominal: 15000000 });
// Antrian: HIGH(TRX-003) → MEDIUM(TRX-002) → LOW(TRX-001)
antrian.proses(); // TRX-003 diproses dulu (HIGH priority)
```

---

## 📊 Visualisasi: Delete Operasi

```
Singly LL: Delete node '20'
SEBELUM: [10] → [20] → [30] → [40]
                  ↑ hapus ini

PROSES:
  - Traverse sampai current.next.data === 20
  - current = node '10'
  - current.next = current.next.next  (bypass '20')
  - '20' tidak ada referensi → GC

SESUDAH: [10] → [30] → [40]


Doubly LL: Delete node '30' (diketahui langsung, tanpa traverse)
SEBELUM: [10]⟷[20]⟷[30]⟷[40]
                       ↑ hapus ini

PROSES (O(1) jika node sudah diketahui):
  - node.prev.next = node.next  (20.next = 40)
  - node.next.prev = node.prev  (40.prev = 20)

SESUDAH: [10]⟷[20]⟷[40]
```

---

## ⚠️ Kesalahan Umum

1. **Lupa handle kasus head = null saat delete** → Selalu cek apakah list kosong sebelum operasi delete atau traversal.

2. **Memory leak: lupa update tail di Doubly LL** → Saat delete node terakhir, pastikan `tail` diupdate ke node sebelumnya.

3. **Infinite loop saat traversal** → Jika ada bug yang membuat `node.next` menunjuk ke node sebelumnya (circular tanpa sengaja), traversal `while (current)` akan loop selamanya.

4. **Mengira linked list selalu lebih baik dari array** → Linked list lebih lambat untuk akses random (O(n) vs O(1)). Gunakan linked list hanya ketika insert/delete di awal/akhir adalah operasi dominan.

5. **Tidak memanfaatkan tail pointer untuk append** → Tanpa tail pointer, append ke singly LL adalah O(n). Tambahkan tail pointer untuk O(1) append.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Jelaskan mengapa delete di tengah linked list tetap O(n) meskipun penghapusan node itu sendiri adalah O(1).

b) Kapan kamu akan memilih doubly linked list dibanding singly linked list? Sebutkan minimal dua skenario konkret.

c) Apa keuntungan array dibanding linked list? Sebutkan skenario di mana array jelas lebih tepat.

### Soal 2 — Coding

1. Implementasikan method `detectCycle(list)` — return `true` jika linked list memiliki siklus (node next-nya menunjuk ke node sebelumnya). Hint: gunakan algoritma Floyd's Tortoise and Hare (dua pointer dengan kecepatan berbeda).

2. Implementasikan `mergeSorted(list1, list2)` — merge dua singly linked list yang sudah terurut menjadi satu linked list terurut. Big-O: O(n + m).

---

## 📌 Ringkasan

* **Linked list** = kumpulan node yang terhubung lewat pointer — tidak berurutan di memori
* **Singly LL** — satu pointer (`next`), traversal hanya ke depan
* **Doubly LL** — dua pointer (`next` + `prev`), traversal dua arah, O(1) delete di akhir dengan tail pointer
* **Kelebihan vs Array:**
  * LL: O(1) insert/delete di awal, O(1) delete di akhir (doubly)
  * Array: O(1) akses random, cache-friendly (data berurutan di memori)
* **Gunakan LL ketika:** banyak insert/delete di kepala/ekor, ukuran data sering berubah
* **Gunakan Array ketika:** banyak akses random by index, data relatif statis
* Doubly LL adalah dasar dari struktur data **Deque** (Double-ended Queue) dan **LRU Cache**

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Sedgewick & Wayne (2011). Algorithms | GeeksforGeeks — Linked List*
