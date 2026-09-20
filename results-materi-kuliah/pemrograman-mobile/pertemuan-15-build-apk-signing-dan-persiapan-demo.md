# Pertemuan 15: Build APK Signing dan Persiapan Demo

> **Acuan RPS:** CPMK-5, Sub-CPMK-8 • **Durasi:** 150 menit • **Artefak:** APK, final repository, dan bahan demo

---

## 1. 🎯 Learning Outcomes

Kamu mampu membedakan debug/profile/release, mengelola versioning, membangun APK/AAB, menjelaskan signing key, menjaga secret build, menguji artefak release, serta menyiapkan demo dan rollback plan sederhana.

---

## 2. 📖 Pengantar

Aplikasi yang berjalan dengan `flutter run` belum siap dibagikan. Release build memiliki optimasi, konfigurasi, signing, permission, dan koneksi layanan yang dapat berbeda dari debug. Banyak bug baru terlihat setelah artefak release dipasang pada perangkat bersih.

---

## 3. 🧩 Konsep Utama

### 3.1 Build Mode

| Mode | Tujuan |
|---|---|
| Debug | Pengembangan, assertion, hot reload |
| Profile | Analisis performa |
| Release | Distribusi, optimasi, tanpa fasilitas debug |

### 3.2 APK dan AAB

- **APK:** paket yang dapat dipasang langsung.
- **AAB:** app bundle untuk distribusi store; store menghasilkan APK yang sesuai perangkat.

Google Play lebih menyukai app bundle. RPS meminta APK sebagai artefak yang dapat diinstal; proyek dapat menghasilkan keduanya sesuai kebutuhan.

### 3.3 Versioning

Pada `pubspec.yaml`:

```yaml
version: 1.0.0+1
```

Bagian sebelum `+` adalah versi yang terlihat; angka setelahnya adalah build number. Setiap rilis store memerlukan build number yang meningkat.

### 3.4 Signing

Signing membuktikan identitas penerbit dan memungkinkan update. Kehilangan akses key dapat menghambat pembaruan. Jangan commit keystore, password, atau `key.properties` ke repository publik. Simpan backup terenkripsi dan batasi akses.

### 3.5 Environment

Pisahkan konfigurasi dev/staging/prod. Pastikan release tidak menunjuk API lokal, emulator Firebase, logging sensitif, atau endpoint pengujian.

### 3.6 Release Checklist

```text
[ ] analyze dan test lulus
[ ] version/build number benar
[ ] icon, nama, permission, endpoint benar
[ ] secret tidak masuk repo/artefak
[ ] release build berhasil
[ ] install pada perangkat bersih
[ ] alur kritis dan offline diuji
[ ] crash/log diperiksa
[ ] APK/AAB checksum dan lokasi dicatat
```

---

## 4. 🧠 Analogi

Debug build seperti prototipe di bengkel. Release build seperti kendaraan yang akan dipakai di jalan: identitas, pemeriksaan, konfigurasi, dan pengujian akhir wajib lengkap.

---

## 5. 💻 Perintah Build

```bash
flutter clean
flutter pub get
flutter analyze
flutter test
flutter build apk --release
flutter build apk --split-per-abi
flutter build appbundle
```

Lokasi umum:

```text
build/app/outputs/flutter-apk/
build/app/outputs/bundle/release/app.aab
```

Pasang pada perangkat terhubung:

```bash
flutter install
```

Jangan membagikan key signing bersama APK. Instruksi signing detail mengikuti dokumentasi resmi dan sistem operasi yang digunakan.

### Rencana Demo

```text
1. Masalah dan pengguna (30 detik)
2. Arsitektur ringkas (30 detik)
3. Alur utama (3 menit)
4. API/database/device feature (2 menit)
5. Error handling dan test (1 menit)
6. Keterbatasan dan roadmap (1 menit)
```

Gunakan data sintetis, bukan data pribadi nyata.

---

## 6. 🏦 Studi Kasus Fintech

### Release Mengarah ke Server Staging

**Masalah:** build produksi memakai base URL staging dan logging request lengkap.

**Dampak:** data pengguna masuk lingkungan salah dan token berpotensi tercatat.

**Solusi:** environment configuration eksplisit, build-time validation, pipeline terpisah, secret manager, log redaction, smoke test release, dan approval checklist. Mobile dan backend harus menyepakati versi API serta mekanisme rollback/feature flag.

---

## 7. 📊 Aktivitas 150 Menit

20 menit build mode/artefak, 20 menit version/signing, 20 menit release security, 55 menit build dan install, 20 menit rehearsal, 15 menit peer feedback.

---

## 8. ⚠️ Kesalahan Umum

- Hanya menguji debug build.
- Keystore/password di-commit.
- Build number tidak dinaikkan.
- Release menunjuk emulator atau `localhost`.
- Permission berlebihan tetap ada.
- Demo memakai data sensitif.
- Tidak menyiapkan fallback saat jaringan/perangkat bermasalah.

---

## 9. 🧪 Tugas dan Penilaian

Serahkan APK yang dapat diinstal, hash/checksum, repository final sementara, README, hasil test, release checklist, dan slide/demo script. Dosen menguji instalasi serta alur kritis pada perangkat yang ditentukan.

Rubrik: build/install 30%, konfigurasi/signing hygiene 20%, test release 20%, dokumentasi 15%, rehearsal/demo 15%.

---

## 10. 📌 Ringkasan

- Release build berbeda dari debug.
- APK untuk instalasi; AAB umum untuk store.
- Signing key dan secret wajib dilindungi.
- Uji artefak final pada perangkat bersih.
- Demo harus menunjukkan nilai, bukti teknis, error handling, dan keterbatasan.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| AAB | Android App Bundle untuk distribusi store |
| APK | Paket aplikasi Android yang dapat dipasang |
| Build number | Nomor meningkat untuk membedakan build |
| Keystore | Penyimpanan key untuk signing |
| Release build | Build teroptimasi untuk distribusi |
| Signing | Penandatanganan digital artefak aplikasi |

## 12. Referensi

- Flutter, **Build and release an Android app**, <https://docs.flutter.dev/deployment/android>
- Android Developers, **Sign your app**, <https://developer.android.com/studio/publish/app-signing>

