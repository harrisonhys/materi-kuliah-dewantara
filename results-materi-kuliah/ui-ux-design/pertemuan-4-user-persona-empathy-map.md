# Pertemuan 4: User Persona & Empathy Map

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Membuat Empathy Map berdasarkan data riset pengguna
* Membuat User Persona yang akurat dengan data demografis, goals, dan frustrations
* Menghubungkan Persona dengan temuan dari User Research
* Menggunakan Persona sebagai panduan keputusan desain

---

## 📖 Pengantar (Hook)

"Desain untuk semua orang" = desain yang tidak optimal untuk siapa pun.

Ketika tim produk berargumen "Tapi pengguna tipe A akan suka ini!" vs "Tidak, pengguna tipe B yang lebih banyak!" — perdebatan ini bisa berlangsung berjam-jam tanpa resolusi.

User Persona mengakhiri debat ini. Dengan Persona bernama, berwajah, dan punya cerita hidup, tim produk bisa bertanya: **"Apakah Sari, ibu rumah tangga 34 tahun di Surabaya, akan paham fitur ini?"**

Bukan abstraksi. Bukan asumsi. Tapi representasi nyata dari pengguna kamu.

---

## 🧩 Konsep Utama

### Empathy Map

Empathy Map adalah tools untuk memvisualisasikan apa yang pengguna pikirkan, rasakan, lakukan, dan dengar dalam konteks tertentu. Dibuat dari data riset (wawancara, observasi), bukan asumsi.

**4 Kuadran Empathy Map:**

```
                    THINKS & FEELS
                    (Pikiran & Perasaan)
                    "Semoga transfernya berhasil"
                    "Takut salah klik"
                         ↑
                         │
HEARS               ─────┼─────           SEES
(Mendengar)              │               (Melihat)
"Teman bilang OVO      ←─┤─→           Teman bayar pakai
lebih murah biayanya"    │             QR di kasir warung
                         │
                         ↓
                    SAYS & DOES
                    (Berkata & Melakukan)
                    Mencoba semua menu sebelum
                    menemukan yang dicari
                    "Ini tombolnya mana ya?"
```

**Tambahan:** Di bagian bawah, tambahkan:
* **PAINS** — hambatan, kekhawatiran, frustrasi
* **GAINS** — keinginan, harapan, apa yang ingin dicapai

### User Persona

Persona adalah karakter fiktif yang dibuat berdasarkan data riset nyata, merepresentasikan segmen pengguna yang signifikan.

**Komponen User Persona:**

```
┌─────────────────────────────────────────────────────┐
│  [FOTO]   SARI WULANDARI                            │
│           32 tahun | Surabaya                       │
│           Ibu Rumah Tangga + Penjual Online         │
│           "Saya mau bayar semua serba mudah,         │
│            tidak perlu antri di bank"               │
├──────────────────────┬──────────────────────────────┤
│ PROFIL               │ GOALS                        │
│                      │ • Bayar tagihan tanpa antri  │
│ Pendidikan: SMA      │ • Terima pembayaran dari     │
│ Pendapatan: 4-6 jt/bln│  pembeli online             │
│ Tech: Pengguna medium│ • Tabung uang dengan mudah   │
│ Smartphone: Samsung  │                              │
│ App: WhatsApp, Shopee│ FRUSTRATIONS                 │
│        GoPay, TikTok │ • Error yang tidak jelas     │
│                      │ • Terlalu banyak langkah     │
│ KONTEKS              │ • Takut salah transfer       │
│ Belanja online 3x/mgg│ • Notifikasi terlalu banyak  │
│ Terima pembayaran    │                              │
│ via transfer + COD   │ MOTIVASI                     │
│ Sering di rumah,     │ Ingin usahanya tumbuh sambil │
│ sinyal kadang lemah  │ tetap bisa urus keluarga     │
└──────────────────────┴──────────────────────────────┘
```

### Research Persona vs Proto-Persona

