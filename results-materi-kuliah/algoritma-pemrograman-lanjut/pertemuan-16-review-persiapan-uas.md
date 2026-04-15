# Pertemuan 16: Review & Persiapan UAS

## 1. Learning Outcomes & Course Recap

Mahasiswa yang telah menyelesaikan kuliah ini mampu:

### Algorithms (P1-P9)
- ✅ Analisis kompleksitas (Big-O, Master Theorem)
- ✅ Divide & Conquer (Closest Pair, Strassen)
- ✅ Dynamic Programming (Memoization & Tabulation)
- ✅ Greedy Algorithms (Activity Selection, Huffman)
- ✅ Backtracking (N-Queens, Sudoku)
- ✅ Shortest Path (Dijkstra, Bellman-Ford, Floyd-Warshall)
- ✅ Minimum Spanning Tree (Kruskal, Prim)

### Design Patterns (P10-P12)
- ✅ Creational (Singleton, Factory, Abstract Factory, Builder, Prototype)
- ✅ Structural (Adapter, Decorator, Facade, Proxy, Composite)
- ✅ Behavioral (Observer, Strategy, Command, Iterator, Template Method)

### Software Engineering (P13-P15)
- ✅ Clean Code principles
- ✅ SOLID principles
- ✅ Refactoring techniques
- ✅ Full-stack application development

## 2. Concept Map — Algoritma Pemrograman Lanjut

```
ALGORITHMS
├── Analysis
│   ├── Big-O, Big-Θ, Big-Ω
│   └── Master Theorem
├── Paradigm
│   ├── Divide & Conquer (D&C)
│   │   ├── Merge Sort: O(n log n)
│   │   ├── Closest Pair: O(n log n)
│   │   └── Strassen: O(n^2.81)
│   ├── Dynamic Programming (DP)
│   │   ├── Top-down (Memoization): Fibonacci, Knapsack, LCS
│   │   └── Bottom-up (Tabulation): LIS, Edit Distance
│   ├── Greedy
│   │   ├── Activity Selection
│   │   └── Huffman Coding
│   └── Backtracking
│       ├── N-Queens
│       └── Sudoku Solver
└── Graph
    ├── Shortest Path (Dijkstra, Bellman-Ford)
    └── Minimum Spanning Tree (Kruskal, Prim)

DESIGN PATTERNS
├── Creational (Object creation)
│   ├── Singleton
│   ├── Factory Method & Abstract Factory
│   ├── Builder
│   └── Prototype
├── Structural (Object composition)
│   ├── Adapter, Decorator, Facade
│   ├── Proxy, Composite
│   └── Principle: Composition > Inheritance
└── Behavioral (Object interaction)
    ├── Observer, Strategy, Command
    ├── Iterator, Template Method
    └── Principle: Encapsulate variation

CLEAN CODE & SOLID
├── Naming, Functions, Comments
├── Single Responsibility
├── Open/Closed Principle
├── Liskov Substitution
├── Interface Segregation
├── Dependency Inversion
└── Refactoring techniques
```

## 3. Quick Reference Tables

### Complexity Cheat Sheet

| Task | Algorithm | Complexity | Example |
|------|-----------|-----------|---------|
| Sort | Merge/Quick | O(n log n) | Most general sorting |
| Search | Binary | O(log n) | Sorted array |
| Find closest pair | D&C | O(n log n) | Geolocation |
| Fibonacci | DP | O(n) | Sequence |
| Edit distance | DP | O(mn) | String matching |
| Activity selection | Greedy | O(n log n) | Scheduling |
| MST | Kruskal | O(E log E) | Network design |
| Shortest path | Dijkstra | O((V+E) log V) | Routing |

### Pattern Selection Matrix

| Problem | Pattern | Reason |
|---------|---------|--------|
| Banyak algoritma interchangeable | Strategy | Easy swap at runtime |
| Object creation kompleks | Builder/Factory | Decouple creation |
| Notify many listeners | Observer | Pub-sub architecture |
| Subsystem kompleks | Facade | Simplified interface |
| Multiple related objects | Abstract Factory | Consistent families |
| Add behavior dinamis | Decorator | Flexible composition |

## 4. UAS Soal Tipe & Preparation

### Tipe 1: Kompleksitas & Master Theorem (15%)

```
Soal: Berapa kompleksitas T(n) = 3T(n/2) + O(n²)?
Jawab:
- a=3, b=2, log_2(3) ≈ 1.585
- f(n) = O(n²) = O(n^(1.585 + ε)) → Kasus 3
- T(n) = Θ(n²)
```

**Persiapan:**
- Hafal definisi Big-O, Big-Θ, Big-Ω
- Master Theorem 3 kasus
- Apply ke 5+ contoh

### Tipe 2: Implementasi Algoritma (25%)

Buat code untuk:
- Fibonacci (DP memoization vs iteratif)
- LCS / Edit Distance (DP tabulation)
- Dijkstra / Kruskal (dari scratch)
- N-Queens / Sudoku (Backtracking)

**Persiapan:**
- Code harus runnable (no syntax error)
- Handle edge cases
- Trace manual untuk verify

### Tipe 3: Design Pattern (20%)

Identifikasi pattern dan implementasikan:
- Factory untuk multi-implementation
- Decorator untuk optional behavior
- Observer untuk event-driven
- Strategy untuk algorithm selection

