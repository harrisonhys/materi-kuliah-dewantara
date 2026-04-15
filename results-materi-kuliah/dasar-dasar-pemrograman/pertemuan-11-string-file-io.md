# Pertemuan 11: String Lanjutan & File I/O

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Menggunakan berbagai **string methods** bawaan Python seperti `upper()`, `lower()`, `strip()`, `split()`, `join()`, `replace()`, `find()`, `startswith()`, `endswith()`, dan `count()` untuk memanipulasi teks secara efisien
- Memformat string dengan **f-string** dan `format()` agar output lebih rapi dan dinamis
- Membuka, membaca, dan menulis file teks menggunakan fungsi `open()` dengan berbagai mode (`r`, `w`, `a`)
- Menggunakan **`with` statement** (context manager) agar file selalu tertutup dengan aman
- Membaca dan menulis **file CSV** menggunakan modul `csv` bawaan Python
- Membangun pipeline sederhana: membaca data mentah → membersihkan → menyimpan hasil

---

## 📖 Pengantar (Hook)

Bayangkan kamu bekerja di tim teknologi sebuah perusahaan fintech. Setiap hari, sistem pembayaran menghasilkan ribuan baris transaksi yang disimpan dalam file `.csv`. Tugas kamu adalah mengambil file itu, membersihkan datanya (ada spasi ekstra, format tanggal tidak konsisten, ada nama merchant yang ditulis campur huruf besar-kecil), lalu menyimpan laporan bersih ke file baru.

Tanpa kemampuan manipulasi string dan file I/O, kamu harus memeriksa satu per satu secara manual — dan dengan 50.000 baris transaksi per hari, itu mustahil dilakukan manusia.

Inilah mengapa **string manipulation** dan **file I/O** adalah dua skill fundamental yang wajib dikuasai setiap programmer backend.

---

## 🧩 Konsep Utama

### 1. String Methods — Alat Bedah Teks

Python punya banyak "pisau" bawaan untuk memotong, membersihkan, dan mengubah teks. Kamu tidak perlu install library tambahan — semuanya sudah ada.

**Mengubah Huruf**
- `upper()` → semua huruf jadi KAPITAL
- `lower()` → semua huruf jadi kecil
- `capitalize()` → huruf pertama kapital, sisanya kecil
- `title()` → setiap kata diawali huruf kapital

**Membersihkan Teks**
- `strip()` → hapus spasi/karakter di kiri dan kanan
- `lstrip()` → hapus di kiri saja
- `rstrip()` → hapus di kanan saja

**Mencari dan Memeriksa**
- `find(sub)` → cari posisi substring, return -1 jika tidak ditemukan
- `count(sub)` → hitung berapa kali substring muncul
- `startswith(prefix)` → cek apakah string dimulai dengan prefix
- `endswith(suffix)` → cek apakah string diakhiri dengan suffix

**Memotong dan Menggabungkan**
- `split(sep)` → potong string berdasarkan separator, hasilnya list
- `join(iterable)` → gabungkan list menjadi string dengan separator tertentu
- `replace(old, new)` → ganti semua kemunculan `old` dengan `new`

### 2. F-String dan format()

Dua cara paling populer untuk menyisipkan variabel ke dalam string:

- **f-string** (Python 3.6+): lebih ringkas, langsung tulis variabel dalam `{}`
- **format()**: lebih kompatibel ke versi lama, tapi lebih verbose

### 3. File I/O — Membaca dan Menulis File

File I/O berarti program kamu bisa **berinteraksi dengan file di hard disk** — bukan hanya data yang ada di memori (RAM).

Mode pembukaan file:
| Mode | Artinya | Kalau file tidak ada? |
|------|---------|----------------------|
| `r`  | Read (baca) | Error! |
| `w`  | Write (tulis, dari awal) | Dibuat otomatis |
| `a`  | Append (tambah di akhir) | Dibuat otomatis |
| `r+` | Read & Write | Error! |

### 4. Context Manager (`with` statement)

Saat membuka file, kamu **wajib menutupnya** setelah selesai. Kalau tidak, file bisa corrupt atau memory leak. `with` statement melakukan ini secara otomatis — bahkan jika ada error sekalipun.

### 5. Modul CSV

File CSV (Comma-Separated Values) adalah format paling umum untuk data tabular. Python punya modul `csv` bawaan yang memudahkan membaca dan menulis file ini tanpa harus parsing manual.

---

## 🧠 Ilustrasi / Analogi

Bayangkan string methods seperti **tools di meja kerja tukang kayu**:

