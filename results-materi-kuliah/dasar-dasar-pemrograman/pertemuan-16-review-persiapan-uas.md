# Pertemuan 16: Review & Persiapan UAS

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Mengingat kembali dan menghubungkan semua konsep pasca-UTS: **Modul & Library, List/Tuple, Dictionary/Set, String & File I/O, Exception Handling, dan OOP Dasar**
- Menggunakan **quick reference cheatsheet** sebagai panduan saat mengerjakan soal
- Mengerjakan soal **teori dan coding** tingkat UAS dengan percaya diri
- Menghindari **common mistakes** yang sering terjadi saat ujian
- Mengintegrasikan multiple konsep (OOP + Exception + File I/O) dalam satu solusi

---

## 📖 Pengantar (Hook)

Bayangkan kamu belajar 6 bab berbeda selama sebulan. Sekarang duduk di hadapan soal UAS, dan otak kamu seperti browser dengan 30 tab terbuka — semuanya ada, tapi bingung mana yang harus dibuka duluan.

Itulah perasaan yang paling umum menjelang ujian.

Tapi ada kabar baiknya: 80% soal UAS pemrograman sebenarnya menguji hal yang sama — apakah kamu *benar-benar paham* konsepnya, bukan sekadar hafal sintaksnya. Programmer profesional pun tidak hafal semua sintaks — mereka memahami *kenapa* sesuatu bekerja, sehingga mereka bisa problem-solve bahkan ketika lupa nama method-nya.

Pertemuan ini adalah sesi "defrag otak" — kita akan merapikan, menghubungkan, dan memperkuat semua yang sudah kamu pelajari. Anggap ini sebagai **peta** sebelum ujian, bukan materi baru.

---

## 🧩 Konsep Utama: Review Materi Pasca-UTS

### Ringkasan Topik yang Diuji di UAS

| Pertemuan | Topik | Konsep Kunci |
|-----------|-------|-------------|
| 8 | Modul & Library | `import`, `from ... import`, membuat modul sendiri |
| 9 | List & Tuple | Indexing, slicing, methods, mutability |
| 10 | Dictionary & Set | Key-value, hashing, operasi set |
| 11 | String & File I/O | String methods, `open()`, `read/write`, `with` statement |
| 12 | Exception Handling | `try-except-finally`, raise, custom exception |
| 13 | OOP Dasar | Class, object, `__init__`, method, encapsulation |

---

## 🧠 Ilustrasi / Analogi

### Analogi: Python Pasca-UTS seperti Dapur Restoran

| Konsep | Analogi Dapur |
|--------|--------------|
| **Modul & Library** | Alat dapur spesialis — blender, oven, mixer. Kamu tidak buat dari nol, kamu pakai yang sudah ada (`import`) |
| **List** | Antrian pesanan di kertas — berurutan, bisa diubah, bisa ditambah/dihapus |
| **Tuple** | Resep yang sudah dicetak dan dilaminasi — berurutan, tapi tidak boleh diubah |
| **Dictionary** | Buku menu dengan kode: `{"nasi_goreng": 35000}` — cari berdasarkan nama, bukan nomor urut |
| **Set** | Daftar bahan unik yang dibutuhkan — tidak ada duplikat, urutan tidak penting |
| **String** | Nama menu dan deskripsi hidangan — bisa dimanipulasi, dicari, dipotong |
| **File I/O** | Buku kas restoran — tulis transaksi hari ini, baca laporan kemarin |
| **Exception Handling** | SOP jika terjadi kesalahan: bahan habis → ganti menu, kompor mati → pakai cadangan |
| **OOP** | Blueprint restoran: `class Restoran` dengan atribut (nama, menu, stok) dan method (pesan, masak, bayar) |

---

## 💻 Contoh Teknis

> Bagian ini berisi quick reference cheatsheet per topik — kode ringkas yang menunjukkan cara penggunaan yang benar.

---

### 1. Modul & Library

```python
# Import library standar
import math
import random
from datetime import datetime

# Import fungsi spesifik
from math import sqrt, pi

# Import dengan alias
import json as js

# Membuat modul sendiri (file utils.py)
# utils.py:
#   def hitung_ppn(harga, rate=0.11):
#       return harga * rate

# Menggunakan modul sendiri
from utils import hitung_ppn

# Contoh penggunaan
print(math.factorial(5))          # 120
print(random.randint(1, 100))     # angka acak 1-100
print(datetime.now().strftime("%d/%m/%Y"))  # tanggal hari ini
```

---

### 2. List & Tuple