| | Research Persona | Proto-Persona |
|---|---|---|
| **Berdasarkan** | Data riset nyata (wawancara, survei) | Asumsi tim produk |
| **Validitas** | Tinggi — mewakili pengguna nyata | Rendah — perlu divalidasi |
| **Kapan dipakai** | Setelah melakukan user research | Di awal proyek, sebelum riset |
| **Risiko** | Membutuhkan waktu dan sumber daya riset | Bisa mislead jika asumsi salah |

**Aturan:** Proto-persona oke untuk memulai, tapi harus segera divalidasi dengan riset nyata.

### Jobs-to-be-Done Framework

Selain persona tradisional, framework JTBD membantu memahami **motivasi mendalam** di balik tindakan pengguna.

Format: "Ketika [situasi], saya ingin [motivasi], sehingga [hasil yang diinginkan]"

**Contoh:**
* "Ketika **saya ingin bayar di kasir**, saya ingin **tidak repot buka-buka tas cari dompet**, sehingga **saya tidak memperlambat antrian**"
* "Ketika **gaji masuk**, saya ingin **langsung bayar semua tagihan**, sehingga **saya tidak khawatir lupa bayar dan kena denda**"

JTBD membantu tim fokus pada **pekerjaan yang ingin diselesaikan pengguna**, bukan hanya fitur yang diminta.

---

## 🧠 Ilustrasi / Analogi

**Persona seperti karakter dalam novel:**
* Karakter yang baik punya nama, latar belakang, motivasi, dan konflik
* Penulis selalu bertanya: "Apa yang akan dilakukan karakter INI dalam situasi ini?"
* UI/UX designer bertanya: "Apakah Sari (persona) akan paham navigasi ini?"

**Empathy Map seperti kacamata pengguna:**
* Memakai "kacamata Sari" → melihat produk dari perspektifnya
* Memaksa kita keluar dari perspektif kita sendiri (yang sudah terlalu familiar)

---

## 💻 Praktik: Membangun Persona di Figma

### Template Persona (Bisa Dibuat di Figma/FigJam)

```
LAYOUT PERSONA DI FIGMA:

Frame ukuran: 800 x 600px

[Kolom Kiri - 250px]
- Foto profil (dari Unsplash / Generated photos)
- Nama + Umur
- Lokasi + Pekerjaan
- Quote khas (dalam tanda kutip, font italic)

[Kolom Kanan - 500px]
- Profil (grid 2 kolom):
  - Tech savviness (1-5 bar)
  - Pendapatan
  - Pendidikan
  - Perangkat yang dipakai
  - Aplikasi favorit

- Goals (bullet list dengan ikon ✓)
- Frustrations (bullet list dengan ikon ✗)
- Konteks Penggunaan (narasi singkat)
- Motivasi Utama (Jobs-to-be-Done)

TIPS FIGMA:
- Gunakan Frame + Auto Layout untuk konsistensi
- Buat komponen reusable untuk setiap section
- Gunakan warna yang berbeda untuk setiap persona
  (misal: Persona 1 = biru, Persona 2 = oranye)
```

### Cara Derive Persona dari Data Riset

```
DATA RISET (dari 15 wawancara):
├── 8 responden: usia 25-35, pengguna aktif setiap hari
│   ├── Semua punya usaha sampingan online
│   ├── Pain point utama: waktu
│   └── Goal: efisiensi, tidak perlu keluar rumah
│
└── 7 responden: usia 40-55, pengguna ocasional
    ├── Kebanyakan karyawan kantoran
    ├── Pain point utama: kepercayaan dan keamanan
    └── Goal: aman, tidak takut ditipu

DERIVASI PERSONA:
Persona 1 (dari kluster atas): "Sari" — UMKM online, efisiensi-oriented
Persona 2 (dari kluster bawah): "Pak Budi" — karyawan, security-oriented

Kedua persona mewakili segmen pengguna yang berbeda
dan membutuhkan pendekatan desain yang berbeda pula.
```

---

## 🏢 Studi Kasus Nyata: Persona untuk Redesign Fitur Investasi DANA

**Konteks:** DANA ingin meningkatkan adopsi fitur DANA+ (investasi reksa dana).

**Proses Riset:**
* 25 user interview, segmentasi berdasarkan perilaku finansial
* 400 survei pengguna aktif DANA

