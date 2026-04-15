# Pertemuan 8: Shortest Path — Dijkstra, Bellman-Ford, Floyd-Warshall

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Mengimplementasikan Dijkstra menggunakan Priority Queue dengan kompleksitas O((V+E) log V)
- Menerapkan Bellman-Ford untuk graf dengan bobot negatif dan deteksi negative cycle
- Menggunakan Floyd-Warshall untuk All-Pairs Shortest Path
- Memilih algoritma yang tepat berdasarkan karakteristik graf (bobot negatif, density, single vs all-pairs)

## 2. Pengantar: Hook

Setiap kali kamu order GoFood, sistem Gojek menghitung rute terpendek dari ratusan restoran ke lokasi kamu, mempertimbangkan kondisi lalu lintas real-time. Dengan 200.000+ driver aktif dan jutaan order per hari, algoritma shortest path berjalan **jutaan kali per menit**.

Pilihan algoritma bukan hanya soal kebenaran — tapi **survival**:
- Dijkstra: O((V+E) log V) — cocok untuk graf besar dengan bobot positif
- Bellman-Ford: O(VE) — untuk ada bobot negatif (diskon jarak? unusual tapi ada aplikasinya)
- Floyd-Warshall: O(V³) — hanya untuk V kecil, tapi berikan ALL pairs sekaligus

Salah pilih → sistem down. Benar pilih → 50ms response time untuk 1 juta query/detik.

## 3. Konsep Utama

### 3.1 Representasi Graf

```javascript
// Adjacency List — efisien untuk sparse graph (E << V²)
const graph = {
    'A': [{ node: 'B', weight: 4 }, { node: 'C', weight: 2 }],
    'B': [{ node: 'C', weight: 1 }, { node: 'D', weight: 5 }],
    'C': [{ node: 'B', weight: 1 }, { node: 'D', weight: 8 }, { node: 'E', weight: 10 }],
    'D': [{ node: 'E', weight: 2 }],
    'E': [],
};

// Adjacency Matrix — efisien untuk dense graph, O(1) edge lookup
const matrix = [
    [0, 4, 2, Inf, Inf],  // A
    [Inf, 0, 1, 5, Inf],  // B
    [Inf, 1, 0, 8, 10],   // C
    [Inf, Inf, Inf, 0, 2], // D
    [Inf, Inf, Inf, Inf, 0] // E
];

// Edge List — mudah untuk Bellman-Ford
const edges = [
    ['A', 'B', 4], ['A', 'C', 2], ['B', 'C', 1],
    ['B', 'D', 5], ['C', 'D', 8], ['C', 'E', 10], ['D', 'E', 2]
];
```

### 3.2 Dijkstra — Greedy Shortest Path

**Algoritma:**
1. Inisialisasi: `dist[source] = 0`, semua lainnya `∞`
2. Priority Queue (min-heap) berisi `(distance, node)`
3. Loop: extract node dengan jarak terkecil, relax semua tetangganya
4. Jika tetangga sudah pernah dikunjungi, skip

**Invariant:** Ketika node di-extract dari heap, `dist[node]` sudah final dan optimal.

**Kenapa greedy bekerja?**
- Bobot positif → jarak hanya bisa bertambah
- Node dengan `dist` terkecil tidak mungkin ditemukan jalur yang lebih pendek nantinya

**Kompleksitas:**
- Dengan binary heap: O((V+E) log V)
- Dengan Fibonacci heap: O(E + V log V) — lebih baik untuk dense graph

### 3.3 Bellman-Ford — Dynamic Programming Shortest Path

**Algoritma:**
1. Inisialisasi: `dist[source] = 0`, semua lainnya `∞`
2. Relaxasi: ulangi V-1 kali untuk semua E edge
3. Cek negative cycle: relaksasi ke-V seharusnya tidak mengubah apa pun

**Kenapa V-1 iterasi?** Path terpendek tanpa cycle melewati paling banyak V-1 edge.

**Kompleksitas:** O(VE)

**Keunggulan:** Bisa handle bobot negatif, deteksi negative cycle.

### 3.4 Floyd-Warshall — All-Pairs Shortest Path

