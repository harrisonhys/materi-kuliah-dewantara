# Pertemuan 12: Firebase Authentication dan Cloud Firestore

> **Acuan RPS:** CPMK-3, Sub-CPMK-6 • **Durasi:** 150 menit • **Artefak:** autentikasi dan data cloud tersinkron

---

## 1. 🎯 Learning Outcomes

Kamu mampu mengonfigurasi FlutterFire, menginisialisasi Firebase, memakai authentication state, membaca/menulis Firestore, merancang collection/document sederhana, menulis prinsip security rules, serta membedakan autentikasi, otorisasi, dan App Check.

---

## 2. 📖 Pengantar

Firebase mempercepat prototipe backend, tetapi “berhasil tersambung” bukan berarti aman. Jika rules terbuka, pengguna dapat membaca atau mengubah data orang lain langsung dari aplikasi klien. Keamanan harus menjadi bagian desain data.

---

## 3. 🧩 Konsep Utama

### 3.1 Komponen

- **Firebase Authentication:** identitas dan sesi pengguna.
- **Cloud Firestore:** database dokumen dengan realtime listener dan dukungan offline.
- **Security Rules:** otorisasi dan validasi untuk client mobile/web.
- **App Check:** membantu mengurangi akses dari klien tidak sah; bukan pengganti Auth/Rules.
- **Local Emulator Suite:** pengujian lokal tanpa menyentuh production.

### 3.2 Authentication vs Authorization

Authentication menjawab “siapa kamu?” Authorization menjawab “apa yang boleh kamu lakukan?” Pengguna yang login tidak otomatis boleh membaca seluruh collection.

### 3.3 Model Data

```text
users/{uid}
  displayName
  role

users/{uid}/bookings/{bookingId}
  serviceId
  status
  createdAt
```

Model mengikuti pola query. Firestore bukan SQL; denormalisasi terkontrol sering digunakan, tetapi data kritis membutuhkan strategi konsistensi.

### 3.4 Listener Realtime

Listener menghasilkan stream perubahan. Kelola loading/error dan lepaskan subscription sesuai lifecycle. Pertimbangkan biaya read serta apakah realtime benar-benar diperlukan.

### 3.5 Security Rules

Default aman adalah menolak, lalu buka akses minimum. Jangan memakai rule `allow read, write: if true` pada produksi.

---

## 4. 🧠 Analogi

Authentication seperti kartu identitas. Security Rules seperti petugas akses per ruangan. App Check seperti pemeriksaan bahwa permintaan datang dari aplikasi yang dikenali. Satu lapisan tidak menggantikan yang lain.

---

## 5. 💻 Contoh Teknis

```bash
firebase login
dart pub global activate flutterfire_cli
flutterfire configure
flutter pub add firebase_core firebase_auth cloud_firestore
```

```dart
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  runApp(const MyApp());
}
```

Auth state:

```dart
StreamBuilder<User?>(
  stream: FirebaseAuth.instance.authStateChanges(),
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return const CircularProgressIndicator();
    }
    return snapshot.data == null ? const LoginPage() : const HomePage();
  },
)
```

Write dengan owner:

```dart
final uid = FirebaseAuth.instance.currentUser!.uid;
await FirebaseFirestore.instance
    .collection('users').doc(uid)
    .collection('bookings').add({
  'serviceId': 'general',
  'status': 'pending',
  'createdAt': FieldValue.serverTimestamp(),
});
```

Contoh konsep rule:

```javascript
match /users/{userId}/bookings/{bookingId} {
  allow read, write: if request.auth != null
                     && request.auth.uid == userId;
}
```

Rules harus diuji, termasuk skenario pengguna A mencoba data pengguna B.

---

## 6. 🏦 Studi Kasus Fintech

### Collection Transaksi Dapat Dibaca Semua Pengguna

**Masalah:** developer mengaktifkan rule terbuka saat demo dan lupa memperketatnya.

**Dampak:** kebocoran PII dan transaksi, risiko regulasi, serta hilangnya kepercayaan.

**Solusi:** deny by default, ownership/role checks, validasi field, test Rules di emulator, lingkungan dev/staging/prod terpisah, App Check, audit, dan log terkontrol. Operasi finansial kritis sebaiknya melalui trusted backend, bukan mempercayai nilai dari klien.

---

## 7. 📊 Aktivitas 150 Menit

20 menit arsitektur Firebase, 20 menit konfigurasi, 25 menit Auth, 25 menit Firestore, 20 menit Rules/emulator, 30 menit integrasi proyek, 10 menit uji akses negatif.

---

## 8. ⚠️ Kesalahan Umum

- Rules terbuka.
- Menyamakan login dengan otorisasi.
- Menaruh service account key di aplikasi.
- Mempercayai role yang dikirim klien.
- Listener realtime dibiarkan tanpa pengendalian.
- Menggunakan waktu perangkat untuk audit kritis.
- Menguji hanya akun pemilik data.

---

## 9. 🧪 Latihan dan Penilaian

Implementasikan login/logout, auth gate, satu write dan read Firestore, state loading/error, serta rules ownership. Buktikan pengguna A ditolak membaca data B. Gunakan emulator bila tersedia dan sanitasi screenshot console.

Rubrik: setup/Auth 20%, model/read-write 25%, state UI 15%, rules dan negative test 30%, kualitas kode 10%.

---

## 10. 📌 Ringkasan

- Auth membuktikan identitas; Rules menentukan izin.
- Firestore menyimpan dokumen dan mendukung listener realtime.
- Model data mengikuti kebutuhan query.
- Rules harus deny-by-default dan diuji negatif.
- Jangan menaruh credential server di aplikasi klien.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| App Check | Perlindungan tambahan terhadap akses dari klien tidak sah |
| Collection | Kelompok document Firestore |
| Document | Unit data utama Firestore |
| Firebase Auth | Layanan autentikasi Firebase |
| Security Rules | Aturan akses/validasi client Firebase |

## 12. Referensi

- Firebase, **Get started with Firebase in Flutter**, <https://firebase.google.com/docs/flutter/setup>
- Firebase, **Authentication for Flutter**, <https://firebase.google.com/docs/auth/flutter/start>
- Firebase, **Secure data in Cloud Firestore**, <https://firebase.google.com/docs/firestore/security/overview>
