# Pertemuan 16: Review & Persiapan UAS — UI/UX Design

---

## 🎯 Learning Outcomes

Setelah pertemuan review ini, kamu akan bisa:

* Merangkum semua materi pasca-UTS (Pertemuan 8–14) dalam satu referensi cepat
* Menjawab soal UAS bertipe teori, analisis desain, dan studi kasus integratif
* Mengintegrasikan seluruh proses UX dari wireframe hingga iterasi dalam satu framework
* Menghindari kesalahan konseptual yang paling sering muncul di ujian

---

## 📖 Pengantar (Hook)

Akhir perjalanan. Kamu sudah melewati seluruh Double Diamond — dari riset pengguna, membuat persona dan journey map, merancang IA, wireframing, membangun design system, prototyping, hingga usability testing dan iterasi.

UAS adalah kesempatan untuk membuktikan bahwa kamu tidak hanya hafal langkah-langkahnya — tapi memahami **mengapa** setiap langkah ada dan **bagaimana** satu langkah menginformasikan langkah berikutnya.

---

## 🧩 Ringkasan Materi Pertemuan 8–14

### Pertemuan 8: User Flow & Wireframing

* **User Flow** = alur lengkap termasuk decision point dan error state (vs Task Flow yang linear)
* **Lo-Fi** = sketsa tangan, cepat, untuk eksplorasi ide
* **Mid-Fi** = digital grayscale di Figma, placeholder, untuk feedback dan alignment
* **Crazy 8s** = 8 sketsa berbeda dalam 8 menit — mengalahkan design block
* Prinsip: biaya perbaikan semakin mahal makin terlambat ditemukan → explore di Lo-Fi!

### Pertemuan 9: Figma Intermediate

* **Main Component** = master; perubahan menyebar ke semua instance
* **Variants** = kumpulan state/versi dalam satu Component Set
* **Auto Layout** = frame yang otomatis resize sesuai konten
* **Smart Animate** = animasikan layer dengan nama sama antara dua frame
* **Dev Mode** = specs, colors, dan code snippets untuk developer handoff
* Naming convention: `Button/Primary/Default` bukan `Button1`

### Pertemuan 10: Typography & Color System

* **Type Scale** = minimum 5 level: Display → H1/H2 → Body L/M → Caption
* Line height body: 1.5x; Heading: 1.2x
* **Color System:** Primary + Neutral + Semantic (success, warning, error, info)
* **60-30-10:** 60% neutral, 30% primary, 10% accent
* **WCAG AA:** normal text minimal 4.5:1 contrast ratio
* Dark mode: buat token terpisah, jangan sekedar invert

### Pertemuan 11: Design System

* **Atomic Design:** Atom → Molecule → Organism → Template → Page
* **Design Tokens** = named variables untuk nilai desain (single source of truth)
* Komponen wajib: Button (semua state + variant), Input Field (semua state), Card, Navigation
* Dokumentasi sama pentingnya dengan komponen — DO + DON'T per komponen
* Mulai kecil (10-15 komponen), grow sesuai kebutuhan

### Pertemuan 12: Hi-Fi Prototype

* Konten realistis = perbedaan antara prototipe profesional dan amatir
* Micro-interaction: button feedback, form validation, loading state, toggle
* Transisi: Push untuk navigasi, Dissolve untuk tab, Spring untuk modal
* Test sendiri sebelum UT — jangan ada yang "stuck"
* Konten nyata: nama, angka, tanggal yang masuk akal

### Pertemuan 13: Usability Testing

* **5 partisipan** menemukan ~85% masalah usability
* **Task yang baik:** skenario nyata, tidak menyebut nama fitur, ada definisi sukses
* **Think-Aloud:** partisipan bersuara keras saat menggunakan produk
* **Moderator:** JANGAN bantu, JANGAN lead, observasi saja
* **Rainbow Sheet:** catat temuan + tandai per partisipan
* Rekam sesi (dengan izin)

### Pertemuan 14: Analisis & Iterasi

