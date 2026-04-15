# Pertemuan 12: Motion Graphics & Animasi Teks dengan Keyframe

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Memahami konsep keyframe dan cara kerja interpolasi animasi (linear, ease in/out, bezier)
- Menganimasikan properti klip di Premiere Pro: Position, Scale, Rotation, dan Opacity
- Membuat lower third yang profesional untuk nama narasumber atau informasi on-screen
- Merancang title sequence sebagai opening animasi video yang menarik
- Menggunakan Essential Graphics Panel untuk membuat dan mengedit motion template
- Memahami hubungan Premiere Pro dengan Adobe After Effects dalam pipeline produksi
- Mengekspor motion graphics sebagai file .mogrt untuk digunakan ulang

---

## 📖 Pengantar (Hook)

Pernah nonton berita di CNBC Indonesia atau IDX Channel? Perhatikan layar TV-nya dengan seksama. Di bagian bawah layar, ada nama pembicara yang muncul dengan animasi halus. Di sudut kanan ada ticker harga saham yang bergerak. Saat grafik data ditampilkan, angkanya "tumbuh" dari nol dengan animasi yang smooth. Transisi antar segmen ada logo channel yang melayang masuk.

Semua itu bukan sihir — itu **motion graphics**.

Yang lebih menariknya lagi: kalau kamu nonton konten kreator di YouTube tentang ekonomi dan investasi, banyak yang *terasa* seperti siaran TV profesional, padahal dibuat oleh tim kecil dengan software yang sama yang bisa kamu akses. Perbedaannya hanya pada penguasaan **keyframe** dan **animasi teks**.

Pertemuan ini akan membuka "kunci" itu untuk kamu.

---

## 🧩 Konsep Utama

### 1. Apa Itu Keyframe?

**Keyframe** adalah "penanda" di titik waktu tertentu pada timeline yang menyimpan nilai sebuah properti (posisi, ukuran, rotasi, opacity).

Ketika kamu menempatkan dua keyframe dengan nilai berbeda, Premiere Pro akan **secara otomatis menghitung dan mengisi** perubahan di antara keduanya. Proses ini disebut **interpolasi**.

**Analogi Sederhana**: Bayangkan kamu punya boneka yang di detik ke-0 ada di pojok kiri layar, dan di detik ke-2 ada di pojok kanan. Keyframe = posisi boneka di detik 0 dan detik 2. Interpolasi = gerakan otomatis boneka dari kiri ke kanan selama 2 detik itu.

### 2. Jenis-Jenis Interpolasi

| Tipe | Karakteristik | Tampilan | Kapan Digunakan |
|---|---|---|---|
| **Linear** | Kecepatan konstan dari awal ke akhir | Kaku, mekanis | Pergerakan teknis/data |
| **Ease In** | Mulai cepat, melambat di akhir | Natural, mulus berhenti | Objek yang berhenti/landing |
| **Ease Out** | Mulai lambat, makin cepat | Natural, mulus mulai | Objek yang mulai bergerak |
| **Ease In & Out** | Lambat di awal, cepat di tengah, lambat di akhir | Paling natural | Hampir semua animasi UI/motion |
| **Bezier** | Kontrol penuh via handle kurva | Custom, bisa sangat kompleks | Animasi presisi tinggi |

### 3. Empat Properti Animasi Utama

Di Premiere Pro, setiap klip video punya properti yang bisa dianimasikan via **Effect Controls Panel**:

- **Position (X, Y)**: Gerakan objek secara horizontal dan vertikal
- **Scale**: Perubahan ukuran objek (100% = ukuran normal)
- **Rotation**: Putaran objek dalam derajat
- **Opacity**: Tingkat transparansi (0% = tidak kelihatan, 100% = penuh)

### 4. Lower Third

**Lower Third** adalah elemen grafis yang muncul di bagian bawah layar (sepertiga bagian bawah) berisi informasi seperti nama narasumber, jabatan, atau keterangan tambahan.

Ini adalah salah satu elemen motion graphics paling penting di industri broadcast dan konten digital karena:
- Memberikan **konteks** kepada penonton
- Meningkatkan **kredibilitas** dan kesan profesional
- Menjadi **brand identifier** yang konsisten

Anatomi lower third standar:
```
┌──────────────────────────────────────┐
│  [nama]  Budi Santoso                │
│  [jabatan] CEO, Pluang               │
└──────────────────────────────────────┘
    ↑ muncul di bagian bawah 1/3 layar
```

