# Pertemuan 6: Backtracking

## 1. Learning Outcomes
Setelah mengikuti perkuliahan ini, mahasiswa mampu:
- Memahami paradigma backtracking sebagai DFS dengan constraint pruning
- Mengimplementasikan N-Queens Problem dan Sudoku Solver dalam JavaScript
- Menerapkan teknik pruning untuk mengurangi search space secara signifikan
- Membedakan kapan menggunakan backtracking vs DP vs greedy

## 2. Pengantar: Hook

Bayangkan algoritma yang harus **mencoba semua kemungkinan** — tapi dengan **kecerdasan untuk berhenti lebih awal** ketika jelas bahwa jalur saat ini tidak akan menghasilkan solusi valid.

Setiap hari, sistem keamanan Gojek menjalankan **constraint satisfaction** untuk penjadwalan driver: driver A tidak boleh kerja > 12 jam, driver B tidak bisa di zona tertentu karena SIM, driver C sudah booking shift lain. Ini adalah masalah yang tidak bisa diselesaikan dengan greedy atau DP — perlu backtracking.

Lebih dekat lagi: **autocomplete puzzle solver**, **code generation**, **test case generation** — semua menggunakan backtracking. Memahami ini membuka pintu ke dunia constraint programming yang digunakan di industri skala besar.

## 3. Konsep Utama

### 3.1 Paradigma Backtracking

**Backtracking = DFS + Pruning**

```
Backtracking(state):
    if isComplete(state):
        addSolution(state)
        return
    
    for each choice in getChoices(state):
        if isValid(state, choice):        ← PRUNING: skip if invalid
            applyChoice(state, choice)
            Backtracking(newState)
            undoChoice(state, choice)     ← BACKTRACK: undo
```

**Tiga komponen kritis:**
1. **State representation:** Apa yang mewakili "di mana kita sekarang"?
2. **Constraint check:** Kapan sebuah state tidak valid (pruning)?
3. **Completion check:** Kapan solusi ditemukan?

### 3.2 Search Tree dan Pruning

Tanpa pruning: search tree berukuran **O(k^n)** di mana k = jumlah pilihan per level, n = kedalaman.

Dengan pruning yang baik: search tree menjadi jauh lebih kecil — inilah kunci efisiensi backtracking.

**Jenis pruning:**
- **Forward checking:** Cek apakah pilihan saat ini membuat pilihan masa depan tidak mungkin
- **Constraint propagation:** Propagasikan efek pilihan ke constraint lain
- **Arc consistency:** Pastikan semua pasangan variabel masih bisa dipenuhi

### 3.3 N-Queens Problem

**Problem:** Tempatkan N ratu di papan N×N sehingga tidak ada dua ratu yang saling menyerang (sama baris, kolom, atau diagonal).

**State:** Posisi kolom setiap ratu, satu per baris
**Pruning:** Ratu di baris baru tidak boleh berada di kolom atau diagonal yang sama dengan ratu sebelumnya

**Insight kritis:** Kita pasang satu ratu per baris → sudah handle row conflict. Cukup cek kolom dan diagonal.

### 3.4 Sudoku Solver

**State:** Grid 9×9 dengan nilai 0-9 (0 = kosong)
**Constraint:** 
- Setiap baris: angka 1-9 muncul tepat sekali
- Setiap kolom: angka 1-9 muncul tepat sekali
- Setiap kotak 3×3: angka 1-9 muncul tepat sekali

**Strategy:** Pilih sel yang paling sedikit kemungkinan (MRV heuristic — Minimum Remaining Values) untuk mengurangi branching factor.

## 4. Ilustrasi dan Analogi

### Analogi: Navigasi dengan GPS di Jalan Buntu

Backtracking seperti GPS yang mencari rute:
1. Ikuti jalan A
2. Temui jalan buntu → **backtrack** ke persimpangan sebelumnya
3. Coba jalan B
4. Berhasil → solusi ditemukan!

**Pruning** = GPS yang sudah tahu "jalan A menuju area terlarang" → langsung skip tanpa mencoba sama sekali.

Tanpa pruning: coba semua 100 jalan bahkan yang jelas tidak ke tujuan.
Dengan pruning efektif: hanya coba 10 jalan yang mungkin.

