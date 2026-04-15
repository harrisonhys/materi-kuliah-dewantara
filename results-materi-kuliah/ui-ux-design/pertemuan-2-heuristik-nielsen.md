# Pertemuan 2: Heuristik Nielsen & Evaluasi Antarmuka

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan dan menerapkan 10 Heuristik Nielsen
* Melakukan Heuristic Evaluation pada aplikasi nyata
* Mengkategorikan temuan masalah berdasarkan severity rating (0–4)
* Membuat laporan evaluasi heuristik yang terstruktur

---

## 📖 Pengantar (Hook)

Bayangkan kamu sedang transfer uang Rp 5 juta. Setelah menekan tombol "Kirim", tidak ada konfirmasi, tidak ada animasi, tidak ada notifikasi. Apakah transfer berhasil? Apakah kamu harus tekan lagi? Kamu tidak tahu.

Frustrasi ini adalah pelanggaran Heuristik #1 Nielsen: **Visibility of System Status**.

Di tahun 1994, Jakob Nielsen — peneliti UX terkemuka dari Nielsen Norman Group — mempublikasikan 10 prinsip desain yang menjadi standar emas evaluasi antarmuka hingga hari ini. Bukan aturan kaku, tapi "heuristik" — panduan berdasarkan pengalaman yang membantu mengidentifikasi masalah usability secara sistematis.

---

## 🧩 10 Heuristik Nielsen

### #1 — Visibility of System Status

**Prinsip:** Sistem selalu memberi tahu pengguna apa yang sedang terjadi, melalui feedback yang tepat waktu.

**Contoh baik:** Loading spinner saat upload foto, progress bar saat download, konfirmasi "Pembayaran Berhasil" setelah transfer.

**Contoh buruk:** Tombol yang tidak berubah tampilan setelah diklik — pengguna tidak tahu apakah aksinya berhasil.

```
❌ Buruk:   [KIRIM]  ← tidak berubah setelah diklik
✅ Bagus:   [KIRIM]  →  [⟳ Mengirim...]  →  [✓ Terkirim!]
```

---

### #2 — Match Between System and the Real World

**Prinsip:** Gunakan bahasa dan konsep yang familiar bagi pengguna, bukan jargon teknis internal sistem.

**Contoh baik:** Ikon "sampah" untuk hapus, "amplop" untuk email, "rumah" untuk home.

**Contoh buruk:** Error message "Error 0x8007000E: Insufficient virtual memory" — pengguna awam tidak mengerti ini.

```
❌ Buruk:   "NullPointerException at line 423"
✅ Bagus:   "Tidak dapat memuat halaman. Coba lagi."
```

---

### #3 — User Control and Freedom

**Prinsip:** Pengguna sering salah pilih menu atau aksi. Berikan "emergency exit" yang jelas — undo, redo, cancel.

**Contoh baik:** Tombol "Urungkan" setelah hapus email, tombol "Back" yang konsisten, konfirmasi sebelum aksi permanen.

**Contoh buruk:** Menghapus file tanpa konfirmasi, tidak ada tombol Cancel saat proses berjalan.

---

### #4 — Consistency and Standards

**Prinsip:** Pengguna tidak harus bertanya-tanya apakah kata, situasi, atau aksi yang berbeda berarti hal yang sama.

**Contoh baik:** Tombol primary selalu warna yang sama di seluruh aplikasi. "Simpan" selalu di posisi yang sama.

**Contoh buruk:** Beberapa halaman menggunakan "Kirim", halaman lain "Submit", dan halaman lain "Lanjutkan" untuk aksi yang identik.

---

### #5 — Error Prevention

**Prinsip:** Desain yang mencegah masalah terjadi lebih baik daripada pesan error yang baik.

**Contoh baik:** Disable tombol "Kirim" selama field wajib masih kosong. Auto-format nomor rekening. Konfirmasi sebelum hapus data.

**Contoh buruk:** Tombol "Hapus Semua" dan "Simpan" bersebelahan tanpa konfirmasi.

```
❌ Buruk:   Izinkan transfer ke nomor format salah, lalu tampilkan error
✅ Bagus:   Validasi format nomor rekening real-time saat pengguna mengetik
```

---

### #6 — Recognition Rather than Recall

**Prinsip:** Minimisasi beban memori pengguna. Buat objek, aksi, dan opsi terlihat jelas, bukan harus diingat.

