# Pertemuan 5: Greedy Algorithms

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Mengidentifikasi masalah yang dapat diselesaikan secara optimal dengan pendekatan greedy
- Membuktikan keoptimalan greedy menggunakan argumen "Exchange Argument"
- Mengimplementasikan Activity Selection dan Huffman Coding dalam JavaScript
- Membangun priority queue (min-heap) dari awal untuk mendukung Huffman Coding

## 2. Pengantar: Hook

Setiap hari, sistem Tokopedia harus mengirim **jutaan notifikasi push** ke pengguna — promosi, konfirmasi order, pengiriman. Setiap notifikasi memiliki kategori: 'P' (promo), 'O' (order update), 'S' (sistem). Jika kamu encode kategori ini secara naif: 2 bit per karakter. Tapi Huffman Coding bisa encode huruf paling sering dengan **1 bit** dan yang jarang dengan lebih banyak bit — menghemat bandwidth hingga **40-60%**.

Di sisi lain, tim operations harus menjadwalkan **ribuan delivery slot** dengan driver terbatas — memilih order mana yang dijadwalkan untuk memaksimalkan jumlah transaksi terselesaikan. Ini adalah **Activity Selection Problem** — dan greedy algorithm memberikan solusi optimal dalam O(n log n).

Kesamaan keduanya: buat keputusan lokal terbaik di setiap langkah → hasilkan solusi global optimal. Itulah **Greedy**.

## 3. Konsep Utama

### 3.1 Kapan Greedy Bekerja?

Greedy berhasil ketika masalah memiliki dua properti:

**1. Greedy Choice Property:**
Solusi optimal global bisa dibangun dari pilihan lokal yang optimal, **tanpa mempertimbangkan submasalah yang akan datang**.

**2. Optimal Substructure:**
Setelah membuat greedy choice, submasalah yang tersisa juga memiliki struktur optimal yang sama.

**Membuktikan Greedy:** Exchange Argument
- Asumsikan ada solusi optimal berbeda dari greedy
- Tunjukkan bahwa kamu bisa "exchange" elemen solusi optimal dengan pilihan greedy
- Tunjukkan bahwa setelah exchange, solusinya tidak menjadi lebih buruk
- Kontradiksi dengan asumsi bahwa solusi non-greedy lebih optimal

### 3.2 Activity Selection Problem

**Problem:** Diberikan n aktivitas dengan waktu mulai dan selesai. Pilih maksimum aktivitas yang tidak saling tumpang tindih.

**Greedy Strategy:** Selalu pilih aktivitas yang **selesai paling awal** dari yang tersisa.

**Mengapa ini optimal?**
- Exchange Argument: Jika solusi optimal memilih aktivitas A yang selesai lebih lambat dari B (yang greedy pilih), kita bisa ganti A dengan B
- B selesai lebih awal → memberi lebih banyak ruang untuk aktivitas selanjutnya
- Jadi mengganti A dengan B tidak mengurangi jumlah aktivitas → B ≥ A dalam hal kontribusi optimal

### 3.3 Huffman Coding

**Problem:** Encode sekumpulan simbol dengan frekuensi berbeda menggunakan bit optimal.

**Prinsip:**
- Simbol sering → kode pendek
- Simbol jarang → kode panjang
- **Prefix-free:** tidak ada kode yang merupakan prefix dari kode lain

**Greedy Strategy:** Selalu gabungkan dua node dengan frekuensi **terkecil** terlebih dahulu.

**Struktur data:** Min-heap (priority queue)

**Kompleksitas:** O(n log n) — setiap merge adalah O(log n), ada n-1 merge

## 4. Ilustrasi dan Analogi

### Analogi Activity Selection: Booking Ruang Meeting

Kamu adalah admin yang harus menjadwalkan penggunaan ruang rapat. Ada 10 tim yang ingin booking, tapi ruang hanya 1. Bagaimana memaksimalkan jumlah tim yang bisa menggunakan ruang?