**DP recurrence:**
```
dp[i][j][k] = shortest path dari i ke j menggunakan vertex 1..k sebagai intermediate

dp[i][j][k] = min(
    dp[i][j][k-1],          // tidak lewat vertex k
    dp[i][k][k-1] + dp[k][j][k-1]  // lewat vertex k
)
```

**Dioptimasi:** Cukup 2D array (eliminasi dimensi k karena setiap level hanya butuh level sebelumnya):
```
dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```

**Kompleksitas:** O(V³) waktu, O(V²) ruang

## 4. Ilustrasi dan Analogi

### Dijkstra: Perluasan Ripple

Bayangkan kamu menjatuhkan batu ke kolam air. Gelombang (wave) menyebar ke segala arah. Node yang paling dekat mendapat gelombang lebih dulu. Dijkstra adalah persis seperti ini — selalu proses node yang paling dekat dulu, sehingga ketika gelombang mencapai node, jaraknya sudah pasti optimal.

### Bellman-Ford: Iterasi Berulang

Bayangkan kamu sedang memberitahu teman-teman tentang sebuah berita. Di round 1, hanya teman langsung yang tahu. Di round 2, teman-teman mereka tahu. Di round V-1, semua orang dalam jaringan V node sudah mendapat informasi.

### Floyd-Warshall: Layering Intermediate Nodes

Bayangkan menghitung biaya perjalanan antar kota. Di iterasi pertama, kamu cek "apakah lewat kota A lebih murah?" Iterasi kedua: "lewat kota B?" Dan seterusnya hingga semua kota dicoba sebagai transit.

## 5. Contoh Teknis

### 5.1 Dijkstra dengan Priority Queue

```javascript
/**
 * Dijkstra's Shortest Path Algorithm
 * 
 * @param {Object} graph - adjacency list: { node: [{node, weight}] }
 * @param {string} source - starting node
 * @returns {Object} - { distances, previous } untuk backtracking
 */

class PriorityQueue {
    constructor() {
        this.heap = [];
    }
    
    enqueue(priority, value) {
        this.heap.push({ priority, value });
        this._bubbleUp(this.heap.length - 1);
    }
    
    dequeue() {
        if (this.isEmpty()) return null;
        this._swap(0, this.heap.length - 1);
        const min = this.heap.pop();
        this._sinkDown(0);
        return min;
    }
    
    isEmpty() { return this.heap.length === 0; }
    
    _bubbleUp(idx) {
        while (idx > 0) {
            const parent = Math.floor((idx - 1) / 2);
            if (this.heap[parent].priority <= this.heap[idx].priority) break;
            this._swap(parent, idx);
            idx = parent;
        }
    }
    
    _sinkDown(idx) {
        const n = this.heap.length;
        while (true) {
            let min = idx;
            const l = 2 * idx + 1, r = 2 * idx + 2;
            if (l < n && this.heap[l].priority < this.heap[min].priority) min = l;
            if (r < n && this.heap[r].priority < this.heap[min].priority) min = r;
            if (min === idx) break;
            this._swap(idx, min);
            idx = min;
        }
    }
    
    _swap(i, j) {
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
    }
}

function dijkstra(graph, source) {
    const nodes = Object.keys(graph);
    const dist = {};
    const prev = {};
    const visited = new Set();
    
    // Inisialisasi
    for (const node of nodes) {
        dist[node] = node === source ? 0 : Infinity;
        prev[node] = null;
    }
    
    const pq = new PriorityQueue();
    pq.enqueue(0, source);
    
    while (!pq.isEmpty()) {
        const { value: current } = pq.dequeue();
        
        // Skip jika sudah dikunjungi (lazy deletion)
        if (visited.has(current)) continue;
        visited.add(current);
        
        // Relaksasi semua tetangga
        for (const { node: neighbor, weight } of (graph[current] || [])) {
            if (visited.has(neighbor)) continue;
            
            const newDist = dist[current] + weight;
            if (newDist < dist[neighbor]) {
                dist[neighbor] = newDist;
                prev[neighbor] = current;
                pq.enqueue(newDist, neighbor);
            }
        }
    }
    
    return { dist, prev };
}

// Rekonstruksi path
function getPath(prev, target) {
    const path = [];
    let current = target;
    while (current !== null) {
        path.unshift(current);
        current = prev[current];
    }
    return path;
}

// Simulasi: Rute GoFood Jakarta
const jakartaMap = {
    'Menteng': [
        { node: 'Sudirman', weight: 3 },
        { node: 'Cikini', weight: 2 },
    ],
    'Sudirman': [
        { node: 'Menteng', weight: 3 },
        { node: 'Semanggi', weight: 2 },
        { node: 'Kuningan', weight: 5 },
    ],
    'Cikini': [
        { node: 'Menteng', weight: 2 },
        { node: 'Salemba', weight: 3 },
    ],
    'Semanggi': [
        { node: 'Sudirman', weight: 2 },
        { node: 'Kuningan', weight: 3 },
        { node: 'Kebayoran', weight: 7 },
    ],
    'Kuningan': [
        { node: 'Sudirman', weight: 5 },
        { node: 'Semanggi', weight: 3 },
        { node: 'Pancoran', weight: 4 },
    ],
    'Salemba': [
        { node: 'Cikini', weight: 3 },
    ],
    'Kebayoran': [
        { node: 'Semanggi', weight: 7 },
        { node: 'Pancoran', weight: 5 },
    ],
    'Pancoran': [
        { node: 'Kuningan', weight: 4 },
        { node: 'Kebayoran', weight: 5 },
    ],
};

console.time('Dijkstra');
const { dist, prev } = dijkstra(jakartaMap, 'Menteng');
console.timeEnd('Dijkstra');

console.log('\n=== GoFood Route from Menteng ===');
for (const [node, d] of Object.entries(dist)) {
    const path = getPath(prev, node);
    console.log(`→ ${node}: ${d} km | Route: ${path.join(' → ')}`);
}

// Dengan bobot waktu (traffic)
const trafficWeightedMap = {
    'Restaurant': [
        { node: 'A', weight: 5 },  // km
        { node: 'B', weight: 3 },
    ],
    'A': [
        { node: 'Customer', weight: 4 },
    ],
    'B': [
        { node: 'Customer', weight: 8 },
    ],
    'Customer': [],
};

const { dist: trafficDist } = dijkstra(trafficWeightedMap, 'Restaurant');
console.log('\nETA ke Customer:', trafficDist['Customer'], 'menit');
```

