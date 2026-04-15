# Pertemuan 15: Proyek Mini — Presentasi & Demo

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Melakukan **finalisasi proyek** dengan menggunakan checklist kualitas kode yang terstruktur
- Membuat **dokumentasi README.md** yang profesional dan informatif
- Menyiapkan dan menyampaikan **presentasi teknis** yang efektif dan meyakinkan
- Melakukan **demo program** yang lancar dan terencana (bukan asal run)
- Menjawab **pertanyaan teknis** dari audiens dengan percaya diri
- Memahami **rubrik penilaian** proyek dan apa yang benar-benar dinilai

---

## 📖 Pengantar (Hook)

Kamu sudah seminggu begadang nulis kode. Program berjalan. Kamu lega. Tapi kemudian tiba saat presentasi — dan kamu bingung bagaimana menjelaskannya, demo-nya error di depan semua orang, dan pertanyaan pertama dari dosen membuat kamu blank.

Ini bukan cerita fiksi. Ini terjadi di hampir setiap kelas pemrograman, dan bahkan di dunia profesional.

Di startup teknologi, ada istilah *"demo effect"* — keajaiban aneh di mana program yang berjalan sempurna saat latihan tiba-tiba error tepat saat dipresentasikan ke investor. Google, Dropbox, bahkan Apple pernah mengalami demo yang gagal di hadapan publik.

Tapi ada bedanya antara programmer yang panik dan programmer yang profesional: yang profesional **sudah menyiapkan plan B**, tahu cara menjelaskan masalah dengan tenang, dan punya dokumentasi yang membuat audiens tetap percaya meski ada error.

Pertemuan ini bukan tentang kode baru. Ini tentang **bagaimana menampilkan karya terbaikmu** — karena presentasi yang baik bisa membuat proyek biasa terlihat luar biasa, dan presentasi yang buruk bisa membuat proyek luar biasa terlihat biasa.

---

## 🧩 Konsep Utama

### 1. Checklist Kualitas Kode (Code Quality Checklist)

Sebelum finalisasi, periksa kode kamu dengan checklist ini:

**Fungsionalitas:**
- [ ] Semua fitur yang dijanjikan di requirement analysis berjalan dengan benar
- [ ] Program tidak crash ketika user input sesuatu yang tidak diharapkan
- [ ] Semua edge case yang sudah diidentifikasi sudah ditangani

**Keterbacaan Kode:**
- [ ] Nama variabel dan fungsi jelas dan deskriptif (bukan `x`, `tmp`, `func1`)
- [ ] Setiap fungsi punya docstring yang menjelaskan tujuan, parameter, dan return value
- [ ] Komentar ada di bagian kode yang kompleks atau tidak langsung jelas
- [ ] Tidak ada kode yang "dicomment out" dan dibiarkan (hapus atau bersihkan)

**Struktur:**
- [ ] Kode dibagi ke dalam fungsi-fungsi yang punya tanggung jawab tunggal
- [ ] Tidak ada kode duplikat yang bisa dijadikan fungsi
- [ ] File main.py tidak terlalu panjang (jika > 200 baris, pertimbangkan pisahkan ke modul)

**Data & File:**
- [ ] Data tersimpan dengan benar (tidak hilang saat program restart, jika ini fitur yang dijanjikan)
- [ ] File data ada di folder yang benar sesuai struktur proyek

**Error Handling:**
- [ ] Ada `try-except` di bagian yang bisa error (input user, operasi file)
- [ ] Pesan error informatif dan membantu user mengerti apa yang salah

---

### 2. Cara Membuat README.md yang Baik

README adalah "kartu nama" proyekmu. Orang yang belum pernah lihat kode kamu harus bisa mengerti dan menjalankan programnya hanya dari README.

**Struktur README yang baik:**

```markdown
# Nama Proyek

Deskripsi singkat (1-2 kalimat): apa yang dilakukan program ini.

## Fitur
- Fitur 1
- Fitur 2
- Fitur 3

## Cara Instalasi & Menjalankan

**Prasyarat:** Python 3.8+

1. Clone repository ini
   git clone https://github.com/username/nama-repo.git

2. Masuk ke folder proyek
   cd nama-repo

3. Jalankan program
   python main.py

## Cara Penggunaan
[Screenshot atau deskripsi step-by-step cara menggunakan program]

## Struktur Proyek
nama-repo/
├── main.py
├── README.md
└── data/

## Tim Pengembang
- Nama 1 (NIM) — Peran
- Nama 2 (NIM) — Peran

## Lisensi
MIT License (atau tulis "Proyek tugas mata kuliah")
```