**Strategi salah (greedy durasi terpendek):**
Tim A: 09:00-10:30 (1.5 jam), Tim B: 09:00-09:30 (0.5 jam), Tim C: 10:00-11:00 (1 jam)
Greedy durasi pilih B, lalu tidak bisa C (tumpang tindih) → 2 tim
Greedy earliest-end pilih B, lalu pilih C → 2 tim juga ✓

**Counter-example untuk greedy durasi:**
Tim A: 09:00-12:00 (3 jam) — paling lama, tapi greedy durasi tidak pilih ini
Tim B: 09:00-10:00, Tim C: 10:30-11:30, Tim D: 12:00-13:00 → 3 tim!
Greedy earliest-end: B (selesai 10:00) → C (selesai 11:30) → D → **3 tim ✓**

### Analogi Huffman: Sistem Kode Morse

Kode Morse secara intuitif menggunakan Huffman-like encoding:
- E (paling sering): `.` (1 sinyal)
- T: `-` (1 sinyal)
- A: `.-` (2 sinyal)
- Z: `--..` (4 sinyal)

Huffman Coding memformalisasi dan mengoptimalkan intuisi ini secara matematis.

## 5. Contoh Teknis

### 5.1 Activity Selection

```javascript
/**
 * Activity Selection Problem
 * 
 * Greedy: Sort by finish time, greedily pick non-conflicting activities
 * 
 * @param {Array} activities - [{id, name, start, end}]
 * @returns {Array} - selected activities
 */
function activitySelection(activities) {
    // Sort by finish time (ascending)
    const sorted = [...activities].sort((a, b) => a.end - b.end);
    
    const selected = [sorted[0]]; // Selalu pilih yang pertama selesai
    let lastEnd = sorted[0].end;
    
    for (let i = 1; i < sorted.length; i++) {
        // Pilih jika mulai setelah (atau saat) aktivitas terakhir selesai
        if (sorted[i].start >= lastEnd) {
            selected.push(sorted[i]);
            lastEnd = sorted[i].end;
        }
    }
    
    return selected;
}

// Simulasi: Penjadwalan delivery slot Tokopedia
const deliverySlots = [
    { id: 1, name: 'Jakarta Selatan Batch A', start: 8, end: 10 },
    { id: 2, name: 'Jakarta Pusat', start: 9, end: 11 },
    { id: 3, name: 'Jakarta Selatan Batch B', start: 10, end: 12 },
    { id: 4, name: 'Depok', start: 11, end: 13 },
    { id: 5, name: 'Bekasi', start: 13, end: 15 },
    { id: 6, name: 'Tangerang', start: 12, end: 14 },
    { id: 7, name: 'Bogor', start: 14, end: 16 },
    { id: 8, name: 'Jakarta Utara', start: 8, end: 9 },
];

const optimal = activitySelection(deliverySlots);
console.log(`Maksimum slot terjadwal: ${optimal.length}`);
optimal.forEach(s => {
    console.log(`  [${s.start}:00-${s.end}:00] ${s.name}`);
});

// Versi dengan multiple resources (k drivers/rooms)
function activitySelectionKResources(activities, k) {
    const sorted = [...activities].sort((a, b) => a.end - b.end);
    
    // Track end time tiap resource (min-heap simulation dengan sorted array)
    const resourceEnds = Array(k).fill(0); // setiap resource mulai available di t=0
    const selected = [];
    
    for (const activity of sorted) {
        resourceEnds.sort((a, b) => a - b); // sort ascending
        
        // Resource paling awal selesai
        if (resourceEnds[0] <= activity.start) {
            selected.push(activity);
            resourceEnds[0] = activity.end; // update resource tersebut
        }
    }
    
    return selected;
}

// Dengan 2 driver:
const with2Drivers = activitySelectionKResources(deliverySlots, 2);
console.log(`\nDengan 2 driver: ${with2Drivers.length} slot`);
with2Drivers.forEach(s => console.log(`  [${s.start}:00-${s.end}:00] ${s.name}`));
```

### 5.2 Min-Heap (Priority Queue) dari Scratch

