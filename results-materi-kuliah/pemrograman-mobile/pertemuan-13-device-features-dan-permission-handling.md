# Pertemuan 13: Camera Geolocation Notification dan Permission Handling

> **Acuan RPS:** CPMK-4, Sub-CPMK-7 • **Durasi:** 150 menit • **Artefak:** minimal satu fitur perangkat

---

## 1. 🎯 Learning Outcomes

Kamu mampu memilih plugin device, menjelaskan alur permission, menerapkan satu fitur kamera/lokasi/notifikasi, menangani penolakan dan keterbatasan platform, mengelola resource sesuai lifecycle, serta menilai risiko privasi dan keamanan.

---

## 2. 📖 Pengantar

Kamera dan lokasi membuat aplikasi terasa benar-benar mobile, tetapi juga membuka akses ke dunia pribadi pengguna. Permission bukan dialog formalitas. Pengguna berhak memahami manfaat, menolak, dan tetap memperoleh pengalaman alternatif yang masuk akal.

---

## 3. 🧩 Konsep Utama

### 3.1 Package dan Plugin

Package berisi kode yang dapat digunakan ulang. Plugin adalah package yang menjembatani Dart dengan implementasi platform seperti Kotlin atau Swift. Sebelum memilih plugin, periksa:

- platform yang didukung;
- dokumentasi dan contoh;
- aktivitas maintenance dan kompatibilitas;
- lisensi;
- permission yang diminta;
- isu keamanan dan batasan;
- kebutuhan konfigurasi Android/iOS.

### 3.2 Alur Permission yang Benar

```text
Pengguna memicu fitur
→ jelaskan manfaat dalam konteks
→ periksa status permission
→ minta bila perlu
→ granted: jalankan fitur
→ denied: tawarkan alternatif
→ permanently denied: jelaskan cara membuka Settings
```

Jangan meminta seluruh permission saat startup. Minta sedekat mungkin dengan fitur yang membutuhkan.

### 3.3 Kamera

Kamera membutuhkan initialization, preview/controller, capture, penyimpanan sementara, kompresi, dan cleanup. Jangan mengunggah foto tanpa konfirmasi dan kebijakan yang jelas.

### 3.4 Lokasi

Bedakan permission dengan status layanan lokasi. Permission dapat diberikan tetapi GPS dimatikan. Tentukan akurasi yang benar-benar diperlukan; akurasi tinggi mengonsumsi lebih banyak daya. Lokasi background memiliki aturan lebih ketat.

### 3.5 Notifikasi

- **Local notification:** dijadwalkan oleh aplikasi pada perangkat.
- **Push notification:** dikirim layanan/backend melalui infrastruktur push.

Payload notifikasi bukan tempat data sensitif karena dapat muncul di lock screen. Tap notifikasi adalah deep link dan tetap memerlukan pemeriksaan sesi/otorisasi.

### 3.6 Lifecycle dan Resource

Controller kamera, stream lokasi, dan subscription harus dihentikan/dilepas. Saat app background, pertimbangkan menghentikan preview atau stream untuk privasi dan baterai.

---

## 4. 🧠 Analogi

Permission seperti kunci kamar hotel. Izin masuk ke kamar tidak berarti boleh membuka brankas, mengambil foto, atau menyimpan kunci selamanya. Setiap kemampuan memiliki tujuan dan batas.

---

## 5. 💻 Contoh Teknis Konseptual

Contoh berikut memakai pola API `permission_handler`. Tambahkan hanya plugin yang benar-benar diperlukan proyek dan baca instruksi konfigurasi platformnya:

```bash
flutter pub add permission_handler
# Tambahkan camera, geolocator, atau plugin notifikasi sesuai fitur pilihan.
```

```dart
Future<void> openCameraFeature() async {
  final status = await Permission.camera.status;

  if (status.isGranted) {
    await initializeCamera();
    return;
  }

  final result = await Permission.camera.request();
  if (result.isGranted) {
    await initializeCamera();
  } else if (result.isPermanentlyDenied) {
    showSettingsExplanation();
  } else {
    showManualUploadAlternative();
  }
}
```