### 5.2 Bellman-Ford dengan Deteksi Negative Cycle

```javascript
/**
 * Bellman-Ford Algorithm
 * 
 * Handles negative weights, detects negative cycles
 * 
 * @param {Array} vertices - list of vertex names
 * @param {Array} edges - [{from, to, weight}]
 * @param {string} source
 * @returns {Object} - { dist, prev, hasNegativeCycle }
 */
function bellmanFord(vertices, edges, source) {
    const dist = {};
    const prev = {};
    
    for (const v of vertices) {
        dist[v] = v === source ? 0 : Infinity;
        prev[v] = null;
    }
    
    // Relaksasi V-1 kali
    for (let i = 0; i < vertices.length - 1; i++) {
        let updated = false;
        
        for (const { from, to, weight } of edges) {
            if (dist[from] !== Infinity && dist[from] + weight < dist[to]) {
                dist[to] = dist[from] + weight;
                prev[to] = from;
                updated = true;
            }
        }
        
        if (!updated) break; // early termination — sudah optimal
    }
    
    // Cek negative cycle: relaksasi ke-V seharusnya tidak berhasil
    let hasNegativeCycle = false;
    const negativeCycleNodes = new Set();
    
    for (const { from, to, weight } of edges) {
        if (dist[from] !== Infinity && dist[from] + weight < dist[to]) {
            hasNegativeCycle = true;
            negativeCycleNodes.add(to);
        }
    }
    
    return { dist, prev, hasNegativeCycle, negativeCycleNodes };
}

// Test dengan graf tanpa negative cycle
const vertices1 = ['A', 'B', 'C', 'D', 'E'];
const edges1 = [
    { from: 'A', to: 'B', weight: -1 },
    { from: 'A', to: 'C', weight: 4 },
    { from: 'B', to: 'C', weight: 3 },
    { from: 'B', to: 'D', weight: 2 },
    { from: 'B', to: 'E', weight: 2 },
    { from: 'D', to: 'B', weight: 1 },
    { from: 'D', to: 'C', weight: 5 },
    { from: 'E', to: 'D', weight: -3 },
];

const result1 = bellmanFord(vertices1, edges1, 'A');
console.log('\n=== Bellman-Ford (no negative cycle) ===');
console.log('Negative cycle:', result1.hasNegativeCycle);
for (const [v, d] of Object.entries(result1.dist)) {
    console.log(`${v}: ${d}`);
}

// Test dengan negative cycle
const vertices2 = ['A', 'B', 'C'];
const edges2 = [
    { from: 'A', to: 'B', weight: 1 },
    { from: 'B', to: 'C', weight: -2 },
    { from: 'C', to: 'A', weight: -1 }, // A→B→C→A = 1-2-1 = -2 → NEGATIVE CYCLE!
];

const result2 = bellmanFord(vertices2, edges2, 'A');
console.log('\n=== Bellman-Ford (with negative cycle) ===');
console.log('Negative cycle detected:', result2.hasNegativeCycle); // true

// Aplikasi fintech: Arbitrage Detection
// Jika currency exchange A→B→C→A menghasilkan profit (negative log weight)
// → deteksi sebagai negative cycle → arbitrage opportunity!
function detectArbitrage(currencies, exchangeRates) {
    // rates = { 'USD→EUR': 0.85, 'EUR→GBP': 0.87, 'GBP→USD': 1.38 }
    // Kalikan: 0.85 × 0.87 × 1.38 = 1.0203 → profit 2.03%! → arbitrage!
    
    // Transform: -log(rate) sebagai bobot edge
    // Arbitrage = negative cycle dalam graf log weights
    
    const vertices = currencies;
    const edges = Object.entries(exchangeRates).map(([pair, rate]) => {
        const [from, to] = pair.split('→');
        return { from, to, weight: -Math.log(rate) };
    });
    
    const { hasNegativeCycle, negativeCycleNodes } = bellmanFord(vertices, edges, currencies[0]);
    
    return { 
        hasArbitrage: hasNegativeCycle, 
        involvedCurrencies: [...negativeCycleNodes] 
    };
}

const currencies = ['USD', 'EUR', 'GBP'];
const rates = {
    'USD→EUR': 0.85,
    'EUR→GBP': 0.87,
    'GBP→USD': 1.38, // 0.85 * 0.87 * 1.38 = 1.0203 → arbitrage!
    'EUR→USD': 1.18,
    'GBP→EUR': 1.15,
    'USD→GBP': 0.73,
};

const arb = detectArbitrage(currencies, rates);
console.log('\n=== Arbitrage Detection ===');
console.log('Arbitrage opportunity:', arb.hasArbitrage);
```