---

### 3. Tips Presentasi Teknis yang Efektif

**Struktur presentasi yang disarankan (10-15 menit):**

| Bagian | Durasi | Isi |
|--------|--------|-----|
| Opening | 1-2 menit | Perkenalan tim + konteks masalah yang diselesaikan |
| Penjelasan Proyek | 2-3 menit | Apa program ini, siapa penggunanya, apa fiturnya |
| Arsitektur/Desain | 2-3 menit | Diagram IPO, struktur kode, keputusan desain penting |
| Demo Live | 3-5 menit | Demo program berjalan (sudah dipersiapkan sebelumnya) |
| Tantangan & Pembelajaran | 1-2 menit | Kesulitan yang dihadapi dan bagaimana mengatasinya |
| Q&A | 2-3 menit | Jawab pertanyaan |

**Tips agar presentasi terlihat profesional:**
- **Latihan minimal 2 kali** sebelum hari H, termasuk demo-nya
- **Jangan baca slide** — slide adalah panduan, bukan naskah
- **Satu slide, satu poin** — jangan padati slide dengan teks panjang
- **Gunakan font besar** (minimal 24pt) agar terlihat dari belakang ruangan
- **Tunjukkan kode hanya yang relevan** — jangan tampilkan seluruh file kode di slide

---

### 4. Cara Melakukan Demo yang Meyakinkan

Demo adalah momen paling kritis dari presentasi teknis. Berikut cara melakukannya dengan baik:

**Persiapan sebelum demo:**
- [ ] Jalankan program sekali sebelum presentasi untuk memastikan berjalan
- [ ] Siapkan data input yang sudah ditest sebelumnya (jangan improvisasi input)
- [ ] Tutup aplikasi lain yang tidak dibutuhkan (bersihkan desktop)
- [ ] Besarkan ukuran font terminal/IDE agar terlihat dari belakang
- [ ] Siapkan "skenario demo" yang sudah diurutkan

**Skenario demo yang baik:**
1. Tunjukkan alur happy path (semua input benar, program berjalan normal)
2. Tunjukkan satu edge case yang sudah ditangani (input salah → pesan error yang baik)
3. Akhiri dengan output/hasil yang paling impressive

**Jika terjadi error saat demo:**
- Jangan panik — katakan "menarik, sepertinya ada edge case yang belum saya tangani"
- Jelaskan *apa* yang seharusnya terjadi dan *kenapa* error ini muncul
- Tawarkan untuk menunjukkan melalui walkthrough kode jika ada waktu

---

### 5. Cara Menjawab Pertanyaan Teknis

**Formula menjawab pertanyaan teknis:**

1. **Ulangi pertanyaannya** (untuk memastikan kamu mengerti) — "Jadi kamu bertanya tentang bagaimana saya menangani..."
2. **Jawab dengan jelas** — mulai dari konsep, baru ke implementasi spesifik
3. **Tunjukkan ke kode** jika relevan — "Kalau kamu lihat di baris 45, saya menggunakan..."
4. **Akui jika tidak tahu** — "Itu pertanyaan yang bagus. Saat ini saya belum mengimplementasikannya karena... tapi cara yang bisa dilakukan adalah..."

**Jangan pernah:**
- Berbohong atau mengarang jawaban teknis
- Menyalahkan anggota tim lain di depan umum
- Panik dan diam terlalu lama — lebih baik bilang "beri saya sebentar untuk memikirkannya"

---

## 🧠 Ilustrasi / Analogi

### Analogi: Presentasi Proyek seperti Restoran Membuka Cabang Baru