```javascript
/**
 * Min-Heap Implementation
 * 
 * Mendukung Huffman Coding dan Dijkstra
 * 
 * Operations:
 * - insert: O(log n)
 * - extractMin: O(log n)
 * - peek: O(1)
 */
class MinHeap {
    constructor(comparator = (a, b) => a - b) {
        this.heap = [];
        this.compare = comparator;
    }
    
    get size() { return this.heap.length; }
    
    peek() { return this.heap[0]; }
    
    insert(value) {
        this.heap.push(value);
        this._bubbleUp(this.heap.length - 1);
    }
    
    extractMin() {
        if (this.size === 0) return null;
        if (this.size === 1) return this.heap.pop();
        
        const min = this.heap[0];
        this.heap[0] = this.heap.pop(); // pindahkan elemen terakhir ke root
        this._sinkDown(0);
        return min;
    }
    
    _bubbleUp(idx) {
        while (idx > 0) {
            const parentIdx = Math.floor((idx - 1) / 2);
            if (this.compare(this.heap[idx], this.heap[parentIdx]) < 0) {
                [this.heap[idx], this.heap[parentIdx]] = [this.heap[parentIdx], this.heap[idx]];
                idx = parentIdx;
            } else break;
        }
    }
    
    _sinkDown(idx) {
        const n = this.heap.length;
        while (true) {
            let smallest = idx;
            const left = 2 * idx + 1;
            const right = 2 * idx + 2;
            
            if (left < n && this.compare(this.heap[left], this.heap[smallest]) < 0) {
                smallest = left;
            }
            if (right < n && this.compare(this.heap[right], this.heap[smallest]) < 0) {
                smallest = right;
            }
            
            if (smallest !== idx) {
                [this.heap[idx], this.heap[smallest]] = [this.heap[smallest], this.heap[idx]];
                idx = smallest;
            } else break;
        }
    }
    
    // Heapify dari array — O(n)
    static fromArray(arr, comparator) {
        const heap = new MinHeap(comparator);
        heap.heap = [...arr];
        // Bottom-up heapify dari node terakhir yang bukan leaf
        for (let i = Math.floor(arr.length / 2) - 1; i >= 0; i--) {
            heap._sinkDown(i);
        }
        return heap;
    }
    
    toSortedArray() {
        const copy = new MinHeap(this.compare);
        copy.heap = [...this.heap];
        const result = [];
        while (copy.size > 0) result.push(copy.extractMin());
        return result;
    }
}

// Test MinHeap
const heap = new MinHeap();
[5, 3, 8, 1, 9, 2, 7].forEach(x => heap.insert(x));
console.log('Sorted:', heap.toSortedArray()); // [1, 2, 3, 5, 7, 8, 9]
```

### 5.3 Huffman Coding — Implementasi Lengkap