* Data kualitatif → Affinity Diagram → cluster → insight
* **SUS Score:** kuesioner 10 pertanyaan, skala 0-100
  * >80 = Excellent A, 68-80 = Good B, 51-68 = OK C, <51 = Poor
  * Hitung: (nilai ganjil - 1) + (5 - nilai genap) × 2.5
* **Impact-Effort Matrix:** Quick Wins dulu, rencanakan Big Bets
* Setiap iterasi wajib dokumentasi: temuan → root cause → before/after → expected impact
* Validasi iterasi dengan UT ulang

---

## 🧠 Framework Terintegrasi: Full Double Diamond

```
DISCOVER          DEFINE           DEVELOP          DELIVER
┌────────────┐   ┌────────────┐   ┌────────────┐   ┌────────────┐
│ User       │   │ Affinity   │   │ Crazy 8s   │   │ Usability  │
│ Research   │──▶│ Diagram    │──▶│ → Lo-Fi    │──▶│ Testing    │
│            │   │ Persona    │   │ → Mid-Fi   │   │            │
│ Wawancara  │   │ Journey Map│   │ Design Sys │   │ Rainbow    │
│ Survei     │   │ HMW Stmt   │   │ → Hi-Fi    │   │ Sheet      │
│ Observasi  │   │ IA/Sitemap │   │ Prototype  │   │ SUS Score  │
└────────────┘   └────────────┘   └────────────┘   └────────────┘
     ↑_______________________________________________|
                     Iterasi (loop kembali)
```

---

## 📊 Tabel Komprehensif: Pilihan Tools Per Fase

| Fase | Aktivitas | Tools |
|---|---|---|
| Discover | User Interview | Zoom, Loom, Google Meet |
| Discover | Survei | Google Forms, Typeform |
| Define | Affinity Diagram | FigJam, Miro |
| Define | Persona | Figma, FigJam |
| Define | Journey Map | FigJam, Miro |
| Define | IA / Card Sort | OptimalSort, FigJam |
| Develop | User Flow | Figma, draw.io |
| Develop | Wireframe | Figma, kertas |
| Develop | Design System | Figma |
| Develop | Hi-Fi Prototype | Figma |
| Deliver | Usability Testing | Figma (share link), Maze |
| Deliver | Remote UT | Zoom + screen share |
| Deliver | SUS Score | Google Forms, spreadsheet |
| Deliver | Impact-Effort | FigJam, Miro |

---

## 💻 Quick Reference: Rumus & Angka Penting

```
SUS SCORE:
- Hitung: (ganjil - 1) + (5 - genap) → sum → × 2.5
- Grade A (Excellent): > 80.3
- Grade B (Good): 68–80.3
- Grade C (OK): 51–68
- Grade D/F (Poor): < 51

CONTRAST RATIO WCAG AA:
- Normal text (< 18px): minimal 4.5:1
- Large text (≥ 18px): minimal 3:1
- UI components: minimal 3:1
- Tools: webaim.org/resources/contrastchecker

TYPOGRAPHY:
- Body: min 16px mobile, line height 1.5x
- Caption: min 11px
- Touch target: min 44×44px

IA:
- Flat IA: ≤ 3 klik ke konten manapun
- Tab bar: max 5 items
- Magic number 5: 5 partisipan UT ≈ 85% masalah ditemukan

60-30-10 COLOR:
- 60% Neutral (background, card)
- 30% Primary (header, section)
- 10% Accent (CTA, highlight)
```

---

## 🧪 Soal Latihan UAS

### Bagian A: Teori (25 poin)

**1.** Jelaskan perbedaan antara Lo-Fi, Mid-Fi, dan Hi-Fi wireframe/prototype. Berikan satu contoh kapan masing-masing paling tepat digunakan.

**2.** Apa itu Design Tokens? Mengapa lebih baik dari hanya menggunakan Color Styles di Figma?

**3.** Jelaskan cara menghitung SUS Score. Jika seorang partisipan menjawab: Q1=4, Q2=3, Q3=4, Q4=1, Q5=3, Q6=2, Q7=4, Q8=1, Q9=3, Q10=2 — berapakah SUS Score-nya?

**4.** Jelaskan Atomic Design methodology dengan contoh dari aplikasi e-commerce.