| Fase Presentasi | Analogi Restoran |
|----------------|-----------------|
| Persiapan (checklist) | Uji coba menu baru sebelum dibuka ke publik — pastikan semua enak dan porsi pas |
| README | Papan menu + kartu nama restoran — orang baru harus tahu apa yang ditawarkan |
| Opening presentasi | Sambutan pemilik restoran: "Selamat datang, kami hadir untuk memecahkan masalah X..." |
| Demo program | Tasting session untuk tamu — suguhkan yang terbaik, urutan yang paling menggugah selera |
| Menjawab Q&A | Chef keluar dapur untuk menjelaskan bahan dan cara masak — jujur dan berpengetahuan |
| Rubrik penilaian | Kriteria Michelin Star: rasa (fungsionalitas), penyajian (kode bersih), konsistensi (dokumentasi) |

---

## 💻 Studi Kasus: Proyek Simulasi ATM Sederhana — Kode Lengkap

> Di pertemuan ini, contoh "teknis" adalah **kode Python fungsional lengkap** dari proyek Simulasi ATM yang sudah direncanakan di Pertemuan 14. Perhatikan bagaimana struktur, docstring, exception handling, dan file I/O diimplementasikan bersama.

```python
"""
Simulasi ATM Sederhana
======================
Program simulasi ATM berbasis CLI yang memungkinkan nasabah
melakukan cek saldo, tarik tunai, transfer, dan melihat riwayat transaksi.

Tim: Budi, Sari, Anton
Mata Kuliah: Dasar-Dasar Pemrograman
"""

import json
import os
from datetime import datetime


# ─────────────────────────────────────────
# DATA: Simulasi "Database" Nasabah
# ─────────────────────────────────────────

DATA_FILE = "data/nasabah.json"

DATA_AWAL = {
    "1234567890": {
        "nama": "Budi Santoso",
        "pin": "1234",
        "saldo": 5000000.0,
        "riwayat": []
    },
    "0987654321": {
        "nama": "Sari Dewi",
        "pin": "5678",
        "saldo": 3000000.0,
        "riwayat": []
    }
}


# ─────────────────────────────────────────
# FUNGSI: Manajemen Data (File I/O)
# ─────────────────────────────────────────

def muat_data():
    """
    Memuat data nasabah dari file JSON.
    Jika file tidak ada, gunakan data awal dan simpan ke file.

    Returns:
        dict: Dictionary berisi data semua nasabah.
    """
    if not os.path.exists("data"):
        os.makedirs("data")

    if os.path.exists(DATA_FILE):
        try:
            with open(DATA_FILE, "r") as f:
                return json.load(f)
        except (json.JSONDecodeError, IOError):
            print("⚠️  File data rusak. Menggunakan data awal.")
            return DATA_AWAL.copy()
    else:
        simpan_data(DATA_AWAL)
        return DATA_AWAL.copy()


def simpan_data(database):
    """
    Menyimpan data nasabah ke file JSON.

    Args:
        database (dict): Dictionary berisi data semua nasabah.
    """
    try:
        with open(DATA_FILE, "w") as f:
            json.dump(database, f, indent=2, ensure_ascii=False)
    except IOError as e:
        print(f"⚠️  Gagal menyimpan data: {e}")


# ─────────────────────────────────────────
# FUNGSI: Autentikasi
# ─────────────────────────────────────────

def proses_login(database):
    """
    Menangani proses login nasabah dengan validasi nomor rekening dan PIN.
    Maksimal 3 kali percobaan PIN yang salah.

    Args:
        database (dict): Data nasabah yang sudah dimuat.

    Returns:
        tuple: (no_rekening, data_nasabah) jika berhasil, atau (None, None) jika gagal.
    """
    print("\n" + "=" * 45)
    print("       SELAMAT DATANG DI ATM SIMULATOR")
    print("=" * 45)

    no_rekening = input("Masukkan Nomor Rekening : ").strip()

    if no_rekening not in database:
        print("\n❌ Nomor rekening tidak ditemukan.")
        return None, None

    nasabah = database[no_rekening]
    max_percobaan = 3

    for percobaan in range(1, max_percobaan + 1):
        pin = input(f"Masukkan PIN (Percobaan {percobaan}/{max_percobaan}): ").strip()

        if pin == nasabah["pin"]:
            print(f"\n✅ Login berhasil. Selamat datang, {nasabah['nama']}!")
            return no_rekening, nasabah
        else:
            sisa = max_percobaan - percobaan
            if sisa > 0:
                print(f"❌ PIN salah. Sisa percobaan: {sisa}")

    print("\n🔒 Kartu Anda telah diblokir. Silakan hubungi bank.")
    return None, None


# ─────────────────────────────────────────
# FUNGSI: Fitur ATM
# ─────────────────────────────────────────

def catat_transaksi(nasabah, jenis, jumlah, keterangan=""):
    """
    Mencatat transaksi ke riwayat nasabah.

    Args:
        nasabah (dict): Data nasabah yang sedang login.
        jenis (str): Jenis transaksi ('Tarik Tunai', 'Transfer Masuk', 'Transfer Keluar').
        jumlah (float): Jumlah uang dalam transaksi.
        keterangan (str): Keterangan tambahan, misal nama penerima transfer.
    """
    transaksi = {
        "waktu": datetime.now().strftime("%d/%m/%Y %H:%M"),
        "jenis": jenis,
        "jumlah": jumlah,
        "saldo_akhir": nasabah["saldo"],
        "keterangan": keterangan
    }
    nasabah["riwayat"].append(transaksi)

    # Simpan hanya 10 transaksi terakhir agar file tidak membengkak
    if len(nasabah["riwayat"]) > 10:
        nasabah["riwayat"] = nasabah["riwayat"][-10:]


def cek_saldo(nasabah):
    """Menampilkan saldo terkini nasabah."""
    print("\n" + "-" * 35)
    print("         INFORMASI SALDO")
    print("-" * 35)
    print(f"  Nama     : {nasabah['nama']}")
    print(f"  Saldo    : Rp {nasabah['saldo']:,.2f}")
    print(f"  Waktu    : {datetime.now().strftime('%d/%m/%Y %H:%M')}")
    print("-" * 35)


def tarik_tunai(no_rekening, nasabah, database):
    """
    Memproses penarikan tunai dari rekening nasabah.

    Args:
        no_rekening (str): Nomor rekening nasabah yang login.
        nasabah (dict): Data nasabah yang sedang login.
        database (dict): Seluruh database nasabah (untuk disimpan setelah transaksi).
    """
    print("\n--- TARIK TUNAI ---")
    try:
        jumlah = float(input("Jumlah penarikan (Rp): "))

        if jumlah <= 0:
            print("❌ Jumlah penarikan harus lebih dari 0.")
            return

        if jumlah > nasabah["saldo"]:
            print(f"❌ Saldo tidak mencukupi. Saldo Anda: Rp {nasabah['saldo']:,.2f}")
            return

        nasabah["saldo"] -= jumlah
        catat_transaksi(nasabah, "Tarik Tunai", jumlah)
        database[no_rekening] = nasabah
        simpan_data(database)

        print("\n✅ Penarikan berhasil!")
        print(f"   Jumlah ditarik : Rp {jumlah:,.2f}")
        print(f"   Saldo tersisa  : Rp {nasabah['saldo']:,.2f}")

    except ValueError:
        print("❌ Input tidak valid. Masukkan angka yang benar.")


def transfer(no_rekening, nasabah, database):
    """
    Memproses transfer uang ke rekening lain.

    Args:
        no_rekening (str): Nomor rekening nasabah pengirim.
        nasabah (dict): Data nasabah pengirim.
        database (dict): Seluruh database nasabah.
    """
    print("\n--- TRANSFER ---")
    no_tujuan = input("Nomor rekening tujuan: ").strip()

    if no_tujuan == no_rekening:
        print("❌ Tidak bisa transfer ke rekening sendiri.")
        return

    if no_tujuan not in database:
        print("❌ Nomor rekening tujuan tidak ditemukan.")
        return

    penerima = database[no_tujuan]
    print(f"   Nama penerima: {penerima['nama']}")
    konfirmasi = input("   Lanjutkan? (y/n): ").strip().lower()

    if konfirmasi != "y":
        print("Transfer dibatalkan.")
        return

    try:
        jumlah = float(input("Jumlah transfer (Rp): "))

        if jumlah <= 0:
            print("❌ Jumlah transfer harus lebih dari 0.")
            return

        if jumlah > nasabah["saldo"]:
            print(f"❌ Saldo tidak mencukupi. Saldo Anda: Rp {nasabah['saldo']:,.2f}")
            return

        # Update saldo pengirim
        nasabah["saldo"] -= jumlah
        catat_transaksi(nasabah, "Transfer Keluar", jumlah, f"ke {penerima['nama']}")

        # Update saldo penerima
        penerima["saldo"] += jumlah
        catat_transaksi(penerima, "Transfer Masuk", jumlah, f"dari {nasabah['nama']}")

        # Simpan kedua perubahan ke database
        database[no_rekening] = nasabah
        database[no_tujuan] = penerima
        simpan_data(database)

        print("\n✅ Transfer berhasil!")
        print(f"   Ke              : {penerima['nama']}")
        print(f"   Jumlah transfer : Rp {jumlah:,.2f}")
        print(f"   Saldo tersisa   : Rp {nasabah['saldo']:,.2f}")

    except ValueError:
        print("❌ Input tidak valid. Masukkan angka yang benar.")


def lihat_riwayat(nasabah):
    """Menampilkan 5 riwayat transaksi terakhir nasabah."""
    print("\n--- RIWAYAT TRANSAKSI (5 Terakhir) ---")
    riwayat = nasabah["riwayat"]

    if not riwayat:
        print("Belum ada riwayat transaksi.")
        return

    # Tampilkan 5 terakhir (dari yang paling baru)
    for trx in reversed(riwayat[-5:]):
        tanda = "+" if "Masuk" in trx["jenis"] else "-"
        print(f"  [{trx['waktu']}] {trx['jenis']}")
        print(f"    {tanda} Rp {trx['jumlah']:,.2f} | Saldo: Rp {trx['saldo_akhir']:,.2f}")
        if trx["keterangan"]:
            print(f"    Ket: {trx['keterangan']}")
        print()


# ─────────────────────────────────────────
# FUNGSI: Menu Utama
# ─────────────────────────────────────────

def menu_utama(no_rekening, nasabah, database):
    """
    Menampilkan dan menangani menu utama ATM.

    Args:
        no_rekening (str): Nomor rekening nasabah aktif.
        nasabah (dict): Data nasabah yang sedang login.
        database (dict): Seluruh database nasabah.
    """
    while True:
        print("\n" + "=" * 35)
        print("          MENU UTAMA ATM")
        print("=" * 35)
        print("  1. Cek Saldo")
        print("  2. Tarik Tunai")
        print("  3. Transfer")
        print("  4. Riwayat Transaksi")
        print("  5. Keluar")
        print("-" * 35)

        pilihan = input("Pilihan Anda: ").strip()

        if pilihan == "1":
            cek_saldo(nasabah)
        elif pilihan == "2":
            tarik_tunai(no_rekening, nasabah, database)
        elif pilihan == "3":
            transfer(no_rekening, nasabah, database)
        elif pilihan == "4":
            lihat_riwayat(nasabah)
        elif pilihan == "5":
            print(f"\nTerima kasih, {nasabah['nama']}. Sampai jumpa! 👋")
            break
        else:
            print("❌ Pilihan tidak valid. Masukkan angka 1-5.")


# ─────────────────────────────────────────
# ENTRY POINT
# ─────────────────────────────────────────

def main():
    """Fungsi utama yang menjalankan program ATM Simulator."""
    database = muat_data()
    no_rekening, nasabah = proses_login(database)

    if nasabah:
        menu_utama(no_rekening, nasabah, database)
    else:
        print("\nSesi berakhir. Terima kasih.")


if __name__ == "__main__":
    main()
```