```javascript
/**
 * Huffman Coding
 * 
 * Langkah:
 * 1. Hitung frekuensi setiap simbol
 * 2. Buat node leaf untuk setiap simbol
 * 3. Insert semua node ke min-heap berdasarkan frekuensi
 * 4. Greedy: extract 2 minimum, gabungkan, insert kembali
 * 5. Ulangi hingga 1 node tersisa (root)
 * 6. Traverse pohon: left = 0, right = 1
 */
class HuffmanNode {
    constructor(char, freq, left = null, right = null) {
        this.char = char;     // null untuk internal node
        this.freq = freq;
        this.left = left;
        this.right = right;
    }
    
    get isLeaf() { return this.left === null && this.right === null; }
}

class HuffmanCoding {
    constructor(text) {
        this.text = text;
        this.codes = {};
        this.root = null;
        this._build();
    }
    
    _build() {
        // Step 1: Hitung frekuensi
        const freq = {};
        for (const char of this.text) {
            freq[char] = (freq[char] || 0) + 1;
        }
        
        // Step 2 & 3: Buat min-heap dari leaf nodes
        const heap = new MinHeap((a, b) => {
            if (a.freq !== b.freq) return a.freq - b.freq;
            return (a.char || '\0') < (b.char || '\0') ? -1 : 1; // tie-breaking
        });
        
        for (const [char, f] of Object.entries(freq)) {
            heap.insert(new HuffmanNode(char, f));
        }
        
        // Edge case: hanya 1 karakter unik
        if (heap.size === 1) {
            const only = heap.extractMin();
            this.root = new HuffmanNode(null, only.freq, only, null);
            this.codes[only.char] = '0';
            return;
        }
        
        // Step 4 & 5: Greedy merge
        while (heap.size > 1) {
            const left = heap.extractMin();  // frekuensi terkecil
            const right = heap.extractMin(); // frekuensi terkecil kedua
            
            const merged = new HuffmanNode(null, left.freq + right.freq, left, right);
            heap.insert(merged);
        }
        
        this.root = heap.extractMin();
        
        // Step 6: Generate codes dengan DFS
        this._generateCodes(this.root, '');
    }
    
    _generateCodes(node, code) {
        if (!node) return;
        
        if (node.isLeaf) {
            this.codes[node.char] = code || '0'; // '0' jika root adalah leaf
            return;
        }
        
        this._generateCodes(node.left, code + '0');
        this._generateCodes(node.right, code + '1');
    }
    
    encode(text = this.text) {
        return text.split('').map(c => this.codes[c]).join('');
    }
    
    decode(encoded) {
        let result = '';
        let current = this.root;
        
        for (const bit of encoded) {
            current = bit === '0' ? current.left : current.right;
            
            if (current.isLeaf) {
                result += current.char;
                current = this.root;
            }
        }
        
        return result;
    }
    
    printStats() {
        console.log('\n=== Huffman Coding Stats ===');
        console.log('Character | Freq | Code    | Bits');
        console.log('----------|------|---------|-----');
        
        const entries = Object.entries(this.codes)
            .sort((a, b) => a[1].length - b[1].length);
        
        let totalBitsHuffman = 0;
        let totalBitsFixed = 0;
        const fixedBits = Math.ceil(Math.log2(entries.length));
        
        for (const [char, code] of entries) {
            const freq = this.text.split(char).length - 1;
            const bits = code.length * freq;
            totalBitsHuffman += bits;
            totalBitsFixed += fixedBits * freq;
            
            const display = char === ' ' ? 'SPACE' : char === '\n' ? '\\n' : char;
            console.log(`${display.padEnd(10)}| ${String(freq).padEnd(5)}| ${code.padEnd(8)}| ${code.length}`);
        }
        
        const original = this.text.length * 8; // ASCII 8-bit per char
        console.log(`\nOriginal (ASCII 8-bit):  ${original} bits`);
        console.log(`Fixed-length encoding:   ${totalBitsFixed} bits (${fixedBits} bits/char)`);
        console.log(`Huffman encoding:        ${totalBitsHuffman} bits`);
        console.log(`Compression ratio:       ${(totalBitsHuffman / original * 100).toFixed(1)}% of original`);
        console.log(`Space savings:           ${((1 - totalBitsHuffman / original) * 100).toFixed(1)}%`);
    }
    
    printTree(node = this.root, prefix = '', isLeft = true) {
        if (!node) return;
        const label = node.isLeaf 
            ? `'${node.char === ' ' ? 'SPACE' : node.char}' (${node.freq})`
            : `[${node.freq}]`;
        console.log(prefix + (isLeft ? '├── ' : '└── ') + label);
        if (!node.isLeaf) {
            this.printTree(node.left, prefix + (isLeft ? '│   ' : '    '), true);
            this.printTree(node.right, prefix + (isLeft ? '│   ' : '    '), false);
        }
    }
}

// Simulasi: Kompresi log event Tokopedia
const eventLog = 'PPPOOOOSSPSOPSOPPSOPPPOOOOOPPSSSPPPOOOO';
// P=promo, O=order, S=shipment, lainnya jarang

console.log('=== Huffman Coding Demo ===');
console.log('Input:', eventLog);

const huffman = new HuffmanCoding(eventLog);

console.log('\nHuffman Tree:');
huffman.printTree();

huffman.printStats();

const encoded = huffman.encode(eventLog);
console.log(`\nEncoded: ${encoded.substring(0, 40)}... (${encoded.length} bits)`);

const decoded = huffman.decode(encoded);
console.log(`Decoded matches original: ${decoded === eventLog}`);

// Test dengan teks lebih realistis
const notifText = 'Promo hari ini! Dapatkan cashback 20% untuk semua pembayaran di Tokopedia.';
const huffman2 = new HuffmanCoding(notifText);
huffman2.printStats();
```

