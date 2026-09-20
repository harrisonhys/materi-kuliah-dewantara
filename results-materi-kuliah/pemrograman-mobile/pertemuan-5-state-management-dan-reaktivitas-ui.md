# Pertemuan 5: State Management dan Reaktivitas UI

> **Acuan RPS:** CPMK-2, Sub-CPMK-4 • **Durasi:** 150 menit • **Artefak:** UI yang otomatis mengikuti perubahan state

---

## 1. 🎯 Learning Outcomes

Kamu mampu mendefinisikan state, membedakan ephemeral dan app state, menerapkan single source of truth, mengelola loading/data/error, memakai `setState` secara tepat, serta memisahkan UI dari business logic sederhana dengan `ChangeNotifier`.

---

## 2. 📖 Pengantar

Ketika jumlah keranjang berubah, badge, total harga, dan tombol checkout harus ikut berubah. Jika tiap widget menyimpan salinan sendiri, data cepat tidak konsisten. State management menjawab dua pertanyaan: **siapa pemilik data** dan **siapa yang perlu diberi tahu ketika data berubah**.

---

## 3. 🧩 Konsep Utama

### 3.1 State sebagai Snapshot

State adalah data yang memengaruhi perilaku atau tampilan pada suatu waktu. UI deklaratif mengikuti prinsip:

```text
UI = f(state)
event → ubah state → framework memperbarui UI
```

### 3.2 Ephemeral vs App State

| Jenis | Contoh | Pemilik yang wajar |
|---|---|---|
| Ephemeral/local | tab aktif, password terlihat, animasi | widget/screen |
| App/shared | sesi pengguna, keranjang, transaksi aktif | model/store di atas pemakai |
| Server state | profil, saldo, status transaksi | backend; aplikasi menyimpan representasi/cache |

Saldo pada UI bukan sumber kebenaran. Ia adalah representasi data backend yang dapat menjadi usang.

### 3.3 Single Source of Truth

Simpan fakta pada satu lokasi otoritatif. Nilai turunan seperti total sebaiknya dihitung dari item, bukan disimpan sebagai salinan yang harus disinkronkan.

### 3.4 State Async

Permintaan jaringan setidaknya memiliki keadaan:

```text
initial → loading → success(data)
                  └→ empty
                  └→ error(message, canRetry)
```

Boolean `isLoading` saja sering tidak cukup karena kombinasi state dapat menjadi ambigu.

### 3.5 `setState` dan Lifting State Up

Gunakan `setState` untuk state lokal yang kecil. Jika dua cabang widget memerlukan data sama, naikkan state ke common ancestor. Untuk state lintas banyak screen, gunakan model yang terpisah dan mekanisme seperti `ChangeNotifier`/Provider atau alternatif yang dipilih tim.

### 3.6 Immutability dan Perubahan Terprediksi

Objek immutable membuat perubahan lebih mudah dilacak. Alih-alih mengubah list diam-diam, buat operasi eksplisit seperti `addItem`, `removeItem`, dan `clear`.

---

## 4. 🧠 Analogi

State seperti papan skor resmi. Semua penonton melihat satu skor yang sama. Jika setiap tribun menulis skor sendiri, hasilnya bertentangan. `notifyListeners()` seperti petugas yang mengumumkan bahwa papan resmi telah berubah.

---

## 5. 💻 Contoh Teknis

Tambahkan dependency:

```bash
flutter pub add provider
```

Model:

```dart
import 'package:flutter/foundation.dart';

class CartModel extends ChangeNotifier {
  final List<int> _prices = [];

  List<int> get prices => List.unmodifiable(_prices);
  int get total => _prices.fold(0, (sum, price) => sum + price);

  void add(int price) {
    if (price <= 0) return;
    _prices.add(price);
    notifyListeners();
  }

  void clear() {
    _prices.clear();
    notifyListeners();
  }
}
```

Menyediakan dan membaca state:

```dart
void main() {
  runApp(
    ChangeNotifierProvider(
      create: (_) => CartModel(),
      child: const MyApp(),
    ),
  );
}

// Hanya bagian ini rebuild ketika cart berubah.
Consumer<CartModel>(
  builder: (_, cart, __) => Text('Total: Rp${cart.total}'),
)

// Aksi tanpa mendengarkan perubahan.
onPressed: () => context.read<CartModel>().add(25000),
```

Aturan praktis: tempatkan listener sedalam mungkin agar perubahan kecil tidak membangun ulang seluruh halaman.

---

## 6. 🏦 Studi Kasus Fintech

### Saldo Berbeda pada Dua Screen

**Masalah:** beranda dan screen transfer menyimpan salinan saldo. Setelah transfer, screen transfer mengurangi saldonya tetapi beranda masih menunjukkan nilai lama.

**Dampak:** pengguna ragu apakah transaksi berhasil dan dapat mencoba ulang.

**Solusi:** backend menjadi sumber kebenaran; model akun menyimpan snapshot beserta waktu sinkronisasi; transaksi sukses memicu refresh/reconciliation; UI membedakan loading, data lama, dan error. Untuk optimisme UI, perubahan harus dapat dibatalkan jika backend menolak.

---

## 7. 📊 Aktivitas 150 Menit

| Waktu | Aktivitas |
|---:|---|
| 25 | State, event, dan UI deklaratif |
| 20 | Ephemeral, app, dan server state |
| 20 | Demo `setState` dan lifting state |
| 25 | Demo `ChangeNotifier`/Provider |
| 45 | Praktik state proyek |
| 15 | Profil rebuild dan peer review |

---

## 8. ⚠️ Kesalahan Umum

- Menyalin state ke banyak widget.
- Mengubah list tanpa `notifyListeners()`.
- Memanggil `notifyListeners()` terlalu sering.
- Menyimpan nilai turunan yang dapat dihitung.
- Menjadikan seluruh state global.
- Menganggap cache mobile sama dengan data resmi backend.
- Mengabaikan error dan empty state.

---

## 9. 🧪 Latihan dan Penilaian

1. Klasifikasikan tab aktif, token sesi, saldo, dan status transaksi sebagai local/app/server state.
2. Buat state untuk fitur inti proyek dengan minimal empat keadaan async.
3. Pastikan dua widget berbeda bereaksi terhadap model yang sama tanpa reload manual.
4. Jelaskan bagaimana aplikasi memulihkan state ketika dibuka kembali.

Rubrik: perubahan state benar 30%, single source of truth 25%, pemisahan UI/model 20%, handling state async 15%, keterbacaan 10%.

---

## 10. 📌 Ringkasan

- Tentukan pemilik dan konsumen state.
- Pisahkan ephemeral, app, dan server state.
- Gunakan satu sumber kebenaran.
- Modelkan loading, success, empty, dan error secara eksplisit.
- Pilih alat state management sesuai kompleksitas, bukan tren.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| App state | State yang digunakan lintas bagian aplikasi |
| Ephemeral state | State lokal berumur pendek |
| Reactive UI | UI memperbarui diri sebagai respons perubahan state |
| Single source of truth | Satu lokasi otoritatif untuk suatu fakta |
| Server state | Data yang kebenaran utamanya berada di backend |

## 12. Referensi

- Flutter, **State management**, <https://docs.flutter.dev/data-and-backend/state-mgmt>
- Flutter, **Simple app state management**, <https://docs.flutter.dev/data-and-backend/state-mgmt/simple>

