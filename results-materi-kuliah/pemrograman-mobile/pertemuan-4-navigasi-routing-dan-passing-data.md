# Pertemuan 4: Navigasi, Routing, dan Passing Data Antar-Screen

> **Acuan RPS:** CPMK-2, Sub-CPMK-3 • **Durasi:** 150 menit • **Artefak:** navigasi multi-screen

---

## 1. 🎯 Learning Outcomes

Kamu mampu membangun navigasi antar-screen, memilih push/replace/pop dengan benar, mengirim data yang terstruktur, menerima hasil dari screen lain, menangani deep link secara konseptual, dan merancang alur yang tidak membingungkan pengguna.

---

## 2. 📖 Pengantar

Aplikasi bukan kumpulan layar yang berdiri sendiri. Ia seperti gedung: pengguna harus tahu sedang berada di mana, bagaimana kembali, dan apa yang terjadi setelah sebuah tindakan. Navigasi yang salah dapat membuat pembayaran dikirim ulang atau pengguna kembali ke layar login setelah berhasil masuk.

---

## 3. 🧩 Konsep Utama

### 3.1 Screen, Route, dan Navigation Stack

- **Screen/page:** UI yang merepresentasikan satu tujuan.
- **Route:** representasi tujuan yang dapat dinavigasi.
- **Stack:** tumpukan riwayat route.

```text
[Beranda] → push [Daftar Produk] → push [Detail]
                                      │ pop
                         [Daftar Produk]
```

### 3.2 Operasi Dasar

| Operasi | Makna | Contoh |
|---|---|---|
| `push` | Menambah route | Beranda → Detail |
| `pop` | Menghapus route teratas | Detail → Beranda |
| `pushReplacement` | Mengganti route saat ini | Login → Beranda |
| `popUntil` | Kembali sampai kondisi tertentu | Selesai bayar → Beranda |

Gunakan replacement setelah login jika pengguna tidak boleh kembali ke form login lewat Back.

### 3.3 Passing Data

Kirim objek minimum yang diperlukan. Untuk data yang dapat berubah di server, mengirim ID lalu mengambil data terbaru sering lebih aman daripada mengirim seluruh snapshot lama.

### 3.4 Mengembalikan Hasil

Screen pemilih dapat mengembalikan nilai ketika ditutup, misalnya alamat pengiriman. Pemanggil menunggu `Future` hasil navigasi.

### 3.5 Deep Link

Deep link membuka tujuan tertentu dari URL/notifikasi. Aplikasi harus tetap memeriksa autentikasi dan otorisasi. Mengetahui URL bukan berarti pengguna berhak membuka data.

### 3.6 Navigation Guard

Sebelum meninggalkan form yang berubah, tanyakan konfirmasi. Sebelum membuka route privat, periksa sesi. Guard adalah bagian UX sekaligus kontrol alur, tetapi otorisasi final tetap di backend.

---

## 4. 🧠 Analogi

Navigation stack seperti tumpukan piring. `push` menaruh piring di atas, `pop` mengambil yang paling atas. `pushReplacement` menukar piring teratas. Deep link seperti lift yang langsung menuju lantai tertentu, tetapi kartu akses masih harus diperiksa.

---

## 5. 💻 Contoh Teknis

```dart
class Product {
  final String id;
  final String name;
  const Product(this.id, this.name);
}

// Membuka detail dan menunggu hasil.
final added = await Navigator.of(context).push<bool>(
  MaterialPageRoute(
    builder: (_) => ProductDetailPage(productId: product.id),
  ),
);

if (context.mounted && added == true) {
  ScaffoldMessenger.of(context).showSnackBar(
    const SnackBar(content: Text('Produk ditambahkan')),
  );
}
```

Pada screen detail:

```dart
FilledButton(
  onPressed: () {
    // Validasi dan ubah state terlebih dahulu.
    Navigator.of(context).pop(true);
  },
  child: const Text('Tambah'),
)
```