```python
# ── LIST (mutable) ──────────────────────
buah = ["apel", "mangga", "jeruk"]

buah.append("durian")          # tambah di akhir
buah.insert(1, "pisang")       # tambah di index 1
buah.remove("jeruk")           # hapus berdasarkan nilai
item = buah.pop()              # ambil dan hapus terakhir
buah.sort()                    # urutkan
print(buah[0])                 # akses index pertama
print(buah[-1])                # akses index terakhir
print(buah[1:3])               # slicing: index 1 sampai 2

# List comprehension
kuadrat = [x**2 for x in range(1, 6)]   # [1, 4, 9, 16, 25]
genap = [x for x in range(10) if x % 2 == 0]  # [0, 2, 4, 6, 8]

# ── TUPLE (immutable) ───────────────────
koordinat = (10.5, -7.2)       # tidak bisa diubah setelah dibuat
lat, lon = koordinat           # tuple unpacking
print(koordinat[0])            # akses index

# Kapan pakai tuple? Data yang tidak boleh berubah:
# koordinat GPS, RGB color, tanggal lahir, dll.
```

---

### 3. Dictionary & Set

```python
# ── DICTIONARY ──────────────────────────
nasabah = {
    "nama": "Budi",
    "saldo": 5000000,
    "aktif": True
}

print(nasabah["nama"])             # "Budi"
print(nasabah.get("email", "N/A")) # "N/A" (key tidak ada, tidak error)
nasabah["email"] = "budi@mail.com" # tambah key baru
del nasabah["aktif"]               # hapus key

# Iterasi dictionary
for key, value in nasabah.items():
    print(f"{key}: {value}")

# Keys, values, items
print(nasabah.keys())    # dict_keys(['nama', 'saldo', 'email'])
print(nasabah.values())  # dict_values(['Budi', 5000000, 'budi@mail.com'])

# Dictionary comprehension
kuadrat = {x: x**2 for x in range(1, 6)}  # {1:1, 2:4, 3:9, 4:16, 5:25}

# ── SET ─────────────────────────────────
kota_a = {"Jakarta", "Bandung", "Surabaya"}
kota_b = {"Bandung", "Surabaya", "Medan"}

print(kota_a & kota_b)   # intersection: {'Bandung', 'Surabaya'}
print(kota_a | kota_b)   # union: semua kota
print(kota_a - kota_b)   # difference: hanya di kota_a
print("Jakarta" in kota_a)  # True — pengecekan cepat O(1)

# Hapus duplikat dari list menggunakan set
data = [1, 2, 2, 3, 3, 3, 4]
unik = list(set(data))   # [1, 2, 3, 4] (urutan tidak dijamin)
```

---

### 4. String & File I/O

```python
# ── STRING METHODS ───────────────────────
teks = "  Selamat Datang di Python!  "

print(teks.strip())          # hapus whitespace di ujung
print(teks.lower())          # huruf kecil semua
print(teks.upper())          # huruf besar semua
print(teks.replace("Python", "Coding"))
print(teks.split())          # pecah jadi list per kata
print(",".join(["a", "b", "c"]))  # "a,b,c"
print("python" in teks.lower())   # True
print(teks.strip().startswith("Selamat"))  # True
print(teks.strip().count("a"))    # hitung huruf 'a'

# f-string (cara modern format string)
nama = "Sari"
saldo = 3000000
print(f"Halo {nama}, saldo kamu: Rp {saldo:,.2f}")
# Output: Halo Sari, saldo kamu: Rp 3,000,000.00

# ── FILE I/O ────────────────────────────
# Tulis file
with open("data.txt", "w", encoding="utf-8") as f:
    f.write("Baris pertama\n")
    f.write("Baris kedua\n")

# Baca file (semua sekaligus)
with open("data.txt", "r", encoding="utf-8") as f:
    isi = f.read()

# Baca file per baris
with open("data.txt", "r", encoding="utf-8") as f:
    for baris in f:
        print(baris.strip())

# Tambah ke file (append)
with open("data.txt", "a", encoding="utf-8") as f:
    f.write("Baris baru ditambahkan\n")

# File CSV dengan library csv
import csv

# Tulis CSV
with open("transaksi.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerow(["Tanggal", "Jenis", "Jumlah"])
    writer.writerow(["2026-04-15", "Tarik", 500000])

# Baca CSV
with open("transaksi.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["Jenis"], row["Jumlah"])
```

---

### 5. Exception Handling

