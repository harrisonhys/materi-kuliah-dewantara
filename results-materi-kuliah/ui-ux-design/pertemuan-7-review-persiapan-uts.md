# Pertemuan 7: Review & Persiapan UTS — UI/UX Design

---

## 🎯 Learning Outcomes

Setelah pertemuan review ini, kamu akan bisa:

* Merangkum konsep pertemuan 1–6 dalam satu referensi cepat
* Menjawab soal UTS bertipe teori, analisis kasus, dan evaluasi desain
* Mengintegrasikan HCD, Heuristik, Riset, Persona, Journey Map, dan IA dalam satu framework
* Mengidentifikasi kesalahan konseptual yang paling sering muncul

---

## 📖 Pengantar (Hook)

UTS UI/UX bukan tentang hafalan nama-nama heuristik atau langkah-langkah Double Diamond.

UTS ini adalah tentang apakah kamu bisa **berpikir seperti UX designer**: menganalisis masalah, menghubungkan observasi dengan framework, dan merumuskan keputusan desain yang bisa dipertanggungjawabkan.

Pertanyaan yang paling sering keluar: "Berikan satu skenario di mana [konsep X] diterapkan. Bagaimana kamu mendekatinya?"

---

## 🧩 Ringkasan Materi Pertemuan 1–6

### Pertemuan 1: Fondasi UI/UX & HCD

* **UI** = tampilan visual | **UX** = pengalaman keseluruhan
* **HCD** = desain berpusat manusia: Empathy → Define → Ideate → Prototype → Test
* **Double Diamond:** Discover (diverge) → Define (converge) → Develop (diverge) → Deliver (converge)
* UX buruk = biaya bisnis nyata (Healthcare.gov: $500 juta perbaikan)
* Prinsip: jangan skip Discover & Define — desain yang salah arah lebih mahal dari tidak desain sama sekali

### Pertemuan 2: Heuristik Nielsen

| # | Heuristik | Contoh Pelanggaran |
|---|---|---|
| 1 | Visibility of System Status | Tidak ada loading indicator setelah klik |
| 2 | Match System & Real World | Error "NullPointerException" |
| 3 | User Control & Freedom | Tidak ada tombol Back/Cancel |
| 4 | Consistency & Standards | "Kirim" di halaman A, "Submit" di halaman B |
| 5 | Error Prevention | Tidak ada validasi format sebelum submit |
| 6 | Recognition over Recall | Tidak ada autocomplete / recent |
| 7 | Flexibility & Efficiency | Tidak ada shortcut untuk pengguna advanced |
| 8 | Aesthetic & Minimalist | Terlalu banyak elemen di satu halaman |
| 9 | Error Recovery | "Transaksi gagal." tanpa penjelasan |
| 10 | Help & Documentation | Tidak ada FAQ atau tooltip |

**Severity Rating:** 0 (bukan masalah) → 4 (catastrophic, wajib perbaiki sebelum launch)

### Pertemuan 3: User Research

* **Kualitatif** (wawancara) = mengapa, motivasi, insight mendalam
* **Kuantitatif** (survei, analytics) = berapa banyak, validasi skala luas
* **Wawancara Semi-Terstruktur** = discussion guide + freedom to probe
* 5 Aturan Emas: masa lalu bukan masa depan, no leading questions, diam adalah emas, probing bukan menghakimi, behavior bukan opini
* **Affinity Diagram** = kelompokkan data kualitatif jadi tema dan insight
* Ingat: 1 jam riset = hemat berminggu-minggu pengembangan yang sia-sia

### Pertemuan 4: User Persona & Empathy Map

| Komponen | Isi |
|---|---|
| **Empathy Map** | Thinks/Feels, Sees, Hears, Says/Does, Pains, Gains |
| **User Persona** | Foto, nama, demografis, quote, goals, frustrations, konteks |
| **JTBD** | "Ketika [situasi], saya ingin [motivasi], sehingga [outcome]" |

* Research Persona (data nyata) vs Proto-Persona (asumsi yang perlu divalidasi)
* Maksimal 2-3 persona per produk
* Persona hanya berguna jika dipakai aktif dalam keputusan desain

### Pertemuan 5: User Journey Map & HMW

* **Journey Map** = visualisasi perjalanan pengguna end-to-end, bukan hanya di dalam app
* 6 lanes: Actions → Thoughts → Emotions (kurva) → Touchpoints → Pain Points → Opportunities
* **Emotional Curve:** titik terendah = prioritas perbaikan utama
* **HMW Statement:** tidak terlalu luas, tidak terlalu sempit, membuka eksplorasi
  * Format: "Bagaimana kita bisa [solusi] sehingga [pengguna] dapat [outcome]?"

### Pertemuan 6: Information Architecture

* **IA** = mengorganisir, melabeli, menghubungkan konten untuk findability
* **4 Komponen:** Organization → Labeling → Navigation → Search
* **Card Sorting:** Open (pengguna buat kategori) vs Closed (pengguna cocokkan ke kategori yang ada)
* **Sitemap** = hierarki visual konten, dari high-level ke detail
* Flat IA (≤3 klik) > Deep IA
* Tab Bar > Hamburger Menu untuk fitur yang sering dipakai