### Penjelasan Setiap Bagian Kode

| Bagian | Baris | Penjelasan |
|--------|-------|-----------|
| **Docstring modul** | 1-9 | Dokumentasi tingkat file: nama program, deskripsi, tim |
| **Import** | 11-13 | `json` untuk file I/O, `os` untuk cek/buat folder, `datetime` untuk timestamp |
| **Data awal** | 18-32 | Dictionary sebagai simulasi database — ini menggantikan database sungguhan |
| **`muat_data()`** | 37-52 | File I/O: baca JSON; jika rusak, gunakan data awal (exception handling) |
| **`simpan_data()`** | 55-64 | File I/O: tulis JSON dengan `ensure_ascii=False` agar karakter Indonesia tersimpan |
| **`proses_login()`** | 70-95 | Autentikasi dengan loop maksimal 3x percobaan |
| **`catat_transaksi()`** | 101-117 | Menambah entry ke riwayat + batasi 10 transaksi agar file tidak membengkak |
| **`tarik_tunai()`** | 131-153 | Validasi: jumlah > 0, saldo cukup; `try-except ValueError` untuk input non-angka |
| **`transfer()`** | 156-198 | Validasi rekening tujuan, konfirmasi user, update 2 saldo sekaligus |
| **`menu_utama()`** | 211-233 | Loop menu dengan `while True` + `break` untuk keluar |
| **`if __name__ == "__main__"`** | 248 | Pastikan `main()` hanya dijalankan saat file ini dieksekusi langsung, bukan di-import |

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Konteks: Tim Magang di Startup Fintech

