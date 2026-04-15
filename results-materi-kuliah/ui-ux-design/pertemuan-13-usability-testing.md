# Pertemuan 13: Usability Testing — Perencanaan & Pelaksanaan

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Membuat test plan yang komprehensif
* Merancang tasks yang efektif untuk usability testing
* Melaksanakan moderated usability testing dengan teknik Think-Aloud
* Menggunakan Rainbow Sheet untuk mencatat observasi

---

## 📖 Pengantar (Hook)

"Kami sudah test produk ini — semua orang di tim berhasil menggunakannya."

Ini bukan usability testing. Ini **confirmation bias**.

Tim yang membangun produk tahu cara menggunakannya. Mereka tidak bisa melihat masalah yang akan dihadapi pengguna pertama kali.

Usability Testing yang sesungguhnya adalah menempatkan **5 orang yang benar-benar tidak kenal produkmu** untuk mencoba menyelesaikan task nyata, dan **mengamati** — tanpa membantu, tanpa menjelaskan, tanpa memberi petunjuk.

Apa yang kamu pelajari dalam satu jam session ini akan mengubah desainmu lebih dari berbulan-bulan kerja tanpa testing.

---

## 🧩 Konsep Utama

### Moderated vs Unmoderated Testing

**Moderated Testing:**
* Moderator hadir (fisik atau online) saat sesi
* Bisa tanya follow-up, observasi ekspresi wajah
* Lebih kaya insight kualitatif
* Lebih mahal, butuh penjadwalan

**Unmoderated Testing:**
* Partisipan menyelesaikan task sendiri, direkam oleh tools
* Skala besar, lebih cepat, lebih murah
* Kurang kaya — tidak bisa probing
* Tools: Maze, Useberry, UserTesting.com

**Untuk proyek kuliah:** Moderated testing selalu lebih direkomendasikan untuk insight yang kaya.

### Merekrut Partisipan

**Aturan "Magic Number 5"** (Jakob Nielsen):
* 5 partisipan menemukan ~85% masalah usability utama
* Penambahan partisipan ke-6 dst memberikan diminishing returns
* Lebih baik 3 sesi berkualitas daripada 10 sesi asal-asalan

**Kriteria rekrutmen:**
* Sesuai dengan persona target (bukan kolega atau teman yang tahu projekmu)
* Berbeda background / tech-savviness untuk variasi
* Tidak pernah melihat prototipe sebelumnya

### Test Plan

```
TEMPLATE TEST PLAN

OVERVIEW
Nama Proyek:     [Nama Aplikasi]
Tujuan Testing:  [Apa yang ingin kamu pelajari?]
Tanggal:         [Range tanggal pelaksanaan]
Lokasi:          [Fisik / Remote via Zoom]

OBJECTIVES (Apa yang ingin dipelajari)
1. Apakah pengguna dapat menemukan fitur X tanpa bantuan?
2. Apakah flow transfer terasa intuitif?
3. Di mana pengguna mengalami kebingungan pertama kali?

TASKS (3-5 task, masing-masing harus)
Task #1:
  Skenario: "Bayangkan kamu baru menerima uang dari seseorang dan 
             ingin mengecek apakah sudah masuk."
  Task: Temukan dan tampilkan konfirmasi penerimaan uang terakhir.
  Sukses: Partisipan berhasil menemukan riwayat transaksi
  Metrik: Success rate, time on task

Task #2:
  Skenario: "Kamu ingin transfer Rp 150.000 ke teman yang bernama Budi."
  Task: Lakukan transfer ke Budi Santoso senilai Rp 150.000.
  Sukses: Partisipan berhasil sampai ke success screen
  Metrik: Success rate, time on task, error rate

METRICS
- Task Completion Rate: berapa % partisipan berhasil per task
- Time on Task: berapa lama per task
- Error Rate: berapa kali salah klik / backtrack per task
- Satisfaction: SUS Score (di akhir sesi)

DISCUSSION GUIDE (pertanyaan follow-up)
- "Bisa ceritakan apa yang kamu pikirkan saat itu?"
- "Apa yang kamu harapkan terjadi?"
- "Pada skala 1-10, seberapa mudah task ini?"
```

### Tasks yang Efektif

```
❌ TASK YANG BURUK:
"Klik tombol Transfer"
→ Terlalu spesifik, memberitahu pengguna apa yang harus diklik

"Gunakan fitur Transfer untuk kirim uang"
→ Menyebut nama fitur, bukan skenario nyata

✅ TASK YANG BAIK:
"Bayangkan kamu baru makan siang bersama teman dan ingin 
membayar bagian kamu sebesar Rp 75.000 kepada Cici."
→ Skenario nyata, tidak menyebut nama fitur, tidak memberi petunjuk

ATURAN MEMBUAT TASK:
1. Gunakan bahasa pengguna, bukan bahasa produk
2. Berikan konteks (skenario) yang realistis
3. Jangan sebut nama fitur atau tombol
4. Task harus memiliki definisi sukses yang jelas
5. Mulai dari yang termudah → paling sulit
```