| Tool Tukang Kayu | String Method Python | Fungsi |
|-----------------|---------------------|--------|
| Amplas | `strip()` | Membersihkan tepi yang kasar (spasi ekstra) |
| Cat | `upper()` / `lower()` | Mengubah tampilan keseluruhan |
| Gergaji | `split()` | Memotong menjadi bagian-bagian |
| Lem | `join()` | Menggabungkan potongan jadi satu |
| Penggaris | `find()` | Mencari posisi tertentu |
| Pengecatan ulang | `replace()` | Mengganti bagian tertentu |

Dan file I/O seperti **lemari arsip kantor**:
- `open('file', 'r')` = membuka laci dan membaca dokumen
- `open('file', 'w')` = membuka laci baru (mengosongkan dulu) dan menulis
- `open('file', 'a')` = membuka laci yang ada dan menambah di bagian belakang
- `with` statement = memastikan lacinya selalu ditutup setelah kamu selesai

---

## 💻 Contoh Teknis

### String Methods Dasar

```python
# Contoh data transaksi mentah dari sistem pembayaran
merchant_name = "  TOKOPEDIA PAYMENT  "
transaction_id = "TRX-2024-001-abc"
amount_str = "Rp150.000,00"

# Membersihkan nama merchant
clean_name = merchant_name.strip()
print(clean_name)           # "TOKOPEDIA PAYMENT"
print(clean_name.lower())   # "tokopedia payment"
print(clean_name.title())   # "Tokopedia Payment"

# Memeriksa format transaction ID
print(transaction_id.startswith("TRX"))    # True
print(transaction_id.endswith("abc"))      # True
print(transaction_id.upper())             # "TRX-2024-001-ABC"

# Memisahkan dan menggabungkan
parts = transaction_id.split("-")
print(parts)   # ['TRX', '2024', '001', 'abc']

# Ubah ke format baru: ganti pemisah - dengan _
new_id = "_".join(parts)
print(new_id)  # "TRX_2024_001_abc"

# Menghitung dan mencari
text = "Transfer ke BCA, transfer ke BNI, transfer ke Mandiri"
print(text.count("transfer"))  # 2 (karena case-sensitive!)
print(text.lower().count("transfer"))  # 3

pos = text.find("BCA")
print(f"BCA ditemukan di posisi: {pos}")  # BCA ditemukan di posisi: 12

# replace
normalized = amount_str.replace("Rp", "").replace(".", "").replace(",00", "")
print(normalized)  # "150000"
print(int(normalized))  # 150000
```

### F-String dan format()

```python
nama = "Budi Santoso"
saldo = 2_500_000  # underscore untuk readability
rekening = "1234567890"

# F-string (direkomendasikan, Python 3.6+)
print(f"Halo, {nama}!")
print(f"Saldo rekening {rekening}: Rp{saldo:,.0f}")
# Output: Saldo rekening 1234567890: Rp2,500,000

# Format dengan alignment (rapi untuk laporan)
items = [("Transfer BCA", 500_000), ("Belanja Online", 150_000), ("QRIS", 25_000)]
print(f"{'Keterangan':<20} {'Nominal':>15}")
print("-" * 36)
for keterangan, nominal in items:
    print(f"{keterangan:<20} Rp{nominal:>12,.0f}")

# Output:
# Keterangan           Nominal
# ------------------------------------
# Transfer BCA          Rp     500,000
# Belanja Online        Rp     150,000
# QRIS                  Rp      25,000

# format() — alternatif lama
pesan = "Nama: {}, Saldo: Rp{:,.0f}".format(nama, saldo)
print(pesan)  # Nama: Budi Santoso, Saldo: Rp2,500,000
```

### Membaca dan Menulis File Teks

```python
# ========== MENULIS FILE ==========
# Membuat file laporan baru
with open("laporan_harian.txt", "w", encoding="utf-8") as f:
    f.write("=== LAPORAN TRANSAKSI HARIAN ===\n")
    f.write("Tanggal: 2024-01-15\n")
    f.write("Total Transaksi: 150\n")
    f.write("Total Nilai: Rp45.000.000\n")

# ========== MEMBACA FILE ==========
with open("laporan_harian.txt", "r", encoding="utf-8") as f:
    isi = f.read()
    print(isi)

# Membaca baris per baris (lebih efisien untuk file besar)
with open("laporan_harian.txt", "r", encoding="utf-8") as f:
    for nomor, baris in enumerate(f, start=1):
        print(f"Baris {nomor}: {baris.strip()}")

# ========== MENAMBAH KE FILE ==========
with open("laporan_harian.txt", "a", encoding="utf-8") as f:
    f.write("Status: COMPLETED\n")
```

### Membaca dan Menulis File CSV