### Analogi N-Queens: Puzzle Catur

Bayangkan kamu taruh pion ratu di papan catur satu per satu, baris demi baris:
- Baris 1: coba kolom 1 → OK
- Baris 2: coba kolom 1 → diserang! Coba kolom 2 → diserang diagonal! Coba kolom 3 → OK
- Baris 3: ... jika semua kolom di baris ini diserang → **backtrack ke baris 2**, coba kolom berikutnya

## 5. Contoh Teknis

### 5.1 N-Queens Problem

```javascript
/**
 * N-Queens Problem
 * 
 * Temukan semua penempatan N ratu di papan N×N
 * yang tidak saling menyerang
 * 
 * @param {number} n - ukuran papan
 * @returns {Array} - semua solusi valid [[col0, col1, ..., colN-1]]
 */
function nQueens(n) {
    const solutions = [];
    const queens = Array(n).fill(-1); // queens[row] = column position
    
    // Set untuk O(1) lookup
    const usedCols = new Set();
    const usedDiag1 = new Set(); // row - col (kiri atas ke kanan bawah)
    const usedDiag2 = new Set(); // row + col (kanan atas ke kiri bawah)
    
    function backtrack(row) {
        // Base case: semua baris sudah terisi → solusi ditemukan!
        if (row === n) {
            solutions.push([...queens]);
            return;
        }
        
        // Coba setiap kolom di baris ini
        for (let col = 0; col < n; col++) {
            // PRUNING: cek apakah posisi ini valid
            if (usedCols.has(col) || 
                usedDiag1.has(row - col) || 
                usedDiag2.has(row + col)) {
                continue; // skip — tidak valid
            }
            
            // Apply choice
            queens[row] = col;
            usedCols.add(col);
            usedDiag1.add(row - col);
            usedDiag2.add(row + col);
            
            // Recurse
            backtrack(row + 1);
            
            // Undo choice (backtrack)
            queens[row] = -1;
            usedCols.delete(col);
            usedDiag1.delete(row - col);
            usedDiag2.delete(row + col);
        }
    }
    
    backtrack(0);
    return solutions;
}

// Visualisasi satu solusi
function visualizeQueens(solution) {
    const n = solution.length;
    console.log(`N=${n} Queens Solution:`);
    for (let row = 0; row < n; row++) {
        let line = '';
        for (let col = 0; col < n; col++) {
            line += solution[row] === col ? '♛ ' : '· ';
        }
        console.log(line);
    }
    console.log();
}

// Test
const solutions4 = nQueens(4);
console.log(`N=4: ${solutions4.length} solusi`);
visualizeQueens(solutions4[0]);

const solutions8 = nQueens(8);
console.log(`N=8: ${solutions8.length} solusi`);
visualizeQueens(solutions8[0]);

// Benchmark
console.time('N=12');
const s12 = nQueens(12);
console.timeEnd('N=12');
console.log(`N=12: ${s12.length} solusi`);

// Hitung solusi tanpa menyimpan semua (lebih hemat memori)
function countQueens(n) {
    let count = 0;
    const usedCols = new Set();
    const usedDiag1 = new Set();
    const usedDiag2 = new Set();
    
    function backtrack(row) {
        if (row === n) { count++; return; }
        for (let col = 0; col < n; col++) {
            if (usedCols.has(col) || usedDiag1.has(row-col) || usedDiag2.has(row+col)) continue;
            usedCols.add(col); usedDiag1.add(row-col); usedDiag2.add(row+col);
            backtrack(row + 1);
            usedCols.delete(col); usedDiag1.delete(row-col); usedDiag2.delete(row+col);
        }
    }
    
    backtrack(0);
    return count;
}

for (let n = 1; n <= 13; n++) {
    console.time(`N=${n}`);
    const count = countQueens(n);
    console.timeEnd(`N=${n}`);
    console.log(`N=${n}: ${count} solusi`);
}
// N=8: 92, N=12: 14200, N=13: 73712
```

### 5.2 Sudoku Solver dengan MRV Heuristic

