# Pertemuan 6: Information Architecture — Sitemap & Card Sorting

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan prinsip Information Architecture (IA)
* Melakukan Card Sorting (open dan closed) untuk mengelompokkan konten
* Membuat Sitemap yang merepresentasikan hierarki konten
* Memilih navigation pattern yang sesuai dengan kebutuhan produk

---

## 📖 Pengantar (Hook)

Pernahkah kamu membuka aplikasi dan tidak bisa menemukan fitur yang kamu cari?

Kamu sudah klik sana-sini, masuk ke submenu yang tidak relevan, dan akhirnya menyerah atau terpaksa gunakan search.

Bukan masalah desain visual. Bukan masalah warna atau font. Ini adalah masalah **Information Architecture** — bagaimana konten dan fitur diorganisir dan diberi label.

IA yang buruk adalah alasan utama mengapa pengguna meninggalkan produk, bahkan ketika fiturnya sendiri bagus.

---

## 🧩 Konsep Utama

### Apa itu Information Architecture?

Information Architecture (IA) adalah seni dan sains mengorganisir dan melabeli konten di website, aplikasi, dan sistem lainnya untuk mendukung usability dan findability.

**4 Komponen IA (Rosenfeld & Morville):**

1. **Organization Systems** — Bagaimana konten dikelompokkan
   * Alfabetis, kronologis, geografis, by topic, by task, by audience
   
2. **Labeling Systems** — Bagaimana konten diberi nama
   * "Riwayat" vs "Mutasi" vs "Transaksi" — label mempengaruhi mental model pengguna
   
3. **Navigation Systems** — Bagaimana pengguna berpindah antar konten
   * Global nav, local nav, breadcrumb, footer nav
   
4. **Search Systems** — Bagaimana pengguna mencari konten
   * Search bar, filter, faceted search

### Card Sorting

Card Sorting adalah teknik riset untuk memahami **mental model pengguna** — bagaimana mereka secara natural mengelompokkan konten.

**Open Card Sorting:**
* Pengguna mengelompokkan kartu konten DAN memberi nama kelompok sendiri
* Tujuan: menemukan kategori yang natural menurut pengguna
* Cocok untuk: merancang IA dari awal

**Closed Card Sorting:**
* Pengguna mengelompokkan kartu ke dalam kategori yang sudah ditentukan
* Tujuan: validasi apakah kategori yang sudah ada intuitif
* Cocok untuk: mengevaluasi dan menyempurnakan IA yang sudah ada

```
PROSES OPEN CARD SORTING (10-15 peserta):

PERSIAPAN:
1. Tulis 30-50 konten/fitur di kartu terpisah
   "Transfer ke Sesama", "Transfer ke Bank", "Bayar Tagihan",
   "Top Up", "Riwayat", "Dompet", "Profil", "Bantuan", ...

PELAKSANAAN:
2. Berikan semua kartu ke peserta
3. Minta mereka kelompokkan kartu yang terasa berhubungan
4. Minta mereka beri nama untuk setiap kelompok
5. Rekam hasilnya (foto / digitalkan)

ANALISIS:
6. Cari pola: kartu mana yang SELALU dikelompokkan bersama?
7. Identifikasi kartu yang ambigu (dikelompokkan berbeda-beda)
8. Gunakan hasil untuk merancang hierarki navigasi

TOOLS: OptimalSort, Maze, atau fisik dengan sticky notes
```

### Sitemap

Sitemap adalah representasi visual dari struktur hierarki konten/fitur dalam suatu produk.

```
SITEMAP APLIKASI FINTECH (contoh):

HOME
│
├── KIRIM UANG
│   ├── Transfer Sesama
│   ├── Transfer ke Bank
│   └── Riwayat Kirim
│
├── BAYAR
│   ├── Scan QR
│   ├── Tagihan
│   │   ├── Listrik
│   │   ├── Air (PDAM)
│   │   ├── Pulsa
│   │   └── Internet
│   └── Riwayat Bayar
│
├── TABUNGAN
│   ├── Saldo Utama
│   ├── Reksa Dana
│   └── Emas Digital
│
├── RIWAYAT (Semua Transaksi)
│
└── PROFIL & PENGATURAN
    ├── Data Diri
    ├── Keamanan (PIN, Biometrik)
    ├── Notifikasi
    └── Bantuan & FAQ
```

### Navigation Patterns

| Pattern | Deskripsi | Kapan Dipakai |
|---|---|---|
| **Tab Bar** | 3-5 ikon di bawah layar | Fitur utama yang sering diakses |
| **Hamburger Menu** | Ikon ≡ yang expands | Banyak fitur, tapi tidak semua sering dipakai |
| **Bottom Sheet** | Panel yang muncul dari bawah | Aksi kontekstual |
| **Breadcrumb** | Home > Kategori > Halaman | Konten dengan hierarki dalam |
| **Search + Browse** | Kombinasi pencarian dan navigation | Konten catalog yang besar |

**Perdebatan: Tab Bar vs Hamburger Menu**
* Tab bar: lebih discoverable (terlihat langsung), tapi terbatas
* Hamburger menu: lebih banyak item, tapi fitur tersembunyi = kurang dipakai
* Riset Nielsen Norman Group: fitur di tab bar digunakan 2-3x lebih sering

### Flat vs Deep IA

**Flat IA:** Semua konten bisa dicapai dalam 2-3 klik dari home
```
Home → Kirim Uang → Konfirmasi (3 klik)
```

**Deep IA:** Konten tersembunyi di banyak level submenu
```
Home → Menu → Keuangan → Transfer → Kirim → Konfirmasi (6 klik)
```

