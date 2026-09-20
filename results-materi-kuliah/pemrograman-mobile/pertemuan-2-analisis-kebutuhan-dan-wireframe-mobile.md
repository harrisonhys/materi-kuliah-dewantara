# Pertemuan 2: Analisis Kebutuhan dan Wireframe Aplikasi Mobile

> **Acuan RPS:** CPMK-1, Sub-CPMK-2 • **Durasi:** 150 menit • **Artefak:** project brief dan wireframe

---

## 1. 🎯 Learning Outcomes

Setelah belajar, kamu mampu:

- memilih domain Semester Project berdasarkan masalah pengguna yang nyata;
- membedakan kebutuhan pengguna, kebutuhan fungsional, dan nonfungsional;
- menyusun persona ringkas, user story, acceptance criteria, dan prioritas MVP;
- menggambar user flow serta wireframe low-fidelity untuk layar utama; dan
- menjelaskan keputusan UI berdasarkan konteks mobile, bukan selera pribadi.

**Indikator RPS:** domain relevan, alur jelas, dan layar utama lengkap. Penilaian menggunakan rubrik wireframe dengan bobot mingguan 2%.

---

## 2. 📖 Pengantar

Tim dapat menulis ribuan baris kode dan tetap menghasilkan aplikasi yang tidak berguna. Penyebabnya sering sederhana: mereka mulai dari fitur, bukan masalah. Aplikasi antrean klinik, misalnya, tidak cukup hanya memiliki tombol “Ambil Nomor”. Pasien perlu tahu klinik yang dipilih, perkiraan waktu, status antrean, dan apa yang dilakukan jika koneksi terputus.

Pertemuan ini mengubah ide kabur menjadi alur yang dapat diuji sebelum coding mahal dilakukan.

---

## 3. 🧩 Konsep Utama

### 3.1 Dari Masalah ke Kebutuhan

Gunakan urutan:

```text
Masalah → Pengguna → Tujuan → Skenario → Kebutuhan → Alur → Layar
```

Contoh masalah yang baik: “Pemilik warung kesulitan memastikan pesanan mana yang sudah dibayar ketika ramai.” Ini lebih berguna daripada “Buat aplikasi kasir modern”.

### 3.2 Teknik Pengumpulan Data

- **Wawancara:** menggali tujuan, kebiasaan, dan kesulitan.
- **Observasi:** melihat perbedaan antara ucapan dan perilaku.
- **Survei:** mengumpulkan pola dari lebih banyak responden.
- **Analisis proses:** memetakan langkah, pelaku, input, dan kegagalan.
- **Studi kompetitor:** memahami pola umum, bukan menyalin tampilan.

Hindari pertanyaan mengarahkan seperti “Apakah kamu mau fitur notifikasi?” Tanyakan “Bagaimana kamu mengetahui giliran sudah dekat?”

### 3.3 Jenis Kebutuhan

| Jenis | Pertanyaan | Contoh aplikasi antrean |
|---|---|---|
| Pengguna | Tujuan apa yang ingin dicapai? | Mengetahui giliran tanpa terus menunggu di lokasi |
| Fungsional | Sistem harus bisa melakukan apa? | Mengambil nomor dan menampilkan status |
| Nonfungsional | Seberapa baik sistem harus bekerja? | Status tampil cepat, data aman, teks mudah dibaca |
| Batasan | Apa yang membatasi solusi? | Android dan iOS, tim 3 orang, 14 minggu |

### 3.4 Persona dan User Story

Persona adalah representasi pola pengguna berbasis temuan, bukan karakter fiksi yang dihias berlebihan.

```text
Sebagai pasien,
saya ingin melihat perkiraan giliran,
agar saya tidak menunggu terlalu lama di klinik.
```

Acceptance criteria:

```text
Given pasien memiliki nomor aktif
When halaman status dibuka
Then aplikasi menampilkan nomor, antrean berjalan, dan status sinkronisasi
```

### 3.5 Menentukan MVP

**MVP** adalah versi terkecil yang memberikan nilai dan menguji asumsi penting. Gunakan prioritas MoSCoW:

- **Must:** wajib agar alur inti bekerja;
- **Should:** penting tetapi masih dapat ditunda;
- **Could:** tambahan jika waktu cukup;
- **Won't for now:** sengaja tidak dikerjakan pada versi ini.

Untuk Semester Project, satu alur yang selesai, teruji, dan terintegrasi lebih bernilai daripada sepuluh menu setengah jadi.

### 3.6 User Flow dan Wireframe

User flow menunjukkan urutan tindakan dan keputusan. Wireframe menunjukkan struktur informasi pada layar.

```text
Mulai → Login → Beranda → Pilih Layanan → Konfirmasi
                                      ├─ valid → Nomor Antrean
                                      └─ gagal → Pesan + Coba Lagi
```

Wireframe low-fidelity cukup memakai kotak, label, tombol, dan anotasi. Fokuskan pada hierarki, bukan warna.

```text
┌──────────────────────────────┐
│ Antrean Klinik               │
│ Halo, Rina                   │
│                              │
│ Pilih layanan                │
│ [ Dokter Umum          v ]   │
│ [ Ambil Nomor Antrean ]      │
│                              │
│ Antrean aktif                │
│ A-021 • menunggu             │
└──────────────────────────────┘
```