### Think-Aloud Protocol

Teknik di mana partisipan diminta **bersuara keras** menceritakan apa yang mereka pikirkan saat menggunakan produk.

```
INSTRUKSI KE PARTISIPAN:
"Saat kamu menggunakan aplikasi ini, tolong ceritakan
apa yang kamu pikirkan. Misalnya:
- Apa yang kamu lihat?
- Apa yang akan kamu klik?
- Apa yang kamu harapkan terjadi?
- Apa yang membingungkan?

Tidak ada jawaban yang salah. Kami ingin memahami 
bagaimana kamu berpikir, bukan menguji kemampuan kamu."

TUGAS MODERATOR:
✓ Diam dan observasi
✓ Catat: waktu, klik, ekspresi, kata-kata kunci
✓ Jika partisipan diam terlalu lama: "Apa yang kamu pikirkan sekarang?"
✗ JANGAN bantu atau beri petunjuk
✗ JANGAN jawab pertanyaan tentang cara pakai
✗ JANGAN tunjukkan reaksi (angguk/geleng)
```

### Rainbow Sheet

Rainbow Sheet adalah template pencatatan observasi dari beberapa partisipan dalam satu dokumen.

```
RAINBOW SHEET (6 kolom, 1 baris per temuan)

┌────────────────────┬─────┬─────┬─────┬─────┬─────┬────────────┐
│ TEMUAN             │ P1  │ P2  │ P3  │ P4  │ P5  │ FREQ       │
├────────────────────┼─────┼─────┼─────┼─────┼─────┼────────────┤
│ Tidak tahu menu    │  ✓  │     │  ✓  │  ✓  │     │ 3/5 (60%)  │
│ "Kirim" ada di mana│     │     │     │     │     │            │
├────────────────────┼─────┼─────┼─────┼─────┼─────┼────────────┤
│ Ekspektasi tombol  │     │  ✓  │  ✓  │     │  ✓  │ 3/5 (60%)  │
│ "Lanjut" ada di kiri│    │     │     │     │     │            │
├────────────────────┼─────┼─────┼─────┼─────┼─────┼────────────┤
│ Gagal input nominal│  ✓  │     │     │  ✓  │  ✓  │ 3/5 (60%)  │
│ (format tidak jelas)│    │     │     │     │     │            │
└────────────────────┴─────┴─────┴─────┴─────┴─────┴────────────┘

Temuan dengan frekuensi tinggi = prioritas perbaikan tinggi
```

---

## 🧠 Ilustrasi / Analogi

**Usability Testing seperti test drive:**
* Sebelum beli mobil, kamu test drive sendiri
* Instruktur tidak mengemudikan — kamu yang pegang setir
* Dealer memperhatikan: adakah yang membingungkan? Tombol mana yang dicari-cari?

**Think-Aloud seperti komentator olahraga:**
* Komentator mendeskripsikan apa yang mereka lihat saat itu terjadi
* Pengguna mendeskripsikan apa yang mereka pikirkan saat menggunakan produk

**Rainbow Sheet seperti tally di polling:**
* Setiap "tanda centang" = satu orang mengalami masalah yang sama
* Semakin banyak tanda = semakin penting diperbaiki

---

## 💻 Praktik: Template Sesi UT

### Checklist Sebelum Sesi

```
H-1 (Hari sebelum sesi):
□ Prototipe sudah di-test sendiri dari awal sampai akhir
□ Test plan sudah final
□ Discussion guide sudah siap
□ Rainbow Sheet sudah disiapkan
□ Consent form sudah disiapkan
□ Recording consent sudah ada
□ Link prototipe sudah di-share + bisa diakses

H-0 (Saat sesi):
□ Konfirmasi partisipan hadir
□ Siapkan device (atau link Figma prototype)
□ Buka recording tool (Loom / OBS / Zoom record)
□ Set timer untuk setiap task
□ Siapkan blocknotes untuk catatan

PASCA SESI:
□ Tulis notes segar selagi ingat
□ Review rekaman jika ada
□ Update Rainbow Sheet
□ Thank you message ke partisipan
```

### Script Pembuka Sesi

