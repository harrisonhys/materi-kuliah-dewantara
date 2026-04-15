# Pertemuan 9: Minimum Spanning Tree — Kruskal & Prim

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Mengimplementasikan Union-Find (Disjoint Set Union) dengan Path Compression dan Union by Rank
- Menerapkan Kruskal's Algorithm menggunakan Union-Find untuk Minimum Spanning Tree
- Mengimplementasikan Prim's Algorithm menggunakan Priority Queue
- Memilih antara Kruskal dan Prim berdasarkan karakteristik graf (dense vs sparse)

## 2. Pengantar: Hook

Indosat membangun jaringan fiber optik untuk menghubungkan 50 kota besar di Indonesia. Setiap pasangan kota bisa dihubungkan dengan kabel, tapi biayanya berbeda-beda. Pertanyaannya: **kabel mana yang harus dipasang agar semua kota terhubung dengan biaya minimum?**

Ini adalah Minimum Spanning Tree problem. Jaringan internet nasional, distribusi listrik PLN, infrastruktur pipeline minyak — semua dioptimasi menggunakan MST.

Di dunia software: clustering algorithm, image segmentation, network design di cloud providers (AWS, GCP) — semua menggunakan variasi MST. Memahami Kruskal dan Prim adalah memahami fondasi infrastructure optimization.

## 3. Konsep Utama

### 3.1 Spanning Tree dan MST

**Spanning Tree:** Subgraf yang:
- Menghubungkan semua V vertex (spanning)
- Adalah tree (acyclic, connected)
- Memiliki tepat V-1 edge

**Minimum Spanning Tree (MST):** Spanning tree dengan total bobot edge minimum.

**Properties:**
- Untuk graf terhubung dengan bobot unik: MST unik
- MST tidak harus unique untuk bobot yang sama
- Cut Property: edge dengan bobot terkecil yang melintasi setiap "cut" selalu ada di MST

### 3.2 Union-Find (Disjoint Set Union)

Struktur data untuk operasi:
- **Find(x):** Temukan "representatif" (root) dari komponen yang mengandung x
- **Union(x, y):** Gabungkan dua komponen

**Dua optimasi kritis:**

**1. Path Compression:** Saat Find(x), langsung sambungkan semua node di jalur ke root.
```
Before:  1 → 2 → 3 → 4 (root)
After:   1 → 4, 2 → 4, 3 → 4  (semua langsung ke root)
```

**2. Union by Rank:** Selalu sambungkan pohon yang lebih rendah ke yang lebih tinggi (rank = perkiraan ketinggian).

**Kompleksitas:** O(α(n)) per operasi — di mana α adalah inverse Ackermann function, praktis konstan (≤ 4 untuk n < 10^600).

### 3.3 Kruskal's Algorithm

**Greedy Strategy:** Selalu pilih edge dengan bobot terkecil yang tidak membuat cycle.

```
1. Sort semua edge berdasarkan bobot (ascending)
2. Inisialisasi Union-Find (setiap node = komponen sendiri)
3. Untuk setiap edge (u, v, w) dari terkecil:
   a. Jika Find(u) ≠ Find(v): TIDAK ada cycle → tambahkan ke MST, Union(u,v)
   b. Jika Find(u) = Find(v): AKAN ada cycle → skip
4. Berhenti ketika V-1 edge sudah ditambahkan
```

**Kompleksitas:** O(E log E) untuk sorting, O(E α(V)) untuk Union-Find → **O(E log E)**

**Cocok untuk:** Sparse graphs (E << V²) karena dominasi sorting.

### 3.4 Prim's Algorithm

**Greedy Strategy:** Mulai dari satu node, selalu tambahkan edge termurah yang menghubungkan ke node baru.

```
1. Inisialisasi: mulai dari node sumber, semua jarak = ∞
2. Priority Queue berisi (weight, node)
3. Extract minimum → jika node belum dikunjungi, tambahkan ke MST
4. Update jarak tetangga yang lebih kecil
5. Ulangi hingga semua node dikunjungi
```

**Kompleksitas:** O((V+E) log V) dengan binary heap, O(E + V log V) dengan Fibonacci heap.

**Cocok untuk:** Dense graphs (E ≈ V²) karena tidak perlu sort semua edge di awal.

## 4. Ilustrasi dan Analogi

### Analogi Kruskal: Menyusun Jembatan