**Aturan umum:** Pengguna tidak boleh butuh lebih dari 3 klik untuk mencapai konten utama. Semakin dalam, semakin frustrasi.

---

## 🧠 Ilustrasi / Analogi

**Sitemap seperti denah gedung:**
* Arsitektur bangunan yang buruk → orang kesasar di dalam gedung
* Arsitektur informasi yang buruk → pengguna kesasar di dalam aplikasi
* Denah yang baik → setiap ruangan punya label jelas, koridor logis

**Card Sorting seperti meminta bantuan menyusun rak buku:**
* Kamu punya 100 buku tanpa kategori
* Kamu minta 10 orang berbeda untuk mengelompokkan buku tersebut
* Hasil: kamu tahu bagaimana orang "alami" mengkategorikan buku — berdasarkan genre? Penulis? Ukuran?

---

## 💻 Praktik: Membuat Sitemap di Figma

### Template Sitemap di Figma

```
SETUP:
- Gunakan rectangle untuk setiap halaman/fitur
- Warna berbeda untuk setiap level hierarki:
  Level 0 (Home) = biru tua
  Level 1 (Section utama) = biru medium
  Level 2 (Sub-section) = biru muda
  Level 3 (Detail pages) = abu-abu

- Hubungkan dengan connector arrows

CONTOH KOMPONEN FIGMA:
Component "Sitemap Node":
  - Frame 160x48px
  - Corner radius 8
  - Fill: [warna per level]
  - Text di tengah: nama halaman
  - Variant: normal, selected, error

BEST PRACTICES:
- Mulai dari kiri ke kanan (mobile: top ke bottom)
- Gunakan alignment tool untuk menjaga konsistensi
- Tambahkan legend warna di pojok
- Export sebagai PDF untuk dokumentasi
```

---

## 🏢 Studi Kasus Nyata: IA Redesign Tokopedia

**Masalah:** Pengguna kesulitan menemukan produk tertentu karena navigasi kategori yang terlalu dalam dan label yang ambigu.

**Riset yang Dilakukan:**
* Open card sorting dengan 50 pengguna aktif Tokopedia
* Tree testing (uji navigasi): berapa % pengguna bisa menemukan "Skincare Wajah" dalam hierarki yang ada?
* Heatmap: fitur mana yang paling sering diklik di home

**Temuan:**
* 67% pengguna mencari produk via search, bukan browse kategori
* Kategori "Kecantikan" sering tidak diidentifikasi sebagai tempat "Skincare"
* "Voucher" dan "Promo" → pengguna mengira itu satu hal padahal berbeda

**Solusi IA:**
* Home: search bar lebih prominan
* Kategori: relabel "Kecantikan & Skincare" (tidak lagi ambigu)
* Navigation: tambahkan "trending categories" based on user segment
* Voucher dan Promo digabung menjadi satu entry point

**Hasil:** Click-through rate ke produk meningkat 18%, bounce rate turun 12%.

---

## ⚠️ Kesalahan Umum

1. **Membuat IA berdasarkan struktur organisasi internal** → Tim produk sering mengorganisir fitur berdasarkan bagaimana tim internal bekerja, bukan bagaimana pengguna berpikir. Card sorting membantu menghindari ini.

2. **Label yang menggunakan jargon teknis** → "Mutasi Rekening" vs "Riwayat Transaksi" — pengguna awam lebih familiar dengan yang kedua.

3. **Terlalu dalam (deep IA)** → Setiap level tambahan = lebih banyak klik = lebih banyak kesempatan untuk sesat.

4. **Tidak melakukan card sorting** → Desainer pikir mereka tahu kategori yang tepat. Pengguna sering punya mental model yang berbeda.

5. **Sitemap tidak diperbarui** → Saat fitur baru ditambahkan, sitemap harus diperbarui agar tetap jadi referensi yang akurat.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan Open Card Sorting vs Closed Card Sorting. Kapan kamu menggunakan masing-masing?

b) Apa yang dimaksud dengan "mental model" pengguna dalam konteks IA? Berikan contoh kasus di mana mental model pengguna berbeda dari asumsi desainer.

### Soal 2 — Praktik (Tugas)

Untuk proyek desain kamu:
1. Tulis **30-40 konten/fitur** dalam kartu terpisah
2. Lakukan **Open Card Sorting** dengan minimal **5 partisipan** (bisa menggunakan kertas fisik atau OptimalSort online)
3. Analisis hasil: kelompok apa yang konsisten? Kartu mana yang ambigu?
4. Buat **Sitemap lengkap** di Figma berdasarkan hasil card sorting
5. Tuliskan **rationale** (alasan) untuk setiap keputusan struktur

---

## 📌 Ringkasan

* **IA** = cara mengorganisir, melabeli, dan menghubungkan konten agar mudah ditemukan
* **4 komponen:** Organization → Labeling → Navigation → Search
* **Card Sorting** = riset untuk memahami mental model pengguna
  * Open = pengguna buat kategori sendiri
  * Closed = pengguna cocokkan ke kategori yang sudah ada
* **Sitemap** = representasi visual hierarki konten, dari level tinggi ke detail
* **Flat IA** (≤3 klik ke konten manapun) lebih baik dari Deep IA
* Navigation patterns: Tab Bar > Hamburger Menu untuk fitur sering dipakai
* IA yang baik = pengguna tidak perlu berpikir saat mencari sesuatu

---

*📚 Referensi: Rosenfeld & Morville (2015). Information Architecture for the Web | Krug, S. (2014). Don't Make Me Think | Nielsen Norman Group — Card Sorting*