### 5.4 Fractional Knapsack (Greedy Optimal)

```javascript
/**
 * Fractional Knapsack — Greedy by ratio value/weight
 * 
 * Berbeda dari 0/1 Knapsack: kita BISA ambil sebagian item
 * → Greedy BEKERJA di sini (tidak untuk 0/1!)
 * 
 * Aplikasi: portfolio optimization, resource allocation
 */
function fractionalKnapsack(items, capacity) {
    // Greedy: sort by value/weight ratio (descending)
    const sorted = [...items]
        .map(item => ({ ...item, ratio: item.value / item.weight }))
        .sort((a, b) => b.ratio - a.ratio);
    
    let totalValue = 0;
    let remainingCap = capacity;
    const taken = [];
    
    for (const item of sorted) {
        if (remainingCap <= 0) break;
        
        if (item.weight <= remainingCap) {
            // Ambil seluruh item
            taken.push({ ...item, fraction: 1 });
            totalValue += item.value;
            remainingCap -= item.weight;
        } else {
            // Ambil sebagian
            const fraction = remainingCap / item.weight;
            taken.push({ ...item, fraction });
            totalValue += item.value * fraction;
            remainingCap = 0;
        }
    }
    
    return { totalValue, taken };
}

// Contoh: alokasi dana investasi
const investments = [
    { name: 'Reksa Dana Saham', value: 12, weight: 4 },  // return 12%, butuh 4M modal
    { name: 'Deposito BCA', value: 5, weight: 2 },
    { name: 'ORI (Obligasi Ritel)', value: 8, weight: 3 },
    { name: 'P2P Lending', value: 15, weight: 6 },
    { name: 'Emas Digital', value: 7, weight: 2 },
];

const result = fractionalKnapsack(investments, 10);
console.log('\n=== Portfolio Optimization ===');
console.log('Budget: 10M');
result.taken.forEach(item => {
    const frac = (item.fraction * 100).toFixed(0);
    const allocated = (item.weight * item.fraction).toFixed(1);
    console.log(`  ${item.name}: ${frac}% → ${allocated}M modal → return ${(item.value * item.fraction).toFixed(1)}%`);
});
console.log(`Total expected return: ${result.totalValue.toFixed(2)}%`);
```

## 6. Studi Kasus Nyata: Tokopedia Notification Compression

**Konteks:** Tokopedia mengirim 5 juta notifikasi push per hari. Setiap notifikasi berisi teks promosi yang dikirim via WebSocket ke app pengguna. Bandwidth adalah biaya signifikan.

**Sebelum Huffman:** Setiap karakter = 8 bit ASCII
- Rata-rata notifikasi: 100 karakter = 800 bit = 100 byte
- 5 juta notif/hari × 100 byte = **500 MB/hari** bandwidth

**Setelah Huffman per bahasa Indonesia:**

```javascript
// Analisis frekuensi karakter dalam teks notifikasi Bahasa Indonesia
const sampleText = `
Selamat! Promo cashback 20% menanti Anda.
Belanja sekarang dan hemat lebih banyak.
Penawaran terbatas, segera gunakan!
Tokopedia menyediakan ribuan produk pilihan.
`;

const huffman3 = new HuffmanCoding(sampleText);
huffman3.printStats();

// Hasil tipikal untuk Bahasa Indonesia:
// 'a' (17.5%) → 2-3 bit
// 'e' (12.1%) → 3 bit
// 'i' (9.8%)  → 3 bit
// 'n' (9.1%)  → 3-4 bit
// 'z' (0.1%)  → 8-9 bit

// Compression ratio: ~60% dari ASCII
// 500 MB × 0.60 = 300 MB → hemat 200 MB/hari
// Dengan 10 Gbps bandwidth cost ~$0.08/GB → hemat ~$16/hari = $5840/tahun
```