Bayangkan kamu kontraktor jembatan. Ada 10 pulau dan kamu harus menghubungkan semuanya dengan biaya minimum.

Kruskal: Buat daftar semua kemungkinan jembatan, urutkan dari yang termurah. Bangun jembatan termurah — tapi **skip** jika dua pulau sudah terhubung (langsung atau melalui pulau lain). Teruskan hingga semua pulau terhubung.

Union-Find = "buku catatan" yang mencatat pulau mana terhubung dengan mana, dengan query sangat cepat.

### Analogi Prim: Menyebar dari Pusat

Prim: Mulai dari satu pulau. Bangun jembatan ke pulau terdekat. Lanjutkan dari kelompok pulau yang sudah terhubung, selalu ke pulau terdekat yang belum terhubung.

Mirip Dijkstra tapi untuk "menyebar" bukan "jarak ke source".

## 5. Contoh Teknis

### 5.1 Union-Find dengan Path Compression + Union by Rank

```javascript
/**
 * Disjoint Set Union (Union-Find)
 * 
 * Mendukung Kruskal's MST dan banyak algoritma graph lainnya
 * Kompleksitas: O(α(n)) per operasi ≈ O(1) praktis
 */
class UnionFind {
    constructor(n) {
        // Bisa gunakan array untuk node integer
        this.parent = Array.from({length: n}, (_, i) => i);
        this.rank = Array(n).fill(0);
        this.size = Array(n).fill(1);
        this.components = n; // jumlah komponen terhubung
    }
    
    // Find dengan Path Compression
    find(x) {
        if (this.parent[x] !== x) {
            this.parent[x] = this.find(this.parent[x]); // path compression rekursif
        }
        return this.parent[x];
    }
    
    // Union by Rank
    union(x, y) {
        const rootX = this.find(x);
        const rootY = this.find(y);
        
        if (rootX === rootY) return false; // sudah satu komponen
        
        // Sambungkan rank lebih rendah ke rank lebih tinggi
        if (this.rank[rootX] < this.rank[rootY]) {
            this.parent[rootX] = rootY;
            this.size[rootY] += this.size[rootX];
        } else if (this.rank[rootX] > this.rank[rootY]) {
            this.parent[rootY] = rootX;
            this.size[rootX] += this.size[rootY];
        } else {
            // Sama → pilih salah satu, naikkan rank
            this.parent[rootY] = rootX;
            this.rank[rootX]++;
            this.size[rootX] += this.size[rootY];
        }
        
        this.components--;
        return true;
    }
    
    connected(x, y) {
        return this.find(x) === this.find(y);
    }
    
    getSize(x) {
        return this.size[this.find(x)];
    }
}

// Test Union-Find
const uf = new UnionFind(6); // nodes: 0, 1, 2, 3, 4, 5

uf.union(0, 1); // komponen: {0,1}, {2}, {3}, {4}, {5}
uf.union(2, 3); // komponen: {0,1}, {2,3}, {4}, {5}
uf.union(1, 2); // komponen: {0,1,2,3}, {4}, {5}

console.log('0 and 3 connected:', uf.connected(0, 3)); // true
console.log('0 and 4 connected:', uf.connected(0, 4)); // false
console.log('Component count:', uf.components);         // 3
console.log('Size of 0\'s component:', uf.getSize(0));  // 4

// Version dengan string nodes
class UnionFindMap {
    constructor() {
        this.parent = new Map();
        this.rank = new Map();
        this.size = new Map();
    }
    
    add(x) {
        if (!this.parent.has(x)) {
            this.parent.set(x, x);
            this.rank.set(x, 0);
            this.size.set(x, 1);
        }
    }
    
    find(x) {
        this.add(x);
        if (this.parent.get(x) !== x) {
            this.parent.set(x, this.find(this.parent.get(x)));
        }
        return this.parent.get(x);
    }
    
    union(x, y) {
        const rootX = this.find(x);
        const rootY = this.find(y);
        
        if (rootX === rootY) return false;
        
        const rankX = this.rank.get(rootX);
        const rankY = this.rank.get(rootY);
        
        if (rankX < rankY) {
            this.parent.set(rootX, rootY);
            this.size.set(rootY, this.size.get(rootY) + this.size.get(rootX));
        } else if (rankX > rankY) {
            this.parent.set(rootY, rootX);
            this.size.set(rootX, this.size.get(rootX) + this.size.get(rootY));
        } else {
            this.parent.set(rootY, rootX);
            this.rank.set(rootX, rankX + 1);
            this.size.set(rootX, this.size.get(rootX) + this.size.get(rootY));
        }
        
        return true;
    }
}
```