**Skenario:** Tiga mahasiswa magang di startup fintech kecil diminta membuat prototype "ATM Simulator" untuk demonstrasi kepada investor. Mereka punya 1 minggu untuk coding dan 1 hari untuk presentasi. Berikut bagaimana mereka mempersiapkan segalanya.

---

**Masalah yang Dihadapi Tim:**

Dua hari sebelum presentasi, Budi menemukan bug: ketika user input huruf (bukan angka) di jumlah penarikan, program crash dengan pesan `ValueError: could not convert string to float`. Di hadapan investor, ini akan terlihat sangat buruk.

**Dampak Bisnis:**
- Investor melihat program crash → meragukan kemampuan tim
- Jika ini sistem nyata, bug seperti ini bisa dieksploitasi (input `abc` untuk bypass validasi)
- Kepercayaan investor hilang = tidak ada pendanaan

**Solusi Teknis:**
Tim menambahkan `try-except ValueError` di semua fungsi yang menerima input angka (seperti yang terlihat di kode di atas). Ini adalah implementasi *defensive programming* — program selalu mengasumsikan user bisa input apa saja yang tidak terduga.

---

**Cara Tim Menyiapkan Demo:**

1. **Buat skenario demo tertulis** — bukan improvisasi:
   ```
   SKENARIO DEMO ATM SIMULATOR
   
   1. Login sebagai Budi (rek: 1234567890, PIN: 1234)
   2. Cek saldo → tunjukkan saldo awal Rp 5.000.000
   3. Tarik tunai Rp 500.000 → tunjukkan saldo berkurang
   4. Transfer Rp 200.000 ke Sari (rek: 0987654321)
   5. Lihat riwayat transaksi → tunjukkan kedua transaksi tercatat
   6. Keluar dan login sebagai Sari → tunjukkan saldo Sari bertambah
   7. Demo edge case: input PIN salah 3x → tunjukkan pesan blokir
   ```