```javascript
/**
 * Sudoku Solver dengan optimasi MRV (Minimum Remaining Values)
 * 
 * MRV = pilih sel yang memiliki PALING SEDIKIT nilai yang mungkin
 * → Kurangi branching factor → Lebih cepat menemukan kontradiksi
 */
class SudokuSolver {
    constructor(grid) {
        // Deep copy grid 9×9
        this.grid = grid.map(row => [...row]);
    }
    
    // Cek apakah angka valid di posisi (row, col)
    isValid(row, col, num) {
        // Cek baris
        if (this.grid[row].includes(num)) return false;
        
        // Cek kolom
        for (let r = 0; r < 9; r++) {
            if (this.grid[r][col] === num) return false;
        }
        
        // Cek kotak 3×3
        const boxRow = Math.floor(row / 3) * 3;
        const boxCol = Math.floor(col / 3) * 3;
        for (let r = boxRow; r < boxRow + 3; r++) {
            for (let c = boxCol; c < boxCol + 3; c++) {
                if (this.grid[r][c] === num) return false;
            }
        }
        
        return true;
    }
    
    // Dapatkan semua nilai yang mungkin untuk sel (row, col)
    getPossibleValues(row, col) {
        if (this.grid[row][col] !== 0) return []; // sudah terisi
        
        const possible = [];
        for (let num = 1; num <= 9; num++) {
            if (this.isValid(row, col, num)) {
                possible.push(num);
            }
        }
        return possible;
    }
    
    // MRV: Temukan sel kosong dengan paling sedikit kemungkinan
    findMRVCell() {
        let minOptions = 10;
        let bestCell = null;
        
        for (let row = 0; row < 9; row++) {
            for (let col = 0; col < 9; col++) {
                if (this.grid[row][col] === 0) {
                    const options = this.getPossibleValues(row, col);
                    
                    if (options.length === 0) return { row, col, options: [] }; // dead end
                    
                    if (options.length < minOptions) {
                        minOptions = options.length;
                        bestCell = { row, col, options };
                        
                        if (minOptions === 1) return bestCell; // optimal
                    }
                }
            }
        }
        
        return bestCell;
    }
    
    // Solve menggunakan backtracking + MRV
    solve() {
        const cell = this.findMRVCell();
        
        if (!cell) return true; // semua sel terisi → solved!
        
        const { row, col, options } = cell;
        
        if (options.length === 0) return false; // dead end → backtrack
        
        for (const num of options) {
            // Apply
            this.grid[row][col] = num;
            
            // Recurse
            if (this.solve()) return true;
            
            // Undo (backtrack)
            this.grid[row][col] = 0;
        }
        
        return false; // no valid number found
    }
    
    print() {
        for (let row = 0; row < 9; row++) {
            if (row % 3 === 0 && row !== 0) {
                console.log('------+-------+------');
            }
            let line = '';
            for (let col = 0; col < 9; col++) {
                if (col % 3 === 0 && col !== 0) line += '| ';
                line += (this.grid[row][col] || '.') + ' ';
            }
            console.log(line.trim());
        }
        console.log();
    }
}

// Test Sudoku (0 = kosong)
const puzzle = [
    [5, 3, 0,  0, 7, 0,  0, 0, 0],
    [6, 0, 0,  1, 9, 5,  0, 0, 0],
    [0, 9, 8,  0, 0, 0,  0, 6, 0],
    
    [8, 0, 0,  0, 6, 0,  0, 0, 3],
    [4, 0, 0,  8, 0, 3,  0, 0, 1],
    [7, 0, 0,  0, 2, 0,  0, 0, 6],
    
    [0, 6, 0,  0, 0, 0,  2, 8, 0],
    [0, 0, 0,  4, 1, 9,  0, 0, 5],
    [0, 0, 0,  0, 8, 0,  0, 7, 9],
];

const solver = new SudokuSolver(puzzle);

console.log('=== Sudoku Puzzle ===');
solver.print();

console.time('Solve');
const solved = solver.solve();
console.timeEnd('Solve');

if (solved) {
    console.log('=== Solution ===');
    solver.print();
} else {
    console.log('No solution!');
}

// Sudoku "Hardest" - AI Escargot (diklaim paling sulit)
const hardest = [
    [1, 0, 0,  0, 0, 7,  0, 9, 0],
    [0, 3, 0,  0, 2, 0,  0, 0, 8],
    [0, 0, 9,  6, 0, 0,  5, 0, 0],
    
    [0, 0, 5,  3, 0, 0,  9, 0, 0],
    [0, 1, 0,  0, 8, 0,  0, 0, 2],
    [6, 0, 0,  0, 0, 4,  0, 0, 0],
    
    [3, 0, 0,  0, 0, 0,  0, 1, 0],
    [0, 4, 0,  0, 0, 0,  0, 0, 7],
    [0, 0, 7,  0, 0, 0,  3, 0, 0],
];

const hardSolver = new SudokuSolver(hardest);
console.time('Solve hardest');
hardSolver.solve();
console.timeEnd('Solve hardest');
// Dengan MRV: biasanya <5ms bahkan untuk "hardest"!
```