### 5.2 Kruskal's Algorithm

```javascript
/**
 * Kruskal's MST Algorithm
 * 
 * @param {Array} nodes - list of node names
 * @param {Array} edges - [{from, to, weight}]
 * @returns {Object} - {mst: [{from,to,weight}], totalWeight}
 */
function kruskal(nodes, edges) {
    // Sort edges by weight ascending
    const sortedEdges = [...edges].sort((a, b) => a.weight - b.weight);
    
    // Map node names to indices
    const nodeIdx = {};
    nodes.forEach((node, i) => { nodeIdx[node] = i; });
    
    const uf = new UnionFind(nodes.length);
    const mst = [];
    let totalWeight = 0;
    
    for (const edge of sortedEdges) {
        const u = nodeIdx[edge.from];
        const v = nodeIdx[edge.to];
        
        // Jika tidak cycle (berbeda komponen) → tambahkan ke MST
        if (uf.union(u, v)) {
            mst.push(edge);
            totalWeight += edge.weight;
            
            // Berhenti ketika sudah V-1 edge
            if (mst.length === nodes.length - 1) break;
        }
    }
    
    // Cek apakah graf terhubung
    const connected = uf.components === 1;
    
    return { mst, totalWeight, connected };
}

// Simulasi: Jaringan Fiber Optik Indosat
const cities = ['Jakarta', 'Bandung', 'Semarang', 'Surabaya', 'Yogyakarta', 'Malang', 'Medan'];

const fiberLinks = [
    { from: 'Jakarta', to: 'Bandung', weight: 150 },      // km/biaya
    { from: 'Jakarta', to: 'Semarang', weight: 450 },
    { from: 'Jakarta', to: 'Medan', weight: 1700 },
    { from: 'Bandung', to: 'Semarang', weight: 320 },
    { from: 'Bandung', to: 'Yogyakarta', weight: 310 },
    { from: 'Semarang', to: 'Surabaya', weight: 300 },
    { from: 'Semarang', to: 'Yogyakarta', weight: 150 },
    { from: 'Surabaya', to: 'Malang', weight: 90 },
    { from: 'Surabaya', to: 'Yogyakarta', weight: 330 },
    { from: 'Yogyakarta', to: 'Malang', weight: 270 },
    { from: 'Medan', to: 'Jakarta', weight: 1700 },        // duplikat untuk undirected
];

console.time('Kruskal');
const { mst, totalWeight, connected } = kruskal(cities, fiberLinks);
console.timeEnd('Kruskal');

console.log('\n=== Kruskal MST: Fiber Optik Indosat ===');
console.log('Terhubung:', connected);
console.log('Total biaya:', totalWeight, 'unit');
console.log('\nKabel yang dipasang:');
mst.forEach(edge => {
    console.log(`  ${edge.from} ↔ ${edge.to}: ${edge.weight} unit`);
});

// Verifikasi: MST harus punya tepat V-1 edge
console.log(`\nJumlah edge MST: ${mst.length} (seharusnya ${cities.length - 1})`);
```

### 5.3 Prim's Algorithm