```python
# ── STRUKTUR DASAR ───────────────────────
try:
    angka = int(input("Masukkan angka: "))
    hasil = 10 / angka
    print(f"Hasil: {hasil}")

except ValueError:
    print("Error: input bukan angka!")

except ZeroDivisionError:
    print("Error: tidak bisa dibagi nol!")

except Exception as e:
    print(f"Error tidak terduga: {e}")

else:
    # Dieksekusi HANYA jika tidak ada exception
    print("Operasi berhasil!")

finally:
    # SELALU dieksekusi, ada exception atau tidak
    print("Proses selesai.")

# ── RAISE EXCEPTION ─────────────────────
def validasi_saldo(saldo, jumlah):
    if jumlah <= 0:
        raise ValueError("Jumlah harus lebih dari 0")
    if jumlah > saldo:
        raise ValueError(f"Saldo tidak cukup. Saldo: Rp {saldo:,.2f}")
    return saldo - jumlah

# ── CUSTOM EXCEPTION ────────────────────
class SaldoTidakCukup(Exception):
    """Exception khusus untuk error saldo tidak mencukupi."""
    def __init__(self, saldo, jumlah):
        self.saldo = saldo
        self.jumlah = jumlah
        super().__init__(f"Saldo {saldo} tidak cukup untuk transaksi {jumlah}")

# Penggunaan
try:
    raise SaldoTidakCukup(100000, 500000)
except SaldoTidakCukup as e:
    print(f"Transaksi gagal: {e}")
```

---

### 6. OOP Dasar

```python
# ── CLASS & OBJECT ──────────────────────
class RekeningBank:
    """Representasi rekening bank nasabah."""
    
    # Class variable (shared semua instance)
    bunga_per_tahun = 0.035
    
    def __init__(self, no_rekening, nama, saldo_awal=0):
        """
        Inisialisasi rekening baru.
        
        Args:
            no_rekening (str): Nomor rekening unik
            nama (str): Nama pemilik rekening
            saldo_awal (float): Saldo awal (default 0)
        """
        # Instance variable (unik per object)
        self.no_rekening = no_rekening
        self.nama = nama
        self.__saldo = saldo_awal  # private attribute (encapsulation)
        self.riwayat = []
    
    # Getter untuk private attribute
    @property
    def saldo(self):
        return self.__saldo
    
    def setor(self, jumlah):
        """Menyetor uang ke rekening."""
        if jumlah <= 0:
            raise ValueError("Jumlah setoran harus positif")
        self.__saldo += jumlah
        self.riwayat.append(f"Setor: +{jumlah}")
        return self.__saldo
    
    def tarik(self, jumlah):
        """Menarik uang dari rekening."""
        if jumlah <= 0:
            raise ValueError("Jumlah penarikan harus positif")
        if jumlah > self.__saldo:
            raise ValueError("Saldo tidak mencukupi")
        self.__saldo -= jumlah
        self.riwayat.append(f"Tarik: -{jumlah}")
        return self.__saldo
    
    def __str__(self):
        """String representation object ini."""
        return f"Rekening {self.no_rekening} | {self.nama} | Saldo: Rp {self.__saldo:,.2f}"
    
    def __repr__(self):
        return f"RekeningBank('{self.no_rekening}', '{self.nama}', {self.__saldo})"


# Membuat object (instance dari class)
rek1 = RekeningBank("001", "Budi", 1000000)
rek2 = RekeningBank("002", "Sari", 500000)

print(rek1)                    # __str__ dipanggil
rek1.setor(200000)
rek1.tarik(50000)
print(f"Saldo: {rek1.saldo}")  # gunakan property getter
print(rek1.riwayat)

# Inheritance (pewarisan) — konsep dasar
class RekeningTabungan(RekeningBank):
    """Rekening tabungan dengan fitur bunga."""
    
    def __init__(self, no_rekening, nama, saldo_awal=0):
        super().__init__(no_rekening, nama, saldo_awal)
        self.jenis = "Tabungan"
    
    def hitung_bunga(self):
        """Hitung bunga tahunan berdasarkan saldo saat ini."""
        return self.saldo * self.bunga_per_tahun
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Soal Integratif: Sistem Transaksi Bank dengan File CSV

**Konteks:** Kamu adalah junior developer di startup fintech. Kamu diminta membuat sistem pencatatan transaksi sederhana yang mengintegrasikan OOP, exception handling, dan file I/O.

**Spesifikasi:**
- Buat `class TransaksiBank` yang menyimpan riwayat transaksi dalam List
- Setiap transaksi punya: tanggal, jenis (debit/kredit), jumlah, dan keterangan
- Transaksi bisa disimpan ke file CSV dan dimuat kembali dari file CSV
- Semua operasi sensitif harus punya exception handling yang proper

```python
"""
Soal Integratif UAS: Sistem Transaksi Bank
Mencakup: OOP, Exception Handling, List, File I/O (CSV)
"""

import csv
import os
from datetime import datetime


class TransaksiInvalid(Exception):
    """Custom exception untuk transaksi yang tidak valid."""
    pass