### 5.3 Floyd-Warshall

```javascript
/**
 * Floyd-Warshall — All-Pairs Shortest Path
 * 
 * @param {Array} nodes - list of node names
 * @param {Array} edges - [{from, to, weight}]
 * @returns {Object} - { dist, next } untuk path reconstruction
 */
function floydWarshall(nodes, edges) {
    const n = nodes.length;
    const idx = {};
    nodes.forEach((node, i) => { idx[node] = i; });
    
    // Inisialisasi dengan Infinity
    const dist = Array.from({length: n}, (_, i) => 
        Array.from({length: n}, (_, j) => i === j ? 0 : Infinity)
    );
    const next = Array.from({length: n}, () => Array(n).fill(null));
    
    // Set direct edges
    for (const { from, to, weight } of edges) {
        const i = idx[from], j = idx[to];
        if (weight < dist[i][j]) {
            dist[i][j] = weight;
            next[i][j] = j;
        }
    }
    // Set self-next
    for (let i = 0; i < n; i++) next[i][i] = i;
    
    // DP: coba setiap node sebagai intermediate
    for (let k = 0; k < n; k++) {
        for (let i = 0; i < n; i++) {
            for (let j = 0; j < n; j++) {
                if (dist[i][k] !== Infinity && dist[k][j] !== Infinity) {
                    if (dist[i][k] + dist[k][j] < dist[i][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j];
                        next[i][j] = next[i][k];
                    }
                }
            }
        }
    }
    
    // Deteksi negative cycle: diagonal negatif
    const hasNegativeCycle = nodes.some((_, i) => dist[i][i] < 0);
    
    // Path reconstruction
    function getPath(from, to) {
        const i = idx[from], j = idx[to];
        if (dist[i][j] === Infinity) return null; // tidak ada path
        
        const path = [from];
        let curr = i;
        while (curr !== j) {
            curr = next[curr][j];
            path.push(nodes[curr]);
        }
        return path;
    }
    
    // Print distance matrix
    function printMatrix() {
        process.stdout.write('     ');
        nodes.forEach(n => process.stdout.write(n.padStart(8)));
        console.log();
        
        for (let i = 0; i < n; i++) {
            process.stdout.write(nodes[i].padEnd(5));
            for (let j = 0; j < n; j++) {
                const d = dist[i][j] === Infinity ? '∞' : dist[i][j];
                process.stdout.write(String(d).padStart(8));
            }
            console.log();
        }
    }
    
    return { dist, next, hasNegativeCycle, getPath, printMatrix, nodes };
}

// Simulasi: Biaya pengiriman antar hub Tokopedia
const hubs = ['Jakarta', 'Bandung', 'Surabaya', 'Medan', 'Makassar'];
const hubEdges = [
    { from: 'Jakarta', to: 'Bandung', weight: 3 },
    { from: 'Bandung', to: 'Jakarta', weight: 3 },
    { from: 'Jakarta', to: 'Surabaya', weight: 8 },
    { from: 'Surabaya', to: 'Jakarta', weight: 8 },
    { from: 'Jakarta', to: 'Medan', weight: 15 },
    { from: 'Medan', to: 'Jakarta', weight: 15 },
    { from: 'Jakarta', to: 'Makassar', weight: 12 },
    { from: 'Makassar', to: 'Jakarta', weight: 12 },
    { from: 'Bandung', to: 'Surabaya', weight: 6 },
    { from: 'Surabaya', to: 'Bandung', weight: 6 },
    { from: 'Surabaya', to: 'Makassar', weight: 5 },
    { from: 'Makassar', to: 'Surabaya', weight: 5 },
];

const fw = floydWarshall(hubs, hubEdges);

console.log('\n=== All-Pairs Shortest Path (Delivery Cost) ===');
fw.printMatrix();

console.log('\nRoute Bandung → Makassar:');
const path = fw.getPath('Bandung', 'Makassar');
console.log(path ? path.join(' → ') : 'No path');
console.log('Total cost:', fw.dist[fw.nodes.indexOf('Bandung')][fw.nodes.indexOf('Makassar')]);

// Aplikasi: Transitive closure (apakah ada jalur antar node?)
function transitiveClosure(nodes, edges) {
    const fw = floydWarshall(nodes, edges);
    const n = nodes.length;
    return Array.from({length: n}, (_, i) => 
        Array.from({length: n}, (_, j) => fw.dist[i][j] !== Infinity)
    );
}
```