### 3.7 Prinsip UI Mobile Dasar

- satu layar memiliki tujuan utama yang jelas;
- tindakan utama mudah ditemukan dan dijangkau;
- teks, warna, dan status tidak menjadi satu-satunya penanda;
- sediakan loading, empty, error, offline, dan success state;
- minta permission saat dibutuhkan dan jelaskan manfaatnya;
- minimalkan input, terutama pada layar kecil;
- gunakan istilah pengguna, bukan nama tabel database.

---

## 4. 🧠 Analogi

Wireframe seperti denah rumah. Memindahkan kamar pada denah sangat murah; memindahkan kamar setelah rumah dibangun mahal. User flow seperti rute tamu dari pintu masuk sampai tujuan. Jika rute buntu, dekorasi yang cantik tidak menyelesaikan masalah.

---

## 5. 💻 Contoh Teknis: Model Project Brief

```markdown
# Project Brief AntreanKita
Masalah: pasien tidak mengetahui estimasi giliran.
Pengguna utama: pasien klinik rawat jalan.
Nilai utama: status antrean yang jelas dan dapat dipulihkan.
Must-have: login, pilih layanan, ambil nomor, status, pembatalan.
Risiko: data terlambat, notifikasi gagal, nomor ganda.
Metrik: alur selesai tanpa bantuan; status sinkron; tidak ada nomor ganda.
```

Setiap layar diberi anotasi: tujuan, data masuk, aksi, validasi, keadaan kosong, dan keadaan gagal. Ini menjadi kontrak awal antara desain dan implementasi.

---

## 6. 🏦 Studi Kasus Nyata Fintech

### Onboarding Dompet Digital Terlalu Panjang

**Masalah:** pengguna diminta mengisi banyak data sebelum memahami manfaat produk. Ketika unggah identitas gagal, aplikasi kembali ke awal tanpa menjelaskan sebab.

**Dampak:** conversion rate turun, biaya akuisisi terbuang, dan tiket dukungan meningkat.

**Solusi high-level:** petakan tahap onboarding, minta data secara bertahap, simpan progress aman, jelaskan alasan pengambilan data, sediakan retry unggahan, dan bedakan status “sedang diverifikasi” dari “ditolak”. Backend menyimpan status proses; UI hanya mempresentasikan keadaan yang tervalidasi.

---

## 7. 📊 Aktivitas 150 Menit

| Waktu | Aktivitas | Luaran |
|---:|---|---|
| 15 | Bedah masalah aplikasi sehari-hari | problem statement |
| 25 | Kebutuhan, user story, acceptance criteria | daftar kebutuhan |
| 20 | Prioritas MVP | matriks MoSCoW |
| 25 | Demo user flow dan wireframe | contoh layar |
| 50 | Workshop kelompok di Figma/kertas | 5–7 wireframe |
| 10 | Peer review | catatan revisi |
| 5 | Exit ticket | satu keputusan desain |

---

## 8. ⚠️ Kesalahan Umum

- Membuat solusi sebelum memvalidasi masalah.
- Persona hanya berisi umur dan foto tanpa tujuan/perilaku.
- Semua fitur dianggap wajib.
- Wireframe langsung dibuat high-fidelity sehingga diskusi terjebak warna.
- Tidak mendesain error, loading, empty, dan offline state.
- Tombol Back atau pembatalan tidak dipikirkan.
- Menganggap data dari UI otomatis valid; backend tetap wajib memvalidasi.

---

## 9. 🧪 Latihan dan Penilaian

**Soal konsep:** bedakan kebutuhan fungsional dan nonfungsional; jelaskan mengapa MVP bukan produk asal jadi.

**Studi kasus:** rancang alur reservasi lapangan. Sertakan benturan jadwal, pembayaran tertunda, dan koneksi putus.

**Tugas:** kumpulkan project brief, 3 user story dengan acceptance criteria, matriks prioritas, user flow, serta 5–7 wireframe termasuk minimal satu error state.

| Kriteria | Bobot |
|---|---:|
| Kejelasan masalah dan pengguna | 20% |
| Konsistensi user flow | 25% |
| Kelengkapan layar dan state | 30% |
| Kesesuaian MVP | 15% |
| Alasan keputusan | 10% |

---

## 10. 📌 Ringkasan

- Desain dimulai dari masalah dan pengguna.
- Kebutuhan harus dapat ditelusuri menuju alur dan layar.
- MVP memprioritaskan alur inti yang bernilai.
- User flow memetakan perjalanan; wireframe memetakan struktur layar.
- Loading, error, empty, offline, dan success state adalah bagian desain.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Acceptance criteria | Kondisi terukur agar kebutuhan dianggap terpenuhi |
| MVP | Versi minimum yang sudah memberi nilai dan menguji asumsi |
| Persona | Representasi pola pengguna berdasarkan temuan |
| Problem statement | Rumusan masalah, pengguna, konteks, dan dampaknya |
| User flow | Urutan langkah dan keputusan pengguna |
| User story | Kebutuhan dalam sudut pandang pengguna |
| Wireframe | Rancangan struktur layar sebelum visual rinci |

## 12. Referensi

- Material Design, <https://m3.material.io/>
- Flutter UI, <https://docs.flutter.dev/ui>
- RPS Pemrograman Mobile MU5502, Pertemuan 2.