**Implementasi di produksi Tokopedia:**
- Gunakan **pre-built Huffman table** untuk Bahasa Indonesia (bukan rebuild tiap pesan)
- Kombinasi dengan **LZ77/LZ78** (basis zlib/gzip) untuk kompresi lebih baik
- **Brotli** (Google, 2013): Huffman + LZ77 + context modeling → 20-26% lebih baik dari gzip
- Pada HTTP/2 headers: **HPACK** menggunakan Huffman coding untuk header compression

## 7. Visualisasi

### Activity Selection Timeline

```
Aktivitas    |  8  |  9  | 10  | 11  | 12  | 13  | 14  | 15  | 16
-------------|-----|-----|-----|-----|-----|-----|-----|-----|-----
Jkt Utara   [■■■■■]
Jkt Sel A   [■■■■■■■■■■■]
Jkt Pusat        [■■■■■■■■■■■]
Jkt Sel B              [■■■■■■■■■■■]
Tangerang                   [■■■■■■■■■■■]
Depok                   [■■■■■■■■■■■]
Bekasi                              [■■■■■■■■■■■]
Bogor                                    [■■■■■■■■■■■]

Greedy picks: Jkt Utara(8-9) → Jkt Sel B(10-12) → Bekasi(13-15) → Bogor(14-16)?
→ Bekasi(13-15) → Bogor(14-16): TUMPANG TINDIH!
→ Jkt Utara(8-9) → Jkt Sel B(10-12) → Tangerang(12-14) → Bogor(14-16) = 4 ✓
```

### Huffman Tree untuk "AABBBCCDD"

```
        [9]
       /   \
     [4]   [5]
    /   \  /  \
   [2] [2][2] [3]
   A   C  D   B

A=freq 2, B=freq 3, C=freq 2, D=freq 2

Merge sequence:
1. {A:2, C:2} → merge → [AC:4]
2. {D:2, B:3} → merge → [DB:5]  
3. {[AC]:4, [DB]:5} → merge → root[9]

Codes: A=00, C=01, D=10, B=11
Bits: A(2×2=4) + C(2×2=4) + D(2×2=4) + B(3×2=6) = 18 bits
vs Fixed 2-bit: 9×2 = 18 bits (sama karena frekuensi mirip)
```

### Huffman vs Fixed Encoding

```
Karakter  Freq  Fixed(3bit)  Huffman  Savings
----------|-----|------------|---------|-------
'a'        45    000          0        (45×3)-(45×1) = 90 bits saved
'b'        13    001          101      (13×3)-(13×3) = 0
'c'        12    010          100      (12×3)-(12×3) = 0
'd'        16    011          111      (16×3)-(16×3) = 0
'e'         9    100          1101     (9×3)-(9×4) = -9 (9 bits more)
'f'         5    101          1100     (5×3)-(5×4) = -5 (5 bits more)

Total saves: 90+0+0+0-9-5 = 76 bits saved dari 100 karakter
```

## 8. Kesalahan Umum

### ❌ Kesalahan 1: Greedy untuk 0/1 Knapsack

```javascript
// SALAH — greedy ratio tidak optimal untuk 0/1 Knapsack!
function knapsackGreedyWrong(items, capacity) {
    const sorted = [...items].sort((a, b) => (b.value/b.weight) - (a.value/a.weight));
    // Ini benar hanya untuk FRACTIONAL knapsack!
    // Untuk 0/1: HARUS gunakan DP
}

// Counter-example:
const items = [
    { value: 60, weight: 10 },  // ratio 6 — greedy pilih ini
    { value: 100, weight: 20 }, // ratio 5
    { value: 120, weight: 30 }, // ratio 4
];
// capacity = 50
// Greedy: 60 + 100 = 160 (sisa 20 tidak terpakai)
// Optimal: 100 + 120 = 220 ✓
```

### ❌ Kesalahan 2: Activity Selection dengan Greedy Salah

