# Pertemuan 3: Widget, Layout, dan Styling Dasar Flutter

> **Acuan RPS:** CPMK-2, Sub-CPMK-3 • **Durasi:** 150 menit • **Artefak:** layar pertama sesuai wireframe

---

## 1. 🎯 Learning Outcomes

Kamu akan mampu menjelaskan widget tree, membedakan stateless dan stateful widget, menyusun layout responsif dengan constraints, menerapkan theme dan komponen Material, serta membangun satu layar yang sesuai wireframe dan tetap terbaca pada ukuran berbeda.

---

## 2. 📖 Pengantar

UI Flutter dibangun seperti menyusun LEGO. Setiap bagian adalah widget: teks, jarak, baris, tombol, bahkan aplikasi dan tema. Tantangannya bukan menghafal ratusan widget, melainkan memahami komposisi dan aturan layout.

---

## 3. 🧩 Konsep Utama

### 3.1 Everything Is a Widget

Widget adalah deskripsi immutable dari bagian UI. Widget membentuk tree:

```text
MaterialApp
└── Scaffold
    ├── AppBar
    └── SafeArea
        └── Padding
            └── Column
                ├── Text
                ├── Card
                └── FilledButton
```

Pecah widget ketika bagian memiliki tanggung jawab jelas, digunakan ulang, atau membuat `build()` sulit dibaca.

### 3.2 Stateless dan Stateful

- `StatelessWidget`: output bergantung pada input yang tidak dikelola secara mutable oleh widget.
- `StatefulWidget`: mempunyai objek `State` yang dapat berubah selama widget berada pada tree.

Jangan membuat semuanya stateful “untuk berjaga-jaga”. Letakkan state sedekat mungkin dengan pemakai, tetapi cukup tinggi untuk dibagi oleh semua komponen yang membutuhkan.

### 3.3 Mental Model Layout

Aturan penting Flutter:

```text
Constraints turun → ukuran naik → parent menentukan posisi
```

Parent memberi batas minimum/maksimum; child memilih ukuran yang diizinkan; parent menempatkannya. Banyak error overflow muncul karena child menginginkan ruang lebih besar daripada constraints.

Widget layout umum:

| Widget | Kegunaan |
|---|---|
| `Row` | Menyusun horizontal |
| `Column` | Menyusun vertikal |
| `Expanded` | Mengambil sisa ruang dalam flex |
| `Flexible` | Memberi keluwesan ukuran |
| `Wrap` | Memindah item ke baris berikutnya |
| `ListView` | Daftar yang dapat di-scroll |
| `Stack` | Menumpuk elemen |
| `Padding` | Memberi ruang di sekitar child |
| `SafeArea` | Menghindari notch dan area sistem |
| `LayoutBuilder` | Merespons constraints yang tersedia |

### 3.4 Styling yang Konsisten

Gunakan `ThemeData` sebagai sumber warna, tipografi, dan bentuk. Hindari hard-code warna dan style berulang. Komponen semantik seperti `FilledButton`, `Card`, dan `TextField` membantu konsistensi serta aksesibilitas.

### 3.5 Responsif dan Adaptif

Responsif berarti layout menyesuaikan ruang. Adaptif berarti perilaku/komponen dapat menyesuaikan platform atau kelas perangkat. Jangan memakai ukuran layar satu perangkat sebagai kebenaran universal.

---

## 4. 🧠 Analogi

Constraints seperti pemilik apartemen memberi ukuran kamar kepada desainer. Furnitur boleh memilih ukuran di dalam batas itu, tetapi tidak dapat menembus dinding. `Expanded` seperti penyewa yang setuju mengambil sisa ruang setelah kebutuhan lain dipenuhi.

---

## 5. 💻 Contoh Teknis

