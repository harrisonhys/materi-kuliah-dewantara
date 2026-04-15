# Pertemuan 12: Tree & Binary Search Tree (BST)

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan terminologi tree: root, node, edge, leaf, height, depth
* Membedakan Binary Tree vs Binary Search Tree (BST)
* Mengimplementasikan BST dengan operasi insert, search, dan delete
* Melakukan tree traversal: In-order, Pre-order, Post-order
* Menganalisis kompleksitas BST dan menjelaskan kondisi worst-case

---

## 📖 Pengantar (Hook)

Database menyimpan miliaran record. Bagaimana cara mencari satu record dari sekian banyak data dalam hitungan millisecond?

Jawabannya bukan linear search (O(n)) — database menggunakan **tree-based index** (B-Tree, B+ Tree). Setiap kali kamu menulis `WHERE id = 12345` di SQL, query optimizer memanfaatkan struktur pohon untuk menemukan record tersebut dalam O(log n) operasi.

Binary Search Tree adalah fondasi untuk memahami mengapa hal ini mungkin.

---

## 🧩 Konsep Utama

### Terminologi Tree

```
         ┌───┐
         │ 8 │  ← Root (tidak punya parent)
         └─┬─┘
      ┌────┴────┐
    ┌─┴─┐     ┌─┴─┐
    │ 3 │     │10 │  ← Internal nodes
    └─┬─┘     └─┬─┘
  ┌───┴───┐     └───┐
┌─┴─┐   ┌─┴─┐   ┌───┴┐
│ 1 │   │ 6 │   │ 14 │ ← Leaf nodes (tidak punya anak)
└───┘   └─┬─┘   └────┘
       ┌───┴───┐
     ┌─┴─┐   ┌─┴─┐
     │ 4 │   │ 7 │
     └───┘   └───┘
```

| Istilah | Definisi |
|---|---|
| **Root** | Node paling atas, tidak punya parent |
| **Node** | Setiap elemen dalam tree |
| **Edge** | Koneksi antara dua node |
| **Leaf** | Node tanpa anak |
| **Parent/Child** | Node di atas/bawah suatu node |
| **Height** | Jumlah edge dari root ke leaf terdalam |
| **Depth** | Jumlah edge dari root ke node tertentu |
| **Subtree** | Tree yang dibentuk oleh suatu node dan semua keturunannya |

### Binary Tree

Binary tree adalah tree di mana setiap node memiliki **maksimal dua anak**: `left` dan `right`.

### Binary Search Tree (BST)

BST adalah binary tree dengan satu aturan kritis:
* Semua node di **subtree kiri** < node saat ini
* Semua node di **subtree kanan** > node saat ini
* Aturan ini berlaku **rekursif** di setiap subtree

```
BST yang valid:
       8
      / \
     3   10
    / \    \
   1   6   14
      / \
     4   7

Aturan: 1 < 3 < 4 < 6 < 7 < 8 < 10 < 14
```

### Kompleksitas BST

| Operasi | Average | Worst (degenerate) |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

**Worst case** terjadi ketika BST menjadi **degenerate** (mirip linked list) — semua node hanya punya satu anak, karena insert selalu terurut:

```
Degenerate BST (insert: 1, 2, 3, 4, 5):
1
 \
  2
   \
    3
     \
      4
       \
        5
```

Ini kenapa ada **Self-balancing BST** (AVL Tree, Red-Black Tree) — tapi itu topik lanjutan.

---

## 🧠 Ilustrasi / Analogi

| Konsep | Analogi |
|---|---|
| **Tree** | Silsilah keluarga — satu nenek moyang (root), cabang ke anak-cucu |
| **BST** | Kamus — kata yang lebih kecil di halaman kiri, lebih besar di kanan |
| **In-order traversal** | Membaca semua kata di kamus dari A ke Z (menghasilkan urutan sorted!) |
| **Height BST** | Jumlah "generasi" dalam silsilah |
| **Degenerate BST** | Silsilah keluarga yang setiap orang hanya punya satu anak — menjadi rantai lurus |

---

## 💻 Contoh Teknis

