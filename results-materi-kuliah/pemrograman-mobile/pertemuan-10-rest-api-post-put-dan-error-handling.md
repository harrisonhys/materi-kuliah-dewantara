# Pertemuan 10: REST API POST PUT dan Error Handling

> **Acuan RPS:** CPMK-3, Sub-CPMK-5 • **Durasi:** 150 menit • **Artefak:** create/update melalui API

---

## 1. 🎯 Learning Outcomes

Kamu mampu membedakan POST, PUT, dan PATCH; membentuk request JSON; menangani status sukses dan gagal; memodelkan validation error; mencegah request ganda; serta merancang retry yang aman.

---

## 2. 📖 Pengantar

GET membaca data. POST/PUT mengubah dunia nyata: membuat reservasi, mengubah alamat, atau mengirim transaksi. Karena dampaknya lebih besar, aplikasi harus tahu apakah request diterima, ditolak, timeout sebelum diproses, atau sebenarnya berhasil tetapi response hilang.

---

## 3. 🧩 Konsep Utama

### 3.1 Semantik Method

| Method | Penggunaan umum | Idempotent secara semantik |
|---|---|---|
| POST | Membuat resource/menjalankan command | Umumnya tidak |
| PUT | Mengganti representasi pada URI tertentu | Ya |
| PATCH | Memperbarui sebagian | Bergantung operasi |
| DELETE | Menghapus resource | Dirancang idempotent |

Idempotent berarti pengulangan request yang sama memberi efek akhir yang sama, bukan berarti response selalu identik.

### 3.2 Kontrak Request

Tentukan field, tipe, required, batas, format tanggal, satuan uang, dan versi. Untuk uang, hindari floating point mentah; gunakan integer satuan terkecil sesuai kontrak.

### 3.3 Error Taxonomy

- **Validation:** input tidak memenuhi kontrak.
- **Authentication/authorization:** sesi atau izin.
- **Conflict:** versi/status berubah, misalnya slot sudah diambil.
- **Rate limit:** terlalu banyak request.
- **Network/timeout:** hasil mungkin belum diketahui.
- **Server:** gangguan internal.

### 3.4 Timeout yang Ambigu

Jika POST timeout, jangan langsung menganggap gagal. Server mungkin sudah memproses request. Gunakan idempotency key dan endpoint status sebelum retry.

---

## 4. 🧠 Analogi

POST seperti menyerahkan formulir pembuatan rekening. Jika tanda terima tidak keluar, kamu tidak otomatis mengisi formulir kedua; pertama-tama periksa apakah rekening sudah dibuat.

---

## 5. 💻 Contoh Teknis

```dart
Future<String> createBooking({
  required http.Client client,
  required String serviceId,
  required String idempotencyKey,
}) async {
  final response = await client.post(
    Uri.https('api.example.com', '/bookings'),
    headers: {
      'Content-Type': 'application/json',
      'Idempotency-Key': idempotencyKey,
    },
    body: jsonEncode({'serviceId': serviceId}),
  ).timeout(const Duration(seconds: 10));

  switch (response.statusCode) {
    case 201:
      final json = jsonDecode(response.body) as Map<String, dynamic>;
      return json['id'] as String;
    case 409:
      throw StateError('Slot sudah tidak tersedia');
    case 422:
      throw FormatException('Data tidak memenuhi aturan layanan');
    default:
      throw Exception('Request gagal (${response.statusCode})');
  }
}
```

Idempotency key harus stabil untuk satu niat operasi dan berbeda untuk operasi baru. Detail implementasinya disepakati dengan backend.

### State Submit

```text
idle → validating → submitting → success
                         ├→ rejected
                         ├→ unknown (timeout)
                         └→ retryable error
```

State `unknown` penting untuk operasi finansial: periksa status sebelum meminta pengguna mengulang.

---

## 6. 🏦 Studi Kasus Fintech

### Response Hilang Setelah Debit Berhasil

**Masalah:** backend mendebit saldo, tetapi koneksi putus sebelum response diterima. Aplikasi menampilkan “gagal” dan mengizinkan ulang.

**Dampak:** risiko debit ganda dan sengketa.

**Solusi:** client-generated idempotency key, status `processing/unknown`, query status transaksi, tombol retry terkontrol, serta reconciliation backend. Jangan menjanjikan kegagalan jika hasil sebenarnya belum diketahui.

---

## 7. 📊 Aktivitas 150 Menit

20 menit semantik method, 20 menit kontrak dan status, 20 menit demo Postman, 25 menit implementasi repository, 45 menit praktik POST/PUT, 20 menit simulasi error dan timeout.

---

## 8. ⚠️ Kesalahan Umum

- Semua error dijadikan “koneksi bermasalah”.
- Retry otomatis pada POST tanpa idempotency.
- Nominal uang memakai `double` tanpa aturan.
- Menampilkan pesan/stack trace backend mentah.
- Update lokal dianggap sukses sebelum server mengonfirmasi tanpa rollback.
- Token/PII masuk log.

---

## 9. 🧪 Latihan dan Penilaian

Buat fitur create atau update dengan request typed, state submit lengkap, mapping minimal empat status code, retry aman, dan pembuktian melalui Postman collection. Jelaskan perilaku ketika timeout.

Rubrik: kontrak request 25%, status/error mapping 25%, state UI 20%, idempotency/retry 20%, keamanan 10%.

---

## 10. 📌 Ringkasan

- Method HTTP membawa semantik.
- Operasi tulis membutuhkan penanganan konflik dan hasil ambigu.
- Idempotency melindungi pengulangan niat yang sama.
- Error harus diklasifikasikan agar UI memberi tindakan tepat.
- Backend tetap menjadi pengendali integritas.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Conflict | Kondisi resource tidak sesuai asumsi request |
| Idempotent | Pengulangan menghasilkan efek akhir yang sama |
| Idempotency key | Penanda unik untuk mengenali operasi berulang |
| PATCH | Pembaruan sebagian resource |
| PUT | Penggantian representasi resource |

## 12. Referensi

- Flutter, **Send data to the internet**, <https://docs.flutter.dev/cookbook/networking/send-data>
- HTTP Semantics, <https://www.rfc-editor.org/rfc/rfc9110>