```javascript
/**
 * Prim's MST Algorithm
 * 
 * @param {Object} graph - adjacency list: { node: [{node, weight}] }
 * @param {string} start - starting node
 * @returns {Object} - {mst, totalWeight}
 */
function prim(graph, start) {
    const visited = new Set();
    const mst = [];
    let totalWeight = 0;
    
    // Priority Queue: (weight, fromNode, toNode)
    const pq = new MinHeap((a, b) => a[0] - b[0]);
    
    // Mulai dari node start
    visited.add(start);
    for (const { node, weight } of (graph[start] || [])) {
        pq.insert([weight, start, node]);
    }
    
    while (!pq.isEmpty() && visited.size < Object.keys(graph).length) {
        const [weight, from, to] = pq.extractMin();
        
        if (visited.has(to)) continue; // sudah dikunjungi → skip
        
        // Tambahkan ke MST
        visited.add(to);
        mst.push({ from, to, weight });
        totalWeight += weight;
        
        // Tambahkan edge dari node baru ke PQ
        for (const { node: next, weight: w } of (graph[to] || [])) {
            if (!visited.has(next)) {
                pq.insert([w, to, next]);
            }
        }
    }
    
    return { 
        mst, 
        totalWeight, 
        connected: visited.size === Object.keys(graph).length 
    };
}

// MinHeap untuk Prim (reuse dari pertemuan 5)
class MinHeap {
    constructor(compare = (a, b) => a - b) {
        this.heap = [];
        this.compare = compare;
    }
    
    get size() { return this.heap.length; }
    isEmpty() { return this.size === 0; }
    
    insert(val) {
        this.heap.push(val);
        this._up(this.heap.length - 1);
    }
    
    extractMin() {
        if (this.size === 1) return this.heap.pop();
        const min = this.heap[0];
        this.heap[0] = this.heap.pop();
        this._down(0);
        return min;
    }
    
    _up(i) {
        while (i > 0) {
            const p = (i - 1) >> 1;
            if (this.compare(this.heap[i], this.heap[p]) >= 0) break;
            [this.heap[i], this.heap[p]] = [this.heap[p], this.heap[i]];
            i = p;
        }
    }
    
    _down(i) {
        const n = this.size;
        while (true) {
            let min = i;
            const l = 2*i+1, r = 2*i+2;
            if (l < n && this.compare(this.heap[l], this.heap[min]) < 0) min = l;
            if (r < n && this.compare(this.heap[r], this.heap[min]) < 0) min = r;
            if (min === i) break;
            [this.heap[i], this.heap[min]] = [this.heap[min], this.heap[i]];
            i = min;
        }
    }
}

// Build adjacency list dari edges (undirected)
function buildAdjList(nodes, edges) {
    const graph = {};
    nodes.forEach(n => { graph[n] = []; });
    edges.forEach(({ from, to, weight }) => {
        graph[from].push({ node: to, weight });
        graph[to].push({ node: from, weight }); // undirected
    });
    return graph;
}

const adjList = buildAdjList(cities, fiberLinks.filter(e => e.from !== 'Medan' || e.to !== 'Jakarta'));

console.time('Prim');
const primResult = prim(adjList, 'Jakarta');
console.timeEnd('Prim');

console.log('\n=== Prim MST: Fiber Optik Indosat ===');
console.log('Total biaya:', primResult.totalWeight, 'unit');
console.log('\nKabel yang dipasang:');
primResult.mst.forEach(edge => {
    console.log(`  ${edge.from} → ${edge.to}: ${edge.weight} unit`);
});
```

### 5.4 Aplikasi: Network Clustering dengan MST

```javascript
/**
 * Cluster Detection menggunakan MST
 * 
 * Teknik: Build MST, lalu remove K-1 edge terberat
 * → K komponen terhubung = K cluster
 * 
 * Aplikasi: Customer segmentation di Tokopedia, network community detection
 */
function mstClustering(nodes, edges, k) {
    // Build MST
    const { mst } = kruskal(nodes, edges);
    
    // Sort MST edges by weight descending
    const sortedMST = [...mst].sort((a, b) => b.weight - a.weight);
    
    // Remove K-1 terberat → K clusters
    const removedEdges = sortedMST.slice(0, k - 1);
    const keptEdges = sortedMST.slice(k - 1);
    
    console.log(`Removed edges to create ${k} clusters:`);
    removedEdges.forEach(e => {
        console.log(`  Cut: ${e.from} ↔ ${e.to} (weight: ${e.weight})`);
    });
    
    // Temukan komponen menggunakan Union-Find dengan keptEdges
    const nodeIdx = {};
    nodes.forEach((n, i) => { nodeIdx[n] = i; });
    const uf = new UnionFind(nodes.length);
    
    for (const { from, to } of keptEdges) {
        uf.union(nodeIdx[from], nodeIdx[to]);
    }
    
    // Group nodes by component
    const clusters = {};
    nodes.forEach((node, i) => {
        const root = uf.find(i);
        if (!clusters[root]) clusters[root] = [];
        clusters[root].push(node);
    });
    
    return Object.values(clusters);
}

// Simulasi: Clustering kota berdasarkan kedekatan
const allCities = ['Jakarta', 'Bandung', 'Bogor', 'Semarang', 'Yogyakarta', 'Surabaya', 'Malang'];
const distances = [
    { from: 'Jakarta', to: 'Bandung', weight: 150 },
    { from: 'Jakarta', to: 'Bogor', weight: 60 },
    { from: 'Bandung', to: 'Bogor', weight: 130 },
    { from: 'Bandung', to: 'Yogyakarta', weight: 310 },
    { from: 'Semarang', to: 'Yogyakarta', weight: 150 },
    { from: 'Semarang', to: 'Surabaya', weight: 300 },
    { from: 'Surabaya', to: 'Malang', weight: 90 },
    { from: 'Yogyakarta', to: 'Surabaya', weight: 330 },
];

console.log('\n=== MST Clustering: 3 Cluster ===');
const clusters = mstClustering(allCities, distances, 3);
clusters.forEach((cluster, i) => {
    console.log(`Cluster ${i+1}: ${cluster.join(', ')}`);
});
// Diharapkan: [Jakarta,Bandung,Bogor], [Semarang,Yogyakarta], [Surabaya,Malang]
```