class TransaksiBank:
    """
    Sistem pencatatan transaksi bank sederhana.
    
    Attributes:
        nama_rekening (str): Nama pemilik rekening
        riwayat (list): List berisi dictionary transaksi
    
    Example:
        >>> tb = TransaksiBank("Budi Santoso")
        >>> tb.tambah_transaksi("kredit", 1000000, "Gaji bulan April")
        >>> tb.simpan_csv("transaksi_budi.csv")
    """
    
    JENIS_VALID = ("debit", "kredit")
    
    def __init__(self, nama_rekening):
        """
        Inisialisasi sistem transaksi.
        
        Args:
            nama_rekening (str): Nama pemilik rekening
        
        Raises:
            ValueError: Jika nama_rekening kosong
        """
        if not nama_rekening or not nama_rekening.strip():
            raise ValueError("Nama rekening tidak boleh kosong")
        
        self.nama_rekening = nama_rekening.strip()
        self.riwayat = []
    
    def tambah_transaksi(self, jenis, jumlah, keterangan=""):
        """
        Menambahkan transaksi baru ke riwayat.
        
        Args:
            jenis (str): Jenis transaksi: 'debit' atau 'kredit'
            jumlah (float): Nominal transaksi (harus positif)
            keterangan (str): Deskripsi transaksi (opsional)
        
        Returns:
            dict: Data transaksi yang baru ditambahkan
        
        Raises:
            TransaksiInvalid: Jika jenis tidak valid atau jumlah tidak positif
        """
        # Validasi jenis transaksi
        jenis = jenis.lower().strip()
        if jenis not in self.JENIS_VALID:
            raise TransaksiInvalid(
                f"Jenis transaksi tidak valid: '{jenis}'. "
                f"Gunakan: {self.JENIS_VALID}"
            )
        
        # Validasi jumlah
        try:
            jumlah = float(jumlah)
        except (ValueError, TypeError):
            raise TransaksiInvalid(f"Jumlah harus berupa angka, bukan: {jumlah}")
        
        if jumlah <= 0:
            raise TransaksiInvalid(f"Jumlah harus lebih dari 0, bukan: {jumlah}")
        
        # Buat record transaksi
        transaksi = {
            "tanggal": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "jenis": jenis,
            "jumlah": jumlah,
            "keterangan": keterangan.strip()
        }
        
        self.riwayat.append(transaksi)
        return transaksi
    
    def hitung_saldo(self):
        """
        Menghitung saldo berdasarkan seluruh riwayat transaksi.
        
        Returns:
            float: Total saldo (kredit - debit)
        """
        saldo = 0.0
        for trx in self.riwayat:
            if trx["jenis"] == "kredit":
                saldo += trx["jumlah"]
            else:
                saldo -= trx["jumlah"]
        return saldo
    
    def tampilkan_riwayat(self):
        """Menampilkan seluruh riwayat transaksi ke layar."""
        print(f"\n{'='*55}")
        print(f"  RIWAYAT TRANSAKSI — {self.nama_rekening}")
        print(f"{'='*55}")
        
        if not self.riwayat:
            print("  Belum ada transaksi.")
        else:
            print(f"  {'Tanggal':<22} {'Jenis':<8} {'Jumlah':>15}  Keterangan")
            print(f"  {'-'*52}")
            for trx in self.riwayat:
                tanda = "+" if trx["jenis"] == "kredit" else "-"
                print(
                    f"  {trx['tanggal']:<22} "
                    f"{trx['jenis']:<8} "
                    f"{tanda}Rp {trx['jumlah']:>12,.2f}  "
                    f"{trx['keterangan']}"
                )
        
        print(f"  {'-'*52}")
        print(f"  SALDO AKHIR: Rp {self.hitung_saldo():>12,.2f}")
        print(f"{'='*55}\n")
    
    def simpan_csv(self, nama_file):
        """
        Menyimpan riwayat transaksi ke file CSV.
        
        Args:
            nama_file (str): Nama file CSV tujuan (misal: 'transaksi.csv')
        
        Raises:
            IOError: Jika file tidak bisa ditulis
        """
        try:
            with open(nama_file, "w", newline="", encoding="utf-8") as f:
                fieldnames = ["tanggal", "jenis", "jumlah", "keterangan"]
                writer = csv.DictWriter(f, fieldnames=fieldnames)
                writer.writeheader()
                writer.writerows(self.riwayat)
            
            print(f"✅ Data berhasil disimpan ke '{nama_file}' ({len(self.riwayat)} transaksi)")
        
        except IOError as e:
            raise IOError(f"Gagal menyimpan file '{nama_file}': {e}")
    
    def muat_csv(self, nama_file):
        """
        Memuat riwayat transaksi dari file CSV.
        Data yang ada akan DITIMPA oleh data dari file.
        
        Args:
            nama_file (str): Nama file CSV sumber
        
        Raises:
            FileNotFoundError: Jika file tidak ditemukan
            ValueError: Jika format data di CSV tidak valid
        """
        if not os.path.exists(nama_file):
            raise FileNotFoundError(f"File '{nama_file}' tidak ditemukan")
        
        riwayat_baru = []
        
        try:
            with open(nama_file, "r", encoding="utf-8") as f:
                reader = csv.DictReader(f)
                for nomor_baris, baris in enumerate(reader, start=2):
                    # Validasi jumlah
                    try:
                        jumlah = float(baris["jumlah"])
                    except ValueError:
                        raise ValueError(
                            f"Baris {nomor_baris}: jumlah tidak valid — '{baris['jumlah']}'"
                        )
                    
                    riwayat_baru.append({
                        "tanggal": baris["tanggal"],
                        "jenis": baris["jenis"],
                        "jumlah": jumlah,
                        "keterangan": baris.get("keterangan", "")
                    })
        
        except (KeyError, csv.Error) as e:
            raise ValueError(f"Format file CSV tidak valid: {e}")
        
        self.riwayat = riwayat_baru
        print(f"✅ Berhasil memuat {len(self.riwayat)} transaksi dari '{nama_file}'")
    
    def __str__(self):
        return (
            f"TransaksiBank({self.nama_rekening!r}, "
            f"{len(self.riwayat)} transaksi, "
            f"saldo: Rp {self.hitung_saldo():,.2f})"
        )
    
    def __len__(self):
        return len(self.riwayat)


