# Pertemuan 6: Adobe Illustrator — Vector Art & Desain Logo / Brand Identity

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan perbedaan antara grafis vektor dan raster serta kapan menggunakannya
* Menavigasi antarmuka Adobe Illustrator dengan percaya diri
* Menggunakan Pen Tool untuk membuat path dan bentuk yang presisi
* Menerapkan proses desain logo dari konsep hingga file siap pakai
* Membuat brand identity sederhana (logo, warna, tipografi) untuk sebuah brand
* Mengekspor logo dalam format yang tepat untuk berbagai kebutuhan

---

## 📖 Pengantar (Hook)

Logo yang bagus bisa bernilai miliaran rupiah.

Logo Apple yang sederhana? Senilai brand yang diperkirakan > $500 miliar.
Logo Nike Swoosh? Dibeli tahun 1971 seharga $35. Sekarang tak ternilai.
Logo Tokopedia dengan burung hantu hijau? Menjadi ikon e-commerce terbesar Indonesia.

Apa rahasia logo-logo itu? **Dibuat dengan vektor.**

Vektor memungkinkan logo dipasang di kartu nama 5cm dan di billboard 10 meter — dengan kualitas yang persis sama, tanpa pecah. Inilah mengapa setiap logo profesional di dunia dibuat menggunakan Adobe Illustrator atau tool vektor lainnya.

Hari ini, kamu akan belajar cara kerjanya.

---

## 🧩 Konsep Utama

### Vector vs Raster — Perbedaan Fundamental

| | **Raster (Bitmap)** | **Vector** |
|---|---|---|
| **Terdiri dari** | Piksel (kotak kecil berwarna) | Koordinat matematis & path |
| **Saat diperbesar** | Pecah / blur | Tetap tajam sempurna |
| **Ukuran file** | Besar (bergantung resolusi) | Kecil (bergantung kompleksitas) |
| **Format** | JPG, PNG, PSD, GIF | SVG, AI, EPS, PDF |
| **Software** | Photoshop | Illustrator, CorelDRAW, Figma |
| **Dipakai untuk** | Foto, gambar kompleks | Logo, ikon, ilustrasi, tipografi |

**Aturan sederhana:**
* **Foto** → Raster (Photoshop)
* **Logo, ikon, ilustrasi** → Vector (Illustrator)

### Interface Adobe Illustrator

**Area utama:**
1. **Artboard** — "kanvas" tempat kamu berkarya. Bisa punya banyak artboard dalam satu file.
2. **Tools Panel (kiri)** — semua tool yang tersedia
3. **Properties / Control Bar (atas)** — opsi tool yang aktif
4. **Layers Panel (kanan)** — manajemen layer (mirip Photoshop)
5. **Swatches Panel** — warna yang tersimpan

**Tools paling penting:**
| Tool | Fungsi | Shortcut |
|---|---|---|
| **Selection Tool** | Pilih dan pindah objek | V |
| **Direct Selection** | Edit anchor point | A |
| **Pen Tool** | Buat path vektor bebas | P |
| **Shape Tools** | Kotak, lingkaran, poligon | M, L, dll |
| **Type Tool** | Teks | T |
| **Pathfinder** | Gabung/potong bentuk | Window → Pathfinder |

### Pen Tool — Senjata Utama Illustrator

Pen Tool adalah tool paling powerful di Illustrator — dan yang paling sering bikin frustasi pemula.

**Konsep dasar:**
* Setiap klik = **Anchor Point** (titik jangkar)
* Garis antara anchor point = **Path/Segment**
* Klik + drag = membuat **Bezier Curve** (kurva yang bisa dikontrol)
* Klik kembali ke titik pertama = **menutup path**

**Mode Pen Tool:**
* **Klik biasa** → titik sudut tajam
* **Klik + drag** → titik dengan kurva (muncul handle/gagang)
* **Alt/Opt + klik anchor point** → ubah kurva jadi sudut

### Pathfinder — Kombinasi Bentuk

Pathfinder memungkinkan kamu menggabungkan, memotong, dan mengubah bentuk-bentuk:

| Operasi | Fungsi |
|---|---|
| **Unite** | Gabungkan semua bentuk jadi satu |
| **Minus Front** | Potong bentuk depan dari bentuk belakang |
| **Intersect** | Ambil hanya bagian yang tumpang tindih |
| **Exclude** | Kebalikan Intersect |

---

## 🧠 Ilustrasi / Analogi

Bedanya vector dan raster itu seperti:

**Raster = foto dari foto kopi mesin:**
Jika kamu perbesar fotokopian, gambarnya jadi buram dan kotak-kotak.

**Vector = gambar dari peta geometri:**
Peta bisa diperbesar sebesar apapun — garis tetap tajam karena dibuat dari rumus matematika, bukan piksel.

