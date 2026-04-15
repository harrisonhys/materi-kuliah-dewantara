# Pertemuan 13: Graph — DFS & BFS

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan representasi graph dengan adjacency list dan adjacency matrix
* Membedakan directed vs undirected graph, weighted vs unweighted
* Mengimplementasikan DFS (Depth-First Search) dan BFS (Breadth-First Search)
* Menentukan kapan menggunakan DFS vs BFS berdasarkan jenis masalah
* Menerapkan graph traversal untuk masalah nyata seperti deteksi fraud

---

## 📖 Pengantar (Hook)

Bagaimana Instagram tahu "Orang yang mungkin kamu kenal"?

Bagaimana Google Maps menemukan rute terpendek dari Bandung ke Jakarta?

Bagaimana sistem anti-fraud bank menemukan jaringan akun penipu yang saling terhubung?

Semuanya menggunakan **Graph** — struktur data yang merepresentasikan hubungan antara entitas. Dan algoritma yang menelusuri graph — DFS dan BFS — adalah fondasi dari semua sistem tersebut.

---

## 🧩 Konsep Utama

### Apa itu Graph?

Graph terdiri dari:
* **Vertex (Node)** — entitas (orang, kota, akun bank)
* **Edge** — hubungan antara dua vertex

**Jenis Graph:**
* **Undirected** — hubungan dua arah (pertemanan: A kenal B, maka B kenal A)
* **Directed (Digraph)** — hubungan satu arah (follow: A follow B ≠ B follow A)
* **Weighted** — edge punya nilai/bobot (jarak antar kota, biaya transaksi)
* **Unweighted** — edge tidak punya bobot

```
Undirected:        Directed:          Weighted:
A — B              A → B              A --5-- B
|   |              ↑   ↓              |       |
C — D              C ← D              3       2
                                      |       |
                                      C --4-- D
```

### Representasi Graph

**Adjacency List** — untuk setiap vertex, simpan daftar tetangga:
```
A: [B, C]
B: [A, D]
C: [A, D]
D: [B, C]
```
* Space: O(V + E) — efisien untuk sparse graph
* Check edge: O(degree of vertex) — bukan O(1)

**Adjacency Matrix** — matrix V×V, nilai 1 jika ada edge:
```
   A  B  C  D
A [0, 1, 1, 0]
B [1, 0, 0, 1]
C [1, 0, 0, 1]
D [0, 1, 1, 0]
```
* Space: O(V²) — boros untuk sparse graph
* Check edge: O(1) — langsung lookup
* Cocok untuk dense graph atau saat check edge sangat sering

### DFS vs BFS

| | DFS (Depth-First) | BFS (Breadth-First) |
|---|---|---|
| **Struktur** | Stack (rekursif atau eksplisit) | Queue |
| **Pola** | Masuk sedalam mungkin dulu | Level by level |
| **Cocok untuk** | Path finding, cycle detection, topological sort | Shortest path (unweighted), level traversal |
| **Space** | O(H) — H = height graph | O(W) — W = width level terlebar |
| **Menemukan path** | Tidak selalu terpendek | Selalu terpendek (unweighted) |

---

## 🧠 Ilustrasi / Analogi

| Konsep | Analogi |
|---|---|
| **Graph** | Peta kota dengan jalan yang menghubungkan |
| **Vertex** | Kota atau persimpangan jalan |
| **Edge** | Jalan yang menghubungkan dua kota |
| **DFS** | Penelusuran labirin: masuk satu lorong sampai mentok, baru balik dan coba lorong lain |
| **BFS** | Gelombang air yang menyebar ke semua arah sama rata — semua tetangga terdekat dikunjungi dulu |
| **Weighted graph** | Peta dengan jarak antar kota yang berbeda |

---

## 💻 Contoh Teknis