# ─────────────────────────────────────────
# DEMO PENGGUNAAN
# ─────────────────────────────────────────

def main():
    print("=== Demo Sistem Transaksi Bank ===\n")
    
    # Buat object transaksi
    try:
        tb = TransaksiBank("Budi Santoso")
    except ValueError as e:
        print(f"Gagal buat rekening: {e}")
        return
    
    # Tambah beberapa transaksi
    operasi = [
        ("kredit", 5000000, "Gaji April 2026"),
        ("debit", 500000, "Bayar listrik"),
        ("kredit", 200000, "Transfer dari Sari"),
        ("debit", 150000, "Beli groceries"),
        ("debit", 75000, "Transportasi"),
    ]
    
    for jenis, jumlah, ket in operasi:
        try:
            tb.tambah_transaksi(jenis, jumlah, ket)
        except TransaksiInvalid as e:
            print(f"⚠️  Transaksi ditolak: {e}")
    
    # Test validasi — ini seharusnya gagal
    print("--- Test Exception Handling ---")
    test_cases = [
        ("transfer", 100000, "Jenis tidak valid"),
        ("debit", -50000, "Jumlah negatif"),
        ("kredit", "abc", "Jumlah bukan angka"),
    ]
    for jenis, jumlah, label in test_cases:
        try:
            tb.tambah_transaksi(jenis, jumlah, label)
        except TransaksiInvalid as e:
            print(f"  ✅ Tertangkap: {e}")
    
    # Tampilkan riwayat
    tb.tampilkan_riwayat()
    
    # Simpan ke CSV
    try:
        tb.simpan_csv("transaksi_budi.csv")
    except IOError as e:
        print(f"Error simpan: {e}")
    
    # Muat ulang dari CSV
    tb2 = TransaksiBank("Budi Santoso — Loaded")
    try:
        tb2.muat_csv("transaksi_budi.csv")
        tb2.tampilkan_riwayat()
    except (FileNotFoundError, ValueError) as e:
        print(f"Error muat: {e}")
    
    print(f"Total transaksi: {len(tb)}")
    print(f"Representasi: {tb}")


if __name__ == "__main__":
    main()