### 5. Title Sequence

Opening animasi video (title sequence) berfungsi sebagai "kartu nama" konten. Elemen yang biasanya ada:
- Logo atau nama channel yang muncul dengan animasi
- Musik intro yang khas
- Durasi: **3–8 detik** (lebih panjang dari itu penonton skip)

### 6. Essential Graphics Panel

Window → Essential Graphics — panel khusus Premiere Pro untuk:
- Browse dan gunakan template motion graphics (.mogrt)
- Edit teks, warna, dan elemen grafis dari template
- Simpan desain kamu sebagai template .mogrt baru
- Import .mogrt dari Motion Graphics Marketplace atau Adobe Stock

### 7. Hubungan Premiere Pro dan After Effects

Ini pertanyaan yang sering muncul: *kenapa ada dua software berbeda?*

| Aspek | Premiere Pro | After Effects |
|---|---|---|
| **Fungsi Utama** | Editing & assembly video | Motion graphics & visual effects |
| **Timeline** | Linear, berbasis klip video | Komposisi berbasis layer |
| **Animasi Teks** | Basic hingga intermediate | Sangat kompleks & fleksibel |
| **3D** | Tidak ada | Ada (basic 3D layer) |
| **Integrasi** | Terima .mogrt dari AE | Kirim .mogrt ke Premiere |
| **Kapan Dipakai** | Editing keseluruhan video | Buat elemen grafis animasi |

**Dynamic Link**: Fitur Adobe yang memungkinkan kamu membuka komposisi After Effects langsung dari Premiere tanpa perlu render dulu. Perubahan di AE langsung terlihat di Premiere.

**Pipeline Standar Industri**:
```
After Effects → Buat animasi/grafis → Export .mogrt → Import ke Premiere Pro
```

### 8. Export Motion Graphics (.mogrt)

File `.mogrt` (Motion Graphics Template) adalah format standar untuk berbagi motion graphics antara Premiere Pro dan After Effects. Keunggulannya:
- Bisa diedit teks/warna tanpa perlu After Effects
- Bisa dibagikan ke seluruh tim
- Bisa diupload/download dari Motion Graphics Marketplace

---

## 🧠 Ilustrasi / Analogi

### Keyframe = Koreografi Tari

Bayangkan kamu adalah sutradara pertunjukan tari. Kamu tidak perlu memberi instruksi ke penari untuk *setiap* langkah — kamu cukup memberikan **posisi kunci** di momen-momen tertentu: "Di hitungan ke-1, kamu di sudut kiri. Di hitungan ke-8, kamu di tengah panggung." Penari yang terlatih (= Premiere Pro) akan **mengisi sendiri gerakan di antaranya** dengan cara yang paling mulus.

| Konsep Tari | Konsep Keyframe |
|---|---|
| Posisi kunci di hitungan tertentu | Keyframe di titik waktu tertentu |
| Gerakan antara dua posisi kunci | Interpolasi otomatis |
| Gaya gerakan (kaku vs mengalir) | Tipe interpolasi (linear vs ease) |
| Urutan keseluruhan koreografi | Timeline animasi |
| Sutradara yang atur posisi kunci | Kamu sebagai animator |

### Ease In/Out = Mobil di Lampu Merah

- **Ease Out** = mobil mulai jalan dari lampu merah: **pelan dulu, makin lama makin kencang**
- **Ease In** = mobil mendekati lampu merah: **kencang dulu, makin lama makin pelan**
- **Ease In & Out** = perjalanan normal di jalan raya: **berangkat pelan, kencang di tengah, pelan saat sampai**

Animasi yang terasa "natural" di mata manusia selalu punya **ease in dan ease out** karena itu yang kita rasakan di dunia fisik — tidak ada benda yang bergerak dengan kecepatan konstan dari awal ke akhir.

---

## 💻 Praktik / Contoh Teknis

### Step-by-Step 1: Animasi Teks Muncul dengan Fade & Slide

**Tujuan**: Teks judul muncul dari bawah sambil fade in

1. **Buat teks baru**:
   - Pilih **Type Tool (T)** di toolbar
   - Klik di Program Monitor → ketik teks: "INVESTASI CERDAS"
   - Atur font dan ukuran di **Essential Graphics Panel** kanan

2. **Buka Effect Controls**:
   - Klik teks di Timeline → buka **Window → Effect Controls**
   - Kamu akan lihat: Motion → Position, Scale, Rotation; Opacity