```javascript
// ============= REPRESENTASI GRAPH =============
class Graph {
  constructor(directed = false) {
    this.adjacencyList = {};
    this.directed = directed;
  }

  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) {
      this.adjacencyList[vertex] = [];
    }
  }

  addEdge(v1, v2, weight = null) {
    this.addVertex(v1);
    this.addVertex(v2);

    this.adjacencyList[v1].push({ node: v2, weight });
    if (!this.directed) {
      // Undirected: tambahkan edge di kedua arah
      this.adjacencyList[v2].push({ node: v1, weight });
    }
  }

  getNeighbors(vertex) {
    return this.adjacencyList[vertex] || [];
  }
}

// ============= DFS (Depth-First Search) =============
// Menggunakan rekursi (stack implisit via call stack)
function dfsRekursif(graph, start, visited = new Set()) {
  visited.add(start);
  console.log('Kunjungi:', start);

  for (const { node } of graph.getNeighbors(start)) {
    if (!visited.has(node)) {
      dfsRekursif(graph, node, visited);
    }
  }

  return visited;
}

// DFS iteratif (menggunakan explicit stack)
function dfsIteratif(graph, start) {
  const stack = [start];
  const visited = new Set();
  const order = [];

  while (stack.length > 0) {
    const vertex = stack.pop(); // Stack → LIFO

    if (visited.has(vertex)) continue;
    visited.add(vertex);
    order.push(vertex);

    // Push tetangga ke stack (urutan terbalik untuk konsistensi)
    const neighbors = graph.getNeighbors(vertex);
    for (let i = neighbors.length - 1; i >= 0; i--) {
      if (!visited.has(neighbors[i].node)) {
        stack.push(neighbors[i].node);
      }
    }
  }

  return order;
}

// ============= BFS (Breadth-First Search) =============
function bfs(graph, start) {
  const queue = [start]; // Queue → FIFO
  const visited = new Set([start]);
  const order = [];
  const distance = { [start]: 0 };

  while (queue.length > 0) {
    const vertex = queue.shift(); // shift = dequeue dari depan
    order.push(vertex);

    for (const { node } of graph.getNeighbors(vertex)) {
      if (!visited.has(node)) {
        visited.add(node);
        queue.push(node); // enqueue ke belakang
        distance[node] = distance[vertex] + 1;
      }
    }
  }

  return { order, distance };
}

// BFS untuk shortest path
function shortestPath(graph, start, end) {
  if (start === end) return [start];

  const queue = [start];
  const visited = new Set([start]);
  const parent = { [start]: null }; // untuk rekonstruksi path

  while (queue.length > 0) {
    const vertex = queue.shift();

    for (const { node } of graph.getNeighbors(vertex)) {
      if (!visited.has(node)) {
        visited.add(node);
        parent[node] = vertex;
        queue.push(node);

        if (node === end) {
          // Rekonstruksi path dari end ke start
          const path = [];
          let current = end;
          while (current !== null) {
            path.unshift(current);
            current = parent[current];
          }
          return path;
        }
      }
    }
  }

  return null; // tidak ada path
}

// ============= TEST =============
const g = new Graph();
// Network pertemanan
['Andi', 'Budi', 'Cici', 'Dodi', 'Evi'].forEach(n => g.addVertex(n));
g.addEdge('Andi', 'Budi');
g.addEdge('Andi', 'Cici');
g.addEdge('Budi', 'Dodi');
g.addEdge('Cici', 'Evi');
g.addEdge('Dodi', 'Evi');

console.log('DFS dari Andi:', dfsIteratif(g, 'Andi'));
// ['Andi', 'Budi', 'Dodi', 'Evi', 'Cici']

const { order: bfsOrder, distance } = bfs(g, 'Andi');
console.log('BFS dari Andi:', bfsOrder);
// ['Andi', 'Budi', 'Cici', 'Dodi', 'Evi']
console.log('Jarak dari Andi:', distance);
// {Andi: 0, Budi: 1, Cici: 1, Dodi: 2, Evi: 2}

console.log('Path Andi → Evi:', shortestPath(g, 'Andi', 'Evi'));
// ['Andi', 'Cici', 'Evi'] atau ['Andi', 'Budi', 'Dodi', 'Evi']
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Graph-Based Fraud Detection: Jaringan Akun Penipu

**Skenario:** Tim risk management di platform fintech mendeteksi pola penipuan di mana sekelompok akun saling mentransfer uang untuk "mencuci" dana ilegal. Pola ini tidak terlihat dari transaksi individual, tapi menjadi jelas ketika transaksi dimodelkan sebagai graph.

**Model:**
* Vertex = akun pengguna
* Edge directed = transfer (dari pengirim ke penerima)
* Fraud pattern: cycle detection — uang berputar di grup kecil akun

```javascript
class FraudDetector {
  constructor() {
    this.transferGraph = new Graph(true); // directed graph
  }

  tambahTransfer(pengiriimId, penerimaId, nominal) {
    this.transferGraph.addEdge(pengiriimId, penerimaId, nominal);
  }

  // Deteksi apakah ada siklus — indikasi money laundering
  detectCycle() {
    const visited = new Set();
    const rekStack = new Set(); // stack rekursi untuk DFS

    const hasCycle = (vertex) => {
      visited.add(vertex);
      rekStack.add(vertex);

      for (const { node } of this.transferGraph.getNeighbors(vertex)) {
        if (!visited.has(node)) {
          if (hasCycle(node)) return true;
        } else if (rekStack.has(node)) {
          return true; // menemukan back edge → ada siklus!
        }
      }

      rekStack.delete(vertex);
      return false;
    };

    for (const vertex of Object.keys(this.transferGraph.adjacencyList)) {
      if (!visited.has(vertex)) {
        if (hasCycle(vertex)) return true;
      }
    }

    return false;
  }

  // Cari semua akun dalam radius 2 hop dari akun mencurigakan (BFS)
  temukanJaringan(akunMencurigakan, radius = 2) {
    const jaringan = new Map();
    const queue = [{ node: akunMencurigakan, depth: 0 }];
    const visited = new Set([akunMencurigakan]);

    while (queue.length > 0) {
      const { node, depth } = queue.shift();

      if (depth > radius) continue;
      jaringan.set(node, depth);

      for (const { node: tetangga } of this.transferGraph.getNeighbors(node)) {
        if (!visited.has(tetangga)) {
          visited.add(tetangga);
          queue.push({ node: tetangga, depth: depth + 1 });
        }
      }
    }

    return jaringan;
  }
}

