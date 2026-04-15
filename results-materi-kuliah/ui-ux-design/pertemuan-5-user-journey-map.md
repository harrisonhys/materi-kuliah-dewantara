# Pertemuan 5: User Journey Map & Pain Points

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Mendefinisikan skenario dan tahapan perjalanan pengguna
* Membuat User Journey Map yang mencakup touchpoints, actions, thoughts, emotions
* Mengidentifikasi pain points dan moments of delight
* Merumuskan How Might We (HMW) statements sebagai peluang desain

---

## 📖 Pengantar (Hook)

Kamu ingin mentransfer uang untuk bayar kos. Proses ini tidak dimulai saat kamu buka aplikasi — tapi saat kamu ingat besok tanggal 1 dan mulai khawatir.

Dan prosesnya tidak berakhir saat kamu klik "Kirim" — tapi saat kamu yakin uangnya sudah diterima, atau sampai kamu telepon pemilik kos menanyakan konfirmasi.

**User Journey Map** menangkap **keseluruhan** pengalaman ini — bukan hanya momen di dalam aplikasi. Hasilnya sering mengungkap masalah yang tidak pernah terpikirkan oleh tim desain.

---

## 🧩 Konsep Utama

### Apa itu User Journey Map?

User Journey Map adalah visualisasi langkah-langkah yang dilakukan pengguna untuk mencapai tujuan tertentu, lengkap dengan emosi, pikiran, dan titik-titik friksi di setiap langkah.

**Empat komponen utama:**

1. **Actor** — Siapa yang melakukan perjalanan (biasanya persona spesifik)
2. **Scenario** — Skenario apa yang dijalani, dan apa goal-nya
3. **Journey Phases** — Tahapan dari awal hingga akhir
4. **Lanes** (baris dalam tabel):
   * **Actions** — Apa yang dilakukan pengguna
   * **Thoughts** — Apa yang pengguna pikirkan (dari data wawancara)
   * **Emotions** — Bagaimana perasaan pengguna (kurva emosi)
   * **Touchpoints** — Di mana interaksi terjadi (app, website, CS, notif)
   * **Pain Points** — Friksi, masalah, kekhawatiran
   * **Opportunities** — Peluang desain untuk improve

### Struktur Journey Map

```
PERSONA:  Sari (Penjual Online, 32 thn)
SKENARIO: Menerima pembayaran dari pembeli dan mencatat di hari yang sama
GOAL:     Memastikan pembayaran masuk dan dicatat tanpa perlu buka laptop

┌──────────┬────────────┬──────────────┬────────────┬────────────┐
│  FASE    │ SADAR ADA  │  CARI CARA   │ KONFIRMASI │  SELESAI   │
│          │ PEMBAYARAN │  CEPAT       │ PEMBAYARAN │  & CATAT   │
├──────────┼────────────┼──────────────┼────────────┼────────────┤
│ ACTIONS  │Dapat WA    │Buka app,     │Cek riwayat │Screenshot  │
│          │dari pembeli│cari menu     │transaksi   │dan kirim ke│
│          │"sudah TF"  │mutasi        │            │pembeli     │
├──────────┼────────────┼──────────────┼────────────┼────────────┤
│ THOUGHTS │"Sudah masuk│"Ini menunya  │"Kok belum  │"Alhamdulil-│
│          │belum ya?"  │mana ya?"     │ada ya?"    │lah masuk"  │
├──────────┼────────────┼──────────────┼────────────┼────────────┤
│ EMOTIONS │  😐 Netral │  😕 Bingung  │ 😟 Khawatir│  😊 Lega  │
│ (kurva)  │     ──     │     ↘        │    ↘↘      │     ↗↗    │
├──────────┼────────────┼──────────────┼────────────┼────────────┤
│TOUCHPOINT│ WhatsApp   │  GoPay App   │  GoPay App │  WhatsApp  │
│          │            │  Notifikasi  │  Riwayat   │  kamera HP │
├──────────┼────────────┼──────────────┼────────────┼────────────┤
│  PAIN    │Tidak ada   │Menu mutasi   │Delay notif │Tidak bisa  │
│  POINTS  │notif saat  │tersembunyi   │tidak konsisten│export   │
│          │uang masuk  │di dalam menu │            │otomatis    │
├──────────┼────────────┼──────────────┼────────────┼────────────┤
│OPPORTUNIT│Push notif  │Shortcut      │Real-time   │Fitur       │
│          │saat uang   │"Terima Uang" │refresh     │export CSV/ │
│          │masuk       │di home screen│mutasi      │laporan harian│
└──────────┴────────────┴──────────────┴────────────┴────────────┘
```

