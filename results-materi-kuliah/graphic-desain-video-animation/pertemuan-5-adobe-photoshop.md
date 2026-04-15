# Pertemuan 5: Adobe Photoshop — Editing Foto & Manipulasi Digital

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menavigasi antarmuka Photoshop dengan percaya diri (panel, workspace, shortcut)
* Memahami konsep Layer dan menggunakannya untuk mengelola elemen desain
* Melakukan seleksi objek dengan berbagai tools (Quick Selection, Pen Tool, Masking)
* Melakukan retouching foto dasar (skin retouching, background removal)
* Membuat komposit foto sederhana menggunakan teknik layer blending
* Mengekspor hasil kerja dalam format yang tepat untuk berbagai kebutuhan

---

## 📖 Pengantar (Hook)

Kamu pernah lihat foto iklan produk di billboard atau majalah?

Model yang kulitnya sempurna. Background yang bersih dan konsisten. Produk yang terlihat lebih menarik dari aslinya. Cahaya yang dramatic padahal foto diambil di studio sederhana.

Semua itu adalah hasil kerja Photoshop.

Adobe Photoshop sudah 35+ tahun menjadi standar industri editing foto. Tapi yang menarik — sekarang skill Photoshop bukan lagi monopoli desainer profesional. Content creator, tim marketing fintech, bahkan founder startup perlu bisa Photoshop untuk membuat konten visual yang kredibel dan profesional.

Di pertemuan ini, kita akan bongkar "rahasia dapur" Photoshop dari nol.

---

## 🧩 Konsep Utama

### Interface Photoshop

Saat pertama buka Photoshop, kamu akan melihat beberapa area utama:

**1. Menu Bar (paling atas)**
File, Edit, Image, Layer, Type, Select, Filter, View, Window, Help

**2. Options Bar (di bawah Menu Bar)**
Menampilkan opsi dari tool yang sedang aktif — berubah-ubah sesuai tool yang dipilih

**3. Tools Panel (kiri)**
Kumpulan semua tool: seleksi, cropping, painting, retouching, dll.
Klik kanan tool untuk melihat tools tersembunyi di dalamnya.

**4. Canvas / Document Window (tengah)**
Area kerja utama tempat kamu mengedit gambar

**5. Panel-panel (kanan)**
* **Layers Panel** — manajemen semua layer
* **Properties Panel** — properti layer/adjustment yang aktif
* **History Panel** — riwayat perubahan (bisa undo berkali-kali)
* **Color / Swatches Panel** — pemilihan warna

### Konsep Layer — Fondasi Photoshop

Layer adalah konsep paling penting di Photoshop. Bayangkan layer seperti **tumpukan kertas transparan** — kamu bisa menggambar di setiap kertas tanpa merusak yang lain.

**Jenis-jenis Layer:**

| Jenis Layer | Fungsi |
|---|---|
| **Normal Layer** | Layer gambar/foto biasa |
| **Adjustment Layer** | Koreksi warna non-destructive (Brightness, Curves, Hue/Saturation) |
| **Text Layer** | Teks yang bisa diedit kapan saja |
| **Shape Layer** | Bentuk vektor |
| **Smart Object** | Layer yang bisa di-transform tanpa kehilangan kualitas |
| **Group** | Folder untuk mengorganisir layer |

**Blending Modes** = cara layer berinteraksi dengan layer di bawahnya:
* **Normal** — layer penuh, tidak ada interaksi
* **Multiply** — menggelapkan, menghilangkan putih
* **Screen** — mencerahkan, menghilangkan hitam
* **Overlay** — meningkatkan kontras
* **Soft Light** — efek halus seperti cahaya lembut

### Selection Tools — Cara Memilih Area

Sebelum mengedit, kamu harus *memilih* (select) area yang ingin diedit:

| Tool | Kegunaan | Shortcut |
|---|---|---|
| **Rectangular/Elliptical Marquee** | Seleksi bentuk kotak/lingkaran | M |
| **Quick Selection** | Seleksi otomatis berdasarkan warna | W |
| **Magic Wand** | Seleksi area warna yang sama | W (hold) |
| **Lasso Tool** | Seleksi bebas dengan tangan | L |
| **Pen Tool** | Seleksi presisi tinggi dengan path | P |
| **Object Selection** | Seleksi objek otomatis dengan AI | W (hold) |

---

## 🧠 Ilustrasi / Analogi

Photoshop itu seperti **meja kerja desainer tradisional** yang sudah di-digitalisasi:

| Dunia Tradisional | Photoshop |
|---|---|
| Kertas gambar bertumpuk | Layers |
| Penghapus | Eraser Tool / Layer Mask |
| Gunting & lem | Select + Cut/Paste |
| Filter foto kamar gelap | Adjustment Layers |
| Kertas kalkir (transparan) | Layer dengan Opacity |
| Undo (tipe-ex) | History Panel (bisa undo 50+ langkah) |
| Cat/brush | Brush Tool |