2. **Test skenario ini minimal 3 kali** sebelum hari H

3. **Siapkan "recovery plan"** jika ada yang error: screenshot output yang benar sebagai backup

**Pelajaran Bisnis:** Di fintech nyata, setiap demo ke investor atau klien memiliki "demo environment" tersendiri — bukan environment production. Data sudah disiapkan, state program sudah direset, dan skenario sudah ditest berkali-kali. Bahkan perusahaan seperti Stripe dan Midtrans memiliki dedicated "sandbox mode" untuk demo dan testing.

---

## 📊 Visualisasi

### Contoh README.md Lengkap untuk Proyek ATM Simulator

```markdown
# ATM Simulator — Simulasi ATM Berbasis CLI

Aplikasi simulasi ATM sederhana berbasis Command Line Interface (CLI)
menggunakan Python. Program ini memungkinkan nasabah melakukan
transaksi perbankan dasar: cek saldo, tarik tunai, transfer, dan
melihat riwayat transaksi.

## Fitur
- Login aman dengan validasi PIN (maksimal 3 percobaan)
- Cek saldo real-time
- Tarik tunai dengan validasi saldo mencukupi
- Transfer antar rekening dengan konfirmasi
- Riwayat 5 transaksi terakhir
- Data tersimpan secara persisten (tidak hilang saat restart)

## Cara Menjalankan

**Prasyarat:** Python 3.8 atau lebih baru

1. Clone atau download repository ini
2. Buka terminal, masuk ke folder proyek:
   cd atm-simulator
3. Jalankan program:
   python main.py

## Akun Demo
| Nomor Rekening | PIN  | Nama          | Saldo Awal    |
|----------------|------|---------------|---------------|
| 1234567890     | 1234 | Budi Santoso  | Rp 5.000.000  |
| 0987654321     | 5678 | Sari Dewi     | Rp 3.000.000  |

## Struktur Proyek
atm-simulator/
├── main.py          # Program utama
├── README.md        # Dokumentasi ini
└── data/
    └── nasabah.json # File penyimpanan data nasabah

## Tim Pengembang
| Nama  | NIM       | Peran                              |
|-------|-----------|------------------------------------|
| Budi  | 123456001 | Project Lead, Login & Auth         |
| Sari  | 123456002 | Fitur Transaksi (Tarik & Transfer) |
| Anton | 123456003 | Dokumentasi & File I/O             |

## Teknologi yang Digunakan
- Python 3.10
- Library standar: json, os, datetime (tidak ada library eksternal)
```

