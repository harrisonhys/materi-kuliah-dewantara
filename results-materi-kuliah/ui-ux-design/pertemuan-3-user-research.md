# Pertemuan 3: User Research — Wawancara, Observasi & Survei

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Merancang discussion guide untuk wawancara semi-terstruktur
* Melakukan wawancara pengguna dengan teknik probing yang efektif
* Membuat kuesioner survei digital yang valid
* Menganalisis data kualitatif menggunakan Affinity Diagram

---

## 📖 Pengantar (Hook)

Tim produk sebuah startup fintech yakin bahwa pengguna mereka tidak pakai fitur investasi karena "UI-nya tidak menarik."

Mereka spend 3 bulan redesign UI. Launch. Tidak ada perubahan signifikan.

Saat akhirnya mereka melakukan user interview, jawabannya mengejutkan: pengguna tidak paham **apa bedanya investasi reksa dana dengan tabungan biasa**. Bukan masalah UI — tapi literasi keuangan.

Satu jam riset pengguna bisa menghemat tiga bulan kerja sia-sia. Itulah mengapa user research adalah investasi terbaik dalam proses desain.

---

## 🧩 Konsep Utama

### Dua Jenis Data Riset

**Kualitatif** — Menggali pemahaman mendalam tentang motivasi, perasaan, dan konteks.
* Wawancara mendalam, observasi, focus group
* Output: tema, insight, "mengapa"
* Sample kecil (5-15 orang) tapi kaya informasi

**Kuantitatif** — Mengukur pola dan tren dari data besar.
* Survei, analytics, A/B testing
* Output: angka, persentase, "berapa banyak"
* Sample besar (100+ responden) untuk validitas statistik

```
Kualitatif menjawab: "Mengapa pengguna tidak mau top-up?"
Kuantitatif menjawab: "Berapa persen pengguna yang tidak pernah top-up?"

Keduanya saling melengkapi:
- Mulai kualitatif → temukan insight
- Validasi dengan kuantitatif → konfirmasi seberapa luas
```

### Wawancara Semi-Terstruktur

Metode paling berharga dalam user research. "Semi-terstruktur" berarti ada panduan (discussion guide), tapi interviewer bebas menggali lebih dalam dengan probing questions.

**5 Aturan Emas Wawancara:**

1. **Tanya tentang masa lalu, bukan masa depan** → "Ceritakan satu kali kamu transfer uang yang paling berkesan" bukan "Apakah kamu akan menggunakan fitur X?"
2. **Hindari leading questions** → "Kamu setuju kan kalau fitur ini membantu?" mendorong jawaban tertentu
3. **Diam adalah emas** → Setelah jawaban, tunggu 3-5 detik sebelum bertanya lagi — pengguna sering menambahkan insight penting saat ada keheningan
4. **Probing, bukan menghakimi** → "Bisa ceritakan lebih detail?" bukan "Mengapa kamu tidak melakukan X?"
5. **Fokus pada behavior, bukan opini** → Apa yang pengguna lakukan vs apa yang mereka katakan sering berbeda

### Discussion Guide Template

```
DISCUSSION GUIDE: [Nama Proyek]
Durasi: 45-60 menit
Tujuan: Memahami perilaku dan motivasi pengguna terkait [topik]

PEMBUKA (5 menit)
- Perkenalan dan tujuan sesi
- Minta izin rekam
- "Tidak ada jawaban yang salah — saya ingin memahami pengalaman Anda"
- "Saya bukan yang membuat aplikasi ini, jadi komentar apapun sangat membantu"

PEMANASAN (10 menit)
- Ceritakan sedikit tentang dirimu — pekerjaan, aktivitas harian
- Seberapa sering kamu menggunakan smartphone?
- Aplikasi apa yang paling sering kamu buka setiap hari?

TOPIK UTAMA (30-35 menit)
[Blok 1: Konteks & Kebiasaan]
- Kapan terakhir kamu [aktivitas yang relevan]? Ceritakan situasinya.
- Apa yang biasanya kamu lakukan sebelum dan sesudah itu?
- Apa yang membuat kamu memilih cara itu?

[Blok 2: Pain Points]
- Apa yang paling tidak kamu sukai dari proses tersebut?
- Pernah mengalami situasi yang sangat membuat frustrasi? Ceritakan.
- Apa yang ideal menurutmu?

[Blok 3: Tool & Alur]
- Tunjukkan padaku bagaimana kamu biasanya melakukan [task]
  (Observasi langsung — "contextual inquiry")
- Apa yang kamu pikir saat melakukan langkah itu?
- Apa yang kamu harapkan terjadi vs apa yang sebenarnya terjadi?

PENUTUP (5 menit)
- Ada hal lain yang ingin kamu ceritakan yang belum sempat?
- Jika kamu bisa ubah satu hal dari pengalaman ini, apa itu?
```