**Contoh baik:** Autocomplete saat mencari kontak. Recent transfers di halaman transfer. Breadcrumb navigation.

**Contoh buruk:** Command-line interface yang membutuhkan hafal perintah. Form yang tidak tampilkan field yang sudah diisi sebelumnya.

---

### #7 — Flexibility and Efficiency of Use

**Prinsip:** Accelerator (shortcut) memungkinkan pengguna berpengalaman bekerja lebih cepat, tanpa mengganggu pengguna baru.

**Contoh baik:** Keyboard shortcut Ctrl+Z untuk undo. Quick transfer ke kontak favorit. Facial recognition untuk login.

**Contoh buruk:** Semua pengguna dipaksa melalui proses yang sama panjangnya, tidak ada cara untuk skip langkah yang sudah familiar.

---

### #8 — Aesthetic and Minimalist Design

**Prinsip:** Setiap informasi ekstra yang tidak relevan bersaing dengan informasi yang relevan dan mengurangi visibilitasnya.

**Contoh baik:** Apple Maps yang hanya menampilkan info yang dibutuhkan saat navigasi. Dashboard yang fokus pada metric utama.

**Contoh buruk:** Halaman home yang penuh dengan banner promosi sehingga pengguna tidak bisa menemukan fitur utama.

---

### #9 — Help Users Recognize, Diagnose, and Recover from Errors

**Prinsip:** Pesan error harus dinyatakan dalam bahasa biasa, menjelaskan masalah secara tepat, dan menyarankan solusi.

**Contoh baik:** "Saldo tidak cukup. Saldo saat ini Rp 50.000, dibutuhkan Rp 150.000. [Top Up Sekarang]"

**Contoh buruk:** "Transaksi gagal." — tidak ada informasi mengapa atau apa yang harus dilakukan.

---

### #10 — Help and Documentation

**Prinsip:** Meskipun lebih baik jika sistem bisa digunakan tanpa dokumentasi, kadang bantuan perlu tersedia — mudah dicari, fokus pada tugas pengguna, mencantumkan langkah konkret.

**Contoh baik:** FAQ yang bisa dicari, chatbot support yang relevan, tooltip kontekstual.

**Contoh buruk:** Manual 200 halaman yang harus dibaca dari awal untuk menemukan satu fitur.

---

## 🧠 Severity Rating

Nielsen mendefinisikan skala severity untuk memprioritaskan perbaikan:

| Rating | Kategori | Deskripsi |
|---|---|---|
| **0** | Not a problem | Tidak setuju bahwa ini masalah usability |
| **1** | Cosmetic | Hanya perlu diperbaiki jika ada waktu ekstra |
| **2** | Minor | Perbaikan prioritas rendah |
| **3** | Major | Perbaikan prioritas tinggi, sangat mengganggu pengalaman |
| **4** | Catastrophic | Wajib diperbaiki sebelum produk dirilis |

---

## 💻 Praktik: Template Heuristic Evaluation

```
LAPORAN HEURISTIC EVALUATION
Aplikasi: [Nama Aplikasi]
Platform: [iOS / Android / Web]
Evaluator: [Nama]
Tanggal: [DD/MM/YYYY]

────────────────────────────────────────────────────────
TEMUAN #1
Lokasi:        [Halaman / Fitur spesifik]
Heuristik:     #[Nomor] — [Nama Heuristik]
Severity:      [0-4]
Deskripsi:     [Deskripsikan masalah yang ditemukan]
Screenshot:    [Referensi ke screenshot yang di-annotate]
Rekomendasi:   [Solusi yang disarankan]
────────────────────────────────────────────────────────

RANGKUMAN TEMUAN:
Severity 4 (Catastrophic): X temuan
Severity 3 (Major):        X temuan
Severity 2 (Minor):        X temuan
Severity 1 (Cosmetic):     X temuan
```

---

## 🏢 Studi Kasus Nyata: Heuristic Evaluation OVO

**Aplikasi:** OVO (mobile wallet)
**Evaluator:** Senior UX Designer (simulasi)

**Temuan 1:**
* **Lokasi:** Halaman transfer — field jumlah nominal
* **Heuristik:** #5 Error Prevention
* **Severity:** 3 (Major)
* **Deskripsi:** Pengguna bisa memasukkan nominal yang melebihi saldo tanpa peringatan real-time. Error baru muncul setelah menekan "Lanjut".
* **Rekomendasi:** Tampilkan sisa saldo di atas field nominal. Berikan warning real-time jika nominal melebihi saldo.

