# Pertemuan 16: UAS Mobile Application Final Report dan Demo

> **Evaluasi:** UAS • **Bobot RPS:** 40% • **Luaran:** aplikasi, APK, laporan, repository, dan presentasi

---

## 1. 🎯 Tujuan Evaluasi

UAS mengukur kemampuan merancang, membangun, mengintegrasikan, menguji, mendokumentasikan, dan mendistribusikan aplikasi mobile untuk layanan nyata. Produk harus menunjukkan alur fungsional, integrasi API atau basis data, satu fitur perangkat, kualitas error handling, dan tanggung jawab keamanan dasar.

---

## 2. 📖 Skenario

Tim melakukan handover produk kepada stakeholder teknis. Stakeholder harus dapat memahami masalah, memasang aplikasi, menjalankan alur utama, memeriksa source code, meninjau test, dan mengetahui keterbatasan. Demo bukan pertunjukan layar; demo adalah pembuktian klaim.

---

## 3. Capaian Wajib

### Produk

- minimal satu alur utama end-to-end;
- UI konsisten dan navigasi benar;
- state/form tervalidasi;
- integrasi GET dan operasi tulis atau Firebase;
- local storage/cache yang relevan;
- minimal satu device feature;
- loading, empty, error, offline, dan success state;
- APK release dapat diinstal.

### Engineering

- struktur kode dapat dijelaskan;
- tidak ada credential sensitif di repository;
- error dan lifecycle ditangani;
- test plan, automated test dasar, dan hasil uji tersedia;
- README memungkinkan reproduksi;
- commit history dan kontribusi tim jelas.

### Dokumentasi

- masalah, pengguna, dan scope;
- kebutuhan dan desain;
- arsitektur aplikasi/data;
- kontrak API atau model Firebase;
- privacy/security consideration;
- pengujian dan defect;
- cara build/install;
- keterbatasan dan roadmap.

---

## 4. Struktur Final Report

```text
1. Ringkasan eksekutif
2. Masalah, pengguna, dan tujuan
3. Ruang lingkup dan requirement
4. User flow dan desain UI
5. Arsitektur dan struktur proyek
6. Implementasi fitur inti
7. API, database, dan sinkronisasi
8. Device feature dan permission
9. Keamanan dan privasi
10. Strategi dan hasil pengujian
11. Build, instalasi, dan distribusi
12. Keterbatasan, technical debt, roadmap
13. Kontribusi dan referensi
```

Diagram arsitektur minimum:

```text
Flutter UI
   ↓ events / state
State or ViewModel
   ↓
Repository
   ├── Local storage / SQLite
   └── REST API / Firebase
             ↓
       Backend / Cloud database
```

---

## 5. Alur Demo 10–12 Menit

1. Jelaskan masalah dan nilai produk.
2. Tunjukkan instalasi/identitas build.
3. Jalankan alur utama.
4. Tunjukkan integrasi data dan satu device feature.
5. Demonstrasikan satu error/offline/permission denied state.
6. Tunjukkan struktur kode dan satu test penting.
7. Jelaskan keamanan, keterbatasan, dan roadmap.

Setelah demo ada tanya jawab teknis. Setiap anggota harus mampu menjelaskan kontribusi dan bagian sistem yang terkait.

---

## 6. 🏦 Skenario Verifikasi Industri

Untuk aplikasi yang menangani transaksi/status, penguji dapat melakukan:

- menekan submit cepat berulang;
- memutus jaringan setelah request;
- memindahkan aplikasi ke background;
- login dengan pengguna berbeda;
- mencoba membaca/mengubah data milik akun lain;
- menolak permission;
- membuka aplikasi dari notifikasi/deep link;
- menutup dan membuka ulang aplikasi;
- memasukkan nilai batas dan tidak valid.

Produk harus gagal dengan aman: tidak crash, tidak membocorkan data, tidak menggandakan operasi, dan memberi status yang jujur.

---

## 7. Rubrik UAS