### 5.4 Perbandingan Tiga Algoritma

```javascript
/**
 * Performance Comparison: Dijkstra vs Bellman-Ford vs Floyd-Warshall
 */
function benchmark(V, E) {
    // Generate random graph
    const nodes = Array.from({length: V}, (_, i) => `N${i}`);
    const edges = [];
    
    // Connected graph
    for (let i = 0; i < V - 1; i++) {
        edges.push({ from: nodes[i], to: nodes[i+1], weight: Math.random() * 10 + 1 });
    }
    
    // Random additional edges
    for (let i = 0; i < E - (V-1); i++) {
        const from = nodes[Math.floor(Math.random() * V)];
        const to = nodes[Math.floor(Math.random() * V)];
        if (from !== to) {
            edges.push({ from, to, weight: Math.random() * 10 + 1 });
        }
    }
    
    // Build adjacency list for Dijkstra
    const adjList = {};
    nodes.forEach(n => { adjList[n] = []; });
    edges.forEach(({ from, to, weight }) => {
        adjList[from].push({ node: to, weight });
    });
    
    console.log(`\nV=${V}, E=${E}`);
    
    // Dijkstra (single source)
    console.time(`Dijkstra V=${V}`);
    dijkstra(adjList, nodes[0]);
    console.timeEnd(`Dijkstra V=${V}`);
    
    // Bellman-Ford (single source)
    console.time(`BellmanFord V=${V}`);
    bellmanFord(nodes, edges, nodes[0]);
    console.timeEnd(`BellmanFord V=${V}`);
    
    // Floyd-Warshall (all pairs) — hanya untuk V kecil
    if (V <= 100) {
        console.time(`FloydWarshall V=${V}`);
        floydWarshall(nodes, edges);
        console.timeEnd(`FloydWarshall V=${V}`);
    } else {
        console.log(`FloydWarshall V=${V}: SKIP (V³ = ${(V**3).toLocaleString()} terlalu besar)`);
    }
}

benchmark(50, 200);
benchmark(500, 2000);
benchmark(1000, 5000);
```