### 5.3 Permutation & Combination Generator

```javascript
/**
 * Generate permutasi dan kombinasi menggunakan backtracking
 */

// Permutasi
function permutations(arr) {
    const result = [];
    const used = Array(arr.length).fill(false);
    const current = [];
    
    function backtrack() {
        if (current.length === arr.length) {
            result.push([...current]);
            return;
        }
        
        for (let i = 0; i < arr.length; i++) {
            if (used[i]) continue;
            
            // Pruning: skip duplicate untuk array dengan elemen sama
            if (i > 0 && arr[i] === arr[i-1] && !used[i-1]) continue;
            
            used[i] = true;
            current.push(arr[i]);
            backtrack();
            current.pop();
            used[i] = false;
        }
    }
    
    arr.sort(); // sort untuk handle duplikasi
    backtrack();
    return result;
}

// Kombinasi
function combinations(arr, k) {
    const result = [];
    const current = [];
    
    function backtrack(start) {
        if (current.length === k) {
            result.push([...current]);
            return;
        }
        
        // Pruning: tidak cukup elemen tersisa
        if (arr.length - start < k - current.length) return;
        
        for (let i = start; i < arr.length; i++) {
            current.push(arr[i]);
            backtrack(i + 1);
            current.pop();
        }
    }
    
    backtrack(0);
    return result;
}

console.log('Permutations of [1,2,3]:', permutations([1, 2, 3]).length); // 6
console.log('Permutations of [1,1,2]:', permutations([1, 1, 2]).length); // 3 (bukan 6, ada duplikat)

console.log('C(5,3):', combinations([1,2,3,4,5], 3).length); // 10

// Aplikasi: Generate semua test case kombinasi filter
function generateTestCases(filters) {
    const keys = Object.keys(filters);
    const values = keys.map(k => filters[k]);
    const testCases = [];
    
    function backtrack(idx, current) {
        if (idx === keys.length) {
            testCases.push({ ...current });
            return;
        }
        
        for (const val of values[idx]) {
            current[keys[idx]] = val;
            backtrack(idx + 1, current);
        }
    }
    
    backtrack(0, {});
    return testCases;
}

const searchFilters = {
    category: ['electronics', 'fashion', 'food'],
    priceRange: ['0-100k', '100k-500k', '500k+'],
    rating: [3, 4, 5],
};

const cases = generateTestCases(searchFilters);
console.log(`Total test cases: ${cases.length}`); // 27 (3×3×3)
console.log('First 3 cases:', cases.slice(0, 3));
```

### 5.4 Word Search (Boggle-style)