**Proses desain logo** itu seperti membangun rumah:
1. **Riset & Brief** = baca kebutuhan, ukur tanah
2. **Sketsa** = gambar denah kasar
3. **Digitalisasi** = buat blueprint presisi di komputer
4. **Refinement** = perbaiki detail, pilih material
5. **Serah Terima** = kasih kunci ke klien dalam berbagai format

---

## 💻 Praktik / Contoh Teknis

### Proses Desain Logo dari Nol

**Tahap 1: Riset & Brief**

Sebelum buka Illustrator, jawab pertanyaan ini dulu:
* Siapa target audiens brand ini?
* Apa nilai-nilai brand (trustworthy, playful, premium, modern)?
* Kompetitor siapa saja? Logo mereka seperti apa?
* Dipakai di mana saja? (digital only, atau juga cetak?)

**Tahap 2: Sketsa Tangan**

Buat 10-20 sketsa kasar di kertas. Tujuan: eksplorasi sebanyak mungkin konsep, jangan langsung sempurna. Pilih 3 terbaik untuk didigitalisasi.

**Tahap 3: Digitalisasi di Illustrator**

Setup file:
1. File → New → pilih ukuran Artboard (biasanya 500x500px untuk logo)
2. Color Mode: RGB (untuk digital), atau CMYK (untuk cetak)

Buat bentuk dasar:
1. Gunakan Shape Tools untuk bentuk geometris dasar
2. Combine dengan Pathfinder untuk buat bentuk unik
3. Gunakan Pen Tool untuk elemen custom

Tambahkan teks (wordmark):
1. Pilih Type Tool (T)
2. Klik canvas, ketik nama brand
3. Pilih font yang sesuai dengan karakter brand
4. Atur kerning (spasi huruf) agar rapi

**Tahap 4: Refinement**

* Coba logo di background hitam, putih, dan berwarna
* Coba ukuran kecil (16x16px) — apakah masih terbaca?
* Minta feedback dari orang lain
* Pastikan tidak mirip logo lain

**Tahap 5: Finalisasi & Export**

Format yang harus disiapkan:
```
logo_primary.svg      — untuk web (scalable)
logo_primary.png      — transparan, 1000px
logo_primary.eps      — untuk cetak/vendor
logo_white.svg        — versi putih (untuk background gelap)
logo_black.svg        — versi hitam (grayscale)
logo_favicon.png      — 32x32px untuk icon website
```

### Membuat Brand Guideline Sederhana

Brand guideline = dokumen yang mendefinisikan cara pakai brand secara konsisten.

Isi minimal:
1. **Logo** — versi utama, variasi warna, ukuran minimum
2. **Color Palette** — kode HEX, RGB, CMYK setiap warna
3. **Typography** — font heading dan body, ukuran minimal
4. **Logo Clear Space** — area kosong minimal di sekitar logo (biasanya = tinggi huruf 'x' di logo)
5. **Logo Don'ts** — contoh penggunaan yang dilarang (jangan stretch, jangan rotate, jangan pakai warna lain)

---

## 🏦 Studi Kasus Nyata (Industri Kreatif / Digital)

#### Mendesain Logo Startup Fintech: Dari Konsep ke Produk

**Skenario:** Startup "KoinKita" — aplikasi tabungan digital untuk pelajar dan mahasiswa — butuh logo baru sebelum launching. Brief dari founder:
* Target: pelajar/mahasiswa usia 16-25 tahun
* Nilai brand: **terpercaya, muda, mudah, progresif**
* Tagline: "Nabung itu gampang"
* Warna: terbuka, tapi harus terasa *fresh* dan *tidak tua*

**Proses desain:**

**Riset:**
* Kompetitor: Jago (biru muda, playful), Jenius (cyan, modern), Bibit (hijau, organic)
* Gap: belum ada yang benar-benar menyasar pelajar dengan visual yang "anak muda banget"

**Eksplorasi konsep:**
1. Koin + tanda panah ke atas (pertumbuhan) → terlalu klise
2. Piggy bank dengan modernisasi → terlalu childish
3. Huruf "K" yang dibentuk dari koin bertumpuk → unik, bisa jadi lettermark

