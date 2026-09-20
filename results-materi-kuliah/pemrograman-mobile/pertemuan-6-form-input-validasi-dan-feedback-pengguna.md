# Pertemuan 6: Form Input, Validasi, dan Feedback Pengguna

> **Acuan RPS:** CPMK-2, Sub-CPMK-4 • **Durasi:** 150 menit • **Artefak:** form tervalidasi

---

## 1. 🎯 Learning Outcomes

Kamu mampu memilih kontrol input, mengelola `TextEditingController`, menerapkan validasi sintaks dan bisnis, menampilkan error yang dapat ditindaklanjuti, mencegah submit ganda, serta membersihkan resource form.

---

## 2. 📖 Pengantar

Form adalah titik saat pengguna mempercayakan data kepada aplikasi. Pesan “Invalid input” tidak membantu. Form yang baik mencegah kesalahan, menjelaskan cara memperbaiki, menjaga data yang sudah benar, dan tidak mengirim dua kali ketika jaringan lambat.

---

## 3. 🧩 Konsep Utama

### 3.1 Siklus Input

```text
input → normalisasi → validasi klien → submit
      → validasi server → success/error → feedback
```

Validasi klien memperbaiki UX; validasi server menjaga sistem. Klien tidak dapat dipercaya sepenuhnya.

### 3.2 Jenis Validasi

- **Required:** nilai wajib.
- **Format:** email, tanggal, panjang.
- **Range:** nominal atau jumlah.
- **Cross-field:** tanggal akhir setelah tanggal awal.
- **Business rule:** saldo cukup, kuota tersedia.
- **Server validation:** data unik, izin, status mutakhir.

### 3.3 Feedback yang Baik

Pesan error harus dekat dengan field, spesifik, tidak menyalahkan, dan memberi tindakan. “Nominal minimal Rp10.000” lebih baik daripada “Nominal salah”. Jangan menghapus input valid setelah server menolak satu field.

### 3.4 Keamanan Input

- jangan mencatat password, PIN, OTP, atau token;
- gunakan keyboard type sesuai, tetapi jangan menganggapnya sebagai validasi;
- batasi panjang input dan normalisasi dengan hati-hati;
- sembunyikan data sensitif saat perlu;
- validasi dan otorisasi ulang di backend.

### 3.5 Submit State

Tombol submit memiliki keadaan idle, submitting, success, atau error. Saat submitting, cegah aksi ulang dan tunjukkan progres. Untuk transaksi finansial, backend memerlukan idempotency.

---

## 4. 🧠 Analogi

Form seperti petugas loket. Petugas yang baik memeriksa kelengkapan sebelum berkas dikirim, menunjuk bagian yang salah, dan tidak meminta seluruh formulir diulang hanya karena satu kolom keliru.

---

## 5. 💻 Contoh Teknis

```dart
class TransferForm extends StatefulWidget {
  const TransferForm({super.key});

  @override
  State<TransferForm> createState() => _TransferFormState();
}

class _TransferFormState extends State<TransferForm> {
  final _formKey = GlobalKey<FormState>();
  final _amountController = TextEditingController();
  bool _submitting = false;

  @override
  void dispose() {
    _amountController.dispose();
    super.dispose();
  }

  Future<void> _submit() async {
    if (_submitting || !_formKey.currentState!.validate()) return;
    setState(() => _submitting = true);
    try {
      final amount = int.parse(_amountController.text);
      await Future<void>.delayed(const Duration(seconds: 1));
      if (!mounted) return;
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Permintaan Rp$amount diterima')),
      );
    } catch (_) {
      if (!mounted) return;
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Gagal mengirim. Coba lagi.')),
      );
    } finally {
      if (mounted) setState(() => _submitting = false);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(children: [
        TextFormField(
          controller: _amountController,
          keyboardType: TextInputType.number,
          decoration: const InputDecoration(labelText: 'Nominal'),
          validator: (value) {
            final amount = int.tryParse(value ?? '');
            if (amount == null) return 'Masukkan nominal berupa angka';
            if (amount < 10000) return 'Nominal minimal Rp10.000';
            return null;
          },
        ),
        FilledButton(
          onPressed: _submitting ? null : _submit,
          child: Text(_submitting ? 'Mengirim…' : 'Lanjutkan'),
        ),
      ]),
    );
  }
}
```

Contoh memakai simulasi delay. Pada aplikasi nyata, service mengirim request dan memetakan error server menjadi pesan yang aman.

---

## 6. 🏦 Studi Kasus Fintech

### Submit Transfer Ganda

**Masalah:** jaringan lambat, pengguna menekan “Kirim” tiga kali, dan aplikasi mengirim tiga request.

**Dampak:** potensi transaksi ganda dan komplain.

**Solusi mobile:** validasi, disable tombol, tampilkan progress, pertahankan state, dan tampilkan hasil tunggal. **Solusi backend:** idempotency key, constraint transaksi, dan status yang dapat diperiksa. Disable tombol saja tidak cukup karena retry jaringan tetap dapat terjadi.

---

## 7. 📊 Aktivitas 150 Menit

| Waktu | Aktivitas |
|---:|---|
| 20 | Prinsip form dan jenis validasi |
| 20 | Demo `Form`, controller, validator |
| 20 | Async submit dan feedback |
| 15 | Keamanan dan aksesibilitas |
| 60 | Praktik form proyek |
| 15 | Uji input salah dan peer review |

---

## 8. ⚠️ Kesalahan Umum

- Validasi hanya di klien.
- Regex email terlalu rumit dan menolak alamat valid.
- Controller tidak di-`dispose`.
- Error hanya ditampilkan dengan warna.
- Pesan backend mentah ditampilkan ke pengguna.
- Tombol tetap aktif selama submit.
- Data sensitif masuk log.

---

## 9. 🧪 Latihan dan Penilaian

Bangun form proyek dengan minimal tiga jenis input, validasi required/format/range, state submitting, success/error feedback, dan perlindungan submit ganda. Uji input kosong, batas minimum, karakter tidak valid, jaringan gagal, dan submit cepat berulang.

Rubrik: validasi 30%, feedback informatif 25%, handling async 20%, keamanan/resource cleanup 15%, UX 10%.

---

## 10. 📌 Ringkasan

- Validasi klien membantu UX; server menjaga integritas.
- Error harus spesifik dan dapat diperbaiki.
- State submit perlu eksplisit.
- Resource controller harus dibersihkan.
- Transaksi ganda membutuhkan perlindungan mobile dan backend.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Cross-field validation | Aturan yang melibatkan beberapa field |
| Normalisasi | Mengubah input ke bentuk konsisten |
| Server validation | Pemeriksaan pada sisi backend |
| Submit state | Keadaan proses pengiriman form |
| Validator | Fungsi pemeriksa validitas nilai |

## 12. Referensi

- Flutter, **Build a form with validation**, <https://docs.flutter.dev/cookbook/forms/validation>
- Flutter, **Forms cookbook**, <https://docs.flutter.dev/cookbook/forms>