Bedanya: di Photoshop kamu bisa **undo segalanya** dan **tidak ada yang permanen** — selama kamu pakai teknik non-destructive (Adjustment Layer + Smart Object).

---

## 💻 Praktik / Contoh Teknis

### Tutorial: Background Removal & Komposit Foto

**Skenario:** Kamu punya foto produk dengan background berantakan, dan ingin menempatkannya di background baru yang bersih.

**Langkah 1: Buka foto di Photoshop**
* File → Open → pilih foto produk kamu
* Cek resolusi: Image → Image Size (minimal 72dpi untuk digital, 300dpi untuk cetak)

**Langkah 2: Duplicate Layer (SELALU lakukan ini dulu)**
* Di Layers Panel, klik kanan layer → Duplicate Layer
* Atau shortcut: Ctrl+J (Windows) / Cmd+J (Mac)
* *Ini melindungi foto original*

**Langkah 3: Seleksi Objek**
* Pilih **Object Selection Tool** (W)
* Klik dan drag di sekitar objek utama
* Photoshop akan otomatis mendeteksi tepi objek

* Jika kurang presisi, klik **"Select and Mask"** di Options Bar
  * Gunakan Refine Edge Brush untuk rambut/bulu yang halus
  * Output: "New Layer with Layer Mask"

**Langkah 4: Bersihkan dengan Layer Mask**
* Dengan Layer Mask aktif, pilih Brush Tool (B)
* **Hitam** = sembunyikan / hapus area
* **Putih** = tampilkan kembali area
* Lukis dengan hitam untuk hapus sisa background, putih untuk kembalikan area yang terhapus

**Langkah 5: Tempatkan di Background Baru**
* File → Place Embedded → pilih foto background baru
* Atur posisi di bawah layer objek (drag di Layers Panel)
* Sesuaikan ukuran dengan Free Transform: Ctrl/Cmd+T

**Langkah 6: Match Lighting & Color**
* Tambahkan Adjustment Layer di atas layer objek, clipped ke layer itu
* Layer → New Adjustment Layer → Curves → clip to layer
* Sesuaikan agar cahaya objek cocok dengan background

**Langkah 7: Export**
* File → Export → Export As
* Format: PNG (transparan) atau JPEG (tanpa transparan)
* Atur kualitas dan ukuran

### Shortcut Penting Photoshop

| Shortcut (Win/Mac) | Fungsi |
|---|---|
| Ctrl/Cmd + Z | Undo |
| Ctrl/Cmd + Alt/Opt + Z | Undo berkali-kali |
| Ctrl/Cmd + J | Duplicate layer |
| Ctrl/Cmd + T | Free Transform |
| Ctrl/Cmd + D | Deselect |
| [ / ] | Perkecil/perbesar brush |
| Space + Drag | Pan / geser canvas |
| Ctrl/Cmd + 0 | Fit canvas di layar |
| Ctrl/Cmd + S | Save |
| Ctrl/Cmd + Shift + S | Save As |

---

## 🏦 Studi Kasus Nyata (Industri Kreatif / Digital)

#### Konten Iklan Fintech: Dari Foto Biasa ke Visual Profesional

**Skenario:** Tim marketing startup fintech "QuickPay" perlu membuat konten iklan untuk Instagram dalam waktu 2 hari. Budget terbatas — tidak ada anggaran untuk sesi foto profesional. Yang ada: foto produk diambil dengan iPhone di meja kantor, dan beberapa foto model dari Unsplash.

**Masalah:**
* Foto produk (smartphone dengan app) diambil di atas meja kayu dengan background berantakan
* Pencahayaan tidak konsisten — satu foto terlalu gelap, satu terlalu terang
* Perlu 5 variasi konten (berbeda background/model) untuk A/B testing iklan

**Solusi dengan Photoshop:**

1. **Background Removal**: Gunakan Object Selection + Layer Mask untuk isolasi produk dari semua 5 foto
2. **Batch Processing**: Rekam Action (Window → Actions) untuk otomasi proses yang berulang — proses 5 foto hanya butuh 1 klik setelah action dibuat
3. **Color Grading Konsisten**: Buat satu Adjustment Layer preset (Curves + Hue/Saturation) yang di-apply ke semua foto agar terlihat konsisten sebagai satu campaign
4. **Background Replacement**: Pakai gradient atau foto background premium dari Unsplash — letakkan di bawah layer produk
5. **Text & CTA**: Tambahkan text layer "Transfer Gratis Selamanya" dengan font sesuai brand guideline

**Hasil:** 5 variasi konten siap dalam 4 jam (bukan 2 hari). Biaya: Rp 0 (foto dari Unsplash gratis, software sudah ada).

**Dampak bisnis:** Iklan dengan visual profesional mendapat CTR (Click-Through Rate) 3x lebih tinggi dibanding konten "apa adanya" yang biasa dipakai sebelumnya.

---