---

### Contoh Slide Presentasi Teknis (Outline)

```
Slide 1 — Cover
  Judul: ATM Simulator
  Nama tim + NIM
  Mata kuliah, tanggal

Slide 2 — Masalah yang Diselesaikan
  "Kami membuat simulasi ATM untuk..."
  User persona: siapa yang menggunakan?
  Pain point: apa masalahnya?

Slide 3 — Fitur Program (dengan screenshot/GIF)
  6 fitur utama dalam bullet point
  Screenshot tampilan program

Slide 4 — Arsitektur Program
  Diagram IPO sederhana
  Daftar fungsi dan tanggung jawabnya

Slide 5 — Tantangan & Solusi
  "Kami menghadapi masalah X..."
  "Kami menyelesaikannya dengan Y..."

Slide 6 — Demo (Live)
  [Tidak ada konten — langsung demo terminal]

Slide 7 — Pembelajaran
  Apa yang dipelajari masing-masing anggota?
  Apa yang akan diperbaiki jika ada waktu lebih?

Slide 8 — Terima Kasih + Q&A
```

---

### Rubrik Penilaian Proyek

| Kriteria | Bobot | Deskripsi Penilaian |
|----------|-------|---------------------|
| **Fungsionalitas** | 30% | Apakah semua fitur yang dijanjikan berjalan? Apakah edge case ditangani? |
| **Kualitas Kode** | 25% | Keterbacaan, struktur fungsi, penggunaan docstring dan komentar |
| **Dokumentasi** | 15% | Kelengkapan dan kejelasan README.md |
| **Presentasi & Demo** | 20% | Kejelasan penjelasan, demo berjalan lancar, kemampuan menjawab Q&A |
| **Kolaborasi Tim** | 10% | Bukti pembagian tugas (commit history Git, cerita masing-masing anggota) |

**Poin bonus (+5%):** Exception handling yang komprehensif, atau fitur tambahan yang tidak diminta tapi relevan.

**Yang TIDAK dinilai:**
- Tampilan yang fancy (program CLI sederhana sudah cukup)
- Jumlah baris kode (kode lebih pendek tapi efisien lebih baik dari kode panjang tapi berantakan)
- Menggunakan library eksternal yang tidak perlu

---

## ⚠️ Kesalahan Umum

### 1. Baru "finishing" kode 1 jam sebelum presentasi
**Masalah:** Tidak ada waktu untuk test, demo tidak pernah dicoba, program crash di depan audiens.
**Solusi:** Freeze kode minimal 1 hari sebelum presentasi. Gunakan waktu tersisa untuk latihan presentasi dan demo.

### 2. Demo dengan data yang tidak disiapkan
**Masalah:** Improvisasi input saat demo → hasilnya tidak terencana, bisa reveal bug yang tidak terduga.
**Solusi:** Siapkan skenario demo tertulis dengan input yang sudah ditest. Ikuti skenario itu saat demo.

### 3. README.md hanya berisi judul dan nama tim
**Masalah:** Orang lain tidak bisa menjalankan program tanpa bertanya ke kamu — README tidak berguna.
**Solusi:** README minimal harus berisi: deskripsi program, cara menjalankan, dan akun demo (jika ada).