## 6. Studi Kasus Nyata: PLN Infrastruktur Listrik

**Konteks:** PLN (Perusahaan Listrik Negara) membangun jaringan transmisi listrik untuk menghubungkan pembangkit listrik (PLTA, PLTU) ke kota-kota. Biaya kabel transmisi per km sangat mahal — penghematan 10% pada jaringan = ratusan miliar rupiah.

**Problem:** Minimal spanning tree dari jaringan transmisi.

```javascript
/**
 * PLN Grid Optimization
 * 
 * Input: lokasi pembangkit dan kota (node), estimasi biaya kabel (edge weight)
 * Output: koneksi minimal yang menghubungkan semua dengan biaya minimum
 */
const gridNodes = [
    'PLTA_Asahan', 'PLTU_Suralaya', 'PLTA_Cirata',
    'Medan', 'Jakarta', 'Bandung', 'Semarang', 'Surabaya'
];

const possibleLines = [
    { from: 'PLTA_Asahan', to: 'Medan', weight: 200 },
    { from: 'PLTU_Suralaya', to: 'Jakarta', weight: 100 },
    { from: 'PLTU_Suralaya', to: 'Bandung', weight: 150 },
    { from: 'PLTA_Cirata', to: 'Bandung', weight: 80 },
    { from: 'PLTA_Cirata', to: 'Jakarta', weight: 120 },
    { from: 'Jakarta', to: 'Bandung', weight: 150 },
    { from: 'Jakarta', to: 'Semarang', weight: 500 },
    { from: 'Bandung', to: 'Semarang', weight: 380 },
    { from: 'Semarang', to: 'Surabaya', weight: 350 },
    { from: 'Medan', to: 'Jakarta', weight: 1800 }, // via udara/bawah laut
];

const gridResult = kruskal(gridNodes, possibleLines);
console.log('\n=== PLN Grid Optimization ===');
console.log(`Total panjang kabel: ${gridResult.totalWeight} km-equivalent`);
console.log(`Hemat vs semua kabel: ${possibleLines.reduce((s,e) => s+e.weight, 0) - gridResult.totalWeight} unit`);
gridResult.mst.forEach(e => {
    console.log(`  ✓ ${e.from} ↔ ${e.to}: ${e.weight}`);
});

// Robustness: apa yang terjadi jika satu jalur putus?
function findAlternativePath(mst, brokenEdge, allEdges) {
    const mstWithout = mst.filter(e => 
        !(e.from === brokenEdge.from && e.to === brokenEdge.to)
    );
    
    // Cari edge alternatif yang terhubung kembali
    const alternatives = allEdges.filter(e => 
        e.from !== brokenEdge.from || e.to !== brokenEdge.to
    ).filter(e => !mstWithout.some(m => m.from === e.from && m.to === e.to));
    
    return alternatives.sort((a, b) => a.weight - b.weight)[0];
}
```

## 7. Visualisasi

### Union-Find Path Compression

```
Before Find(6):
1 ← 2 ← 3 ← 4 (root)
            ↑
        5 ← 6

Find(6): traverse 6→5→4 (root)
After Path Compression:
6 → 4 (root) ← langsung!
5 → 4 (root) ← langsung!

Benefit: Find(6) berikutnya = O(1)
```

### Kruskal Step-by-Step (5 kota)