### Teknik Probing

| Situasi | Probing yang Tepat |
|---|---|
| Jawaban terlalu singkat | "Bisa ceritakan lebih detail?" |
| Pengguna menyebutkan sesuatu menarik | "Kamu tadi bilang [X] — maksudnya seperti apa?" |
| Jawaban tidak jelas | "Saya ingin memastikan saya paham — maksud kamu...?" |
| Pengguna menjawab hipotetis | "Kapan terakhir itu terjadi? Ceritakan situasinya." |
| Ingin tahu motivasi | "Kenapa itu penting buat kamu?" |

### Survei Online

Survei cocok untuk validasi skala besar. Beberapa prinsip penting:

**Pertanyaan yang baik:**
* Satu pertanyaan = satu hal
* Hindari double-barreled: "Apakah fitur ini mudah dan cepat?" (dua hal sekaligus)
* Gunakan skala Likert 5-7 poin untuk rating
* Sediakan opsi "Tidak relevan" atau "Saya tidak tahu"

**Skala Likert:**
```
Seberapa mudah melakukan transfer di aplikasi ini?
① Sangat Sulit  ② Sulit  ③ Netral  ④ Mudah  ⑤ Sangat Mudah
```

### Affinity Diagram

Setelah wawancara, data mentah perlu diorganisir. Affinity Diagram adalah teknik untuk mengelompokkan temuan kualitatif berdasarkan tema.

```
PROSES AFFINITY DIAGRAM:

1. TULIS — setiap insight dari wawancara di sticky note terpisah
   "Pengguna bingung perbedaan Transfer dan Kirim Uang"
   "Pengguna takut salah input nomor rekening"
   "Pengguna ingin tahu saldo realtime sebelum transfer"

2. KELOMPOKKAN — seret sticky notes yang berkaitan berdekatan

3. BERI LABEL — namai setiap kelompok dengan tema
   KELOMPOK: "Kekhawatiran saat Transfer"
   KELOMPOK: "Kebutuhan Informasi Realtime"
   KELOMPOK: "Kebingungan Terminologi"

4. IDENTIFIKASI INSIGHT — dari setiap kelompok, rumuskan insight
   Insight: "Pengguna membutuhkan konfirmasi visual yang lebih kuat
             sebelum eksekusi transaksi"
```

---

## 🧠 Ilustrasi / Analogi

**Wawancara pengguna seperti menjadi dokter:**
* Dokter tidak langsung meresepkan obat tanpa diagnosis
* Diagnosis butuh: wawancara gejala (interview), observasi langsung (pemeriksaan), dan data lab (survei/analytics)
* Dokter yang baik TIDAK menyarankan pengobatan berdasarkan asumsi — mereka mendengarkan dan mengamati dulu

**Affinity Diagram seperti menyortir laundry:**
* Semua baju kotor = semua data mentah dari wawancara
* Sortir: putih, warna, hitam = kelompokkan berdasarkan tema
* Setiap tumpukan = satu tema/insight utama

---

## 💻 Praktik: Rancang Survei dengan Google Forms

### Contoh Pertanyaan Survei untuk Aplikasi Fintech

```
SURVEI: Pengalaman Pengguna Aplikasi Pembayaran Digital
(Estimasi waktu: 5 menit)

BAGIAN 1: Profil Responden
Q1. Berapa usia Anda?
   ○ Di bawah 18  ○ 18-24  ○ 25-34  ○ 35-44  ○ 45+

Q2. Aplikasi pembayaran digital apa yang paling sering Anda gunakan?
   □ GoPay  □ OVO  □ Dana  □ ShopeePay  □ Lainnya: ___

BAGIAN 2: Perilaku Penggunaan
Q3. Seberapa sering Anda menggunakan aplikasi tersebut?
   ○ Beberapa kali sehari  ○ Sekali sehari  ○ Beberapa kali seminggu
   ○ Sekali seminggu  ○ Lebih jarang

Q4. Fitur apa yang PALING SERING Anda gunakan? (Pilih semua yang sesuai)
   □ Transfer ke sesama  □ Transfer ke bank  □ Bayar merchant (QR)
   □ Bayar tagihan  □ Top-up  □ Investasi / Tabungan

BAGIAN 3: Evaluasi Pengalaman
Q5. Seberapa mudah menemukan fitur yang Anda cari?
   ① Sangat Sulit ② Sulit ③ Netral ④ Mudah ⑤ Sangat Mudah

Q6. Seberapa percaya Anda bahwa transaksi akan berhasil setelah dikirim?
   ① Tidak Percaya ② Kurang Percaya ③ Netral ④ Cukup Percaya ⑤ Sangat Percaya

Q7. Apa SATU hal yang paling membuat Anda frustrasi dari aplikasi ini?
   [Jawaban terbuka]

Q8. Jika Anda bisa mengubah satu fitur, apa yang akan diubah?
   [Jawaban terbuka]
```