| Komponen | Bobot |
|---|---:|
| Kesesuaian masalah, requirement, dan nilai produk | 10% |
| UI, UX, navigasi, state, dan form | 15% |
| Integrasi API/database serta konsistensi data | 20% |
| Device feature dan permission handling | 10% |
| Error handling, lifecycle, keamanan, privasi | 15% |
| Testing dan kualitas kode | 10% |
| APK, repository, README, final report | 10% |
| Demo, argumentasi, dan kontribusi tim | 10% |

### Kriteria Kelulusan Teknis Minimum

- aplikasi dapat dipasang atau dijalankan sesuai instruksi;
- alur utama tidak mengalami crash;
- data rahasia tidak disertakan dalam repository;
- tim dapat menjelaskan kode yang dikumpulkan;
- artefak dan atribusi lengkap.

---

## 8. ⚠️ Kesalahan Fatal dan Risiko

- APK tidak dapat dipasang dan tidak ada bukti build alternatif.
- Credential/server key dipublikasikan.
- Data pribadi nyata digunakan tanpa izin.
- Security Rules terbuka atau otorisasi tidak diuji.
- Kode/asset pihak lain tidak diberi atribusi.
- Tim tidak dapat menjelaskan kode yang diserahkan.
- Demo menyembunyikan error kritis yang sudah diketahui.

Temuan keamanan dinilai berdasarkan dampak dan tanggung jawab penanganan, bukan sekadar jumlah fitur.

---

## 9. Checklist Handover

- [ ] APK release dan checksum.
- [ ] Source code final dengan tag/release.
- [ ] README setup, run, test, dan build.
- [ ] `.env.example` tanpa secret jika digunakan.
- [ ] Final report.
- [ ] Test report dan defect/known issues.
- [ ] Postman collection atau dokumentasi Firebase.
- [ ] Diagram arsitektur dan data.
- [ ] Akun/data demo sintetis.
- [ ] Slide dan video fallback.
- [ ] Daftar kontribusi dan referensi.

---

## 10. 🧪 Pertanyaan Viva/Review

1. Mengapa kamu memilih Flutter/native dan apa trade-off-nya?
2. Di mana sumber kebenaran data utama?
3. Apa yang terjadi ketika request timeout setelah backend memprosesnya?
4. Bagaimana mencegah pengguna A mengakses data pengguna B?
5. State apa yang dipulihkan setelah aplikasi dihentikan?
6. Mengapa storage yang dipilih sesuai?
7. Test mana yang paling melindungi bisnis?
8. Apa technical debt paling berisiko?
9. Jika pengguna meningkat 100 kali, bagian apa yang pertama bermasalah?
10. Apa yang perlu dilakukan sebelum benar-benar masuk production?

---

## 11. 📌 Ringkasan Akhir Semester

- Produk mobile yang baik dimulai dari masalah pengguna.
- UI, state, navigasi, data, lifecycle, dan backend merupakan satu sistem.
- Klien tidak boleh menjadi satu-satunya penjaga integritas atau otorisasi.
- Offline, timeout, permission denial, dan process death adalah kondisi normal yang perlu didesain.
- Testing dan dokumentasi adalah bagian produk.
- APK yang dapat dipasang, repository yang bersih, dan demo yang dapat dibuktikan menunjukkan kesiapan engineering.

## 12. Glosarium

| Istilah | Penjelasan |
|---|---|
| Final report | Dokumentasi akhir masalah, desain, implementasi, dan evaluasi |
| Handover | Serah terima produk dan pengetahuan |
| Known issue | Masalah yang diketahui dan didokumentasikan |
| Production readiness | Tingkat kesiapan sistem untuk penggunaan nyata |
| Technical debt | Beban perbaikan akibat kompromi teknis |
| UAS | Evaluasi akhir semester |

## 13. Referensi

- RPS Pemrograman Mobile MU5502, Evaluasi Pertemuan 16.
- Flutter Documentation, <https://docs.flutter.dev/>
- Firebase Documentation, <https://firebase.google.com/docs/flutter>
- Android Developers, <https://developer.android.com/>