**Persiapan:**
- Pahami when/why setiap pattern
- Bisa code dari scratch
- Bisa refactor bad code ke pattern

### Tipe 4: Clean Code & SOLID (15%)

Refactor kode buruk:
- Identifikasi code smells
- Apply SOLID principles
- Extract classes/methods
- Improve testability

**Persiapan:**
- Hafal code smells
- Pahami 5 SOLID principles
- Praktik refactoring 3+ kode buruk

### Tipe 5: Analysis & Design (25%)

Mini project:
- Design architecture untuk problem
- Pilih algorithms dan patterns yang tepat
- Explain trade-offs
- Code & test

**Persiapan:**
- Solve 3+ medium problems
- Code, test, review sendiri
- Write simple documentation

## 5. Study Tips

### Do's ✅
- [ ] Solve problems by hand first (trace manual)
- [ ] Code dari scratch tanpa copy-paste
- [ ] Pahami WHY, bukan hanya WHAT
- [ ] Test code dengan berbagai input
- [ ] Review code orang lain (GitHub)
- [ ] Time yourself (simulasi UAS)
- [ ] Tulis dokumentasi sambil coding

### Don'ts ❌
- [ ] Hafal kode tanpa paham
- [ ] Copy-paste dari tutorial
- [ ] Skip testing
- [ ] Only understand "happy path"
- [ ] Stay up all night before exam
- [ ] Panic jika lupa satu pattern

## 6. Mock Exam (90 minutes)

### Section A: Teori (30 points, 30 min)

1. (10) Jelaskan Optimal Substructure dan Overlapping Subproblems dengan 2 contoh.
2. (10) Master Theorem — selesaikan 3 rekurensi berbeda.
3. (10) Pilih pattern yang tepat untuk 3 skenario, jelaskan alasan.

### Section B: Implementation (40 points, 45 min)

1. (20) Implement LCS dengan DP tabulation, analisis kompleksitas.
2. (20) Refactor ugly code ke 2 design patterns, jelaskan perubahan.

### Section C: Analysis (30 points, 15 min)

Diberikan masalah: "Sistem delivery harus menemukan rute terpendek untuk 10 lokasi delivery dengan kemungkinan traffic berbeda-beda. Desain solusi."

- Algoritma mana? Kenapa?
- Kompleksitas?
- Pattern mana? Kenapa?
- Trade-offs?

## 7. Last-Minute Checklist (1 hari sebelum UAS)

### Pahami (jangan hafal):
- [ ] Big-O analysis konsep
- [ ] Kapan pakai D&C vs DP vs Greedy
- [ ] Trade-off setiap algoritma
- [ ] Problem → pattern mapping
- [ ] SOLID principles filosofi

### Bisa coding:
- [ ] Quick sort pivot selection
- [ ] Binary search
- [ ] DP memoization (add, remove, query)
- [ ] Simple backtracking (N-Queens basic)
- [ ] Dijkstra basic (10 node)
- [ ] Kruskal + Union-Find basic
- [ ] 1-2 design patterns dari scratch

### Tahu reference:
- [ ] Complexity tabel
- [ ] Pattern selection matrix
- [ ] SOLID principles list
- [ ] Master Theorem 3 kasus

## 8. Exam Strategy

1. **Baca semua soal** (5 min) — pahami scope
2. **Mulai dari soal yang paling confident** — build momentum
3. **Trace code manual** sebelum code — pastikan logika benar
4. **Test edge cases** — off-by-one, empty, null
5. **Explain your thinking** — comment kode, write Why not just What
6. **Double-check time** — jangan kehabisan waktu di soal terakhir

## 9. If You Get Stuck

| Situation | Action |
|-----------|--------|
| Lupa kompleksitas master theorem | Ingat pola: eksponensial > polinomial > logaritmik |
| Tidak tahu pattern mana | Tanyakan: what problem is this trying to solve? |
| Code error | Trace dengan input kecil, check base case dulu |
| Time running out | Submit apa yang ada, explain di comment |
| Blank mind | Coba dengan contoh konkret, jangan abstrak |

## 10. Post-Exam Reflection

Setelah UAS, write reflection:
- Soal apa yang mudah? Sulit?
- Apa yang kurang dipahami?
- Gimana strategy yang lebih baik?
- Apa ambil pelajaran ke project berikutnya?

## 11. Final Words

> "The best programmer is one who understands fundamentals deeply and can apply them creatively to new problems." — Dijkstra

Algoritma dan Design Patterns bukan untuk menghafal, tapi untuk **membangun intuisi** dalam mendesain solusi. Setiap code decision adalah trade-off: readability vs performance, simplicity vs flexibility, generality vs specificity.

Di industri, kamu akan bertemu 100+ problems yang belum pernah dihadapi. Tapi mereka adalah variasi dari 10-20 fundamental patterns yang sudah kamu pelajari.

**Maka: pahami fundamental, latihan banyak, dan jangan takut untuk eksperimen.**

Sukses untuk UAS! 🚀

---

### Pre-Exam Resources

- **GitHub:** [awesome-algorithms](https://github.com/tayllan/awesome-algorithms)
- **Visualization:** [visualgo.net](https://visualgo.net)
- **Problem Practice:** [LeetCode](https://leetcode.com) (Medium-Hard level)
- **Patterns:** [refactoring.guru](https://refactoring.guru)
- **Books:** CLRS, GoF Design Patterns, Clean Code

**Good luck! 💪**