```

**Relevansi Bisnis:** Di sistem perbankan nyata (seperti core banking system BCA, Mandiri, atau BRI), setiap transaksi melewati proses validasi berlapis sebelum dicatat. Exception handling bukan pilihan — itu wajib. Bug yang melewatkan validasi bisa menyebabkan transaksi ganda, saldo minus yang tidak terdeteksi, atau data corruption. Class seperti `TransaksiBank` di atas adalah abstraksi sederhana dari apa yang disebut "Transaction Record" atau "Ledger Entry" di sistem keuangan nyata.

---

## 📊 Visualisasi

### Quick Reference Cheatsheet — Pasca-UTS

```
┌──────────────────────────────────────────────────────────────────────┐
│                    CHEATSHEET PYTHON PASCA-UTS                       │
├─────────────────┬────────────────────────────────────────────────────┤
│ MODUL           │ import math / from os import path                  │
│                 │ from mymodule import fungsi_ku                     │
├─────────────────┼────────────────────────────────────────────────────┤
│ LIST            │ lst = [1,2,3]   lst.append(4)  lst.pop()           │
│                 │ lst[0]  lst[-1]  lst[1:3]  len(lst)                │
│                 │ [x**2 for x in lst]  # list comprehension          │
├─────────────────┼────────────────────────────────────────────────────┤
│ TUPLE           │ t = (1, 2, 3)  # immutable                         │
│                 │ a, b, c = t   # unpacking                          │
├─────────────────┼────────────────────────────────────────────────────┤
│ DICT            │ d = {"k": "v"}  d["k"]  d.get("k", default)       │
│                 │ d.keys()  d.values()  d.items()                    │
│                 │ {k: v for k, v in lst}  # dict comprehension       │
├─────────────────┼────────────────────────────────────────────────────┤
│ SET             │ s = {1,2,3}  s.add(4)  s.remove(1)                 │
│                 │ s1 & s2 (irisan)  s1 | s2 (gabungan)               │
├─────────────────┼────────────────────────────────────────────────────┤
│ STRING          │ s.strip()  s.split()  s.join()  s.replace()        │
│                 │ s.lower()  s.upper()  s.startswith()               │
│                 │ f"Halo {nama}, saldo: Rp {saldo:,.2f}"             │
├─────────────────┼────────────────────────────────────────────────────┤
│ FILE I/O        │ with open("f.txt","r",encoding="utf-8") as f:      │
│                 │     isi = f.read()  / for baris in f:              │
│                 │ mode: "r"=baca  "w"=tulis  "a"=tambah              │
├─────────────────┼────────────────────────────────────────────────────┤
│ EXCEPTION       │ try: ... except ValueError: ... finally: ...       │
│                 │ raise ValueError("pesan")                           │
│                 │ class MyErr(Exception): pass                        │
├─────────────────┼────────────────────────────────────────────────────┤
│ OOP             │ class Nama:                                         │
│                 │     def __init__(self, x): self.x = x              │
│                 │     def method(self): return self.x                 │
│                 │ obj = Nama(10)  obj.method()                        │
│                 │ @property  /  __str__  /  __len__                   │
│                 │ class Child(Parent): super().__init__(...)          │
└─────────────────┴────────────────────────────────────────────────────┘
```

---

### Peta Keterhubungan Konsep

```
                    ┌─────────────────┐
                    │   OOP (Class)   │
                    │  - Atribut      │
                    │  - Method       │
                    └────────┬────────┘
                             │ method bisa menggunakan semua konsep di bawah
          ┌──────────────────┼──────────────────┐
          │                  │                  │
          v                  v                  v
   ┌─────────────┐   ┌──────────────┐   ┌─────────────────┐
   │  List/Dict  │   │  Exception   │   │   File I/O      │
   │  (storage)  │   │  Handling    │   │  (persistensi)  │
   └─────┬───────┘   └──────┬───────┘   └────────┬────────┘
         │                  │                     │
         └──────────────────┴─────────────────────┘
                             │
                    ┌────────v────────┐
                    │  String & Modul │
                    │  (utilitas)     │
                    └─────────────────┘