```
Edges sorted: AB=1, CD=2, BC=3, AE=4, BD=5, CE=7

Step 1: Add AB (weight=1). Components: {A,B},{C},{D},{E}
Step 2: Add CD (weight=2). Components: {A,B},{C,D},{E}
Step 3: Add BC (weight=3). B and C: different components!
        Components: {A,B,C,D},{E}. MST edges: AB,CD,BC
Step 4: Add AE (weight=4). A and E: different components!
        Components: {A,B,C,D,E}. Done! V-1=4 edges.

Final MST: AB(1) + CD(2) + BC(3) + AE(4) = total 10
```

### Kruskal vs Prim — When to Use

```
Graph Properties       → Use
─────────────────────────────────────────────
Sparse (E ≈ V)         → Kruskal O(E log E)
Dense (E ≈ V²)         → Prim O(V² atau E+VlogV)
Need specific start    → Prim
Edge-based thinking    → Kruskal
Cycle detection needed → Kruskal (Union-Find gratis)
Parallel computation   → Kruskal (sort → embarrassingly parallel)
```

## 8. Kesalahan Umum

### ❌ Kesalahan 1: Union-Find Tanpa Path Compression

```javascript
// LAMBAT — O(n) per Find tanpa path compression
function findSlow(parent, x) {
    while (parent[x] !== x) {
        x = parent[x]; // tidak ada path compression → rantai panjang
    }
    return x;
}

// CEPAT — O(α(n)) dengan path compression
function findFast(parent, x) {
    if (parent[x] !== x) {
        parent[x] = findFast(parent, parent[x]); // path compression!
    }
    return parent[x];
}
```

### ❌ Kesalahan 2: Kruskal pada Graf Tak Terhubung (Disconnected)

```javascript
// SALAH — tidak cek apakah graph connected
function kruskalWrong(nodes, edges) {
    // ...
    // Jika graph disconnected, hasilnya bukan spanning TREE
    // tapi spanning FOREST (kumpulan tree)
    // dan mst.length < nodes.length - 1
}

// BENAR — selalu cek konektivitas
function kruskalSafe(nodes, edges) {
    const { mst, totalWeight, connected } = kruskal(nodes, edges);
    
    if (!connected) {
        console.warn('Graph is not connected! Result is Minimum Spanning Forest.');
    }
    
    return { mst, totalWeight, connected };
}
```

### ❌ Kesalahan 3: Prim dengan Graf Berarah (Directed)

```javascript
// HATI-HATI: Prim untuk MST hanya untuk undirected graph
// Untuk directed graph → gunakan Minimum Spanning Arborescence (Edmonds' algorithm)

// Jika membangun adj list dari directed edges:
edges.forEach(({ from, to, weight }) => {
    graph[from].push({ node: to, weight });
    // LUPA menambahkan edge balik!
    // graph[to].push({ node: from, weight }); // ← ini yang sering lupa
});
```

### ❌ Kesalahan 4: Menggunakan MST untuk Shortest Path

```javascript
// MST ≠ Shortest Path Tree!
// MST: minimumkan total weight semua edge
// Shortest Path: minimumkan jarak dari source ke setiap node

// Counter-example:
// A--1--B--1--C   vs   A--10--C
// MST: A-B-C (total 2) tapi jarak A ke C = 2 (melalui B)
// Shortest A to C directly: 10 (lebih besar!)
// 
// Tapi jika ada shortcut: A-B-C (via B, total 2) vs A-C (direct 10)
// Shortest: via B = 2 (lebih kecil)
// MST: A-B-C (sama)
// 
// Tapi: A--1--B--100--C   dan   A--50--C
// MST: A-B(1) + A-C(50) = 51 (skip B-C yang 100)
// Shortest A to C: 50 (langsung, bukan via B)
// Tapi MST tidak memberikan A-B-C path = 101 vs 50 — MST structure ≠ shortest path!
```

## 9. Latihan dan Studi Kasus

### Latihan 1 — Union-Find Operations

Trace operasi Union-Find berikut dengan 7 node (0-6):
1. union(0,1), union(2,3), union(4,5)
2. union(1,2), union(5,6)
3. find(0), find(6) — apakah connected?
4. union(3,4)
5. Berapa jumlah komponen?