### Emotional Curve (Kurva Emosi)

Bagian terpenting dari Journey Map adalah kurva emosi — visualisasi naik-turunnya perasaan pengguna sepanjang perjalanan.

```
EMOSI
  +5 ┤                                          ●  Sangat Puas
  +4 ┤                          ●──────────────
  +3 ┤  ●──────●
  +2 ┤
  +1 ┤
   0 ┤
  -1 ┤         ↘
  -2 ┤              ●
  -3 ┤                  ↘
  -4 ┤                       ●  ← PAIN POINT TERDALAM
  -5 ┤                          Sangat Frustrasi
     └──────────────────────────────────────────
      Fase 1   Fase 2   Fase 3   Fase 4   Fase 5

Titik terendah (pain point terdalam) = prioritas perbaikan utama
```

### How Might We (HMW) Statements

Setiap pain point yang ditemukan di Journey Map diubah menjadi peluang desain menggunakan format HMW.

**Format:** "Bagaimana kita bisa [solusi] sehingga [pengguna] dapat [outcome]?"

**Aturan penting:**
* HMW tidak terlalu luas: "Bagaimana kita bisa membuat aplikasi lebih baik?" → terlalu umum
* HMW tidak terlalu sempit: "Bagaimana kita bisa tambahkan tombol biru di halaman 3?" → terlalu spesifik
* HMW harus membuka ruang eksplorasi: banyak solusi mungkin untuk satu HMW

**Contoh dari Journey Map Sari:**

| Pain Point | HMW Statement |
|---|---|
| Tidak ada notifikasi saat uang masuk | HMW beri tahu merchant secara instan saat pembayaran masuk agar mereka tidak perlu aktif mengecek? |
| Menu mutasi tersembunyi | HMW membuat akses ke riwayat penerimaan lebih cepat untuk merchant aktif? |
| Tidak bisa export laporan | HMW membantu penjual kecil mencatat penerimaan harian tanpa effort tambahan? |

---

## 🧠 Ilustrasi / Analogi

**Journey Map seperti peta wisata:**
* Kamu tidak hanya peduli dengan hotel (destination) — tapi perjalanan dari rumah ke bandara, check-in, transit, dst.
* Setiap momen dalam perjalanan mempengaruhi pengalaman keseluruhan
* Masalah di bagian tengah perjalanan bisa merusak keseluruhan experience meskipun destinasinya bagus

**HMW seperti brainstorm brief:**
* Brief yang terlalu spesifik membunuh kreativitas
* Brief yang terlalu umum tidak memberikan arah
* HMW yang baik = brief yang tepat untuk sesi ideasi

---

## 💻 Praktik: Membuat Journey Map di FigJam

### Template FigJam untuk Journey Map

```
SETUP DI FIGJAM:

1. Buat tabel dengan sticky note warna berbeda per lane:
   - Actions    = sticky note warna biru
   - Thoughts   = sticky note warna kuning
   - Emotions   = sticky note warna pink + connector line
   - Touchpoints= ikon atau teks
   - Pain Points= sticky note warna merah
   - Opportunities= sticky note warna hijau

2. Tambahkan gambar kurva emosi:
   - Gunakan drawing tool FigJam
   - Titik di atas garis = emosi positif
   - Titik di bawah garis = emosi negatif

3. Kolaborasi:
   - Share link FigJam ke anggota tim
   - Setiap orang bisa tambah/edit sticky notes
   - Gunakan cursor tracking untuk kolaborasi real-time

ALTERNATIF: Miro, Google Slides, atau bahkan kertas fisik
dengan sticky notes berwarna
```

### Cara Derive Tahapan dari Data Riset

Tahapan Journey Map tidak boleh dibuat-buat — harus berdasarkan data wawancara.

```
DATA WAWANCARA SARI:
"Saya biasanya dapat WA dulu dari pembeli bilang sudah transfer..."
"Terus saya buka GoPay, tapi agak lama nemu menu mutasinya..."
"Kalau belum masuk, saya refresh beberapa kali..."
"Setelah masuk, saya screenshot terus kirim ke pembeli buat konfirmasi..."
"Setelah itu saya catat di buku atau WA group..." 

↓ DERIVE ↓

FASE 1: Terima Notifikasi / Info Transfer
FASE 2: Cek Mutasi di App
FASE 3: Tunggu Konfirmasi Masuk
FASE 4: Konfirmasi ke Pembeli
FASE 5: Catat Penerimaan
```