```

---

## ⚠️ Kesalahan Umum

### Saat Ujian — Common Mistakes & Cara Menghindarinya

| No. | Kesalahan | Contoh Salah | Contoh Benar |
|-----|-----------|-------------|-------------|
| 1 | Lupa `self` di parameter method | `def hitung(saldo):` | `def hitung(self, saldo):` |
| 2 | Akses attribute tanpa `self` | `return saldo` | `return self.saldo` |
| 3 | Buka file tanpa `with` | `f = open("x.txt")` | `with open("x.txt") as f:` |
| 4 | `except` terlalu lebar | `except:` (tangkap semua) | `except ValueError:` (spesifik) |
| 5 | Modifikasi list saat iterasi | `for x in lst: lst.remove(x)` | Buat list baru atau gunakan filter |
| 6 | Dict key tidak ada → KeyError | `d["key"]` jika key tidak ada | `d.get("key", default)` |
| 7 | Tuple dikira list | `t = (1,2); t[0] = 5` → error | Tuple tidak bisa diubah |
| 8 | Lupa `__init__` memanggil `super()` | class Child: def `__init__`: ... | `super().__init__(...)` |
| 9 | f-string tidak pakai `f` di depan | `"Halo {nama}"` | `f"Halo {nama}"` |
| 10 | `return` di luar fungsi | Indentasi `return` salah | Pastikan `return` di dalam `def` |

---

## 🧪 Latihan / Studi Kasus

> Di bawah ini adalah soal-soal latihan UAS. Kerjakan sendiri sebelum melihat petunjuk.

---

### Soal Teori

**Soal 1 (Pilihan Ganda):** Perhatikan kode berikut:
```python
data = {"a": 1, "b": 2, "c": 3}
hasil = [v * 2 for k, v in data.items() if v > 1]
print(hasil)
```
Output yang dihasilkan adalah...
- A) `[4, 6]`
- B) `[2, 4, 6]`
- C) `{'b': 4, 'c': 6}`
- D) `[1, 4, 6]`

*(Jawaban: A — hanya nilai > 1 yaitu 2 dan 3, masing-masing dikali 2)*

---

**Soal 2 (Pilihan Ganda):** Blok `finally` dalam `try-except-finally` akan dieksekusi...
- A) Hanya jika tidak ada exception
- B) Hanya jika ada exception
- C) Selalu, ada exception atau tidak
- D) Hanya jika exception ditangkap oleh `except`

*(Jawaban: C)*

---

**Soal 3 (Pilihan Ganda):** Apa perbedaan utama antara List dan Tuple?
- A) List bisa menyimpan tipe data campuran, Tuple tidak
- B) List mutable (bisa diubah), Tuple immutable (tidak bisa diubah)
- C) List menggunakan `[]`, Tuple menggunakan `{}` 
- D) Tuple lebih lambat dari List

*(Jawaban: B)*

---

**Soal 4 (Essay):** Jelaskan apa yang dimaksud dengan **encapsulation** dalam OOP dan mengapa itu penting. Berikan contoh konkret dalam konteks rekening bank.

*Panduan jawaban:*
Encapsulation adalah prinsip OOP yang menyembunyikan detail implementasi internal (atribut) dari luar dan hanya mengeksposnya melalui method yang terkontrol. Contoh: atribut `__saldo` dibuat private (dua underscore) agar tidak bisa diakses/diubah langsung dari luar class. Perubahan saldo hanya boleh melalui method `setor()` dan `tarik()` yang sudah memiliki validasi. Ini penting karena: (1) mencegah saldo diubah sembarangan tanpa validasi, (2) logika bisnis terpusat di satu tempat.

---

**Soal 5 (Essay):** Apa keuntungan menggunakan `with open(...) as f:` dibandingkan `f = open(...)` + `f.close()`?

*Panduan jawaban:*
`with` statement (context manager) secara otomatis memanggil `f.close()` ketika blok `with` selesai, bahkan jika terjadi exception di tengah operasi. Dengan cara lama (`f = open()` + manual `f.close()`), jika ada exception sebelum `f.close()` dipanggil, file tidak pernah ditutup → resource leak, kemungkinan data tidak tertulis sempurna, atau file tetap terkunci. `with` lebih aman dan kode lebih bersih.

---

### Soal Coding

**Soal Coding 1** — List & Dictionary

Diberikan list transaksi berikut:
```python
transaksi = [
    {"jenis": "kredit", "jumlah": 500000},
    {"jenis": "debit", "jumlah": 200000},
    {"jenis": "kredit", "jumlah": 300000},
    {"jenis": "debit", "jumlah": 150000},
    {"jenis": "kredit", "jumlah": 100000},
]
```

Tulis kode Python untuk:
a) Menghitung total kredit dan total debit menggunakan **list comprehension**
b) Menghitung saldo akhir (total kredit - total debit)
c) Menampilkan hanya transaksi dengan jumlah > 200000

*Jawaban:*
```python
total_kredit = sum(t["jumlah"] for t in transaksi if t["jenis"] == "kredit")
total_debit  = sum(t["jumlah"] for t in transaksi if t["jenis"] == "debit")
saldo_akhir  = total_kredit - total_debit

besar = [t for t in transaksi if t["jumlah"] > 200000]

print(f"Total Kredit : Rp {total_kredit:,.2f}")  # 900.000
print(f"Total Debit  : Rp {total_debit:,.2f}")   # 350.000
print(f"Saldo Akhir  : Rp {saldo_akhir:,.2f}")   # 550.000
print(f"Transaksi besar: {besar}")
```

---

**Soal Coding 2** — Exception Handling & String

Tulis fungsi `validasi_email(email)` yang:
- Mengembalikan `True` jika email valid (mengandung `@` dan `.`, tidak ada spasi)
- Raise `ValueError` dengan pesan yang deskriptif jika tidak valid

```python
def validasi_email(email):
    """
    Validasi format email sederhana.
    
    Args:
        email (str): Alamat email yang akan divalidasi
    
    Returns:
        bool: True jika email valid
    
    Raises:
        ValueError: Jika format email tidak valid
    """
    if not isinstance(email, str):
        raise ValueError("Email harus berupa string")
    
    email = email.strip()
    
    if " " in email:
        raise ValueError("Email tidak boleh mengandung spasi")
    
    if "@" not in email:
        raise ValueError("Email harus mengandung karakter '@'")
    
    bagian = email.split("@")
    if len(bagian) != 2 or not bagian[0] or not bagian[1]:
        raise ValueError("Format email tidak valid")
    
    if "." not in bagian[1]:
        raise ValueError("Domain email harus mengandung '.'")
    
    return True

# Test
for email in ["budi@mail.com", "invalid-email", "no space@mail.com", "budi@"]:
    try:
        validasi_email(email)
        print(f"✅ '{email}' valid")
    except ValueError as e:
        print(f"❌ '{email}': {e}")