3. **Set keyframe Position (Slide dari bawah)**:
   - Pindahkan playhead ke **detik 0:00**
   - Klik ikon jam (stopwatch) di sebelah **Position** → keyframe pertama terbuat
   - Ubah nilai Y (koordinat vertikal) menjadi lebih besar: misal `650` (posisi di bawah layar)
   - Pindahkan playhead ke **detik 0:20** (20 frame)
   - Ubah nilai Y ke posisi normal: `540` → keyframe kedua otomatis terbuat

4. **Set keyframe Opacity (Fade In)**:
   - Kembali ke detik `0:00`
   - Klik stopwatch di **Opacity** → set ke `0%`
   - Pindah ke detik `0:20` → set Opacity ke `100%`

5. **Ubah interpolasi ke Ease**:
   - Di Effect Controls, tampak grafik di sebelah kanan (klik segitiga kecil untuk expand)
   - Klik kanan pada keyframe pertama → **Temporal Interpolation → Ease Out**
   - Klik kanan pada keyframe kedua → **Temporal Interpolation → Ease In**

6. **Preview**: Tekan `Spacebar` di area timeline — teks akan muncul dari bawah sambil fade in secara smooth.

---

### Step-by-Step 2: Membuat Lower Third Profesional

**Tujuan**: Lower third dengan nama dan jabatan narasumber

1. **Import atau buat dari template**:
   - Buka **Essential Graphics Panel** (Window → Essential Graphics)
   - Klik **Browse** → cari template "Lower Third" yang tersedia
   - Drag template ke V2 di Timeline, posisikan di bawah klip wawancara

2. **Edit teks**:
   - Klik klip lower third di Timeline
   - Di **Essential Graphics Panel** (tab Edit), klik field nama → ubah teks
   - Ubah nama: "Budi Santoso" dan jabatan: "CEO, Pluang"

3. **Animasi slide dari kiri** (jika buat manual):
   - Pilih layer teks di Timeline
   - Effect Controls → Position:
     - Detik 0:00 → X: `-300` (di luar frame kiri)
     - Detik 0:15 → X: `200` (posisi final di layar kiri bawah)
   - Set interpolasi: Ease Out pada keyframe pertama, Ease In pada keyframe kedua

4. **Timing yang baik**:
   - Lower third muncul **0.5 detik setelah narasumber mulai berbicara**
   - Bertahan selama **3–5 detik**
   - Keluar dengan animasi balik (slide ke kiri atau fade out)

---

### Step-by-Step 3: Title Sequence Opening Video

**Tujuan**: Opening 5 detik dengan logo + teks channel muncul

1. **Buat sequence baru** untuk title: File → New → Sequence → nama: `Title_Sequence`

2. **Tambah background**:
   - Import file background (warna solid atau gambar)
   - Atau: File → New → Color Matte → pilih warna brand

3. **Animasi logo (Scale + Opacity)**:
   - Import file logo (PNG transparan) → drag ke V2
   - Effect Controls → Scale:
     - Detik 0:00 → Scale: `120` (lebih besar dari normal)
     - Detik 0:20 → Scale: `100` (ukuran normal)
   - Opacity: 0% di detik 0 → 100% di detik 0:15

4. **Animasi teks nama channel**:
   - Tambah teks di V3
   - Animasi Opacity: 0% → 100% mulai di detik 0:20

5. **Export sebagai .mogrt**:
   - Klik sequence di Project Panel
   - **Graphics → Export as Motion Graphics Template**
   - Pilih lokasi → Save
   - File .mogrt ini bisa digunakan di project lain tanpa perlu rebuild dari awal

---

### Step-by-Step 4: Transisi Animasi Kreatif Antar Scene

**Tujuan**: Transisi "zoom in" dari satu scene ke scene berikutnya

1. Letakkan dua klip bersebelahan di Timeline

2. **Pada akhir klip pertama** (2 detik terakhir):
   - Effect Controls → Scale: dari `100` → `120` (zoom in cepat)
   - Opacity: dari `100` → `0` (fade out)

3. **Pada awal klip kedua** (2 detik pertama):
   - Scale: dari `120` → `100` (zoom out ke normal)
   - Opacity: dari `0` → `100` (fade in)

4. **Hasilnya**: transisi yang terasa "push zoom" — lebih dinamis dari cross dissolve biasa

---

## 🏦 Studi Kasus Nyata (Industri Kreatif / Digital)