---

## 🧠 Framework Terintegrasi: Alur Proses Desain UX

```
DISCOVER                    DEFINE
┌──────────────────┐    ┌──────────────────────┐
│ User Research    │    │ Affinity Diagram      │
│ • Wawancara      │───▶│ → Key Insights        │
│ • Observasi      │    │ → User Persona        │
│ • Survei         │    │ → User Journey Map    │
│ • Contextual     │    │ → HMW Statements      │
│   Inquiry        │    │ → Problem Statement   │
└──────────────────┘    └──────────────────────┘
                                    │
                                    ▼
DELIVER                     DEVELOP
┌──────────────────┐    ┌──────────────────────┐
│ Usability Testing│    │ Information Arch.     │
│ → SUS Score      │◀───│ → Sitemap             │
│ → Iteration      │    │ → User Flow           │
│ → Before/After   │    │ → Wireframe Lo-Fi     │
│                  │    │ → Prototype Hi-Fi     │
└──────────────────┘    │ → Design System       │
                        └──────────────────────┘
```

---

## 💻 Soal Latihan Tipe UTS

### Tipe 1: Identifikasi Heuristik

**Contoh Soal:**
Aplikasi transfer uang menampilkan pesan: "Error 404. Please try again." setelah pengguna gagal transfer.

*Identifikasi heuristik apa yang dilanggar, berikan severity rating, dan rekomendasikan perbaikan.*

**Jawaban Model:**
* Heuristik #9 (Error Recovery) — pesan tidak menjelaskan masalah, tidak ada panduan solusi
* Heuristik #2 (Match Real World) — "Error 404" adalah jargon teknis
* Severity: 3 (Major) — menyebabkan kebingungan dan ketidakpercayaan pengguna
* Rekomendasi: "Transfer gagal karena koneksi internet terputus. Coba lagi atau hubungi CS kami."

### Tipe 2: Analisis Journey Map

**Contoh Soal:**
Gambarkan Journey Map untuk skenario "Pengguna baru yang ingin top-up GoPay untuk pertama kali." Identifikasi minimal 3 pain point dan 3 HMW statement.

**Kerangka Jawaban:**
1. Tentukan persona (siapa pengguna baru? usia, tech-savviness)
2. Identifikasi 5-6 fase: Download → Register → Verifikasi → Top-up → Berhasil
3. Untuk setiap fase: apa yang mereka lakukan, pikirkan, rasakan?
4. Pain points: scan KTP gagal, email verif masuk spam, bingung pilih metode top-up
5. HMW: dari setiap pain point

### Tipe 3: Justifikasi Keputusan Desain

**Contoh Soal:**
Kamu merancang aplikasi e-commerce untuk ibu rumah tangga 35-50 tahun. Apakah kamu akan menggunakan Tab Bar atau Hamburger Menu? Justifikasi dengan framework HCD.

**Kerangka Jawaban:**
1. Sebutkan target pengguna dan karakteristik relevan
2. Hubungkan dengan heuristik: #6 (Recognition), #7 (Flexibility & Efficiency)
3. Pertimbangan: tab bar lebih discoverable untuk pengguna yang tidak familiar dengan hidden menu
4. Keputusan + justifikasi berbasis data/riset

---

## 🏢 Studi Kasus Review: Evaluasi Fitur OVO Pay Later

Analisis ini mengintegrasikan semua konsep pertemuan 1-6.

**Konteks:** OVO meluncurkan fitur Pay Later (cicilan tanpa kartu kredit).

**Temuan dari Heuristic Evaluation:**
* H#1: Limit kredit yang tersisa tidak terlihat jelas di halaman utama (Severity 3)
* H#5: Tidak ada konfirmasi eksplisit total cicilan sebelum checkout (Severity 4)
* H#8: Halaman detail transaksi terlalu penuh informasi (Severity 2)

**Temuan dari User Research:**
* Pain point: pengguna tidak yakin berapa yang sudah mereka "pakai" dari limit
* Mental model: "Saya pikir Pay Later seperti rekening terpisah, ternyata memengaruhi saldo utama"
* JTBD: "Ketika mau belanja online, saya ingin tahu berapa limit tersisa, agar saya tidak terkejut ditagih"

**Persona yang Teridentifikasi:**
* "Budi" (27 thn, fresh grad) — sering pakai Pay Later untuk cicilan gadget
* "Ibu Nina" (41 thn) — coba Pay Later tapi takut denda keterlambatan

**HMW dari Journey Map:**
* "HMW membuat pengguna selalu aware dengan sisa limit Pay Later mereka?"
* "HMW menjelaskan konsekuensi keterlambatan dengan cara yang tidak menakutkan?"