```javascript
/**
 * Word Search in Grid
 * 
 * Temukan apakah kata bisa ditemukan dalam grid 2D
 * dengan bergerak ke sel adjacent (8 arah)
 * 
 * Aplikasi: Pencarian kata di puzzle, game Boggle
 */
function wordSearch(board, word) {
    const rows = board.length;
    const cols = board[0].length;
    const visited = Array.from({length: rows}, () => Array(cols).fill(false));
    
    const directions = [
        [-1,-1], [-1,0], [-1,1],
        [0,-1],          [0,1],
        [1,-1],  [1,0],  [1,1]
    ];
    
    function backtrack(row, col, idx) {
        // Base case: semua karakter cocok!
        if (idx === word.length) return true;
        
        // Cek batas dan validitas
        if (row < 0 || row >= rows || col < 0 || col >= cols) return false;
        if (visited[row][col]) return false;
        if (board[row][col] !== word[idx]) return false; // PRUNING!
        
        // Apply
        visited[row][col] = true;
        
        // Recurse ke 8 arah
        for (const [dr, dc] of directions) {
            if (backtrack(row + dr, col + dc, idx + 1)) {
                visited[row][col] = false; // cleanup
                return true;
            }
        }
        
        // Undo
        visited[row][col] = false;
        return false;
    }
    
    // Coba mulai dari setiap sel
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (backtrack(r, c, 0)) return true;
        }
    }
    
    return false;
}

const board = [
    ['A', 'B', 'C', 'E'],
    ['S', 'F', 'C', 'S'],
    ['A', 'D', 'E', 'E']
];

console.log(wordSearch(board, 'ABCCED')); // true
console.log(wordSearch(board, 'SEE'));    // true
console.log(wordSearch(board, 'ABCB'));   // false (B sudah dipakai)
```

## 6. Studi Kasus Nyata: Gojek Driver Scheduling

**Konteks:** Gojek perlu menjadwalkan driver untuk shift harian dengan constraints:
- Setiap driver tidak boleh kerja lebih dari 12 jam/hari
- Setiap zona kota harus ada minimal 5 driver
- Driver dengan SIM tertentu tidak bisa ke zona tertentu
- Minimize jumlah driver yang dipanggil (cost optimization)

**Formulation sebagai CSP (Constraint Satisfaction Problem):**

```javascript
/**
 * Driver Scheduling sebagai CSP dengan Backtracking
 * 
 * Variables: Setiap driver (D1...Dn)
 * Domains: Shift yang bisa diambil per driver
 * Constraints: Jam kerja, zona coverage, SIM requirement
 */

const drivers = [
    { id: 'D1', name: 'Budi', maxHours: 10, zones: ['A', 'B', 'C'], sim: 'B' },
    { id: 'D2', name: 'Sari', maxHours: 12, zones: ['A', 'C'], sim: 'A' },
    { id: 'D3', name: 'Ahmad', maxHours: 8, zones: ['B', 'C'], sim: 'B' },
    { id: 'D4', name: 'Dewi', maxHours: 12, zones: ['A', 'B'], sim: 'A' },
    { id: 'D5', name: 'Rudi', maxHours: 10, zones: ['A', 'B', 'C'], sim: 'B' },
];

const shifts = [
    { id: 'S1', hours: 8, zone: 'A', simRequired: 'A', priority: 3 },
    { id: 'S2', hours: 6, zone: 'B', simRequired: 'B', priority: 2 },
    { id: 'S3', hours: 8, zone: 'C', simRequired: 'B', priority: 3 },
    { id: 'S4', hours: 10, zone: 'A', simRequired: 'A', priority: 1 },
    { id: 'S5', hours: 4, zone: 'B', simRequired: 'B', priority: 2 },
];

function scheduleDrivers(drivers, shifts) {
    const assignment = {}; // shiftId → driverId
    const driverHours = {}; // driverId → total hours assigned
    drivers.forEach(d => { driverHours[d.id] = 0; });
    
    // Sort shifts by priority (descending) — high priority shifts assigned first
    const sortedShifts = [...shifts].sort((a, b) => b.priority - a.priority);
    
    function canAssign(driver, shift) {
        // Cek SIM
        if (shift.simRequired && driver.sim !== shift.simRequired) return false;
        // Cek zona
        if (!driver.zones.includes(shift.zone)) return false;
        // Cek jam maksimum
        if (driverHours[driver.id] + shift.hours > driver.maxHours) return false;
        return true;
    }
    
    function backtrack(shiftIdx) {
        if (shiftIdx === sortedShifts.length) return true; // semua shift terassign
        
        const shift = sortedShifts[shiftIdx];
        
        // Coba assign ke setiap driver
        for (const driver of drivers) {
            if (canAssign(driver, shift)) {
                // Apply
                assignment[shift.id] = driver.id;
                driverHours[driver.id] += shift.hours;
                
                // Recurse
                if (backtrack(shiftIdx + 1)) return true;
                
                // Undo
                delete assignment[shift.id];
                driverHours[driver.id] -= shift.hours;
            }
        }
        
        // Tidak ada driver yang bisa → shift ini skip (optional shift)
        assignment[shift.id] = 'UNASSIGNED';
        if (backtrack(shiftIdx + 1)) return true;
        delete assignment[shift.id];
        
        return false;
    }
    
    backtrack(0);
    return assignment;
}

const schedule = scheduleDrivers(drivers, shifts);
console.log('\n=== Gojek Driver Schedule ===');
for (const [shiftId, driverId] of Object.entries(schedule)) {
    const shift = shifts.find(s => s.id === shiftId);
    const driver = drivers.find(d => d.id === driverId);
    const driverName = driver ? driver.name : 'UNASSIGNED';
    console.log(`Shift ${shiftId} (Zone ${shift.zone}, ${shift.hours}h): → ${driverName}`);
}
```