### Kasus: CNBC Indonesia & IDX Channel — Motion Graphics sebagai Pembentuk Kepercayaan

**Latar Belakang**

CNBC Indonesia dan IDX Channel adalah dua media berita ekonomi-keuangan terkemuka di Indonesia. Keduanya berkompetisi untuk menjadi referensi utama investor ritel dan profesional keuangan di Indonesia.

Salah satu elemen yang membedakan mereka dari channel YouTube biasa atau podcast adalah **kualitas motion graphics** — elemen visual bergerak yang muncul sepanjang tayangan.

---

**Masalah**

Bayangkan jika sebuah channel berita ekonomi tidak menggunakan motion graphics sama sekali:
- Penonton tidak tahu siapa yang sedang berbicara dan apa keahliannya
- Data dan angka hanya disebutkan secara verbal, tidak ada visualisasi yang mendukung
- Perpindahan antar segmen terasa datar dan membingungkan
- Tidak ada identitas visual yang konsisten → sulit dikenali

**Dampak**: Kepercayaan dan *perceived credibility* audiens akan jauh lebih rendah, meskipun kontennya berkualitas sama.

---

**Elemen Motion Graphics yang Membuat Tayangan Terlihat "TV-Quality"**

Tim produksi CNBC Indonesia menggunakan paket motion graphics yang terdiri dari:

| Elemen | Deskripsi | Fungsi |
|---|---|---|
| **Lower Third** | Nama + jabatan narasumber muncul 0.5 detik setelah narasumber mulai bicara, stay 4 detik, keluar smooth | Memberikan konteks dan kredibilitas narasumber |
| **Ticker** | Data harga saham/kurs yang berjalan di bagian bawah layar secara real-time | Memberikan informasi tambahan tanpa mengganggu konten utama |
| **Full Screen Grafis** | Grafik data, chart, infografis yang muncul dengan animasi | Memperkuat data yang sedang dibahas secara verbal |
| **Bug/Logo Channel** | Logo kecil di sudut kanan atas yang always-on | Brand awareness konsisten |
| **Bumper In/Out** | Animasi singkat 3–5 detik di awal dan akhir setiap segmen | Menandai pergantian segmen, menjaga ritme tayangan |
| **Wipe Transition** | Transisi antar reporter/studio dengan wipe berlogo | Perpindahan yang smooth dan beridentitas |

---

**Analisis: Kenapa Lower Third Sangat Penting?**

Penelitian tentang *trust in media* menunjukkan bahwa **informasi narasumber** adalah salah satu faktor terbesar yang mempengaruhi kepercayaan audiens terhadap konten berita.

Ketika lower third muncul:
- "Budi Santoso — Analis Senior, Mandiri Sekuritas" → penonton langsung percaya bahwa orang ini kompeten berbicara soal saham
- Jika tidak ada lower third → penonton bertanya-tanya "ini siapa ya, bisa dipercaya tidak?"

**Implementasi untuk Konten Creator Independen**

Channel YouTube tentang investasi seperti **Felicia Putri Tjiasaka** atau **Jouska** mengadopsi praktik yang sama:
- Lower third dengan nama host + branding konsisten di setiap video
- Intro animasi 3–5 detik yang menjadi "signature" channel
- Grafis data animasi untuk menampilkan angka-angka investasi

**Hasil**: Penonton mempersepsi channel tersebut sebagai lebih profesional dan terpercaya dibanding channel tanpa motion graphics, meskipun kualitas konten verbalnya serupa.

---

**Pelajaran Kunci**

> Motion graphics bukan sekadar dekorasi — mereka adalah alat komunikasi yang menyampaikan **kredibilitas, konteks, dan identitas brand** secara visual. Tim yang menguasai motion graphics sederhana (lower third, title sequence, transisi animasi) bisa mengangkat kualitas produksi konten mereka ke level yang jauh lebih tinggi tanpa harus memiliki anggaran TV.

---

## 📊 Visualisasi

### Cara Kerja Keyframe: Visual Timeline

```
Timeline (detik):    0:00          0:10          0:20          0:30
                      |             |             |             |
Opacity:             [0%]----interpolasi----[100%]----------[100%]
                      ↑                           ↑
                  Keyframe 1                  Keyframe 2
                  (mulai fade)               (selesai fade)

Nilai antara:    0%  →  25%  →  50%  →  75%  →  100%
                     ← Premiere Pro hitung otomatis →
```