## 6. Studi Kasus Nyata: Gojek Real-Time Routing

**Konteks:** Gojek perlu menghitung ETA (Estimated Time of Arrival) untuk ribuan order secara real-time.

**Arsitektur:**

```
Driver (lokasi real-time)
        ↓
   Road Network Graph
   (50.000+ node Jakarta = persimpangan jalan)
   (200.000+ edge = ruas jalan dengan bobot: jarak + traffic)
        ↓
   Pre-computed: Floyd-Warshall untuk hub-to-hub (V=100 hub utama)
   Real-time: Dijkstra untuk driver-to-customer
        ↓
   ETA = jarak / kecepatan rata-rata berdasarkan jam dan hari
```

**Optimasi yang digunakan Gojek:**

1. **Bidirectional Dijkstra:** Jalankan Dijkstra dari source DAN dari target secara bersamaan → bertemu di tengah → 2× lebih cepat

2. **A* (A-star):** Dijkstra + heuristic (jarak Euclidean) → target-directed search → lebih cepat untuk single-pair

3. **Contraction Hierarchies:** Pre-process graph dengan "shortcut edges" → 100-1000× lebih cepat dari Dijkstra plain

4. **Cache agresif:** Rute populer (A→B) di-cache, diupdate setiap 5 menit

```javascript
// Bidirectional Dijkstra — outline
function bidirectionalDijkstra(graph, reverseGraph, source, target) {
    // Forward search dari source
    const forwardDist = { [source]: 0 };
    const forwardPQ = new PriorityQueue();
    forwardPQ.enqueue(0, source);
    
    // Backward search dari target
    const backwardDist = { [target]: 0 };
    const backwardPQ = new PriorityQueue();
    backwardPQ.enqueue(0, target);
    
    const forwardVisited = new Set();
    const backwardVisited = new Set();
    
    let best = Infinity;
    let meeting = null;
    
    while (!forwardPQ.isEmpty() || !backwardPQ.isEmpty()) {
        // Alternate: forward step
        if (!forwardPQ.isEmpty()) {
            const { value: curr, priority: d } = forwardPQ.dequeue();
            if (d > forwardDist[curr] || d > best) continue;
            
            forwardVisited.add(curr);
            
            if (backwardVisited.has(curr)) {
                const total = forwardDist[curr] + backwardDist[curr];
                if (total < best) { best = total; meeting = curr; }
            }
            
            for (const { node: next, weight } of (graph[curr] || [])) {
                const newD = d + weight;
                if (newD < (forwardDist[next] ?? Infinity)) {
                    forwardDist[next] = newD;
                    forwardPQ.enqueue(newD, next);
                }
            }
        }
        
        // Backward step (symmetric)
        // ...
    }
    
    return { distance: best, meetingPoint: meeting };
}
```

**Hasil:** Gojek mencapai ETA calculation **<10ms per request** untuk graph Jakarta dengan 50.000 persimpangan.

## 7. Visualisasi

### Dijkstra Step-by-Step

```
Graf:
A --4-- B --5-- D
|       |       |
2       1       2
|       |       |
C --8-- D      E
|
10
|
E

Step 1: dist={A:0, B:∞, C:∞, D:∞, E:∞}, PQ={A:0}
Step 2: Extract A. Relax B(4), C(2). dist={B:4, C:2}
Step 3: Extract C (dist=2). Relax B(3←min(4,3)), D(10), E(12)
Step 4: Extract B (dist=3). Relax D(8←min(10,8)), ...
...
Final: {A:0, C:2, B:3, D:8, E:10}
```

### Perbandingan Ketiga Algoritma