---

## 🏢 Studi Kasus Nyata: User Research GoPay untuk Fitur GoBisnis

**Konteks:** GoPay ingin menambahkan fitur manajemen keuangan untuk pemilik usaha kecil (warung, pedagang online).

**Metodologi:**
* **Contextual Inquiry** — mengunjungi 20 warung dan toko online di Jakarta dan Surabaya, mengamati langsung cara mereka mencatat transaksi
* **Wawancara** — 30 wawancara mendalam, 45-60 menit, dengan pemilik UMKM
* **Survei** — 500 responden dari database merchant GoPay

**Insight Utama dari Riset:**
1. 78% mencatat transaksi di buku tulis atau WhatsApp — bukan aplikasi khusus
2. Pain point terbesar: tidak bisa tahu "uang di laku hari ini berapa" secara instan
3. Hambatan adopsi teknologi: takut data tidak aman + merasa repot transisi

**Keputusan Desain Berdasarkan Riset:**
* Dashboard utama menampilkan "omzet hari ini" secara prominan (langsung jawab pain point #2)
* Onboarding sangat singkat, 3 langkah saja (atasi hambatan "terlalu repot")
* Data enkripsi dijelaskan dalam bahasa sederhana di layar pertama (atasi kekhawatiran keamanan)

---

## ⚠️ Kesalahan Umum

1. **Menanyakan pertanyaan "ya/tidak"** → Jawaban ya/tidak tidak memberikan insight. Ganti dengan "Ceritakan kapan terakhir kamu...?"

2. **Merekrut teman atau kolega** → Mereka terlalu familiar dengan produk, terlalu sopan untuk mengkritik, dan mungkin bukan target pengguna. Rekrut orang yang benar-benar adalah target pengguna.

3. **Menyelesaikan kalimat responden** → "Jadi maksudnya kamu tidak suka karena...?" Biarkan responden selesaikan sendiri.

4. **Terlalu banyak pertanyaan dalam survei** → Survei >10 menit = drop rate tinggi. Fokus pada pertanyaan paling penting.

5. **Affinity Diagram terlalu besar** → Jika ada 200+ sticky notes, kelompokkan dua level: level 1 (cluster besar) dan level 2 (sub-tema).

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Kapan kamu memilih wawancara kualitatif vs survei kuantitatif? Berikan contoh pertanyaan riset yang paling tepat dijawab dengan masing-masing metode.

b) Mengapa teknik probing penting dalam wawancara pengguna? Berikan contoh situasi di mana probing mengungkap insight yang tidak akan ditemukan dari pertanyaan awal.

### Soal 2 — Praktik (Tugas)

Rancang user research plan untuk topik proyek akhir kamu:
1. Buat **discussion guide** dengan minimal 10 pertanyaan wawancara (pembuka + pemanasan + topik utama)
2. Buat **kuesioner survei** dengan minimal 8 pertanyaan menggunakan Google Forms
3. Lakukan wawancara dengan **minimal 3 orang** yang merupakan target pengguna
4. Buat **Affinity Diagram** digital (FigJam/Miro) dari temuan wawancara
5. Rumuskan **3-5 key insights** yang akan memandu desain kamu

---

## 📌 Ringkasan

* **Kualitatif** (wawancara) = mengapa → insight mendalam
* **Kuantitatif** (survei) = berapa banyak → validasi skala luas
* **Wawancara semi-terstruktur** = discussion guide + flexibility untuk probing
* 5 aturan emas: masa lalu bukan masa depan, no leading questions, diam adalah emas, probing bukan menghakimi, behavior bukan opini
* **Affinity Diagram** = cara sistematis mengelompokkan data kualitatif menjadi tema dan insight
* Rekrut pengguna nyata sesuai target — bukan teman atau kolega
* Satu jam riset = hemat berminggu-minggu pengembangan yang sia-sia

---

*📚 Referensi: Garrett, J.J. (2010). The Elements of User Experience | Nielsen Norman Group — User Interviews | UX Collective — Affinity Diagrams*