```dart
import 'package:flutter/material.dart';

void main() => runApp(const QueueApp());

class QueueApp extends StatelessWidget {
  const QueueApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
        useMaterial3: true,
      ),
      home: const QueueHomePage(),
    );
  }
}

class QueueHomePage extends StatelessWidget {
  const QueueHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('AntreanKita')),
      body: SafeArea(
        child: ListView(
          padding: const EdgeInsets.all(16),
          children: [
            Text('Selamat datang',
                style: Theme.of(context).textTheme.headlineSmall),
            const SizedBox(height: 16),
            const QueueCard(number: 'A-021', waiting: 4),
            const SizedBox(height: 16),
            FilledButton.icon(
              onPressed: () {},
              icon: const Icon(Icons.add),
              label: const Text('Ambil nomor'),
            ),
          ],
        ),
      ),
    );
  }
}

class QueueCard extends StatelessWidget {
  final String number;
  final int waiting;

  const QueueCard({super.key, required this.number, required this.waiting});

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: const EdgeInsets.all(16),
        child: Row(
          children: [
            const Icon(Icons.confirmation_number_outlined, size: 40),
            const SizedBox(width: 12),
            Expanded(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  Text(number, style: Theme.of(context).textTheme.titleLarge),
                  Text('$waiting antrean sebelum kamu'),
                ],
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

**Eksperimen:** perbesar font sistem, ubah orientasi, gunakan layar sempit, dan ganti teks menjadi lebih panjang. Catat overflow atau elemen yang tidak dapat dijangkau.

---

## 6. 🏦 Studi Kasus Fintech

### Kartu Saldo Pecah pada Layar Kecil

**Masalah:** saldo, nama akun, dan tiga tombol disusun dalam `Row` dengan lebar tetap. Pada perangkat kecil atau font besar, nominal terpotong.

**Dampak:** pengguna salah membaca saldo dan tindakan finansial menjadi berisiko.

**Solusi:** gunakan constraints, `Expanded`/`Wrap`, format nominal yang benar, hierarki yang jelas, serta pengujian font scaling. Informasi finansial utama tidak boleh dikorbankan hanya agar desain tampak simetris.

---

## 7. 📊 Aktivitas 150 Menit

| Waktu | Aktivitas |
|---:|---|
| 20 | Widget tree dan immutable configuration |
| 25 | Constraints dan latihan layout |
| 20 | Demo theme dan komponen Material |
| 60 | Membangun layar dari wireframe |
| 15 | Uji ukuran, orientasi, dan font |
| 10 | Code review berpasangan |

---

## 8. ⚠️ Kesalahan Umum

- `Column` panjang tanpa scroll menyebabkan overflow.
- Memakai `Container` untuk semua hal tanpa memahami widget spesifik.
- Hard-code warna, ukuran, dan teks berulang.
- Memanggil fungsi berat di `build()`.
- Mengabaikan `SafeArea`, keyboard, dan font scaling.
- Menyusun seluruh layar dalam satu widget raksasa.

---

## 9. 🧪 Latihan dan Penilaian

1. Jelaskan “constraints turun, ukuran naik”.
2. Kapan `ListView` lebih tepat daripada `Column`?
3. Implementasikan satu layar proyek dengan header, informasi utama, list/card, dan primary action.
4. Sertakan screenshot pada dua ukuran layar dan satu keadaan empty/error.

Rubrik: kesesuaian wireframe 30%, struktur widget 25%, responsivitas 20%, konsistensi theme 15%, kerapian kode 10%.

---

## 10. 📌 Ringkasan

- Flutter menyusun UI melalui komposisi widget.
- Layout mengikuti constraints, ukuran, lalu posisi.
- Theme mengurangi inkonsistensi visual.
- UI harus diuji pada variasi layar dan pengaturan pengguna.
- Komponen kecil dengan tanggung jawab jelas lebih mudah dirawat.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Constraints | Batas ukuran yang diberikan parent kepada child |
| Material 3 | Sistem desain dan komponen UI dari Google |
| Responsive | Menyesuaikan layout terhadap ruang tersedia |
| Theme | Konfigurasi gaya yang digunakan konsisten |
| Widget tree | Hierarki widget pembentuk UI |

## 12. Referensi

- Flutter, **Building user interfaces**, <https://docs.flutter.dev/ui>
- Flutter, **Understanding constraints**, <https://docs.flutter.dev/ui/layout/constraints>
- Material Design 3, <https://m3.material.io/>