## 📊 Visualisasi

### Workflow Editing Foto Profesional

```
[Foto RAW / Original]
        ↓
[Duplicate Layer — SELALU]
        ↓
[Global Correction]
  → Exposure, White Balance (Camera Raw / Adjustment Layer)
        ↓
[Seleksi & Masking]
  → Object Selection → Refine Edge → Layer Mask
        ↓
[Retouching Lokal]
  → Clone Stamp, Healing Brush, Dodge/Burn
        ↓
[Color Grading]
  → Curves, Hue/Saturation, Color Balance
        ↓
[Kompositing]
  → Gabungkan elemen, sesuaikan pencahayaan
        ↓
[Sharpening & Noise Reduction]
  → Filter → Sharpen / Camera Raw
        ↓
[Export]
  → Format & resolusi sesuai kebutuhan
```

### Format Export yang Tepat

| Format | Kegunaan | Kelebihan |
|---|---|---|
| **JPEG** | Foto untuk web/sosmed | File kecil, kompatibel universal |
| **PNG** | Logo, grafik dengan transparansi | Kualitas tinggi, support transparan |
| **PSD** | File kerja Photoshop | Semua layer terjaga, bisa diedit ulang |
| **TIFF** | Cetak berkualitas tinggi | Tanpa kompresi, kualitas maksimal |
| **WebP** | Web modern | File lebih kecil dari JPEG, kualitas lebih baik |

---

## ⚠️ Kesalahan Umum

1. **Langsung edit di layer original (bukan duplicate)**
   → Jika salah, tidak bisa kembali ke kondisi awal. SELALU duplicate dulu!

2. **Menggunakan Eraser Tool untuk hapus background**
   → Eraser bersifat *destructive* — piksel hilang permanent. Gunakan **Layer Mask** agar bisa diedit ulang.

3. **Menyimpan dalam format JPEG berkali-kali**
   → Setiap save JPEG mengurangi kualitas gambar (lossy compression). Simpan master sebagai PSD, export JPEG hanya untuk final output.

4. **Tidak memperhatikan resolusi dari awal**
   → Mulai di resolusi rendah (72dpi) lalu ingin cetak = pecah. Pastikan resolusi sesuai tujuan sejak awal.

5. **Layer tidak terorganisir dan tidak dinamai**
   → Project dengan 30+ layer tanpa nama dan tanpa group = mimpi buruk saat revisi. Biasakan beri nama layer dan buat group.

6. **Lupa simpan file PSD (hanya simpan PNG/JPEG)**
   → Jika perlu revisi, kamu harus mulai dari nol. Selalu simpan file PSD sebagai "master file."

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

a) Apa perbedaan antara **Adjustment Layer** dan langsung mengedit warna di layer gambar? Mengapa Adjustment Layer lebih direkomendasikan?

b) Jelaskan konsep **Layer Mask** dengan analogi sederhana. Apa keunggulannya dibanding menggunakan Eraser Tool?

c) Kapan kamu akan menggunakan **Smart Object** vs layer biasa? Berikan satu contoh use case nyata.

---

### Soal 2 — Praktek

**Tugas: Komposit Foto Produk**

1. Cari foto produk apapun (bisa foto hp kamu, atau download dari Unsplash dengan keyword "product mockup")
2. Cari foto background yang menarik (interior, gradient, outdoor)
3. Di Photoshop, pisahkan produk dari background aslinya menggunakan Object Selection + Layer Mask
4. Tempatkan produk di background baru
5. Tambahkan minimal satu Adjustment Layer untuk menyesuaikan warna/pencahayaan agar terlihat natural
6. Tambahkan teks sederhana (nama produk atau tagline)
7. Export sebagai PNG

**Kriteria penilaian:** tepi seleksi rapi, pencahayaan produk konsisten dengan background, teks terbaca dan terkomposis dengan baik.

---

## 📌 Ringkasan

* **Photoshop** = software editing foto & manipulasi digital standar industri
* **Layer** = tumpukan transparan yang bisa diedit independen — konsep paling fundamental
* **SELALU** duplicate layer sebelum mengedit (non-destructive workflow)
* **Selection Tools:** Quick Selection untuk cepat, Pen Tool untuk presisi
* **Layer Mask** > Eraser — masking non-destructive, bisa diedit ulang
* **Adjustment Layer** = koreksi warna tanpa merusak piksel asli
* **Blending Modes** mengontrol cara layer berinteraksi dengan layer di bawahnya
* **Smart Object** = layer yang bisa di-transform berkali-kali tanpa kehilangan kualitas
* **Format export:** PSD (master), JPEG (web foto), PNG (transparan), TIFF (cetak)
* Shortcut penting: Ctrl+J (duplicate), Ctrl+T (transform), [ / ] (brush size), Ctrl+Z (undo)

---

*📚 Referensi: Adobe Creative Team. (2023). Adobe Photoshop Classroom in a Book | helpx.adobe.com*