**Keputusan desain:**
* **Logo:** Lettermark "KK" dengan bentuk geometris — dua segitiga yang membentuk huruf K, terkesan modern dan stabil
* **Warna:** Biru-ungu gradient (#4F46E5 ke #7C3AED) — kepercayaan (biru) + kreativitas/muda (ungu), pembeda dari kompetitor
* **Font:** Plus Jakarta Sans (modern, bersih, buatan desainer Indonesia!)

**Dampak ke bisnis:**
* Logo yang "instagram-able" membantu viral di media sosial
* Warna konsisten di semua touchpoint (app, website, merchandise) membangun brand recognition
* Setelah 6 bulan, user research menunjukkan 78% target audiens mengasosiasikan "KoinKita" dengan kata "modern" dan "terpercaya"

---

## 📊 Visualisasi

### Jenis-Jenis Logo

| Jenis | Deskripsi | Contoh Brand |
|---|---|---|
| **Wordmark** | Hanya teks, tidak ada ikon | Google, Coca-Cola, FedEx |
| **Lettermark** | Inisial/huruf saja | IBM, HBO, NASA |
| **Pictorial** | Ikon/gambar tanpa teks | Apple, Twitter (burung), Shell |
| **Abstract** | Bentuk abstrak tanpa makna literal | Nike Swoosh, Pepsi |
| **Combination Mark** | Ikon + teks | Adidas, McDonald's, Starbucks |
| **Emblem** | Teks dalam simbol (seperti badge) | Harley Davidson, Starbucks lama |

### Kapan Pakai Jenis Logo Mana?

* **Startup baru** → Combination Mark (ikon + nama, membantu brand recognition awal)
* **Brand sudah sangat terkenal** → Pictorial atau Lettermark saja
* **Profesional/formal** → Wordmark atau Emblem
* **App icon** → Pictorial atau Lettermark (harus bisa di 1:1 ratio)

---

## ⚠️ Kesalahan Umum

1. **Langsung buka Illustrator tanpa sketsa**
   → Software adalah alat, bukan sumber ide. Sketsa di kertas dulu, baru digitalisasi.

2. **Terlalu kompleks — terlalu banyak detail**
   → Logo yang baik tetap terbaca di ukuran kecil. Coba kecilkan logomu ke 32px — masih terbaca?

3. **Pakai terlalu banyak font (lebih dari 2)**
   → Logo biasanya cukup satu font yang kuat. Dua font maksimal jika ada alasan kuat.

4. **Font sudah dikombinasikan langsung dari menu Font**
   → Untuk logo, selalu **Convert to Outlines** (Type → Create Outlines) setelah selesai. Ini mengubah teks jadi path vektor — tidak bergantung font terinstall di komputer orang lain.

5. **Tidak menyiapkan variasi warna logo**
   → Klien akan membutuhkan versi hitam, putih, dan berwarna. Siapkan dari awal.

6. **Menyimpan hanya sebagai PNG/JPEG tanpa file AI/EPS**
   → File AI = file sumber yang bisa diedit kapan saja. Jangan pernah serahkan ke klien tanpa simpan file AI-nya sendiri.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Jelaskan mengapa logo **harus** dibuat dalam format vektor, bukan raster. Berikan dua situasi nyata di mana perbedaan ini sangat krusial.

b) Seorang klien startup fintech memintamu membuat logo. Dia bilang: "Pakai aja foto tangan memegang koin sebagai logo." Apa yang akan kamu jelaskan kepada klien ini?

c) Sebutkan minimal 5 informasi yang harus ada dalam **Brand Guideline** dan jelaskan mengapa masing-masing penting.

---

### Soal 2 — Praktek

**Tugas: Logo & Mini Brand Identity**

Buat logo dan brand identity sederhana untuk bisnis fiktif pilihanmu sendiri. Bisa berupa:
* Startup fintech (dompet digital, investasi, pinjaman)
* Kafe / restoran
* NGO / komunitas
* Toko online

**Deliverables:**
1. **Logo utama** (combination mark — ikon + teks) di Adobe Illustrator
2. **3 variasi warna**: full color, hitam, putih
3. **Mini Brand Guideline** (1 halaman) berisi:
   * Logo primary & variasi
   * Color palette (min. 2 warna + HEX code)
   * Font pilihan (heading & body)
   * Satu contoh "Don't" (cara yang salah menggunakan logo)

**Luaran:** File AI + PDF brand guideline

---

## 📌 Ringkasan

* **Vector** = berbasis rumus matematika, tidak pecah saat diperbesar — wajib untuk logo
* **Raster** = berbasis piksel, pecah jika diperbesar — untuk foto
* **Illustrator** = software standar industri untuk membuat grafis vektor
* **Pen Tool** = tool paling powerful di Illustrator, butuh latihan
* **Pathfinder** = cara gabung/potong bentuk untuk buat bentuk custom
* **Proses Logo:** Riset → Sketsa tangan → Digitalisasi → Refinement → Export
* **Jenis logo:** Wordmark, Lettermark, Pictorial, Abstract, Combination Mark, Emblem
* **Export wajib:** SVG, AI, EPS (cetak), PNG transparan (web), versi hitam & putih
* **Brand Guideline** = dokumen yang menjaga konsistensi visual brand
* SELALU **Convert to Outlines** sebelum menyerahkan file logo ke klien

---

*📚 Referensi: Adobe Creative Team. (2023). Adobe Illustrator Classroom in a Book | Behance.net*