```
                    Dijkstra    Bellman-Ford    Floyd-Warshall
─────────────────────────────────────────────────────────────
Complexity          O((V+E)lgV) O(VE)           O(V³)
Negative weights    ✗           ✓               ✓
Negative cycles     ✗ (crash)   ✓ (detect)      ✓ (detect)
Single source       ✓           ✓               (convert)
All pairs           V× runs     V× runs         ✓ (direct)
Space               O(V+E)      O(V+E)          O(V²)
Praktis untuk       V,E besar   Neg. weights    V kecil (≤500)
```

## 8. Kesalahan Umum

### ❌ Kesalahan 1: Dijkstra dengan Bobot Negatif

```javascript
// SALAH — Dijkstra tidak valid dengan bobot negatif!
const graphWithNegative = {
    'A': [{ node: 'B', weight: 3 }, { node: 'C', weight: 1 }],
    'C': [{ node: 'B', weight: -5 }], // bobot negatif!
};

// Dijkstra mungkin return dist['B'] = 3 (via A→B langsung)
// Tapi optimal adalah 1 + (-5) = -4 (via A→C→B)
// Karena Dijkstra mark B sebagai "final" saat extract dari PQ!

// SOLUSI: Gunakan Bellman-Ford untuk negatif weight
```

### ❌ Kesalahan 2: Floyd-Warshall untuk Graph Besar

```javascript
// TIDAK SCALABLE
// V = 1000 → V³ = 10^9 operasi → ~1000 detik
// V = 500 → 125 juta operasi → ~0.5 detik (masih OK)

// Untuk V besar dengan all-pairs:
// 1. Jalankan Dijkstra dari setiap node: O(V × (V+E) log V)
// 2. Untuk dense graph: Johnson's algorithm O(V² log V + VE)
```

### ❌ Kesalahan 3: Tidak Handle Unreachable Nodes

```javascript
// SALAH — crash jika node unreachable
function wrongPath(prev, target) {
    const path = [];
    let curr = target;
    while (curr !== null) {        // loop infinite jika prev[curr] = curr!
        path.unshift(curr);
        curr = prev[curr];
    }
    return path;
}

// BENAR — cek apakah reachable
function safePath(dist, prev, source, target) {
    if (dist[target] === Infinity) return null; // tidak reachable
    
    const path = [];
    let curr = target;
    const visited = new Set();
    
    while (curr !== null && !visited.has(curr)) {
        visited.add(curr);
        path.unshift(curr);
        curr = prev[curr];
        if (curr === source) { path.unshift(source); break; }
    }
    
    return path;
}
```

### ❌ Kesalahan 4: Lazy Deletion yang Salah di Dijkstra

```javascript
// Penting: gunakan lazy deletion dengan benar
function dijkstraWrong(graph, source) {
    const dist = {};
    const pq = new PriorityQueue();
    // LUPA: tidak skip node yang sudah dikunjungi dengan dist lebih kecil
    
    while (!pq.isEmpty()) {
        const { value: curr, priority: d } = pq.dequeue();
        // Tanpa cek ini, node diproses berkali-kali!
        // if (d > dist[curr]) continue; ← HARUS ada!
    }
}
```

## 9. Latihan dan Studi Kasus

### Latihan 1 — Trace Dijkstra Manual

Graf:
```
    2       3
A --→-- B --→-- E
|       |       ↑
4       6       1
↓       ↓       |
C --→-- D ------+
    8       1
```

Trace Dijkstra dari A. Isi tabel:

| Step | Extracted | dist[A] | dist[B] | dist[C] | dist[D] | dist[E] |
|------|-----------|---------|---------|---------|---------|---------|
| Init | - | 0 | ∞ | ∞ | ∞ | ∞ |
| 1 | A | 0 | 2 | 4 | ∞ | ∞ |
| 2 | ? | 0 | ? | ? | ? | ? |
| ... | | | | | | |

**Jawaban:** Dijkstra extract B(2), C(4), D(min(10,12)=10), E(min(∞,5,11)=5 via B→C? tidak, via D+1=11 atau B+3=5!) → E=5

### Latihan 2 — Bellman-Ford Iteration

Graf: V={A,B,C,D}, edges:
- A→B weight=5, A→C weight=3
- B→C weight=-2, B→D weight=4
- C→D weight=6

Trace 3 iterasi Bellman-Ford dari A.