**Temuan 2:**
* **Lokasi:** Riwayat transaksi
* **Heuristik:** #6 Recognition Rather than Recall
* **Severity:** 2 (Minor)
* **Deskripsi:** Filter transaksi hanya bisa diakses lewat ikon yang tidak berlabel. Banyak pengguna tidak tahu fitur ini ada.
* **Rekomendasi:** Tambahkan label teks di bawah ikon filter. Atau tampilkan chip filter yang selalu visible.

**Temuan 3:**
* **Lokasi:** Konfirmasi pembayaran
* **Heuristik:** #1 Visibility of System Status
* **Severity:** 3 (Major)
* **Deskripsi:** Setelah konfirmasi pembayaran, ada jeda ~3 detik tanpa loading indicator. Pengguna sering double-tap karena tidak ada feedback.
* **Rekomendasi:** Tambahkan loading state yang jelas + disable tombol setelah diklik pertama kali.

---

## 📊 Visualisasi: Distribusi Pelanggaran Heuristik

Berdasarkan riset Nielsen Norman Group pada ratusan produk digital:

```
Heuristik yang paling sering dilanggar:

#1 Visibility of System Status    ████████████ 23%
#9 Error Recovery                 ████████     16%
#4 Consistency & Standards        ███████      14%
#3 User Control & Freedom         ██████       12%
#5 Error Prevention               █████        10%
#8 Aesthetic & Minimalist         ████          8%
Lainnya                           ████████     17%
```

---

## ⚠️ Kesalahan Umum dalam Heuristic Evaluation

1. **Evaluasi tanpa konteks pengguna** → Apa yang terasa "masuk akal" bagi desainer mungkin sangat membingungkan bagi pengguna target (lansia, pengguna baru, dll).

2. **Severity terlalu tinggi untuk semua temuan** → Jika semuanya severity 4, tidak ada prioritas. Calibrate severity berdasarkan dampak nyata.

3. **Rekomendasi yang tidak actionable** → "Desain harus lebih baik" bukan rekomendasi. "Tambahkan pesan konfirmasi setelah tombol Submit ditekan, tampilkan selama 3 detik" adalah rekomendasi yang bisa dieksekusi.

4. **Heuristic Evaluation bukan pengganti User Testing** → Heuristic eval mendeteksi masalah yang bisa dilihat desainer berpengalaman, tapi tidak semua masalah yang dialami pengguna nyata.

---

## 🧪 Latihan

### Soal 1 — Teori

a) Jelaskan perbedaan antara Heuristik #5 (Error Prevention) dan Heuristik #9 (Error Recovery). Berikan contoh implementasi masing-masing pada fitur input nomor rekening bank.

b) Mengapa Heuristik #8 (Aesthetic and Minimalist Design) bukan berarti "desain harus membosankan"? Berikan contoh desain yang minimalis sekaligus menarik secara visual.

### Soal 2 — Praktik (Tugas)

Lakukan Heuristic Evaluation pada satu aplikasi mobile perbankan atau fintech pilihan kamu:
1. Pilih minimal **8 layar berbeda** untuk dievaluasi
2. Identifikasi minimal **10 masalah usability**
3. Untuk setiap temuan: tentukan heuristik yang dilanggar, berikan severity rating, sertakan screenshot yang di-annotate
4. Susun dalam laporan PDF dengan rangkuman di halaman pertama

---

## 📌 Ringkasan

* **10 Heuristik Nielsen** = panduan evaluasi antarmuka, bukan aturan kaku
* Heuristik paling kritis: #1 (Status), #3 (Control), #5 (Prevention), #9 (Recovery)
* **Severity rating 0-4** membantu prioritisasi perbaikan
* Heuristic Evaluation bisa dilakukan oleh satu evaluator, tapi lebih baik 3-5 orang
* Temuan harus disertai: lokasi, heuristik yang dilanggar, severity, deskripsi, dan rekomendasi konkret
* Heuristic Evaluation ≠ User Testing — keduanya melengkapi, bukan menggantikan

---

*📚 Referensi: Nielsen, J. (1994). 10 Usability Heuristics — nngroup.com | Krug, S. (2014). Don't Make Me Think | Garrett, J.J. (2010). Elements of User Experience*