const detector = new FraudDetector();
// Skenario: akun A, B, C saling transfer (money laundering cycle)
detector.tambahTransfer('ACC-A', 'ACC-B', 5000000);
detector.tambahTransfer('ACC-B', 'ACC-C', 4800000); // sedikit dikurangi (biaya)
detector.tambahTransfer('ACC-C', 'ACC-A', 4600000); // kembali ke A → CYCLE!
detector.tambahTransfer('ACC-A', 'ACC-D', 1000000); // transfer normal

console.log('Ada siklus:', detector.detectCycle()); // true → FLAG!
console.log('Jaringan ACC-A:', detector.temukanJaringan('ACC-A'));
// Map { 'ACC-A': 0, 'ACC-B': 1, 'ACC-D': 1, 'ACC-C': 2 }
```

---

## 📊 Visualisasi: DFS vs BFS Traversal

```
Graph:
    A
   / \
  B   C
 / \   \
D   E   F

BFS (level by level):         DFS (depth first):
Level 0: A                    A → B → D (dead end)
Level 1: B, C                     ↑ backtrack
Level 2: D, E, F                  → E (dead end)
                                  ↑ backtrack
Urutan: A, B, C, D, E, F     ↑ backtrack B selesai
                              → C → F (dead end)
                              Urutan: A, B, D, E, C, F

BFS menggunakan QUEUE:        DFS menggunakan STACK:
[A] → dequeue A, enqueue B,C  [A] → pop A, push C,B
[B,C] → dequeue B, enqueue D,E [C,B] → pop B, push E,D
[C,D,E] → dequeue C, enqueue F [C,E,D] → pop D (leaf)
[D,E,F] → dequeue D            [C,E] → pop E (leaf)
...                            [C] → pop C, push F
                               [F] → pop F (leaf)
```

---

## ⚠️ Kesalahan Umum

1. **Tidak track visited node → infinite loop** → Graph bisa punya siklus. Tanpa visited set, DFS/BFS akan loop selamanya. Selalu tandai node sebagai visited.

2. **Menggunakan `shift()` untuk BFS → O(n²)** → `Array.shift()` di JavaScript adalah O(n) karena harus menggeser semua elemen. Untuk BFS performa tinggi, gunakan Queue dengan linked list atau pointer.

3. **DFS rekursif untuk graph besar → Stack Overflow** → Jika graph sangat dalam, rekursi bisa stack overflow. Gunakan DFS iteratif dengan explicit stack.

4. **Mengira BFS selalu optimal** → BFS menemukan shortest path hanya untuk **unweighted** graph. Untuk weighted graph, gunakan Dijkstra atau A*.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Jelaskan perbedaan DFS dan BFS dalam hal pola penelusuran. Berikan satu skenario di mana DFS lebih tepat dan satu skenario di mana BFS lebih tepat.

b) Mengapa adjacency list lebih efisien dari adjacency matrix untuk sparse graph? Kapan adjacency matrix lebih tepat digunakan?

c) Jelaskan mengapa deteksi siklus dalam directed graph penting untuk sistem keuangan. Apa yang direpresentasikan oleh siklus dalam graph transfer keuangan?

### Soal 2 — Coding

1. Implementasikan BFS untuk menentukan apakah graph adalah **bipartite** (bisa dibagi dua kelompok sehingga tidak ada edge dalam kelompok yang sama). Hint: coba "mewarnai" graph dengan dua warna selama BFS.

2. Implementasikan fungsi `temukanKomponenTerhubung(graph)` menggunakan DFS yang mengembalikan array dari semua komponen terhubung (group of vertices yang saling reachable). Berguna untuk mengelompokkan akun yang saling terhubung dalam fraud analysis.

---

## 📌 Ringkasan

* **Graph** = vertex (node) + edge (hubungan) — representasi network apapun
* **Jenis:** directed/undirected, weighted/unweighted
* **Representasi:** Adjacency List (O(V+E), sparse) vs Adjacency Matrix (O(V²), dense, O(1) check edge)
* **DFS** = Stack, masuk sedalam mungkin — gunakan untuk: cycle detection, path finding, topological sort
* **BFS** = Queue, level by level — gunakan untuk: shortest path (unweighted), level traversal, "degrees of separation"
* **Visited set WAJIB** untuk mencegah infinite loop pada graph dengan siklus
* **Aplikasi nyata:**
  * Social network → BFS untuk "people you may know"
  * Maps/navigation → shortest path (Dijkstra, BFS untuk unweighted)
  * Fraud detection → cycle detection + connected components
  * Dependency resolution → topological sort (DFS)

---

*📚 Referensi: Bhargava, A.Y. (2016). Grokking Algorithms | Sedgewick & Wayne (2011). Algorithms | Cormen et al. (2009). Introduction to Algorithms*