```javascript
// ============= BST NODE =============
class BSTNode {
  constructor(data) {
    this.data = data;
    this.left = null;
    this.right = null;
  }
}

// ============= BINARY SEARCH TREE =============
class BST {
  constructor() {
    this.root = null;
  }

  // INSERT — O(log n) average
  insert(data) {
    this.root = this._insertRec(this.root, data);
  }

  _insertRec(node, data) {
    if (!node) return new BSTNode(data); // base case: posisi ditemukan

    if (data < node.data) {
      node.left = this._insertRec(node.left, data);
    } else if (data > node.data) {
      node.right = this._insertRec(node.right, data);
    }
    // data === node.data: duplikat, abaikan (atau handle sesuai kebutuhan)

    return node;
  }

  // SEARCH — O(log n) average
  search(data) {
    return this._searchRec(this.root, data);
  }

  _searchRec(node, data) {
    if (!node) return false;    // base case: tidak ditemukan
    if (data === node.data) return true; // ditemukan!

    if (data < node.data) return this._searchRec(node.left, data);
    return this._searchRec(node.right, data);
  }

  // DELETE — O(log n) average
  delete(data) {
    this.root = this._deleteRec(this.root, data);
  }

  _deleteRec(node, data) {
    if (!node) return null; // base case: tidak ditemukan

    if (data < node.data) {
      node.left = this._deleteRec(node.left, data);
    } else if (data > node.data) {
      node.right = this._deleteRec(node.right, data);
    } else {
      // NODE DITEMUKAN — tiga kasus:
      // Kasus 1: Leaf node (tidak punya anak)
      if (!node.left && !node.right) return null;

      // Kasus 2: Hanya satu anak
      if (!node.left) return node.right;
      if (!node.right) return node.left;

      // Kasus 3: Dua anak → ganti dengan in-order successor
      // (node terkecil di subtree kanan)
      const successor = this._findMin(node.right);
      node.data = successor.data;
      node.right = this._deleteRec(node.right, successor.data);
    }

    return node;
  }

  _findMin(node) {
    while (node.left) node = node.left;
    return node;
  }

  // ============= TREE TRAVERSAL =============

  // IN-ORDER: Left → Root → Right
  // Hasil: SORTED (urutan menaik)!
  inOrder(node = this.root, result = []) {
    if (!node) return result;
    this.inOrder(node.left, result);
    result.push(node.data);
    this.inOrder(node.right, result);
    return result;
  }

  // PRE-ORDER: Root → Left → Right
  // Gunakan: copy tree, serialisasi
  preOrder(node = this.root, result = []) {
    if (!node) return result;
    result.push(node.data);
    this.preOrder(node.left, result);
    this.preOrder(node.right, result);
    return result;
  }

  // POST-ORDER: Left → Right → Root
  // Gunakan: hapus tree, evaluasi ekspresi
  postOrder(node = this.root, result = []) {
    if (!node) return result;
    this.postOrder(node.left, result);
    this.postOrder(node.right, result);
    result.push(node.data);
    return result;
  }

  // HEIGHT of tree
  height(node = this.root) {
    if (!node) return -1;
    return 1 + Math.max(this.height(node.left), this.height(node.right));
  }
}

// ============= TEST =============
const bst = new BST();
[8, 3, 10, 1, 6, 14, 4, 7].forEach(n => bst.insert(n));

console.log('In-order:  ', bst.inOrder());  // [1, 3, 4, 6, 7, 8, 10, 14] — SORTED!
console.log('Pre-order: ', bst.preOrder()); // [8, 3, 1, 6, 4, 7, 10, 14]
console.log('Post-order:', bst.postOrder());// [1, 4, 7, 6, 3, 14, 10, 8]
console.log('Height:    ', bst.height());   // 3
console.log('Search 6:  ', bst.search(6));  // true
console.log('Search 5:  ', bst.search(5));  // false

bst.delete(3);
console.log('After delete 3:', bst.inOrder()); // [1, 4, 6, 7, 8, 10, 14]
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### BST untuk Pencarian Range Transaksi

**Skenario:** Risk management system perlu secara real-time mengidentifikasi transaksi dengan nominal dalam range tertentu (misalnya, semua transaksi antara Rp 9.000.000 – Rp 10.000.000 sebagai indikasi "structuring" atau pecah transaksi untuk menghindari batas pelaporan).

Dengan BST, range query bisa dilakukan dalam O(log n + k) di mana k adalah jumlah hasil:

```javascript
class TransaksiBST extends BST {
  // Range search: cari semua transaksi antara minNominal dan maxNominal
  rangeSearch(minVal, maxVal, node = this.root, result = []) {
    if (!node) return result;

    // Jika node.data > minVal, ada kemungkinan di subtree kiri
    if (node.data > minVal) {
      this.rangeSearch(minVal, maxVal, node.left, result);
    }

    // Cek apakah node ini dalam range
    if (node.data >= minVal && node.data <= maxVal) {
      result.push(node.data);
    }

    // Jika node.data < maxVal, ada kemungkinan di subtree kanan
    if (node.data < maxVal) {
      this.rangeSearch(minVal, maxVal, node.right, result);
    }

    return result;
  }