## 7. Visualisasi

### N-Queens Search Tree (N=4)

```
Row 0: coba col 0, 1, 2, 3

Col 0         Col 1         Col 2         Col 3
♛ · · ·      · ♛ · ·      · · ♛ ·      · · · ♛
Row 1: coba semua kolom...

Col 0         Col 1         Col 2         Col 3
♛ · · ·      · ♛ · ·      → Col 0: diag! → Col 0: diag!
· ♛ · ·  ←  → Col 0: col! → Col 1: col!  → Col 1: col!
              → Col 1: diag → Col 2: col!  → Col 2: diag!
              · · ♛ ·       → Col 3: diag  · ♛ · ·
              Row 2...       PRUNE!         Row 2...

Jalur sukses N=4: [1,3,0,2] dan [2,0,3,1]
(0-indexed: baris 0→col 1, baris 1→col 3, dst)
```

### Sudoku MRV Selection

```
Grid awal (sebagian):
5 3 .  . 7 .  . . .
6 . .  1 9 5  . . .
. 9 8  . . .  . 6 .

Sel dengan paling sedikit kemungkinan:
- (0,2): hanya bisa {4} → pilih ini dulu! (1 pilihan)
- (1,1): hanya bisa {7} → 1 pilihan
- (1,2): bisa {2,7} → 2 pilihan
- (2,0): bisa {1,2,3} → 3 pilihan

MRV selalu pilih sel dengan pilihan terkecil → minimize backtracking!
```

### Pruning Impact

```
N-Queens N=8 tanpa pruning:
  8^8 = 16,777,216 states explored

N-Queens N=8 dengan column pruning:
  8! = 40,320 states (hanya permutasi kolom)

N-Queens N=8 dengan column + diagonal pruning:
  ~15,720 states explored (jauh lebih sedikit!)
```

## 8. Kesalahan Umum

### ❌ Kesalahan 1: Lupa Undo State (Backtrack yang Tidak Benar)

```javascript
// SALAH — tidak undo saat backtrack
function nQueensWrong(n, row = 0, queens = []) {
    if (row === n) { console.log(queens); return; }
    
    for (let col = 0; col < n; col++) {
        if (isValid(queens, row, col)) {
            queens.push(col); // apply
            nQueensWrong(n, row + 1, queens);
            // LUPA queens.pop()! State tercemar untuk iterasi berikutnya
        }
    }
}

// BENAR
function nQueensCorrect(n, row = 0, queens = []) {
    if (row === n) { console.log([...queens]); return; } // snapshot!
    
    for (let col = 0; col < n; col++) {
        if (isValid(queens, row, col)) {
            queens.push(col);
            nQueensCorrect(n, row + 1, queens);
            queens.pop(); // ← WAJIB undo
        }
    }
}
```

### ❌ Kesalahan 2: Tidak Membuat Copy Saat Menyimpan Solusi

```javascript
// SALAH — semua "solusi" menunjuk objek yang sama (akan berubah)!
const solutions = [];

function backtrack(current) {
    if (isDone(current)) {
        solutions.push(current); // SALAH — menyimpan referensi, bukan copy!
        return;
    }
    // ...
}

// BENAR — selalu buat salinan saat menyimpan
function backtrackFixed(current) {
    if (isDone(current)) {
        solutions.push([...current]); // shallow copy untuk array primitif
        // atau: solutions.push(JSON.parse(JSON.stringify(current))); // deep copy
        return;
    }
    // ...
}
```

