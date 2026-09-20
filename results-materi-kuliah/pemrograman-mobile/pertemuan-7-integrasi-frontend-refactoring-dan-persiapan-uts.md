# Pertemuan 7: Integrasi Front-End, Refactoring, dan Persiapan UTS

> **Acuan RPS:** Sub-CPMK 1–4 • **Durasi:** 150 menit • **Artefak:** modul front-end koheren dan draft laporan UTS

---

## 1. 🎯 Learning Outcomes

Kamu mampu mengintegrasikan UI, navigasi, state, dan form; menemukan inkonsistensi antar-screen; melakukan refactoring tanpa mengubah perilaku; menyusun struktur proyek; menjalankan smoke test; dan menyiapkan bukti capaian untuk UTS.

---

## 2. 📖 Pengantar

Fitur yang bekerja sendiri belum tentu bekerja sebagai produk. Screen login, beranda, dan form dapat lolos demo terpisah tetapi gagal ketika disambungkan: state hilang, Back salah, tema berbeda, atau data tidak ikut berubah. Integrasi adalah saat asumsi tersembunyi mulai terlihat.

---

## 3. 🧩 Konsep Utama

### 3.1 Definition of Done Front-End

Sebuah alur dianggap selesai ketika:

- sesuai user flow;
- seluruh route dapat dicapai dan kembali dengan benar;
- state konsisten;
- form tervalidasi;
- loading/empty/error/success tersedia;
- tidak crash pada skenario utama;
- struktur kode dapat dijelaskan;
- bukti pengujian tercatat.

### 3.2 Vertical Slice

Integrasikan satu alur dari awal sampai selesai, misalnya pilih layanan → isi form → konfirmasi → hasil simulasi. Vertical slice lebih mudah dievaluasi daripada banyak layar tanpa hubungan.

### 3.3 Refactoring

Refactoring mengubah struktur internal tanpa mengubah perilaku eksternal. Kandidat:

- widget terlalu panjang;
- style berulang;
- string route tersebar;
- business logic berada di UI;
- state diduplikasi;
- penamaan tidak menjelaskan maksud.

Lakukan perubahan kecil, jalankan test/smoke test, lalu commit. Jangan mencampur refactoring besar dengan penambahan fitur besar.

### 3.4 Struktur Proyek Awal

```text
lib/
├── app.dart
├── core/
│   ├── theme/
│   └── widgets/
├── features/
│   ├── auth/
│   ├── home/
│   └── booking/
└── main.dart
```

Struktur harus membantu menemukan kode. Jangan membuat lapisan abstraksi kosong hanya agar terlihat “enterprise”.

### 3.5 Smoke Test Manual

```text
[ ] aplikasi terbuka tanpa crash
[ ] alur utama selesai
[ ] Back menghasilkan tujuan benar
[ ] input salah menampilkan pesan
[ ] tombol tidak mengirim ganda
[ ] state penting tetap konsisten
[ ] layar kecil dan keyboard tidak merusak layout
```

---

## 4. 🧠 Analogi

Integrasi seperti latihan pertunjukan lengkap. Setiap pemain mungkin mahir, tetapi pergantian adegan, urutan masuk, dan koordinasi baru dapat dinilai ketika seluruh pertunjukan dijalankan.

---

## 5. 💻 Contoh Teknis: Memisahkan Presentasi dan Logika

Sebelum:

```dart
onPressed: () {
  if (amount > 0) {
    items.add(amount);
    total = items.fold(0, (a, b) => a + b);
    setState(() {});
  }
}
```

Sesudah:

```dart
class PaymentDraft extends ChangeNotifier {
  final List<int> _items = [];
  int get total => _items.fold(0, (a, b) => a + b);

  void addAmount(int amount) {
    if (amount <= 0) throw ArgumentError('Amount harus positif');
    _items.add(amount);
    notifyListeners();
  }
}
```

UI menangani presentasi; model menjaga aturan state. Setelah refactor, jalankan kembali alur yang sama untuk memastikan perilaku tidak berubah.

---

## 6. 🏦 Studi Kasus Backend/Fintech

### Status Pembayaran Tidak Konsisten Antar-Screen

**Masalah:** daftar transaksi menampilkan `pending`, tetapi detail lokal diubah menjadi `success` tanpa sumber bersama.

**Dampak:** pengguna melihat dua status berbeda dan support sulit menentukan kondisi sebenarnya.

**Solusi:** satu model/repository untuk status, definisi enum terpusat, refresh dari backend, mapping status yang konsisten, dan timestamp sinkronisasi. Pada tahap front-end, gunakan fake repository agar kontrak state sudah benar sebelum API tersedia.

---

## 7. 📊 Workshop 150 Menit

| Waktu | Aktivitas |
|---:|---|
| 15 | Audit terhadap Definition of Done |
| 20 | Demo refactoring aman |
| 75 | Integrasi dan konsultasi proyek |
| 20 | Peer test menggunakan checklist |
| 15 | Perbaikan prioritas tinggi |
| 5 | Freeze scope UTS |

---

## 8. ⚠️ Kesalahan Umum

- Menambah fitur baru ketika alur inti belum stabil.
- Refactor besar tanpa checkpoint Git.
- Menilai hanya happy path.
- Mengubah desain ketika implementasi sudah dekat UTS tanpa alasan kuat.
- Demo bergantung pada urutan tersembunyi yang tidak didokumentasikan.
- Laporan berisi screenshot tanpa menjelaskan keputusan dan bukti.

---

## 9. 🧪 Tugas dan Penilaian

Kumpulkan draft laporan: masalah/pengguna, user flow, wireframe, daftar screen, arsitektur state, skenario uji, keterbatasan, dan tautan commit/repository. Lampirkan video maksimal lima menit yang memperlihatkan alur utama serta satu validasi gagal.

**Peer review:** tiap kelompok menjalankan proyek kelompok lain dan mencatat blocker, major issue, minor issue, serta saran.

Rubrik kesiapan: integrasi alur 30%, navigasi 20%, state/form 20%, kualitas struktur 15%, bukti dan dokumentasi 15%.

---

## 10. 📌 Ringkasan

- Integrasi memvalidasi asumsi antarfitur.
- Fokuskan pada vertical slice yang selesai.
- Refactoring menjaga perilaku sambil memperbaiki struktur.
- Test ulang setelah perubahan.
- UTS menilai produk dan alasan desain, bukan banyaknya layar.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Definition of Done | Kriteria agar pekerjaan dianggap selesai |
| Fake repository | Implementasi sederhana untuk simulasi sumber data |
| Refactoring | Perbaikan struktur tanpa mengubah perilaku |
| Smoke test | Pemeriksaan cepat fungsi kritis |
| Vertical slice | Alur fitur lengkap dari UI sampai state/data |

## 12. Referensi

- Flutter, **App architecture**, <https://docs.flutter.dev/app-architecture>
- Flutter, **State management**, <https://docs.flutter.dev/data-and-backend/state-mgmt>
- RPS Pemrograman Mobile MU5502, Pertemuan 7.