**5.** Mengapa moderator UT tidak boleh membantu partisipan yang kesulitan? Apa yang terjadi jika moderator membantu?

### Bagian B: Analisis (35 poin)

**6.** (15 poin) Kamu melakukan UT dengan 5 partisipan untuk aplikasi reservasi restoran. Rainbow Sheet menunjukkan:
* 4/5 tidak bisa menemukan tombol "Filter" di halaman daftar restoran
* 3/5 mengira "Pesanan Saya" dan "Riwayat" adalah fitur yang berbeda
* 5/5 kesulitan membaca teks menu karena font terlalu kecil (11px)
* 2/5 tidak tahu artinya ikon bintang di samping nama restoran

Untuk setiap temuan:
a) Identifikasi heuristik Nielsen yang dilanggar
b) Tentukan severity rating (0-4)
c) Petakan ke Impact-Effort Matrix
d) Rekomendasikan satu iterasi

**7.** (20 poin) Kamu diminta redesign halaman home aplikasi dompet digital. Dari riset, diketahui bahwa 3 fitur yang paling sering digunakan adalah: Scan QR, Transfer, dan Cek Saldo.

a) Deskripsikan Typography System yang akan kamu gunakan (minimal 4 level) dan justifikasi pilihan font
b) Deskripsikan Color System yang akan kamu gunakan (minimal 8 warna dengan nama dan hex)
c) Jelaskan bagaimana 60-30-10 rule diterapkan di halaman home ini
d) Komponen apa saja yang perlu masuk ke Design System untuk halaman ini?

### Bagian C: Desain & Justifikasi (40 poin)

**8.** (20 poin) Kamu adalah UX Designer untuk startup fintech yang membuat aplikasi tabungan untuk anak muda (17-25 tahun, Gen Z). Riset awal menunjukkan target pengguna:
* Lebih menyukai visual daripada teks panjang
* Tidak mau baca terms & conditions
* Punya attention span pendek
* Sangat familiar dengan social media UX (TikTok, Instagram)

Jelaskan secara detail:
a) User Flow untuk onboarding pengguna baru (dari download app hingga berhasil buat rekening)
b) Keputusan IA: navigation pattern apa yang dipilih dan mengapa?
c) Micro-interaction apa yang penting ada di flow ini?
d) Task UT apa yang akan kamu buat untuk test flow ini?

**9.** (20 poin) Analisis prototipe berikut:

Sebuah aplikasi pembayaran dirancang dengan:
* 7 warna berbeda di satu halaman
* Tombol "Konfirmasi" dan "Batal" ukurannya sama, berdampingan
* Error message: "Invalid token refresh. Please re-authenticate."
* Tombol Back tidak tersedia di halaman konfirmasi
* Semua teks berwarna abu-abu #9E9E9E di atas background putih

Untuk setiap masalah:
a) Identifikasi heuristik yang dilanggar
b) Berikan severity rating
c) Rekomendasikan perbaikan spesifik
d) Hitung contrast ratio abu-abu #9E9E9E di atas putih #FFFFFF (gunakan rumus: L1 = 0.2126R + 0.7152G + 0.0722B)

---

## 📌 Checklist Pre-UAS

- [ ] Bisa jelaskan perbedaan Lo-Fi, Mid-Fi, Hi-Fi dengan contoh
- [ ] Paham Atomic Design dan bisa identifikasi atom/molekul/organisme
- [ ] Bisa hitung SUS Score dari raw data
- [ ] Paham WCAG contrast ratio dan bisa identify pass/fail
- [ ] Hafal 60-30-10 rule dan bisa terapkan
- [ ] Bisa identifikasi heuristik dari skenario tanpa melihat daftar
- [ ] Paham Impact-Effort Matrix dan cara memprioritaskan iterasi
- [ ] Sudah latihan soal analisis prototipe di atas

---

*📚 Referensi: Norman, D.A. (2013). The Design of Everyday Things | Krug, S. (2014). Don't Make Me Think | Garrett, J.J. (2010). Elements of User Experience | Nielsen, J. 10 Usability Heuristics | Material Design 3 — m3.material.io*