```javascript
// Strategi yang tampak masuk akal tapi SALAH:

// 1. Greedy by start time — SALAH
// Counter: {start:8, end:20}, {start:9, end:10}, {start:11, end:12}
// By start: pilih slot panjang → hanya 1 aktivitas
// Optimal: 3 aktivitas!

// 2. Greedy by shortest duration — SALAH
// Counter: {s:1,e:3}, {s:2,e:4}, {s:3,e:5}
// Duration terpendek semua sama (2) → bisa salah pilih
// Optimal: {1-3} + {3-5} = 2 (bukan {2-4} yang tengah)

// 3. Greedy by fewest conflicts — SALAH untuk kasus tertentu
// Lebih kompleks dan tidak selalu optimal

// BENAR SATU-SATUNYA: greedy by earliest finish time
```

### ❌ Kesalahan 3: Huffman dengan Frekuensi 0

```javascript
// CRASH jika ada karakter dengan frekuensi 0 di kamus
function huffmanBroken(freq) {
    for (const [char, f] of Object.entries(freq)) {
        heap.insert(new HuffmanNode(char, f)); // f bisa 0!
    }
    // Node dengan freq=0 akan selalu diprioritaskan → pohon tidak optimal
}

// BENAR — hanya masukkan karakter yang benar-benar muncul
function huffmanFixed(text) {
    const freq = {};
    for (const char of text) {
        freq[char] = (freq[char] || 0) + 1;
    }
    // freq hanya berisi char dengan frekuensi > 0
}
```

### ❌ Kesalahan 4: Decode Huffman dengan Root yang Salah

```javascript
// SALAH — tidak reset ke root setelah decode satu karakter
function decodeBroken(encoded, root) {
    let current = root;
    const result = [];
    
    for (const bit of encoded) {
        current = bit === '0' ? current.left : current.right;
        if (current.isLeaf) {
            result.push(current.char);
            // LUPA reset ke root!
        }
    }
    return result.join('');
}

// BENAR
function decodeFixed(encoded, root) {
    let current = root;
    const result = [];
    
    for (const bit of encoded) {
        current = bit === '0' ? current.left : current.right;
        if (current.isLeaf) {
            result.push(current.char);
            current = root; // RESET ← ini penting!
        }
    }
    return result.join('');
}
```

## 9. Latihan dan Studi Kasus

### Latihan 1 — Task Scheduling (Weighted Job Scheduling)

```javascript
/**
 * Weighted Job Scheduling
 * 
 * Setiap job punya profit. Pilih job yang tidak tumpang tindih
 * untuk MEMAKSIMALKAN total profit.
 * 
 * Perhatian: Greedy biasa TIDAK optimal di sini!
 * Perlu DP karena ada profit (tidak semua job bernilai sama)
 * 
 * Ini kontras dengan Activity Selection (semua job bernilai sama)
 */
function weightedJobScheduling(jobs) {
    // Sort by finish time
    const sorted = [...jobs].sort((a, b) => a.end - b.end);
    const n = sorted.length;
    
    // Precompute: untuk job i, job terbaru yang tidak tumpang tindih
    function latestNonConflict(idx) {
        for (let j = idx - 1; j >= 0; j--) {
            if (sorted[j].end <= sorted[idx].start) return j;
        }
        return -1;
    }
    
    // DP
    const dp = Array(n).fill(0);
    dp[0] = sorted[0].profit;
    
    for (let i = 1; i < n; i++) {
        const inclProfit = sorted[i].profit;
        const l = latestNonConflict(i);
        const inclTotal = l !== -1 ? dp[l] + inclProfit : inclProfit;
        dp[i] = Math.max(dp[i - 1], inclTotal);
    }
    
    return dp[n - 1];
}

const weightedJobs = [
    { start: 1, end: 2, profit: 50 },
    { start: 3, end: 5, profit: 20 },
    { start: 6, end: 19, profit: 100 },
    { start: 2, end: 100, profit: 200 }, // besar tapi lama
];

console.log('Max profit:', weightedJobScheduling(weightedJobs));
// Expected: 250 (job 1 + job 4 OR job 1 + job 2 + job 3?)
```

### Latihan 2 — Coin Change Greedy vs DP