### 4. Satu anggota tim tidak tahu kode bagian lain
**Masalah:** Pertanyaan diarahkan ke anggota yang "ahli" bagian itu — terlihat tidak kompak.
**Solusi:** Lakukan sesi review kode bersama sebelum presentasi. Semua anggota harus bisa menjelaskan minimal flow utama program.

### 5. Terlalu fokus pada slide, lupa program-nya
**Masalah:** Slide presentasi sangat bagus tapi demo tidak bisa berjalan, atau program punya banyak bug.
**Solusi:** Prioritaskan: program berjalan dengan baik > README lengkap > slide bagus. Bukan sebaliknya.

### 6. Jawab Q&A dengan "itu bagian Anton, bukan saya"
**Masalah:** Dosen/audiens ingin tahu kamu memahami proyekmu, bukan hanya bagianmu.
**Solusi:** Pelajari keseluruhan kode sebelum presentasi. Kalau tidak tahu detail, bilang: "Bagian itu dikerjakan Anton, tapi secara garis besar caranya adalah..."

---

## 🧪 Latihan / Studi Kasus

### Soal 1: Konsep (Rubrik & Checklist)

Misalkan kamu adalah penilai proyek. Dua tim mempresentasikan program yang sama (To-do List CLI):

**Tim A:** Program berjalan sempurna, semua fitur ada, exception handling lengkap. Tapi kode tidak ada komentar sama sekali, README hanya berisi nama tim, dan saat demo mereka improvisasi input sehingga program error sekali di tengah demo.

**Tim B:** Program punya 1 bug kecil (fitur hapus task tidak bekerja). Tapi kode sangat bersih dengan docstring di setiap fungsi, README sangat lengkap termasuk akun demo dan cara instalasi, dan presentasi sangat terstruktur dengan demo yang lancar untuk semua fitur yang berjalan.

**Pertanyaan:**
a) Menggunakan rubrik penilaian di materi ini, berikan estimasi skor (dari 100) untuk Tim A dan Tim B. Jelaskan alasanmu untuk setiap kriteria.
b) Jika kamu jadi anggota salah satu tim, apa 2 hal yang akan kamu perbaiki sebelum presentasi?

---

### Soal 2: Studi Kasus (Demo & Dokumentasi)

**Skenario:** Kamu baru saja menyelesaikan proyek "Kuis Interaktif" CLI. Program bisa:
- Menampilkan 5 soal pilihan ganda dari file `soal.json`
- Menerima jawaban user (a/b/c/d)
- Menghitung dan menampilkan skor akhir
- Menyimpan skor tertinggi ke file `highscore.txt`

**Tugas:**
a) Tulis skenario demo yang terstruktur untuk program ini (urutan aksi + input yang akan digunakan saat demo)
b) Tulis README.md lengkap untuk proyek ini menggunakan template dari materi
c) Identifikasi 3 pertanyaan yang mungkin ditanyakan dosen/penilai, dan siapkan jawaban singkatnya
d) Apa satu edge case yang paling penting untuk kamu demonstrasikan kepada penilai? Kenapa?

---

## 📌 Ringkasan

- **Checklist kualitas** = pastikan fungsionalitas ✓, keterbacaan kode ✓, error handling ✓ sebelum presentasi
- **README.md wajib berisi:** deskripsi, cara install/run, fitur, struktur proyek, tim
- **Struktur presentasi:** Opening → Penjelasan → Arsitektur → Demo → Tantangan → Q&A
- **Demo = performance**, bukan improvisasi: siapkan skenario tertulis, test minimal 3x, siapkan recovery plan
- **Q&A:** ulangi pertanyaan → jawab jelas → tunjukkan ke kode → akui jika tidak tahu
- **Rubrik penilaian:** Fungsionalitas (30%) > Kode (25%) > Presentasi (20%) > Dokumentasi (15%) > Kolaborasi (10%)
- **Freeze kode** minimal 1 hari sebelum presentasi — gunakan waktu tersisa untuk latihan
- **Semua anggota tim** harus bisa menjelaskan keseluruhan program, bukan hanya bagian masing-masing
- Program sederhana yang berjalan sempurna >> program ambisius yang setengah jadi
- Dokumentasi yang baik adalah bentuk profesionalisme — menunjukkan kamu memikirkan orang lain yang akan membaca kodenya