```python
import csv

# ========== MENULIS CSV ==========
data_transaksi = [
    ["ID", "Tanggal", "Merchant", "Nominal", "Status"],
    ["TRX001", "2024-01-15", "Tokopedia", "150000", "SUCCESS"],
    ["TRX002", "2024-01-15", "Grab", "45000", "SUCCESS"],
    ["TRX003", "2024-01-15", "PLN", "500000", "PENDING"],
]

with open("transaksi.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerows(data_transaksi)

print("File CSV berhasil dibuat!")

# ========== MEMBACA CSV ==========
with open("transaksi.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)  # DictReader: tiap baris jadi dict
    for baris in reader:
        print(f"ID: {baris['ID']}, Merchant: {baris['Merchant']}, "
              f"Nominal: Rp{int(baris['Nominal']):,}")

# Output:
# ID: TRX001, Merchant: Tokopedia, Nominal: Rp150,000
# ID: TRX002, Merchant: Grab, Nominal: Rp45,000
# ID: TRX003, Merchant: PLN, Nominal: Rp500,000
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Pipeline ETL Laporan Transaksi Harian

**Masalah:**

Tim Data di perusahaan fintech "PayNow" menerima file `raw_transactions.csv` setiap pukul 00.00 dari sistem core banking. File ini berisi data mentah yang penuh masalah:

1. Ada spasi ekstra di nama merchant (` Tokopedia `, `GoPay  `)
2. Nama merchant tidak konsisten (ada yang `GOPAY`, `gopay`, `GoPay`)
3. Kolom nominal berisi format string dengan titik dan koma (`Rp150.000,00`)
4. Ada baris kosong di beberapa tempat
5. Kolom status ada yang `success`, `SUCCESS`, `Success` — tidak konsisten

**Dampak Bisnis:**

Jika data tidak dibersihkan, laporan dashboard manajemen akan menampilkan data ganda (GoPay dan GOPAY dihitung sebagai merchant berbeda), total nominal salah karena parsing gagal, dan keputusan bisnis (seperti promosi cashback ke merchant terbesar) bisa salah sasaran.

**Solusi Teknis: Pipeline ETL Sederhana**

```python
import csv
import os
from datetime import datetime

def bersihkan_nominal(nominal_str):
    """Mengubah 'Rp150.000,00' menjadi integer 150000"""
    bersih = (nominal_str
              .replace("Rp", "")
              .replace(".", "")
              .replace(",00", "")
              .strip())
    return int(bersih) if bersih.isdigit() else 0

def normalisasi_merchant(nama):
    """Standarisasi nama merchant: strip spasi, title case"""
    return nama.strip().title()

def normalisasi_status(status):
    """Standarisasi status transaksi"""
    status_bersih = status.strip().upper()
    status_valid = {"SUCCESS", "PENDING", "FAILED"}
    return status_bersih if status_bersih in status_valid else "UNKNOWN"

def etl_transaksi(file_input, file_output, file_log):
    """
    Pipeline ETL: Extract → Transform → Load
    - Extract : membaca file CSV mentah
    - Transform: membersihkan dan normalisasi data
    - Load     : menyimpan hasil ke file output bersih
    """
    total_dibaca = 0
    total_valid = 0
    total_skip = 0
    total_nilai = 0
    ringkasan_merchant = {}

    print(f"[ETL] Memulai proses: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")

    with open(file_input, "r", encoding="utf-8") as f_in, \
         open(file_output, "w", newline="", encoding="utf-8") as f_out, \
         open(file_log, "a", encoding="utf-8") as f_log:

        reader = csv.DictReader(f_in)
        fieldnames = ["id", "tanggal", "merchant", "nominal", "status"]
        writer = csv.DictWriter(f_out, fieldnames=fieldnames)
        writer.writeheader()

        for baris in reader:
            total_dibaca += 1

            # Skip baris kosong
            if not baris.get("id", "").strip():
                total_skip += 1
                f_log.write(f"[SKIP] Baris {total_dibaca}: ID kosong\n")
                continue

            # Transform
            try:
                nominal = bersihkan_nominal(baris.get("nominal", "0"))
                merchant = normalisasi_merchant(baris.get("merchant", ""))
                status = normalisasi_status(baris.get("status", ""))

                baris_bersih = {
                    "id": baris["id"].strip().upper(),
                    "tanggal": baris["tanggal"].strip(),
                    "merchant": merchant,
                    "nominal": nominal,
                    "status": status
                }

                writer.writerow(baris_bersih)
                total_valid += 1

                if status == "SUCCESS":
                    total_nilai += nominal
                    ringkasan_merchant[merchant] = (
                        ringkasan_merchant.get(merchant, 0) + nominal
                    )

            except (ValueError, KeyError) as e:
                total_skip += 1
                f_log.write(f"[ERROR] Baris {total_dibaca}: {e}\n")

    # Simpan ringkasan ke file laporan
    with open("laporan_ringkasan.txt", "w", encoding="utf-8") as f_summary:
        f_summary.write("=" * 50 + "\n")
        f_summary.write("    LAPORAN RINGKASAN ETL TRANSAKSI\n")
        f_summary.write("=" * 50 + "\n")
        f_summary.write(f"Total baris dibaca : {total_dibaca}\n")
        f_summary.write(f"Total valid        : {total_valid}\n")
        f_summary.write(f"Total diskip       : {total_skip}\n")
        f_summary.write(f"Total nilai SUCCESS: Rp{total_nilai:,.0f}\n\n")
        f_summary.write("Top Merchant (by nilai transaksi):\n")

        merchant_sorted = sorted(
            ringkasan_merchant.items(),
            key=lambda x: x[1],
            reverse=True
        )
        for i, (merchant, nilai) in enumerate(merchant_sorted[:5], 1):
            f_summary.write(f"  {i}. {merchant:<20} Rp{nilai:>12,.0f}\n")

    print(f"[ETL] Selesai! Valid: {total_valid}, Skip: {total_skip}")
    print(f"[ETL] Output tersimpan di: {file_output}")
    print(f"[ETL] Log tersimpan di: {file_log}")