```javascript
// Greedy BEKERJA untuk denominasi tertentu (mis. USD: 25,10,5,1)
// Greedy TIDAK BEKERJA untuk semua denominasi

// Denominasi: [1, 3, 4], amount = 6
// Greedy: 4 + 1 + 1 = 3 koin
// Optimal DP: 3 + 3 = 2 koin ✓

function coinChangeGreedy(coins, amount) {
    const sorted = [...coins].sort((a, b) => b - a); // descending
    let count = 0;
    let rem = amount;
    const used = [];
    
    for (const coin of sorted) {
        while (rem >= coin) {
            used.push(coin);
            rem -= coin;
            count++;
        }
    }
    
    return rem === 0 ? { count, coins: used } : { count: -1, coins: [] };
}

// Test: Denominasi yang greedy gagal
console.log(coinChangeGreedy([1, 3, 4], 6)); // count=3 (4+1+1)
// Tapi DP: 2 koin (3+3) ← greedy salah!

// Test: Denominasi yang greedy berhasil (sistem kanonik)
console.log(coinChangeGreedy([1, 5, 10, 25], 41)); // count=4 (25+10+5+1) ✓
```

### Latihan 3 — Huffman Decode Challenge

Diberikan encoded string dan tabel kode Huffman:
- `A=0`, `B=10`, `C=110`, `D=111`

Decode: `0101100111`

**Solusi:**
```
0    → A
10   → B
110  → C
0    → A
111  → D

Result: "ABCAD"
```

Implementasikan decoder dari tabel kode (tanpa pohon):

```javascript
function decodeFromTable(encoded, codeTable) {
    // Balik tabel: code → char
    const reverseTable = {};
    for (const [char, code] of Object.entries(codeTable)) {
        reverseTable[code] = char;
    }
    
    let result = '';
    let current = '';
    
    for (const bit of encoded) {
        current += bit;
        if (reverseTable[current]) {
            result += reverseTable[current];
            current = '';
        }
    }
    
    if (current.length > 0) throw new Error('Invalid encoded string');
    return result;
}

const codeTable = { A: '0', B: '10', C: '110', D: '111' };
console.log(decodeFromTable('0101100111', codeTable)); // "ABCAD"
```

## 10. Ringkasan

| Masalah | Greedy? | Strategy | Complexity |
|---------|---------|----------|-----------|
| Activity Selection | ✅ Ya | Earliest finish time | O(n log n) |
| Fractional Knapsack | ✅ Ya | Highest value/weight ratio | O(n log n) |
| Huffman Coding | ✅ Ya | Merge two lowest frequency | O(n log n) |
| 0/1 Knapsack | ❌ Tidak | Butuh DP | O(nW) |
| Coin Change (umum) | ❌ Tidak | Butuh DP | O(amount × coins) |
| Shortest Path | ❌ Tidak* | Dijkstra (greedy heuristic) | O((V+E) log V) |

**Exchange Argument Template:**

```
1. Asumsikan solusi optimal OPT berbeda dari greedy G
2. Temukan titik pertama perbedaan: G pilih X, OPT pilih Y
3. Tunjukkan bisa swap Y dengan X di OPT → OPT' tidak lebih buruk
4. Ulangi hingga OPT' = G → G adalah optimal
```

## 11. Referensi

- Cormen, T.H. et al. — *Introduction to Algorithms (CLRS)*, 4th Ed., Bab 15 (Greedy Algorithms), Bab 16 (Huffman Codes)
- Bhargava, A. — *Grokking Algorithms*, Bab 8 (Greedy Algorithms)
- Huffman, D.A. — "A Method for the Construction of Minimum-Redundancy Codes", 1952 (makalah asli)
- Kleinberg, J. & Tardos, E. — *Algorithm Design*, Bab 4 (Greedy)
- Wikipedia — [Huffman Coding](https://en.wikipedia.org/wiki/Huffman_coding)
- Visualgo.net — Huffman Coding visualization
- MDN Web Docs — [Compression in HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Compression)
- RFC 7541 — HPACK: Header Compression for HTTP/2 (menggunakan Huffman)