**Jawaban:**
```
Setelah 1: {0,1},{2,3},{4,5},{6}
Setelah 2: {0,1,2,3},{4,5,6}
find(0) = find(3) = same root → connected? YES (setelah union(1,2))
find(6) = root of {4,5,6}
Setelah 3: union(3,4) → {0,1,2,3,4,5,6}
Jumlah komponen: 1
```

### Latihan 2 — Kruskal Trace

Graf:
```
A --4-- B
|  \    |
2    3  5
|      \|
C --1-- D
```

Edges: AC=2, AD=3, AB=4, CD=1, BD=5

Trace Kruskal dan temukan MST.

**Solusi:**
```
Sort: CD=1, AC=2, AD=3, AB=4, BD=5

Step 1: CD=1 → add (C dan D berbeda komponen). MST: {CD}
Step 2: AC=2 → add (A dan C berbeda). MST: {CD, AC}
Step 3: AD=3 → A dan D: A terhubung ke C, C terhubung ke D → CYCLE! Skip.
Step 4: AB=4 → add (A,B,C,D berbeda komponen dengan B). MST: {CD, AC, AB}
        V-1 = 3 edges → SELESAI!

MST: CD(1) + AC(2) + AB(4) = total 7
```

### Latihan 3 — MST vs Shortest Path

```javascript
// Buktikan dengan kode bahwa MST ≠ Shortest Path Tree
const nodes = ['A', 'B', 'C'];
const edges = [
    { from: 'A', to: 'B', weight: 1 },
    { from: 'B', to: 'C', weight: 100 },
    { from: 'A', to: 'C', weight: 50 },
];

// MST (Kruskal)
const { mst } = kruskal(nodes, edges);
console.log('MST edges:', mst.map(e => `${e.from}-${e.to}(${e.weight})`));
// A-B(1) + A-C(50) = MST total 51, skip B-C(100)

// Shortest Path dari A (Dijkstra)
const adjList = {};
nodes.forEach(n => { adjList[n] = []; });
edges.forEach(({ from, to, weight }) => {
    adjList[from].push({ node: to, weight });
    adjList[to].push({ node: from, weight });
});

// dijkstra(adjList, 'A') → A to C = 50 (direct), not 101 (via B)
// MST termasuk edge A-B tapi tidak berarti A→B→C adalah shortest A to C!
console.log('\nKesimpulan: MST ≠ Shortest Path Tree!');
console.log('MST minimumkan TOTAL weight semua edge.');
console.log('Shortest Path minimumkan jarak dari SOURCE ke setiap node.');
```

## 10. Ringkasan

| Aspek | Kruskal | Prim |
|-------|---------|------|
| Approach | Edge-centric | Vertex-centric |
| Data Structure | Union-Find | Priority Queue |
| Time Complexity | O(E log E) | O((V+E) log V) |
| Space | O(V) | O(V+E) |
| Dense Graph | Lambat | Cepat |
| Sparse Graph | Cepat | Lebih lambat |
| Implementation | Lebih mudah | Sedikit lebih rumit |
| Starting node | Tidak perlu | Perlu |

**Kapan pilih:**
- **Kruskal:** Graf sparse, perlu cycle detection, bisa sort edges lebih dulu, parallel computation
- **Prim:** Graf dense, dimulai dari node tertentu, hanya ingin expand dari satu titik

**Union-Find use cases lain:**
- Deteksi cycle dalam undirected graph
- Connected components
- Kruskal MST
- Network connectivity (apakah A dan B terhubung?)
- Percolation theory (fisika/kimia)
- Dynamic graph connectivity

## 11. Referensi

- Cormen, T.H. et al. — *Introduction to Algorithms (CLRS)*, 4th Ed., Bab 19-21 (MST, Union-Find)
- Kruskal, J.B. — "On the Shortest Spanning Subtree of a Graph", 1956 (makalah asli)
- Prim, R.C. — "Shortest Connection Networks and Some Generalizations", 1957 (makalah asli)
- Sedgewick, R. — *Algorithms*, 4th Ed., Bab 4.3 (Minimum Spanning Trees)
- Bhargava, A. — *Grokking Algorithms* — Dijkstra chapter (berkaitan dengan MST)
- Visualgo.net — MST visualization (Kruskal dan Prim)
- LeetCode — #1584 (Min Cost to Connect All Points), #1135 (Connecting Cities With Minimum Cost)
- Union-Find Princeton Course — [algs4.cs.princeton.edu](https://algs4.cs.princeton.edu)