Kode bersifat pola; API mengikuti plugin yang dipilih. Tambahkan deskripsi permission di konfigurasi platform, uji pada perangkat fisik, dan jangan berasumsi simulator mempunyai semua kemampuan.

### Abstraksi agar Dapat Diuji

```dart
abstract interface class LocationService {
  Future<bool> requestAccess();
  Future<GeoPoint> currentPosition();
}
```

UI bergantung pada interface sehingga test dapat memakai fake location service tanpa GPS sungguhan.

---

## 6. 🏦 Studi Kasus Fintech

### Verifikasi Identitas Meminta Kamera Tanpa Penjelasan

**Masalah:** dialog kamera muncul saat aplikasi dibuka. Banyak pengguna menolak. Saat ditolak permanen, aplikasi hanya menampilkan spinner.

**Dampak:** onboarding gagal, conversion turun, dan pengguna mencurigai aplikasi.

**Solusi:** minta kamera ketika pengguna memilih scan identitas, jelaskan tujuan dan retensi, berikan upload alternatif jika kebijakan memungkinkan, tangani semua status permission, hapus file sementara, gunakan koneksi aman, serta batasi akses backend. Jangan menulis foto identitas ke log atau galeri tanpa persetujuan.

---

## 7. 📊 Aktivitas 150 Menit

| Waktu | Aktivitas |
|---:|---|
| 20 | Plugin, platform channel, risiko |
| 25 | Permission UX dan privacy |
| 25 | Demo kamera/lokasi/notifikasi |
| 55 | Integrasi satu fitur proyek |
| 15 | Uji denied/permanently denied/background |
| 10 | Review privacy checklist |

---

## 8. ⚠️ Kesalahan Umum

- Semua permission diminta di startup.
- Tidak menyediakan perilaku ketika ditolak.
- Menganggap permission granted berarti layanan tersedia.
- Controller/stream tidak dilepas.
- Payload notifikasi memuat data sensitif.
- Deep link notifikasi tidak memeriksa auth.
- Plugin dipilih tanpa memeriksa dukungan platform.
- Hanya diuji pada emulator.

---

## 9. 🧪 Latihan dan Penilaian

Implementasikan satu fitur perangkat yang relevan. Wajib menunjukkan alasan penggunaan, happy path, permission denied, permanently denied/settings guidance, lifecycle cleanup, dan bukti pada perangkat/emulator yang sesuai.

**Studi kasus:** rancang fitur lokasi kurir. Tentukan kapan lokasi diambil, akurasi, alternatif ketika ditolak, informasi kepada pengguna, dan data yang dikirim ke backend.

Rubrik: fungsi 30%, permission handling 25%, lifecycle/resource 15%, privasi/keamanan 20%, relevansi UX 10%.

---

## 10. 📌 Ringkasan

- Plugin menjembatani Dart dengan API platform.
- Permission diminta dalam konteks dengan tujuan jelas.
- Penolakan adalah kondisi normal yang harus didesain.
- Device resource harus dikelola sesuai lifecycle.
- Gunakan akses minimum dan lindungi data sensitif.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Device feature | Kemampuan perangkat seperti kamera/GPS |
| Local notification | Notifikasi yang dijadwalkan pada perangkat |
| Permission | Persetujuan akses kemampuan/data tertentu |
| Platform channel | Jembatan pesan Dart dan kode native |
| Plugin | Package dengan implementasi platform |
| Push notification | Notifikasi yang dikirim melalui layanan/backend |

## 12. Referensi

- Flutter, **Plugin cookbook**, <https://docs.flutter.dev/cookbook/plugins>
- Flutter, **Using packages**, <https://docs.flutter.dev/packages-and-plugins/using-packages>
- Android Developers, **Permissions**, <https://developer.android.com/guide/topics/permissions/overview>