# Jalankan pipeline
etl_transaksi(
    file_input="raw_transactions.csv",
    file_output="clean_transactions.csv",
    file_log="etl_error.log"
)
```

**Hasilnya:** Data yang sebelumnya kotor dan tidak konsisten menjadi bersih, terstandar, dan siap dianalisis. Dashboard manajemen kini menampilkan angka yang akurat, dan tim bisa mengidentifikasi merchant mana yang berkontribusi paling besar terhadap revenue.

---

## 📊 Visualisasi

### Alur Pipeline ETL

```
File raw_transactions.csv (data kotor)
            │
            ▼
    ┌───────────────┐
    │  EXTRACT      │  ← open() + csv.DictReader
    │  Baca baris   │
    └───────┬───────┘
            │
            ▼
    ┌───────────────┐
    │  TRANSFORM    │  ← strip(), title(), upper(), replace()
    │  Bersihkan    │  ← Validasi, normalisasi format
    │  Normalisasi  │
    └───────┬───────┘
            │
         ┌──┴──┐
         │ OK? │
         └──┬──┘
       ✓ Ya │  ✗ Tidak
            │         └──→ Tulis ke etl_error.log
            ▼
    ┌───────────────┐
    │  LOAD         │  ← csv.DictWriter
    │  Tulis ke     │
    │  clean CSV    │
    └───────┬───────┘
            │
            ▼
    laporan_ringkasan.txt
```

### Mode Open() — Kapan Pakai Apa?

| Situasi | Mode | Contoh Kasus |
|---------|------|-------------|
| Membaca file yang sudah ada | `r` | Baca laporan kemarin |
| Membuat file baru (hapus lama) | `w` | Buat laporan hari ini |
| Menambah data ke file yang ada | `a` | Tambah log error baru |
| Baca dan tulis sekaligus | `r+` | Update file konfigurasi |

---

## ⚠️ Kesalahan Umum

**1. Lupa `encoding="utf-8"` → File error ketika ada karakter Indonesia**

```python
# ❌ SALAH — bisa error jika ada huruf seperti é, ñ, atau emoji
with open("laporan.txt", "r") as f:
    data = f.read()

# ✅ BENAR
with open("laporan.txt", "r", encoding="utf-8") as f:
    data = f.read()
```

**2. Lupa menutup file (tidak pakai `with`)**

```python
# ❌ SALAH — kalau ada error sebelum f.close(), file tidak akan tertutup
f = open("data.txt", "w")
f.write("data")
f.close()  # kalau error sebelum baris ini, file tetap terbuka!

# ✅ BENAR — with otomatis menutup file meski ada error
with open("data.txt", "w") as f:
    f.write("data")
```

**3. Mode `w` menghapus isi file lama**

```python
# ❌ BAHAYA — ini akan menghapus semua isi log yang sudah ada!
with open("error.log", "w") as f:
    f.write("Error baru\n")

# ✅ BENAR — gunakan "a" untuk menambah di akhir
with open("error.log", "a") as f:
    f.write("Error baru\n")