**Solusi IA:**
* Tambahkan widget "Sisa Limit" di home screen (visible tanpa navigasi)
* Onboarding khusus Pay Later: FAQ tentang cara kerja dan konsekuensi

---

## ⚠️ Kesalahan Umum di UTS

1. **Menjawab heuristik dengan nama saja** → Sebutkan nomor + nama + contoh spesifik dari soal.

2. **Journey Map tanpa kurva emosi** → Komponen ini wajib dan sering ditanyakan.

3. **HMW yang terlalu spesifik atau terlalu umum** → "HMW menambahkan loading spinner?" = terlalu spesifik. "HMW membuat app lebih baik?" = terlalu umum.

4. **Persona tanpa JTBD** → Sertakan minimal 1 JTBD per persona untuk menunjukkan pemahaman motivasi mendalam.

5. **Tidak menyebutkan metode riset** → Saat bicara persona atau journey map, selalu sebutkan bahwa ini berdasarkan riset (wawancara, observasi, survei).

---

## 🧪 Soal Latihan UTS

### Bagian A: Teori (25 poin)

**1.** Jelaskan perbedaan antara UI dan UX dengan menggunakan analogi. Berikan satu contoh produk digital di mana UI-nya bagus tapi UX-nya buruk.

**2.** Sebutkan dan jelaskan 5 dari 10 Heuristik Nielsen. Untuk setiap heuristik, berikan satu contoh pelanggaran di aplikasi mobile.

**3.** Jelaskan perbedaan antara kualitatif dan kuantitatif research. Kapan kamu menggunakan masing-masing?

**4.** Apa itu Affinity Diagram? Jelaskan proses membuatnya dari data wawancara mentah.

**5.** Jelaskan perbedaan Flat IA vs Deep IA beserta implikasinya terhadap usability.

### Bagian B: Analisis Kasus (35 poin)

**6.** (15 poin) Lakukan Heuristic Evaluation singkat pada skenario berikut:

Aplikasi pembayaran tagihan: ketika pengguna memasukkan nomor pelanggan yang salah dan menekan "Cek Tagihan", sistem membutuhkan waktu 5 detik tanpa indikator loading, lalu menampilkan "Tagihan tidak ditemukan" tanpa penjelasan langkah selanjutnya. Semua tombol menggunakan label berbeda untuk aksi yang sama di halaman berbeda (Bayar, Lanjutkan, Proses, Submit).

Identifikasi semua heuristik yang dilanggar, berikan severity rating, dan rekomendasikan perbaikan.

**7.** (20 poin) Kamu melakukan riset untuk redesign aplikasi pembayaran jalan tol berbasis mobile. Dari 10 wawancara, kamu menemukan:
* "Saya tidak bisa bayar kalau sinyal lemah di jalan tol"
* "Saya tidak tahu apakah ada saldo cukup untuk perjalanan ini"
* "Kadang saya lupa top-up, tiba-tiba kena denda"

Berdasarkan data ini:
a) Buat Empathy Map sederhana untuk persona utama (gambar boleh kasar/teks saja)
b) Identifikasi 3 pain point dari quotes di atas
c) Rumuskan 3 HMW statement dari pain points tersebut

### Bagian C: Desain & Justifikasi (40 poin)

**8.** (20 poin) Buatlah outline Sitemap untuk aplikasi dompet digital dengan minimal 20 halaman/fitur. Jelaskan mengapa kamu mengelompokkan konten seperti itu (justifikasi berdasarkan mental model pengguna).

**9.** (20 poin) Sebuah startup fintech meminta kamu merancang onboarding untuk pengguna baru yang belum pernah pakai dompet digital. Target pengguna: ibu rumah tangga 40-55 tahun, kota tier 2-3, tech savvy rendah.

Jelaskan secara detail pendekatan desain kamu, dengan menyebutkan:
a) Metode riset yang akan kamu lakukan sebelum desain
b) Poin-poin yang akan kamu masukkan dalam User Persona
c) Pain point yang paling mungkin terjadi dalam onboarding
d) Keputusan IA yang kamu ambil dan alasannya

---

## 📌 Checklist Pre-UTS

- [ ] Hafal dan bisa jelaskan 10 Heuristik Nielsen dengan contoh masing-masing
- [ ] Bisa bedakan UI vs UX dengan analogi yang jelas
- [ ] Paham alur Double Diamond: Discover → Define → Develop → Deliver
- [ ] Bisa buat Empathy Map dari data riset
- [ ] Paham perbedaan Research Persona vs Proto-Persona
- [ ] Bisa rumuskan HMW Statement yang tepat (tidak terlalu luas/sempit)
- [ ] Paham Open vs Closed Card Sorting
- [ ] Bisa buat outline Sitemap dengan hierarki yang logis
- [ ] Sudah latihan soal analisis kasus di atas minimal sekali

---

*📚 Referensi: Norman, D.A. (2013). The Design of Everyday Things | Krug, S. (2014). Don't Make Me Think | Garrett, J.J. (2010). Elements of User Experience | Nielsen, J. 10 Usability Heuristics*