### Kurva Interpolasi: Ease vs Linear

```
Kecepatan
   |
100%|           ___________
   |          /            \
   |        /                \     ← Ease In & Out
   |      /                    \
   |    /                        \
   |  /                            \
  0%|/________________________________\ Waktu
    0:00      0:10      0:20      0:30

vs.

Kecepatan
   |
100%|_________________________________
   |                                  ← Linear (tidak natural)
  0%|_________________________________ Waktu
```

### Anatomi Lower Third: Timing yang Tepat

```
Timeline Klip Wawancara (30 detik):

Video:   [============================== wawancara 30 detik ==============================]
                                                                                           
Lower    0:00 0:05  0:20                                    0:25 0:30
Third:        [in→][====== stay 15 detik ======][ ←out]
         
         ↑         ↑                              ↑
    Narasumber  Lower Third                   Lower Third
    mulai bicara muncul (0.5s                 keluar
                setelah bicara)
```

### Pipeline Produksi Motion Graphics

```
┌────────────────┐    ┌────────────────┐    ┌────────────────┐
│  AFTER EFFECTS │ →  │  Export .mogrt │ →  │  PREMIERE PRO  │
│                │    │                │    │                │
│ • Animasi      │    │ Motion Graphics│    │ • Drag ke      │
│   kompleks     │    │ Template       │    │   Timeline     │
│ • Komposisi    │    │ (editable)     │    │ • Edit teks    │
│   berlayer     │    │                │    │   langsung     │
│ • Visual FX    │    └────────────────┘    │ • Re-use di    │
└────────────────┘                          │   project lain │
                                            └────────────────┘

Atau (lebih sederhana):
┌────────────────┐    ┌────────────────┐
│ ESSENTIAL      │ →  │  PREMIERE PRO  │
│ GRAPHICS       │    │  TIMELINE      │
│                │    │                │
│ • Template     │    │ • Edit & atur  │
│   built-in     │    │   timing       │
│ • Keyframe     │    │ • Render &     │
│   di Premiere  │    │   export       │
└────────────────┘    └────────────────┘
```

---

## ⚠️ Kesalahan Umum

### 1. Lupa Klik Stopwatch = Tidak Ada Keyframe
**Masalah**: Kamu mengubah nilai Position/Scale, tapi tidak mengklik ikon stopwatch terlebih dahulu → perubahan berlaku ke seluruh klip, bukan beranimasi.
**Solusi**: **Selalu klik stopwatch** di sebelah nama properti sebelum mulai membuat keyframe. Ikon stopwatch yang aktif berwarna biru.

### 2. Semua Animasi Menggunakan Interpolasi Linear
**Masalah**: Animasi terlihat kaku, robotik, tidak natural.
**Solusi**: Hampir semua animasi motion graphics profesional menggunakan **Ease In & Out**. Klik kanan pada keyframe → Temporal Interpolation → Ease In/Out, atau tekan `Ctrl+Shift+F9` (Win) / `Cmd+Shift+F9` (Mac).

### 3. Lower Third Terlalu Lama di Layar
**Masalah**: Lower third muncul terlalu lama (lebih dari 6–7 detik) → mengganggu dan terlihat tidak profesional.
**Solusi**: Standar industri adalah **3–5 detik** untuk lower third. Cukup untuk dibaca penonton, tidak terlalu lama.

### 4. Font Terlalu Kecil atau Warna Tidak Kontras
**Masalah**: Teks lower third tidak bisa dibaca karena terlalu kecil atau warnanya mirip background.
**Solusi**: Ukuran font minimal **36–42pt** untuk lower third. Gunakan teks putih dengan background gelap (semi-transparan), atau teks gelap dengan background terang.

### 5. Terlalu Banyak Elemen Bergerak Sekaligus
**Masalah**: Teks slide dari kiri, logo zoom in, background berputar — semuanya sekaligus → penonton tidak tahu harus fokus ke mana.
**Solusi**: Prinsip motion graphics profesional: **satu elemen bergerak pada satu waktu**, atau jika beberapa elemen, pastikan ada **hierarki visual** yang jelas (elemen utama lebih dominan).

### 6. Tidak Menyimpan Template untuk Digunakan Ulang
**Masalah**: Membuat lower third yang bagus, tapi tidak disimpan → harus membuat ulang dari awal di project berikutnya.
**Solusi**: Selalu export desain yang bagus sebagai **Motion Graphics Template (.mogrt)**: Graphics → Export as Motion Graphics Template.