### ❌ Kesalahan 3: Pruning yang Terlalu Agresif (False Pruning)

```javascript
// SALAH — pruning yang memotong solusi valid!
function backtrackWrongPruning(state) {
    // Pruning terlalu dini:
    if (state.current.length === state.max / 2) return; // SALAH! memotong valid paths
    
    // ...
}

// BENAR — hanya prune jika benar-benar tidak mungkin mencapai solusi
function backtrackCorrectPruning(state) {
    // Prune hanya jika current constraint sudah dilanggar
    if (!isConstraintSatisfied(state)) return; // safe pruning
    
    // Atau: prune jika tidak mungkin mencapai target minimum
    if (state.current.length + state.remaining < state.required) return; // safe
}
```

### ❌ Kesalahan 4: Stack Overflow untuk Input Besar

```javascript
// MASALAH: rekursi dalam untuk n besar → stack overflow
function nQueens(n) {
    // Untuk n=20+, call stack bisa overflow di JavaScript default (~10K frames)
}

// SOLUSI 1: Gunakan iterative backtracking dengan explicit stack
function nQueensIterative(n) {
    const stack = [{ row: 0, queens: [], usedCols: new Set(), diag1: new Set(), diag2: new Set() }];
    const solutions = [];
    
    while (stack.length > 0) {
        const { row, queens, usedCols, diag1, diag2, col = 0 } = stack.pop();
        
        if (row === n) {
            solutions.push([...queens]);
            continue;
        }
        
        for (let c = 0; c < n; c++) {
            if (!usedCols.has(c) && !diag1.has(row-c) && !diag2.has(row+c)) {
                stack.push({
                    row: row + 1,
                    queens: [...queens, c],
                    usedCols: new Set([...usedCols, c]),
                    diag1: new Set([...diag1, row-c]),
                    diag2: new Set([...diag2, row+c])
                });
            }
        }
    }
    
    return solutions;
}

// SOLUSI 2: Untuk n ekstrem, gunakan bit manipulation
// (teknik advanced — mencapai n=20+ dalam hitungan detik)
```

## 9. Latihan dan Studi Kasus

### Latihan 1 — Subset Sum dengan Semua Subset

```javascript
/**
 * Temukan SEMUA subset yang jumlahnya = target
 * (bukan hanya apakah ada — semua subset)
 */
function findAllSubsets(arr, target) {
    const results = [];
    
    function backtrack(start, current, remaining) {
        if (remaining === 0) {
            results.push([...current]);
            return;
        }
        
        for (let i = start; i < arr.length; i++) {
            // Pruning: skip jika elemen terlalu besar
            if (arr[i] > remaining) break; // arr harus sudah sorted!
            
            // Pruning: skip duplikat di level yang sama
            if (i > start && arr[i] === arr[i-1]) continue;
            
            current.push(arr[i]);
            backtrack(i + 1, current, remaining - arr[i]);
            current.pop();
        }
    }
    
    arr.sort((a, b) => a - b); // sort untuk enable pruning
    backtrack(0, [], target);
    return results;
}

const arr = [10, 1, 2, 7, 6, 1, 5];
const target = 8;
console.log(`Semua subset yang sum=${target}:`, findAllSubsets(arr, target));
// [[1,1,6], [1,2,5], [1,7], [2,6]]
```

### Latihan 2 — Letter Combinations (Phone Keypad)

```javascript
/**
 * Diberikan digit (2-9), return semua kombinasi huruf
 * seperti keyboard phone lama
 * 
 * Aplikasi: autocomplete keyboard, word prediction
 */
function letterCombinations(digits) {
    const phoneMap = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    };
    
    if (!digits) return [];
    
    const results = [];
    
    function backtrack(idx, current) {
        if (idx === digits.length) {
            results.push(current);
            return;
        }
        
        for (const letter of phoneMap[digits[idx]]) {
            backtrack(idx + 1, current + letter);
        }
    }
    
    backtrack(0, '');
    return results;
}

console.log(letterCombinations('23')); // ["ad","ae","af","bd","be","bf","cd","ce","cf"]
console.log(letterCombinations('2').length); // 3
console.log(letterCombinations('234').length); // 27 (3×3×3)
```