```

**4. `split()` tanpa argumen vs dengan argumen**

```python
teks = "  nama  kamu   "
print(teks.split())       # ['nama', 'kamu'] — otomatis strip & hapus spasi ganda
print(teks.split(" "))    # ['', '', 'nama', '', 'kamu', '', '', ''] — berbeda!
```

**5. `find()` vs `in` — jangan tertukar fungsinya**

```python
teks = "Transfer berhasil"

# find() → mengembalikan posisi (angka), bukan True/False
posisi = teks.find("berhasil")   # 9
tidak_ada = teks.find("gagal")  # -1

# "in" → lebih simpel untuk sekadar cek ada/tidak
print("berhasil" in teks)  # True
print("gagal" in teks)     # False
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep: String Parsing

Diberikan string berikut (data mentah dari API pembayaran):

```python
raw = "  trx_id=TRX-20240115-001 | amount=Rp 150.000 | status=success  "
```

Tugas kamu:
1. Bersihkan string (hapus spasi di awal dan akhir)
2. Pisahkan menjadi 3 bagian berdasarkan ` | `
3. Dari setiap bagian, ambil nilainya (setelah `=`)
4. Normalisasi: `trx_id` jadi uppercase, `amount` jadi integer, `status` jadi uppercase
5. Tampilkan hasil akhir dengan f-string

**Expected Output:**
```
Transaction ID : TRX-20240115-001
Nominal        : Rp150,000
Status         : SUCCESS
```

### Soal 2 — Studi Kasus: Sistem Rekonsiliasi Mutasi

Kamu mendapatkan dua file:

**`bank_statement.csv`** (data dari bank):
```
id,tanggal,keterangan,debit,kredit
1,2024-01-15,Transfer masuk Budi,0,500000
2,2024-01-15,Pembayaran PLN,250000,0
3,2024-01-15,Transfer masuk Ani,0,1000000
```

**`internal_records.csv`** (data dari sistem internal):
```
ref_id,date,description,amount,type
1,2024-01-15,Budi Transfer,500000,CREDIT
2,2024-01-15,PLN Payment,250000,DEBIT
```

Tugas:
1. Baca kedua file CSV
2. Cari transaksi yang ada di `bank_statement.csv` tapi **tidak ada** di `internal_records.csv` (berdasarkan ID)
3. Simpan daftar transaksi yang "tidak cocok" ke file `rekonsiliasi_selisih.csv`
4. Buat ringkasan di `rekonsiliasi_laporan.txt` yang menampilkan: total transaksi bank, total transaksi internal, dan jumlah selisih

**Petunjuk:** Gunakan `set()` untuk membandingkan ID, `csv.DictReader` untuk membaca, dan `csv.DictWriter` untuk menulis hasilnya.

---

## 📌 Ringkasan

**String Methods — Cheatsheet:**

| Method | Contoh | Hasil |
|--------|--------|-------|
| `strip()` | `"  hello  ".strip()` | `"hello"` |
| `upper()` | `"hello".upper()` | `"HELLO"` |
| `lower()` | `"HELLO".lower()` | `"hello"` |
| `title()` | `"hello world".title()` | `"Hello World"` |
| `replace(a,b)` | `"a-b".replace("-","_")` | `"a_b"` |
| `split(sep)` | `"a,b,c".split(",")` | `["a","b","c"]` |
| `join(list)` | `"-".join(["a","b"])` | `"a-b"` |
| `startswith(s)` | `"TRX".startswith("TR")` | `True` |
| `endswith(s)` | `"file.csv".endswith(".csv")` | `True` |
| `find(s)` | `"hello".find("ll")` | `2` |
| `count(s)` | `"aaa".count("a")` | `3` |

**File I/O — Pola Wajib:**

```python
# Selalu gunakan with + encoding="utf-8"
with open("file.txt", "r", encoding="utf-8") as f:
    data = f.read()        # baca semua sekaligus
    # atau
    baris = f.readlines()  # baca jadi list per baris
    # atau
    for line in f:         # iterasi baris per baris (paling efisien)
        ...
```

**CSV — Pola Wajib:**

```python
import csv

# Baca CSV sebagai dict (direkomendasikan)
with open("data.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["kolom_tertentu"])

# Tulis CSV
with open("output.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["kolom1", "kolom2"])
    writer.writeheader()
    writer.writerow({"kolom1": "nilai1", "kolom2": "nilai2"})
```

**Ingat selalu:**
- `with` statement → file selalu tertutup
- `encoding="utf-8"` → aman untuk teks Indonesia
- `newline=""` → wajib saat menulis CSV di Windows agar tidak ada baris kosong
- Mode `w` menghapus file lama — gunakan `a` untuk append