### Latihan 3 — Implementasi A*

```javascript
/**
 * A* Algorithm — Dijkstra dengan heuristic
 * 
 * f(n) = g(n) + h(n)
 * g(n) = actual cost dari source ke n
 * h(n) = heuristic estimate dari n ke target (harus admissible: ≤ actual)
 * 
 * Untuk geolocation: h = Euclidean atau Haversine distance
 */
function astar(graph, source, target, heuristic) {
    const gScore = { [source]: 0 };
    const fScore = { [source]: heuristic(source, target) };
    const prev = {};
    const visited = new Set();
    
    const openSet = new PriorityQueue();
    openSet.enqueue(fScore[source], source);
    
    while (!openSet.isEmpty()) {
        const { value: current } = openSet.dequeue();
        
        if (current === target) {
            // Rekonstruksi path
            const path = [];
            let curr = target;
            while (curr !== undefined) {
                path.unshift(curr);
                curr = prev[curr];
            }
            return { path, cost: gScore[target] };
        }
        
        if (visited.has(current)) continue;
        visited.add(current);
        
        for (const { node: neighbor, weight } of (graph[current] || [])) {
            const tentativeG = gScore[current] + weight;
            
            if (tentativeG < (gScore[neighbor] ?? Infinity)) {
                prev[neighbor] = current;
                gScore[neighbor] = tentativeG;
                fScore[neighbor] = tentativeG + heuristic(neighbor, target);
                openSet.enqueue(fScore[neighbor], neighbor);
            }
        }
    }
    
    return null; // tidak ada path
}

// Heuristic untuk koordinat kota
const cityCoords = {
    'Jakarta': { lat: -6.2088, lng: 106.8456 },
    'Bandung': { lat: -6.9175, lng: 107.6191 },
    'Surabaya': { lat: -7.2575, lng: 112.7521 },
};

function euclideanHeuristic(city1, city2) {
    const c1 = cityCoords[city1];
    const c2 = cityCoords[city2];
    if (!c1 || !c2) return 0;
    return Math.sqrt((c1.lat - c2.lat)**2 + (c1.lng - c2.lng)**2) * 111; // rough km
}

// A* biasanya lebih cepat dari Dijkstra untuk single-pair karena goal-directed
```

## 10. Ringkasan

| Algoritma | Complexity | Negative Weights | Use Case |
|-----------|-----------|-----------------|----------|
| Dijkstra | O((V+E) log V) | ❌ | Road networks, maps |
| Dijkstra Bidi | O((V+E) log V)/2 praktis | ❌ | Single-pair fast |
| A* | O((V+E) log V) | ❌ | Heuristic-guided search |
| Bellman-Ford | O(VE) | ✓ | Negative weights, arbitrage |
| Floyd-Warshall | O(V³) | ✓ | All-pairs, V ≤ 500 |
| Johnson's | O(V² log V + VE) | ✓ | All-pairs, sparse graph |

**Pemilihan Algoritma:**

```
Bobot semua positif?
├── Ya → Single source atau all-pairs?
│   ├── Single: Dijkstra
│   └── All: V kecil? → Floyd | V besar, sparse? → V× Dijkstra
└── Tidak (ada negatif) →
    ├── Perlu all-pairs? → Floyd-Warshall
    ├── Perlu deteksi neg cycle? → Bellman-Ford
    └── Single source only → Bellman-Ford
```

## 11. Referensi

- Cormen, T.H. et al. — *Introduction to Algorithms (CLRS)*, 4th Ed., Bab 22-24 (Elementary Graph, Single-Source, All-Pairs Shortest Path)
- Dijkstra, E.W. — "A Note on Two Problems in Connexion with Graphs", 1959 (makalah asli)
- Bhargava, A. — *Grokking Algorithms*, Bab 7 (Dijkstra)
- Sedgewick, R. — *Algorithms*, 4th Ed., Bab 4.4 (Shortest Paths)
- Visualgo.net — Shortest Path visualization
- LeetCode — #743 (Network Delay Time), #787 (Cheapest Flights Within K Stops), #399 (Evaluate Division)
- Stanford CS161 — Graph Algorithms lecture notes
- Gojek Engineering Blog — "How We Built Real-Time ETA" (tersedia online)