```
"Hai [nama], terima kasih sudah meluangkan waktu.

Saya [nama], dan hari ini kita akan melihat prototipe dari
[nama aplikasi]. Saya ingin belajar dari pengalaman kamu,
jadi tidak ada jawaban yang benar atau salah.

Kalau ada yang membingungkan, itu bukan kesalahan kamu —
itu feedback berharga untuk kami.

Saya akan minta kamu untuk bersuara keras menceritakan apa
yang kamu pikirkan saat menggunakan aplikasi ini.

Apakah kamu keberatan jika sesi ini direkam untuk keperluan
internal tim? [minta consent]

Ada pertanyaan sebelum kita mulai?"
```

---

## 🏢 Studi Kasus: UT yang Mengubah Desain GoPay

**Konteks:** Tim GoPay melakukan UT untuk fitur baru "GoPay Tabungan" (goal-based saving).

**5 partisipan:** Usia 22-35, pengguna GoPay aktif, belum pernah lihat fitur ini.

**Task yang diberikan:**
* Task 1: "Buat tabungan untuk liburan ke Bali bulan depan"
* Task 2: "Tambahkan Rp 100.000 ke tabungan yang sudah kamu buat"

**Temuan kritis (dari Rainbow Sheet):**
* 4/5 partisipan tidak tahu perbedaan "GoPay Tabungan" dan "Saldo GoPay"
* 3/5 partisipan tidak yakin apakah uang di tabungan "aman" atau bisa "hilang"
* 5/5 partisipan melewatkan CTA "Tambah Dana" di layar tabungan

**Iterasi yang dilakukan:**
* Tambahkan tooltip di onboarding: "Uang di Tabungan TERPISAH dari saldo utama"
* Perbesar dan ubah warna CTA "Tambah Dana" — dari abu-abu ke primary color
* Tambahkan visual "progress bar" menuju target — pengguna jadi lebih excited

**Hasil:** Task completion rate naik dari 60% ke 92% setelah iterasi.

---

## ⚠️ Kesalahan Umum

1. **Membantu partisipan yang stuck** → "Oh itu ada di menu atas" → UT menjadi tidak valid. Partisipan yang stuck = data berharga!

2. **Memberikan task yang terlalu mudah** → "Klik tombol Login" bukan task yang berguna. Task harus mencerminkan skenario nyata.

3. **Terlalu banyak task** → Lebih dari 5 task dalam 60 menit = partisipan kelelahan, data tidak valid. Pilih 3-4 task yang paling kritis.

4. **Recording tanpa izin** → Selalu minta consent sebelum merekam, dan jelaskan tujuannya (hanya untuk internal tim).

5. **Moderator yang "leading"** → "Apakah kamu *merasa* tombol ini membingungkan?" → sudah menanamkan sugesti. Gunakan pertanyaan terbuka: "Bagaimana perasaan kamu tentang ini?"

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Mengapa "Magic Number 5" (5 partisipan) cukup untuk usability testing? Dalam kondisi apa kamu mungkin butuh lebih dari 5 partisipan?

b) Jelaskan mengapa moderator TIDAK boleh membantu partisipan yang mengalami kesulitan selama sesi UT.

### Soal 2 — Praktik (Tugas)

Siapkan dan laksanakan sesi Usability Testing untuk prototipe Hi-Fi kamu:

1. **Test Plan:**
   * Objectives (3 hal yang ingin dipelajari)
   * 3 tasks dengan skenario nyata (tidak menyebut nama fitur)
   * Metrics: success rate, time on task

2. **Pelaksanaan:**
   * Rekrut minimal 3 partisipan (bukan anggota tim)
   * Laksanakan moderated UT dengan teknik Think-Aloud
   * Minta izin rekam

3. **Dokumentasi:**
   * Rainbow Sheet dengan semua temuan
   * Video/rekaman audio sesi

---

## 📌 Ringkasan

* **Usability Testing** = amati pengguna nyata menyelesaikan task dengan produkmu — tanpa membantu
* **5 partisipan** cukup untuk menemukan 85% masalah utama
* **Task yang baik:** skenario nyata, tidak menyebut nama fitur, punya definisi sukses
* **Think-Aloud Protocol:** partisipan bersuara keras pikiran mereka
* **Moderator:** diam dan observasi — jangan bantu, jangan lead, jangan bereaksi
* **Rainbow Sheet:** catat semua temuan, tandai partisipan mana yang mengalaminya
* Rekam sesi (dengan izin) — kamu pasti melewatkan sesuatu saat real-time
* UT bukan tentang membuktikan desainmu bagus — tapi tentang menemukan apa yang perlu diperbaiki

---

*📚 Referensi: Krug, S. (2014). Don't Make Me Think | Nielsen, J. nngroup.com/articles/why-you-only-need-to-test-with-5-users | Maze — maze.co*
