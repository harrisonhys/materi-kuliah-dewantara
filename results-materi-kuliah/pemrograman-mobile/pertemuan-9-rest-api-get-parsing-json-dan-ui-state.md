# Pertemuan 9: REST API GET, Parsing JSON, dan UI State

> **Acuan RPS:** CPMK-3, Sub-CPMK-5 • **Durasi:** 150 menit • **Artefak:** daftar data dari API

---

## 1. 🎯 Learning Outcomes

Kamu mampu menjelaskan request-response HTTP, memanggil endpoint GET, membaca status code, memetakan JSON menjadi model Dart, memisahkan service/repository dari UI, serta menampilkan loading, data, empty, error, timeout, dan retry.

---

## 2. 📖 Pengantar

Ketika aplikasi menampilkan riwayat transaksi, data biasanya tidak berasal dari widget. Mobile mengirim request; backend mengautentikasi, membaca database, lalu mengirim response. Masalah jaringan, format berubah, atau sesi kedaluwarsa harus diterjemahkan menjadi UI yang jujur.

---

## 3. 🧩 Konsep Utama

### 3.1 Anatomi HTTP

```text
GET /transactions?page=1 HTTP/1.1
Authorization: Bearer <token>
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json
{"data":[...],"page":1}
```

URL menentukan resource; method menentukan maksud; header membawa metadata; body membawa data bila relevan; status code merangkum hasil.

### 3.2 Status Code Penting

| Kode | Makna umum | Respons aplikasi |
|---:|---|---|
| 200 | Berhasil | parse dan tampilkan |
| 400 | Request tidak valid | periksa input/kontrak |
| 401 | Belum/tidak terautentikasi | perbarui sesi atau login |
| 403 | Tidak diizinkan | tampilkan akses ditolak |
| 404 | Resource tidak ditemukan | empty/not found yang sesuai |
| 429 | Terlalu banyak request | tunggu dan retry terkontrol |
| 500+ | Gangguan server | pesan aman dan observability |

### 3.3 JSON ke Model

Jangan menyebarkan `Map<String, dynamic>` ke seluruh UI. Model memberi tipe, validasi mapping, dan satu tempat menangani nama field.

### 3.4 Lapisan

```text
Widget → ViewModel/State → Repository → API Client → Backend
```

UI tidak perlu mengetahui detail header atau decoding. Repository dapat memilih network, cache, atau fake untuk test.

### 3.5 State dan Retry

Retry aman untuk GET yang tidak mengubah data, tetapi tetap dibatasi. Gunakan timeout, backoff, dan tombol coba lagi; jangan loop tanpa akhir.

---

## 4. 🧠 Analogi

API seperti loket arsip. Kamu mengajukan formulir dengan format tertentu; petugas mengembalikan dokumen dan kode hasil. JSON adalah format isi paket, sedangkan status code adalah label kondisi pengiriman.

---

## 5. 💻 Contoh Teknis

```bash
flutter pub add http
```

```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class TransactionItem {
  final String id;
  final int amount;
  final String status;

  const TransactionItem({required this.id, required this.amount, required this.status});

  factory TransactionItem.fromJson(Map<String, dynamic> json) {
    return TransactionItem(
      id: json['id'] as String,
      amount: json['amount'] as int,
      status: json['status'] as String,
    );
  }
}

class TransactionApi {
  final http.Client client;
  TransactionApi(this.client);

  Future<List<TransactionItem>> fetchAll() async {
    final response = await client
        .get(Uri.https('api.example.com', '/transactions'))
        .timeout(const Duration(seconds: 10));

    if (response.statusCode != 200) {
      throw Exception('Gagal mengambil transaksi (${response.statusCode})');
    }
    final body = jsonDecode(response.body) as Map<String, dynamic>;
    final rows = body['data'] as List<dynamic>;
    return rows
        .map((row) => TransactionItem.fromJson(row as Map<String, dynamic>))
        .toList();
  }
}
```

Gunakan domain uji milik proyek, HTTPS, dan jangan hard-code token. Tangkap error di state layer lalu petakan menjadi pesan pengguna; jangan menampilkan stack trace.

---

## 6. 🏦 Studi Kasus Fintech

### Riwayat Transaksi Tampak Kosong ketika API Timeout

**Masalah:** timeout dipetakan menjadi list kosong sehingga pengguna mengira tidak pernah bertransaksi.

**Dampak:** informasi menyesatkan dan kepercayaan turun.

**Solusi:** bedakan `empty` dari `error`; tampilkan data cache dengan label waktu sinkronisasi jika tersedia; sediakan retry; log correlation ID tanpa data sensitif; backend memonitor latensi dan error rate.

---

## 7. 📊 Aktivitas 150 Menit

20 menit HTTP/REST, 20 menit eksplorasi endpoint di Postman, 25 menit model dan parsing, 25 menit service/repository, 45 menit praktik UI state, 15 menit pengujian timeout/error.

---

## 8. ⚠️ Kesalahan Umum

- Semua response dianggap 200.
- JSON di-cast tanpa validasi kontrak.
- Request dilakukan di `build()`.
- Empty dan error disamakan.
- Retry tanpa batas.
- Token atau response sensitif dicetak ke log.
- HTTP client sulit diganti sehingga test sulit.

---

## 9. 🧪 Latihan dan Penilaian

Integrasikan GET ke proyek. Wajib memiliki model, service/repository, loading, empty, error, retry, timeout, dan bukti request-response yang disanitasi. Uji 200, format tidak sesuai, 401, 500, serta offline.

Rubrik: request/parsing 30%, arsitektur 20%, UI state 25%, error handling 15%, keamanan/kode 10%.

---

## 10. 📌 Ringkasan

- HTTP adalah kontrak request-response.
- Status code dan body harus diperiksa.
- Model typed membatasi penyebaran data dinamis.
- Pisahkan API client, repository, state, dan UI.
- Empty tidak sama dengan gagal memuat.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Endpoint | Alamat operasi API |
| HTTP status code | Kode ringkas hasil request |
| JSON | Format pertukaran data berbasis teks |
| Parsing | Mengubah data mentah menjadi struktur aplikasi |
| Repository | Abstraksi sumber data bagi aplikasi |
| Timeout | Batas waktu menunggu operasi |

## 12. Referensi

- Flutter, **Networking cookbook**, <https://docs.flutter.dev/cookbook/networking>
- MDN, **HTTP response status codes**, <https://developer.mozilla.org/docs/Web/HTTP/Status>

