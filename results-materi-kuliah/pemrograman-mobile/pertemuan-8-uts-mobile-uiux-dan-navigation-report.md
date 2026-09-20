# Pertemuan 8: UTS Mobile UI UX dan Navigation Report

> **Evaluasi:** UTS • **Bobot RPS:** 25% • **Luaran:** laporan, repository, dan demo modul front-end

---

## 1. 🎯 Tujuan Evaluasi

UTS mengukur kemampuan mengubah masalah pengguna menjadi modul front-end yang koheren. Mahasiswa harus menunjukkan hubungan yang dapat ditelusuri antara kebutuhan, wireframe, UI, navigasi, state, form, dan bukti pengujian.

Setelah evaluasi, kamu mampu mempertahankan keputusan desain, mendemonstrasikan alur tanpa crash, menjelaskan struktur kode, serta menyusun rencana perbaikan menuju integrasi backend.

---

## 2. 📖 Skenario

Tim bertindak sebagai pengembang produk mobile. Stakeholder tidak hanya ingin melihat layar yang menarik; mereka ingin memastikan masalah pengguna dipahami, alur utama dapat dijalankan, input ditangani, dan fondasi teknis siap dihubungkan ke API.

---

## 3. Ruang Lingkup Wajib

1. Problem statement dan pengguna sasaran.
2. Minimal tiga user story beserta acceptance criteria.
3. Prioritas MVP dan batasan scope.
4. User flow dan wireframe.
5. Minimal lima screen terhubung.
6. State management untuk satu data lintas komponen.
7. Form dengan minimal tiga jenis validasi.
8. Loading, empty, error, dan success state melalui simulasi/fake repository.
9. Theme dan komponen yang konsisten.
10. Bukti smoke test dan repository Git.

Backend nyata belum wajib. Mock/fake harus diberi label jelas dan tidak boleh ditampilkan seolah-olah transaksi sungguhan.

---

## 4. Struktur Laporan

```text
1. Ringkasan masalah dan pengguna
2. Tujuan dan batasan MVP
3. Hasil analisis kebutuhan
4. User flow dan wireframe
5. Implementasi screen dan navigasi
6. Strategi state dan form validation
7. Struktur source code
8. Skenario dan hasil pengujian
9. Keterbatasan serta technical debt
10. Rencana integrasi API/database
11. Tautan repository dan petunjuk menjalankan
```

Gunakan screenshot sebagai bukti, lalu jelaskan apa yang dibuktikan. Screenshot tanpa narasi bukan analisis.

---

## 5. Alur Demo 7–10 Menit

```text
Masalah (1 menit)
→ User flow dan keputusan UI (1 menit)
→ Alur utama end-to-end (3 menit)
→ Error/validation state (1 menit)
→ Struktur state dan kode (2 menit)
→ Keterbatasan dan rencana berikutnya (1 menit)
```

Siapkan data demo, perangkat cadangan bila memungkinkan, dan video singkat sebagai fallback. Video bukan pengganti source code yang dapat diperiksa.

---

## 6. 🏦 Skenario Uji Fintech/Backend

Untuk proyek yang memiliki pemesanan, pembayaran, atau perubahan status, demonstrasikan minimal:

- submit cepat berulang tidak menggandakan state;
- Back tidak kembali ke langkah yang berbahaya;
- data sensitif tidak terlihat di log atau UI yang tidak tepat;
- error simulasi tidak menghapus seluruh input;
- status berasal dari satu sumber state.

---

## 7. Rubrik UTS

| Komponen | Bobot | Bukti utama |
|---|---:|---|
| Analisis masalah dan kebutuhan | 15% | problem statement, user story, MVP |
| Wireframe, UI, dan konsistensi | 20% | desain dan implementasi |
| Navigasi dan alur | 20% | seluruh route dan perilaku Back |
| State management dan form | 20% | source code dan demo perubahan state |
| Kualitas kode/repository | 10% | struktur, commit, README awal |
| Pengujian dan penanganan error | 10% | checklist dan bukti |
| Presentasi dan argumentasi | 5% | demo runtut dan jawaban |

### Level Kinerja

- **Sangat baik:** alur lengkap, keputusan beralasan, state konsisten, error diuji, dan kode dapat dijelaskan.
- **Baik:** alur utama berjalan dengan sedikit kekurangan nonkritis.
- **Cukup:** beberapa bagian bekerja tetapi integrasi/state belum konsisten.
- **Kurang:** alur utama gagal, bukti minim, atau mahasiswa tidak memahami kode.

---

## 8. ⚠️ Pelanggaran dan Kesalahan Umum

- Repository tidak dapat dijalankan karena dependency/petunjuk hilang.
- Menggunakan template atau kode pihak lain tanpa atribusi.
- Menampilkan data pribadi atau credential dalam repository.
- Anggota tim tidak memahami kontribusi.
- Demo hanya happy path.
- Fitur banyak tetapi alur inti tidak selesai.

Integritas akademik berlaku pada kode, desain, laporan, aset, dan penggunaan bantuan AI. Cantumkan sumber dan pastikan tim mampu menjelaskan hasil.

---

## 9. Checklist Pengumpulan

- [ ] Laporan PDF/Markdown sesuai struktur.
- [ ] URL repository dapat diakses dosen.
- [ ] README berisi setup dan cara menjalankan.
- [ ] Tidak ada secret/credential.
- [ ] Commit history tersedia.
- [ ] Wireframe dan user flow terbaca.
- [ ] Demo alur utama dan error state siap.
- [ ] Kontribusi anggota dicatat.

---

## 10. 📌 Ringkasan

UTS menilai jejak keputusan dari masalah menuju implementasi. Produk yang kecil namun koheren, dapat dijalankan, diuji, dan dijelaskan lebih kuat daripada banyak layar yang tidak terintegrasi.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Artefak | Bukti hasil proses pembelajaran |
| Technical debt | Konsekuensi keputusan teknis yang perlu diperbaiki |
| Traceability | Keterlacakan kebutuhan hingga implementasi/test |
| UTS | Evaluasi tengah semester |

## 12. Referensi

- RPS Pemrograman Mobile MU5502, Evaluasi Pertemuan 8.
- Flutter, **App architecture**, <https://docs.flutter.dev/app-architecture>

