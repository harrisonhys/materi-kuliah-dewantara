# Pertemuan 14: Functional Testing dan Dokumentasi Proyek

> **Acuan RPS:** CPMK-5, Sub-CPMK-8 • **Durasi:** 150 menit • **Artefak:** test report dan README

---

## 1. 🎯 Learning Outcomes

Kamu mampu menyusun test scenario, membedakan unit/widget/integration test, menulis test sederhana, memprioritaskan jalur kritis, mencatat defect yang dapat direproduksi, serta membuat README yang memungkinkan orang lain menjalankan proyek.

---

## 2. 📖 Pengantar

“Berjalan di laptop saya” bukan bukti kualitas. Aplikasi dapat gagal ketika input kosong, koneksi lambat, permission ditolak, atau pengguna menekan tombol dua kali. Testing mengubah keyakinan menjadi bukti yang dapat diulang.

---

## 3. 🧩 Konsep Utama

### 3.1 Testing Pyramid

| Jenis | Fokus | Cepat | Kepercayaan end-to-end |
|---|---|---:|---:|
| Unit | fungsi/class | tinggi | rendah |
| Widget | UI component dan interaksi | tinggi | sedang |
| Integration | alur aplikasi pada device | lebih lambat | tinggi |

Gunakan banyak unit/widget test dan integration test secukupnya untuk alur kritis.

### 3.2 Test Case

```text
ID: PAY-01
Tujuan: pembayaran valid berhasil dikirim sekali
Precondition: pengguna login, saldo cukup
Steps: isi nominal → konfirmasi → tekan bayar
Expected: loading → success; satu transactionId
Evidence: screenshot/log tersanitasi
```

### 3.3 Boundary dan Negative Testing

Uji nilai minimum/maksimum, kosong, format salah, offline, timeout, 401, 500, data kosong, permission ditolak, dan double tap. Jalur kegagalan sering lebih penting daripada happy path.

### 3.4 Defect Report

Sertakan judul, environment, versi, langkah reproduksi, expected, actual, bukti, severity, dan frekuensi. “Aplikasi error” tidak dapat ditindaklanjuti.

### 3.5 README sebagai Pintu Masuk

README minimal:

```text
Nama dan tujuan proyek
Fitur dan screenshot
Arsitektur/struktur singkat
Prasyarat dan versi tool
Konfigurasi environment tanpa secret
Cara install, run, dan test
Akun/data demo
Known limitations
Kontributor dan atribusi
```

---

## 4. 🧠 Analogi

Test seperti checklist pilot. Pengalaman tidak menghilangkan kebutuhan pemeriksaan yang dapat diulang. README seperti petunjuk kokpit untuk tim berikutnya.

---

## 5. 💻 Contoh Teknis

Unit test:

```dart
import 'package:flutter_test/flutter_test.dart';

int calculateFee(int amount) {
  if (amount <= 0) throw ArgumentError('amount');
  return amount < 100000 ? 2500 : 0;
}

void main() {
  test('amount di bawah 100 ribu dikenai biaya', () {
    expect(calculateFee(50000), 2500);
  });

  test('amount nol ditolak', () {
    expect(() => calculateFee(0), throwsArgumentError);
  });
}
```

Widget test:

```dart
testWidgets('tombol submit nonaktif ketika form kosong', (tester) async {
  await tester.pumpWidget(const MaterialApp(home: TransferPage()));
  final button = tester.widget<FilledButton>(find.byType(FilledButton));
  expect(button.onPressed, isNull);
});
```

Perintah:

```bash
flutter analyze
flutter test
flutter test --coverage
```

Coverage adalah indikator, bukan tujuan. Test harus memeriksa perilaku bermakna.

---

## 6. 🏦 Studi Kasus Fintech

### Bug Pembulatan Biaya Tidak Terdeteksi

**Masalah:** biaya transaksi dihitung dengan floating point dan hanya diuji pada angka bulat mudah.

**Dampak:** selisih nominal pada banyak transaksi dan masalah rekonsiliasi.

**Solusi:** representasikan uang dalam integer satuan terkecil, test boundary/table-driven, cocokkan kontrak backend, dan sertakan kasus pembulatan. Integration test memverifikasi nominal UI sama dengan response resmi.

---

## 7. 📊 Aktivitas 150 Menit

20 menit strategi test, 20 menit test case, 25 menit unit/widget demo, 20 menit defect report/README, 50 menit testing proyek, 15 menit triage bug.

---

## 8. ⚠️ Kesalahan Umum

- Test hanya happy path.
- Test bergantung pada urutan atau internet publik.
- Assertion tidak bermakna.
- Coverage tinggi dianggap otomatis berkualitas.
- Defect report tidak memiliki langkah reproduksi.
- README membocorkan key atau credential.
- Automated test dibuat setelah arsitektur terlalu sulit diuji tanpa refactor.

---

## 9. 🧪 Tugas dan Penilaian

Kumpulkan test plan minimal 10 kasus, minimal dua unit test dan satu widget test, hasil `flutter analyze`/`flutter test`, defect list, dan README lengkap. Alur utama harus diuji manual pada perangkat atau emulator.

Rubrik: test coverage perilaku 25%, kualitas kasus 25%, automated test 20%, defect report 10%, README/reproducibility 20%.

---

## 10. 📌 Ringkasan

- Test memverifikasi perilaku pada kondisi normal dan gagal.
- Unit, widget, dan integration test memiliki trade-off.
- Prioritaskan alur berisiko tinggi.
- Bug harus dapat direproduksi.
- README membuat proyek dapat dijalankan oleh orang lain.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Assertion | Pemeriksaan expected terhadap actual |
| Defect | Perbedaan perilaku aktual dan yang diharapkan |
| Integration test | Test alur besar/utuh aplikasi |
| Test case | Skenario, langkah, dan hasil yang diharapkan |
| Unit test | Test fungsi/method/class secara terisolasi |
| Widget test | Test widget pada environment Flutter test |

## 12. Referensi

- Flutter, **Testing Flutter apps**, <https://docs.flutter.dev/testing/overview>
- Flutter, **Integration testing concepts**, <https://docs.flutter.dev/cookbook/testing/integration/introduction>