---

## 🏢 Studi Kasus Nyata: Journey Map untuk Onboarding GoPay

**Skenario:** Pengguna baru mendaftar GoPay dan melakukan transaksi pertama.

**Temuan Journey Map:**
* **Phase 1 (Download):** Emosi positif — antusias, mudah
* **Phase 2 (Registrasi):** Drop pertama — KTP scan sering gagal di pencahayaan kurang
* **Phase 3 (Verifikasi Email):** Frustrasi — email verifikasi sering masuk spam
* **Phase 4 (Top-up Pertama):** Kebingungan — terlalu banyak pilihan metode top-up
* **Phase 5 (Transaksi Pertama):** Puncak emosi positif — berhasil dan senang

**HMW dari Pain Points:**
* "HMW membuat proses scan KTP berhasil di kondisi pencahayaan minim?"
* "HMW memastikan pengguna baru menerima email verifikasi dengan cepat?"
* "HMW menyederhanakan pilihan top-up untuk pengguna pertama kali?"

**Keputusan Desain Berdasarkan HMW:**
* Tambahkan panduan foto KTP real-time (ikon matahari jika pencahayaan kurang)
* Push notifikasi "Cek email kamu" + deep link langsung ke mail app
* Simplifikasi pilihan top-up untuk pengguna baru: tampilkan 3 opsi paling populer saja

---

## ⚠️ Kesalahan Umum

1. **Journey Map dibuat berdasarkan asumsi** → Semua konten Journey Map harus berasal dari data riset nyata (quotes dari wawancara, perilaku dari observasi).

2. **Mengabaikan touchpoint di luar produk** → WhatsApp, telepon CS, dan notifikasi SMS adalah bagian dari journey. Jangan hanya fokus di dalam aplikasi.

3. **Kurva emosi tidak berdasarkan data** → Emosi harus berdasarkan apa yang pengguna katakan dalam wawancara ("Saya frustrasi saat...") bukan apa yang kita kira mereka rasakan.

4. **Terlalu banyak lane** → Journey Map yang terlalu kompleks tidak bisa dibaca. Pilih 4-5 lane yang paling relevan dengan tujuan riset.

5. **HMW yang terlalu solusi-spesifik** → "HMW menambahkan loading spinner?" bukan HMW — itu sudah jadi solusi. HMW harus terbuka untuk banyak solusi.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan antara User Journey Map dan User Flow. Kapan kamu membutuhkan masing-masing dalam proses desain?

b) Mengapa penting untuk memasukkan touchpoints di luar aplikasi (seperti WhatsApp atau telepon) dalam Journey Map? Berikan contoh konkret.

### Soal 2 — Praktik (Tugas)

Berdasarkan Persona yang sudah dibuat di Pertemuan 4:
1. Pilih **1 skenario utama** untuk salah satu persona
2. Identifikasi minimal **5 fase** dalam perjalanan pengguna
3. Buat **Journey Map lengkap** di FigJam atau Miro dengan semua lane:
   * Actions, Thoughts, Emotions (dengan kurva), Touchpoints, Pain Points, Opportunities
4. Rumuskan minimal **5 HMW statements** dari pain points yang ditemukan
5. Tandai **3 pain point prioritas** untuk dijadikan fokus desain

---

## 📌 Ringkasan

* **User Journey Map** = visualisasi perjalanan pengguna dari awal hingga akhir, lengkap dengan emosi
* 6 lane utama: Actions, Thoughts, Emotions, Touchpoints, Pain Points, Opportunities
* **Kurva emosi** = visualisasi naik-turun perasaan — titik terendah = prioritas perbaikan
* Journey dimulai SEBELUM dan berakhir SESUDAH interaksi dengan produk
* **HMW statements** = ubah pain point jadi peluang desain yang terbuka untuk eksplorasi
* Format HMW yang baik: tidak terlalu luas, tidak terlalu sempit, membuka ruang kreatif
* Seluruh konten Journey Map harus berdasarkan data riset — bukan asumsi

---

*📚 Referensi: Garrett, J.J. (2010). The Elements of User Experience | Nielsen Norman Group — Journey Maps | UX Collective — How Might We*