`context.mounted` diperiksa setelah `await` agar kode tidak memakai `BuildContext` yang sudah tidak aktif.

### Peta Route Proyek

```text
/splash
  ├─ sesi ada  → /home
  └─ sesi tidak ada → /login
/home
  ├─ /items
  │    └─ /items/:id
  ├─ /transactions/:id
  └─ /profile
```

Untuk aplikasi besar, gunakan routing deklaratif dan package yang sesuai, tetapi pahami stack dasar sebelum menambah abstraksi.

---

## 6. 🏦 Studi Kasus Fintech

### Tombol Back Mengulang Konfirmasi Transfer

**Masalah:** setelah transfer sukses, aplikasi menumpuk halaman hasil di atas halaman konfirmasi. Pengguna menekan Back dan kembali ke tombol “Kirim”, lalu menekan lagi.

**Dampak:** permintaan ganda, kecemasan pengguna, dan beban rekonsiliasi.

**Solusi:** desain state transaksi eksplisit, ganti route konfirmasi setelah sukses, arahkan Back ke riwayat/beranda, nonaktifkan submit saat request berjalan, dan gunakan idempotency key di backend. Navigasi membantu mencegah kesalahan, tetapi backend tetap harus menjamin transaksi tidak diproses ganda.

---

## 7. 📊 Aktivitas 150 Menit

| Waktu | Aktivitas |
|---:|---|
| 20 | Stack dan operasi navigasi |
| 20 | Demo push, pop, replacement |
| 20 | Passing data dan return result |
| 15 | Deep link dan guard |
| 60 | Praktik menyambungkan wireframe |
| 15 | Uji semua jalur dan peer review |

---

## 8. ⚠️ Kesalahan Umum

- Menaruh semua screen dalam satu file besar.
- Mengirim data sensitif melalui parameter route atau URL.
- Menggunakan `BuildContext` setelah proses async tanpa memeriksa `mounted`.
- Membuat tombol Back menghasilkan alur bisnis yang salah.
- Mempercayai route guard sebagai pengganti otorisasi backend.
- Tidak menguji deep link ketika aplikasi belum login.

---

## 9. 🧪 Latihan dan Penilaian

**Konsep:** bedakan `push`, `pop`, dan `pushReplacement`. Mengapa mengirim ID dapat lebih aman daripada objek lengkap?

**Praktik:** bangun minimal empat screen, kirim ID ke detail, kembalikan hasil, dan pastikan seluruh layar dapat diakses sesuai user flow. Sertakan skenario Back dari setiap screen.

**Kasus:** desain navigasi pembayaran mulai dari keranjang hingga hasil. Jelaskan route stack saat pending, sukses, dan gagal.

Rubrik: kesesuaian user flow 30%, seluruh route dapat diakses 25%, passing data 20%, perilaku Back 15%, kualitas kode 10%.

---

## 10. 📌 Ringkasan

- Navigasi adalah perubahan state tujuan, bukan sekadar pindah layar.
- Stack menentukan perilaku Back.
- Kirim data minimum dan ambil data mutakhir bila diperlukan.
- Deep link tetap membutuhkan autentikasi dan otorisasi.
- Alur transaksi harus aman terhadap pengulangan tindakan.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Deep link | Tautan yang membuka tujuan tertentu dalam aplikasi |
| Navigation stack | Tumpukan route aktif |
| Route | Representasi tujuan navigasi |
| Route guard | Pemeriksaan sebelum memasuki/meninggalkan route |
| Passing data | Pengiriman data antar-screen |

## 12. Referensi

- Flutter, **Navigation and routing**, <https://docs.flutter.dev/ui/navigation>
- Flutter, **Navigate to a new screen and back**, <https://docs.flutter.dev/cookbook/navigation/navigation-basics>
- RPS Pemrograman Mobile MU5502, Pertemuan 4.