```

---

**Soal Coding 3** — File I/O

Tulis fungsi `simpan_daftar_nilai(data, nama_file)` yang menyimpan daftar nilai mahasiswa ke file teks, dan `muat_nilai_tertinggi(nama_file)` yang membaca file tersebut dan mengembalikan nama mahasiswa dengan nilai tertinggi.

Format file:
```
NamaA,85
NamaB,92
NamaC,78
```

```python
def simpan_daftar_nilai(data, nama_file):
    """
    Args:
        data (list of tuple): [(nama, nilai), ...]
        nama_file (str): Nama file output
    """
    with open(nama_file, "w", encoding="utf-8") as f:
        for nama, nilai in data:
            f.write(f"{nama},{nilai}\n")

def muat_nilai_tertinggi(nama_file):
    """
    Returns:
        tuple: (nama, nilai) mahasiswa dengan nilai tertinggi
    """
    nilai_tertinggi = None
    
    with open(nama_file, "r", encoding="utf-8") as f:
        for baris in f:
            baris = baris.strip()
            if not baris:
                continue
            nama, nilai = baris.split(",")
            nilai = float(nilai)
            if nilai_tertinggi is None or nilai > nilai_tertinggi[1]:
                nilai_tertinggi = (nama, nilai)
    
    return nilai_tertinggi

# Test
mahasiswa = [("Budi", 85), ("Sari", 92), ("Anton", 78), ("Dewi", 92)]
simpan_daftar_nilai(mahasiswa, "nilai.txt")
terbaik = muat_nilai_tertinggi("nilai.txt")
print(f"Nilai tertinggi: {terbaik[0]} dengan nilai {terbaik[1]}")
```

---

**Soal Coding 4 — INTEGRATIF (OOP + Exception + File I/O)** *(Bobot terbesar)*

Lengkapi class `TransaksiBank` di bawah ini dengan:
- Method `tambah_transaksi(jenis, jumlah)` — validasi jenis ('debit'/'kredit') dan jumlah (positif)
- Method `hitung_saldo()` — hitung saldo dari semua transaksi
- Method `simpan_csv(nama_file)` — simpan ke CSV
- Method `muat_csv(nama_file)` — muat dari CSV (handle FileNotFoundError)

```python
import csv
from datetime import datetime

class TransaksiBank:
    def __init__(self, nama):
        # Lengkapi di sini
        pass
    
    def tambah_transaksi(self, jenis, jumlah):
        # Lengkapi di sini — raise ValueError jika tidak valid
        pass
    
    def hitung_saldo(self):
        # Lengkapi di sini
        pass
    
    def simpan_csv(self, nama_file):
        # Lengkapi di sini
        pass
    
    def muat_csv(self, nama_file):
        # Lengkapi di sini — handle FileNotFoundError
        pass
```

*Petunjuk pengerjaan:*
1. Lihat kode lengkap di bagian Studi Kasus (Fintech) di atas sebagai referensi
2. Fokus pada: validasi input (raise exception), update `self.riwayat`, dan operasi file
3. Gunakan `with open(...)` untuk semua operasi file
4. Test kodenya dengan menambah minimal 3 transaksi, simpan, muat ulang, dan cek saldo

---

## 📌 Ringkasan

**Kumpulan konsep pasca-UTS:**

- **Modul:** `import nama` atau `from modul import fungsi` — jangan reinvent the wheel
- **List:** mutable, berurutan, `append/pop/remove/sort`, slicing, list comprehension
- **Tuple:** immutable, berurutan, unpacking — gunakan untuk data yang tidak boleh berubah
- **Dictionary:** key-value, `d.get(k, default)` lebih aman dari `d[k]`, `.items()` untuk iterasi
- **Set:** unik, tidak berurutan, operasi `&` `|` `-` untuk himpunan
- **String:** `.strip().split().join().replace()`, f-string `f"{var:,.2f}"`
- **File I/O:** selalu gunakan `with open(...)`, mode `r/w/a`, `encoding="utf-8"`
- **Exception:** `try-except-else-finally`, raise spesifik, custom exception extends `Exception`
- **OOP:** `class` + `__init__` + `self`, method, `@property`, `__str__`, inheritance + `super()`

**Tips mengerjakan UAS:**

1. **Baca soal dua kali** sebelum mulai menulis kode
2. **Mulai dari yang kamu yakin** — jangan macet di soal pertama kalau sulit
3. **Tulis kode bertahap**: mulai dari struktur/skeleton, baru isi logikanya
4. **Test mental**: jalankan kode di kepala dengan input contoh sebelum yakin jawabannya benar
5. **Perhatikan indentasi** — ini syntax Python yang paling sering bikin error
6. **Nama variabel yang jelas** menunjukkan kamu paham — `saldo_awal` lebih baik dari `s`
7. **Tambah komentar singkat** jika kamu ragu — menunjukkan kamu tahu logikanya meski kode tidak sempurna
8. **Kalau stuck**, tulis pseudocode dulu lalu terjemahkan ke Python