**Dua Persona Utama:**

**Persona 1: "Risa" — Si Penabung Baru**
* 23 tahun, fresh graduate, gaji pertama
* Goals: punya tabungan, tidak mau ribet
* Frustrations: tidak paham istilah investasi, takut rugi
* JTBD: "Ketika terima gaji, saya ingin uang sisa otomatis 'bekerja', sehingga saya tidak sadar menabung"

**Persona 2: "Mas Doni" — Si Investor Aktif**
* 31 tahun, sudah investasi 2+ tahun
* Goals: diversifikasi portofolio, kontrol penuh
* Frustrations: fitur terlalu sederhana, tidak ada data historis yang detail
* JTBD: "Ketika memilih reksa dana, saya ingin lihat kinerja 5 tahun terakhir, sehingga saya bisa membuat keputusan berdasarkan data"

**Keputusan Desain:**
* Onboarding dual-path: "Mulai dengan mudah" (untuk Risa) vs "Saya sudah tahu investasi" (untuk Mas Doni)
* Halaman utama: pilihan antara tampilan simplified vs advanced
* Terminologi: glossary mini di samping setiap istilah teknis (untuk Risa), bisa dimatikan (untuk Mas Doni)

---

## ⚠️ Kesalahan Umum

1. **Persona terlalu banyak** → Maksimal 2-3 persona per produk. Lebih dari itu tidak fokus dan sulit dipakai dalam keputusan desain.

2. **Persona tanpa data** → "Kita bayangkan pengguna kita adalah..." bukan persona berbasis data. Ini adalah proto-persona atau asumsi.

3. **Persona tidak pernah dipakai** → Persona yang hanya hidup di dokumen tidak bermanfaat. Tempel di dinding ruang kerja tim. Referensikan saat rapat desain.

4. **Foto tidak representatif** → Gunakan foto yang sesuai dengan target demografis nyata. Hindari foto stok yang terlalu "sempurna" — pengguna nyata lebih beragam.

5. **Goals yang terlalu umum** → "Mau hidup nyaman" bukan goal yang berguna. "Bisa bayar semua tagihan sebelum tanggal 5 setiap bulan tanpa perlu ingat satu per satu" adalah goal yang spesifik dan actionable.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Apa perbedaan antara Research Persona dan Proto-Persona? Dalam kondisi apa kamu akan menggunakan masing-masing?

b) Jelaskan Jobs-to-be-Done framework. Berikan dua contoh JTBD statement untuk pengguna aplikasi transfer uang.

### Soal 2 — Praktik (Tugas)

Berdasarkan hasil User Research dari Pertemuan 3:
1. Buat **Empathy Map** untuk pengguna utama (FigJam atau kertas)
2. Buat **2 User Persona** lengkap di Figma:
   * Foto, nama, usia, lokasi, pekerjaan
   * Quote khas
   * Goals (3-5 poin)
   * Frustrations (3-5 poin)
   * Tech profile dan konteks penggunaan
   * Minimal 1 JTBD statement per persona
3. Tulis **satu paragraf** menjelaskan bagaimana setiap persona akan mempengaruhi keputusan desain proyek kamu

---

## 📌 Ringkasan

* **Empathy Map** = visualisasi pikiran, perasaan, tindakan, dan konteks pengguna — dari data riset
* **User Persona** = karakter fiktif berbasis data nyata yang merepresentasikan segmen pengguna
* Komponen wajib persona: foto, nama, demographics, goals, frustrations, konteks, motivasi
* **Research Persona** = berbasis data riset (akurat) vs **Proto-Persona** = berbasis asumsi (perlu divalidasi)
* **Jobs-to-be-Done** = fokus pada motivasi mendalam: "Ketika [situasi], saya ingin [aksi], sehingga [hasil]"
* Persona berguna hanya jika dipakai aktif — bukan hanya dokumen yang disimpan
* Maksimal 2-3 persona per produk untuk menjaga fokus

---

*📚 Referensi: Garrett, J.J. (2010). The Elements of User Experience | UX Collective — Creating Effective Personas | Nielsen Norman Group — Personas*