  // Cari k transaksi terbesar (untuk laporan top merchant)
  kLargest(k, node = this.root, result = []) {
    if (!node || result.length >= k) return result;

    // Reverse in-order: Right → Root → Left (descending)
    this.kLargest(k, node.right, result);
    if (result.length < k) result.push(node.data);
    this.kLargest(k, node.left, result);

    return result;
  }
}

const trxBST = new TransaksiBST();
[5000000, 9500000, 3000000, 9800000, 10000000, 15000000, 8000000]
  .forEach(n => trxBST.insert(n));

// Cari potensi structuring (transaksi 9-10 juta)
const suspicious = trxBST.rangeSearch(9000000, 10000000);
console.log('Transaksi mencurigakan:', suspicious); // [9500000, 9800000, 10000000]

// Top 3 transaksi terbesar
console.log('Top 3:', trxBST.kLargest(3)); // [15000000, 10000000, 9800000]
```

---

## 📊 Visualisasi: Tree Traversal

```
Tree:
       8
      / \
     3   10
    / \    \
   1   6   14
      / \
     4   7

In-order   (L-Root-R): 1, 3, 4, 6, 7, 8, 10, 14  ← SORTED!
Pre-order  (Root-L-R): 8, 3, 1, 6, 4, 7, 10, 14  ← Root pertama
Post-order (L-R-Root): 1, 4, 7, 6, 3, 14, 10, 8  ← Root terakhir

Ingat dengan analogi:
- In-order    = kunjungi tangan kiri, jabat tangan (proses), kanan
- Pre-order   = jabat tangan dulu, lalu kiri dan kanan
- Post-order  = kiri dan kanan dulu, baru jabat tangan
```

---

## ⚠️ Kesalahan Umum

1. **Tidak handle kasus root = null** → Semua operasi recursive harus punya base case `if (!node) return ...`.

2. **Delete node dengan dua anak — lupa update pointer** → Saat mengganti node dengan in-order successor, data node diubah, tapi node successor di subtree kanan harus dihapus secara rekursif.

3. **Mengira in-order traversal selalu mengembalikan data sorted untuk semua tree** → In-order traversal menghasilkan sorted output **hanya untuk BST yang valid**. Jika BST-nya corrupt (aturan BST dilanggar), hasilnya tidak sorted.

4. **Tidak mempertimbangkan worst case BST** → BST yang tidak balanced bisa menjadi O(n) untuk semua operasi. Untuk production, gunakan self-balancing tree (Red-Black Tree seperti yang digunakan di Java TreeMap, atau B-Tree di database).

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Jelaskan mengapa in-order traversal BST selalu menghasilkan data dalam urutan sorted. Buktikan dengan contoh tree kecil.

b) Apa yang dimaksud dengan "degenerate BST"? Berikan contoh input yang akan menghasilkan degenerate BST, dan jelaskan dampaknya pada kompleksitas.

c) Dalam delete BST dengan dua anak, mengapa kita menggunakan "in-order successor"? Apakah bisa menggunakan "in-order predecessor"? Jelaskan.

### Soal 2 — Coding

1. Implementasikan `isValidBST(root)` — fungsi yang mengecek apakah suatu binary tree adalah BST yang valid. Hint: gunakan parameter `min` dan `max` untuk tracking range yang valid di setiap subtree.

2. Implementasikan `lowestCommonAncestor(root, n1, n2)` — cari LCA (Lowest Common Ancestor) dua node dalam BST. LCA adalah node terdalam yang merupakan ancestor dari kedua node.

---

## 📌 Ringkasan

* **Tree** = struktur hierarkis, satu root, tidak ada siklus
* **BST** = binary tree dengan aturan: left < node < right (berlaku rekursif)
* **Operasi BST:** insert, search, delete — O(log n) average, O(n) worst (degenerate)
* **Tree traversal:**
  * **In-order** (L-Root-R) → hasil sorted, gunakan untuk print semua data terurut
  * **Pre-order** (Root-L-R) → copy/serialize tree
  * **Post-order** (L-R-Root) → delete tree, evaluate expression tree
* **Delete BST:** 3 kasus — leaf (hapus langsung), 1 anak (ganti dengan anak), 2 anak (ganti dengan in-order successor)
* BST adalah fondasi dari: database index (B-Tree), priority queue (heap), file system, routing table
* Untuk production: gunakan **self-balancing BST** (AVL, Red-Black) atau **B-Tree** agar worst case tetap O(log n)

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Sedgewick & Wayne (2011). Algorithms | Cormen et al. (2009). Introduction to Algorithms*