### 7. Mengabaikan Timing dan Ritme
**Masalah**: Animasi teks muncul dan langsung diam — tidak ada "napas" atau timing yang sesuai dengan musik/audio.
**Solusi**: Sinkronkan keyframe animasi dengan **beat musik** atau **jeda bicara** narasumber. Animasi yang "on beat" terasa jauh lebih profesional.

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

Seorang video editor membuat animasi teks yang bergerak dari posisi X:0 ke X:500. Dia menggunakan interpolasi **Linear**. Teman satu timnya menyarankan untuk mengganti ke **Ease In & Out**. Jelaskan:
1. Apa perbedaan visual yang akan terjadi antara Linear dan Ease In & Out?
2. Mengapa Ease In & Out umumnya terlihat lebih profesional dan natural?
3. Dalam kondisi apa kamu mungkin tetap memilih interpolasi Linear?

*(Kunci: Linear = kecepatan konstan, terlihat mekanis/robotik; Ease = mulai lambat, cepat di tengah, lambat di akhir, meniru hukum fisika dunia nyata; Linear cocok untuk animasi data/teknis yang memang ingin terlihat mekanis)*

---

### Soal 2 — Analisis

Kamu diminta menganalisis sebuah konten YouTube dari channel berita ekonomi. Tonton minimal 5 menit konten tersebut dan identifikasi:

1. **Berapa jenis motion graphics** yang digunakan? (lower third, bumper, grafis data, ticker, dsb.)
2. **Durasi** setiap elemen motion graphics (misalnya: lower third bertahan berapa detik?)
3. **Jenis animasi** yang digunakan (fade, slide, scale, kombinasi?)
4. **Penilaian kualitas**: apakah motion graphics tersebut mendukung atau mengganggu konten? Mengapa?

---

### Soal 3 — Praktik

**Tugas**: Buat sebuah **lower third animasi** di Adobe Premiere Pro dengan ketentuan berikut:

1. Nama: [nama kamu sendiri]
2. Jabatan: "Mahasiswa, Universitas [kamu]"
3. Animasi masuk: slide dari kiri (ease out), fade in bersamaan
4. Durasi tampil: 4 detik
5. Animasi keluar: slide ke kiri (ease in), fade out bersamaan
6. Warna teks: putih, background: biru gelap semi-transparan (#1A237E, opacity 80%)
7. Simpan sebagai file .mogrt

**Deliverable**: Screenshot dari Effect Controls yang menampilkan keyframe yang dibuat + file .mogrt hasil export

---

## 📌 Ringkasan

**Cheatsheet Motion Graphics & Keyframe — Pertemuan 12**

**Konsep Dasar**
- `Keyframe` → penanda nilai properti di titik waktu tertentu
- `Interpolasi` → perhitungan otomatis nilai di antara dua keyframe
- `Stopwatch` → harus diklik untuk aktifkan sistem keyframe pada properti

**4 Properti Animasi Utama** (di Effect Controls)
- `Position` → gerakan X, Y
- `Scale` → perubahan ukuran
- `Rotation` → putaran (derajat)
- `Opacity` → transparansi (0–100%)

**Jenis Interpolasi**
- `Linear` → kecepatan konstan, terlihat mekanis
- `Ease Out` → mulai lambat, makin cepat (natural "start")
- `Ease In` → makin lambat di akhir (natural "stop")
- `Ease In & Out` → paling natural, paling sering dipakai

**Lower Third — Standar Industri**
- Muncul: **0.5s** setelah narasumber mulai bicara
- Durasi tampil: **3–5 detik**
- Font minimal: **36–42pt**
- Warna: kontras tinggi (putih di background gelap)

**Pipeline Produksi**
```
After Effects → .mogrt → Premiere Pro
(animasi kompleks)    (edit & assembly)
```

**Shortcut Berguna**
- `Ctrl+Shift+F9` / `Cmd+Shift+F9` → Ease In & Out pada keyframe terpilih
- Klik kanan keyframe → Temporal Interpolation → pilih tipe

**Anti-Gagal**
- Klik **stopwatch dulu** sebelum ubah nilai properti
- Hampir selalu gunakan **Ease In & Out** bukan Linear
- **Satu elemen bergerak** dalam satu waktu — jangan ramai-ramai
- Lower third **maksimal 5 detik** di layar
- Simpan desain bagus sebagai **.mogrt** agar bisa reuse