### Latihan 3 — Challenge: Knight's Tour

```javascript
/**
 * Knight's Tour
 * 
 * Gerakan kuda catur untuk mengunjungi setiap sel tepat sekali
 * Menggunakan Warnsdorff's heuristic (greedy + backtracking)
 */
function knightsTour(n) {
    const board = Array.from({length: n}, () => Array(n).fill(-1));
    const moves = [
        [-2,-1], [-2,1], [-1,-2], [-1,2],
        [1,-2],  [1,2],  [2,-1],  [2,1]
    ];
    
    function isValid(r, c) {
        return r >= 0 && r < n && c >= 0 && c < n && board[r][c] === -1;
    }
    
    // Warnsdorff: hitung jumlah gerakan valid dari sel berikutnya
    function getDegree(r, c) {
        return moves.filter(([dr, dc]) => isValid(r + dr, c + dc)).length;
    }
    
    function backtrack(row, col, moveCount) {
        board[row][col] = moveCount;
        
        if (moveCount === n * n - 1) return true; // selesai!
        
        // Warnsdorff: urutkan gerakan berdasarkan degree (ascending)
        const nextMoves = moves
            .map(([dr, dc]) => [row + dr, col + dc])
            .filter(([r, c]) => isValid(r, c))
            .map(([r, c]) => ({ r, c, degree: getDegree(r, c) }))
            .sort((a, b) => a.degree - b.degree);
        
        for (const { r, c } of nextMoves) {
            if (backtrack(r, c, moveCount + 1)) return true;
        }
        
        board[row][col] = -1; // undo
        return false;
    }
    
    if (backtrack(0, 0, 0)) {
        return board;
    }
    return null;
}

console.time('Knight Tour N=8');
const tour = knightsTour(8);
console.timeEnd('Knight Tour N=8');

if (tour) {
    console.log('Knight\'s Tour solved! (first 3 rows):');
    tour.slice(0, 3).forEach(row => 
        console.log(row.map(n => String(n).padStart(2)).join(' '))
    );
}
```

## 10. Ringkasan

| Problem | Pendekatan | Key Insight | Complexity |
|---------|-----------|-------------|-----------|
| N-Queens | Backtracking | One queen per row, check col+diag | O(n!) tanpa pruning |
| Sudoku | Backtracking + MRV | Pilih sel paling constrainted | Empirically fast |
| Permutations | Backtracking | Tandai elemen yang dipakai | O(n!) |
| Combinations | Backtracking | Start index untuk hindari duplikat | O(C(n,k)) |
| Subset Sum | Backtracking | Sort untuk enable pruning | O(2^n) worst |

**Kapan pilih Backtracking:**
- Masalah memiliki banyak constraints yang harus dipenuhi bersamaan
- Perlu semua solusi (exhaustive search)
- Search space eksplisit (finite, enumerable)
- Greedy tidak optimal, DP state space terlalu besar

**Teknik optimasi:**
1. **MRV (Minimum Remaining Values)** — pilih variabel paling constrainted
2. **LCV (Least Constraining Value)** — pilih nilai yang membatasi paling sedikit
3. **Forward Checking** — propagasikan constraint ke depan
4. **Arc Consistency (AC-3)** — periksa konsistensi semua arc

## 11. Referensi

- Cormen, T.H. et al. — *Introduction to Algorithms (CLRS)*, 4th Ed., Appendix: Backtracking
- Russell, S. & Norvig, P. — *Artificial Intelligence: A Modern Approach*, Bab 6 (Constraint Satisfaction)
- Skiena, S. — *The Algorithm Design Manual*, 3rd Ed., Bab 9.1 (Backtracking)
- Bhargava, A. — *Grokking Algorithms*, Bab 8 (Greedy) — kontras dengan backtracking
- LeetCode — #51 (N-Queens), #37 (Sudoku Solver), #46 (Permutations), #39 (Combination Sum)
- Wikipedia — [Warnsdorff's Rule](https://en.wikipedia.org/wiki/Knight%27s_tour#Warnsdorff's_rule)
- Visualgo.net — Backtracking visualization
- GeeksForGeeks — N-Queens, Sudoku Solver implementations
