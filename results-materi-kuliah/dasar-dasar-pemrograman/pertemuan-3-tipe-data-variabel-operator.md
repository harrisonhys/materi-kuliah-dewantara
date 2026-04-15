# Pertemuan 3: Tipe Data, Variabel, Operator & Ekspresi

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Membedakan tipe data dasar Python: `int`, `float`, `str`, `bool`, dan `NoneType`
- Membuat dan menamai variabel sesuai konvensi Python (`snake_case`)
- Melakukan konversi tipe data menggunakan `int()`, `float()`, dan `str()`
- Menggunakan operator aritmatika, perbandingan, dan logika dengan benar
- Memahami *operator precedence* dan menerapkannya dalam ekspresi kompleks
- Mengaplikasikan konsep-konsep ini untuk kalkulasi keuangan sederhana

---

## 📖 Pengantar (Hook)

Bayangkan kamu baru bergabung sebagai junior developer di startup fintech. Tugas pertamamu: hitung berapa cicilan yang harus dibayar nasabah setiap bulannya.

Data yang kamu terima dari database:

```
pinjaman_pokok: "15000000"    ← dari form web, datangnya string!
bunga_tahunan: 12             ← persen per tahun
tenor_bulan: 24               ← bulan
```

Kamu langsung nulis:
```python
cicilan = pinjaman_pokok / tenor_bulan
```

Dan dapat error:
```
TypeError: unsupported operand type(s) for /: 'str' and 'int'
```

Panik? Tenang. Masalahnya jelas: kamu mencoba **membagi string dengan angka**. Python tidak bisa melakukan itu — sama seperti kamu tidak bisa membagi kata "lima belas juta" dengan angka 24.

Ini bukan bug, ini **Python melindungi kamu** dari perhitungan yang tidak masuk akal. Solusinya? Konversi tipe data: `int("15000000")` — dan semua beres.

Inilah mengapa memahami tipe data, variabel, dan operator adalah fondasi absolut pemrograman.

---

## 🧩 Konsep Utama

### Variabel: Kotak Bernama di Memori

Variabel adalah **nama yang merujuk ke sebuah nilai di memori komputer**. Kamu bisa bayangkan sebagai label yang ditempel ke kotak penyimpanan.

```python
saldo = 1500000        # kotak bernama "saldo" berisi angka 1500000
nama_nasabah = "Budi"  # kotak bernama "nama_nasabah" berisi teks "Budi"
```

### Aturan Penamaan Variabel

| Aturan | Contoh Benar | Contoh Salah |
|---|---|---|
| Huruf, angka, underscore saja | `total_bayar` | `total-bayar` (ada tanda `-`) |
| Tidak boleh diawali angka | `nilai2` | `2nilai` |
| Case-sensitive | `Saldo ≠ saldo` | — |
| Tidak boleh keyword Python | `penghasilan` | `if`, `for`, `class` |
| Gunakan `snake_case` | `bunga_tahunan` | `bungaTahunan` (itu camelCase, gaya Java) |

**Konvensi di Python — gunakan `snake_case`:**
```python
# Benar (Pythonic)
bunga_per_bulan = 0.01
nama_lengkap = "Siti Rahayu"
total_transaksi_hari_ini = 0

# Hindari
BungaPerBulan = 0.01      # ini PascalCase, biasanya untuk nama class
bungaPerBulan = 0.01      # ini camelCase, gaya JavaScript
```

### Tipe Data Dasar Python

#### 1. `int` — Bilangan Bulat
Angka tanpa koma. Bisa positif, negatif, atau nol.
```python
tenor_bulan = 24
jumlah_transaksi = 150
tahun_lahir = 1998
skor_kredit = -50  # bisa negatif
```

#### 2. `float` — Bilangan Desimal
Angka dengan koma/pecahan.
```python
bunga_tahunan = 0.12       # 12%
nilai_tukar = 15750.50
komisi_persen = 2.5
```

Hati-hati dengan floating point precision!
```python
print(0.1 + 0.2)   # Output: 0.30000000000000004 (bukan 0.3!)
# Solusi untuk keuangan: gunakan round() atau library decimal
print(round(0.1 + 0.2, 2))  # Output: 0.3
```

#### 3. `str` — String (Teks)
Teks apapun, diapit tanda kutip satu `'...'` atau dua `"..."`.
```python
nama = "Ahmad Fauzi"
nomor_rekening = "1234567890"   # Nomor rekening itu STRING, bukan int!
status = 'aktif'
pesan = "Transaksi senilai Rp 500.000 berhasil"
```

#### 4. `bool` — Boolean (Benar/Salah)
Hanya dua nilai: `True` atau `False`. Perhatikan huruf kapital di awal!
```python
is_verified = True
has_active_loan = False
is_premium_member = True
```

#### 5. `NoneType` — Tidak Ada Nilai
`None` digunakan untuk menyatakan "tidak ada nilai" atau "belum diisi".
```python
tanggal_jatuh_tempo = None   # belum ada, pinjaman belum disetujui
alasan_penolakan = None      # belum ada alasan (belum diproses)
```

### Cek Tipe Data dengan `type()`
```python
saldo = 1500000
print(type(saldo))        # <class 'int'>
print(type(3.14))         # <class 'float'>
print(type("Halo"))       # <class 'str'>
print(type(True))         # <class 'bool'>
print(type(None))         # <class 'NoneType'>
```

### Konversi Tipe Data (Type Conversion)

```python
# String ke Integer
angka_str = "500000"
angka_int = int(angka_str)      # 500000
print(type(angka_int))          # <class 'int'>

# String ke Float
persen_str = "2.5"
persen_float = float(persen_str)   # 2.5

# Angka ke String (untuk digabung dengan teks)
saldo = 1500000
pesan = "Saldo kamu: Rp " + str(saldo)    # "Saldo kamu: Rp 1500000"

# Float ke Int (membulatkan ke bawah, bukan rounding!)
komisi_float = 12750.99
komisi_int = int(komisi_float)    # 12750 (bukan 12751!)
```

**Yang TIDAK bisa dikonversi:**
```python
int("dua ratus")    # ValueError! "dua ratus" bukan format angka valid
int("12.5")         # ValueError! Gunakan float() dulu, baru int()
                    # atau: int(float("12.5")) → 12
```

---

## 🧠 Ilustrasi / Analogi

### Analogi: Tipe Data = Jenis Wadah di Dapur

| Tipe Data | Analogi Wadah | Isi yang Cocok | Isi yang TIDAK Cocok |
|---|---|---|---|
| `int` | Laci bilangan bulat | 5, -3, 1000000 | 3.14, "lima" |
| `float` | Gelas ukur | 1.5, 0.001, 99.99 | "dua koma lima" |
| `str` | Amplop teks | "Ahmad", "Rp 500.000", "123" | (bisa isi apa saja tapi jadi teks) |
| `bool` | Saklar lampu | True, False | Tidak ada posisi tengah |
| `None` | Wadah kosong | — | — |

### Analogi: Operator = Kalkulator Khusus

Bayangkan kamu punya beberapa kalkulator berbeda:
- **Kalkulator Aritmatika** → hitung angka: `+`, `-`, `*`, `/`, `//`, `%`, `**`
- **Kalkulator Perbandingan** → cek hubungan: `==`, `!=`, `>`, `<`, `>=`, `<=` → hasilnya selalu `True`/`False`
- **Kalkulator Logika** → gabungkan kondisi: `and`, `or`, `not` → hasilnya selalu `True`/`False`

---

## 💻 Contoh Teknis

### Operator Aritmatika

```python
pinjaman = 12000000   # Rp 12 juta
tenor = 12            # 12 bulan
bunga_per_bulan = 0.01  # 1% per bulan

# Operator dasar
cicilan_pokok = pinjaman / tenor          # 1000000.0  (pembagian float)
cicilan_pokok_int = pinjaman // tenor     # 1000000    (pembagian bulat)
bunga_bulan_ini = pinjaman * bunga_per_bulan  # 120000.0
sisa = pinjaman % tenor                   # 0 (sisa bagi)
bunga_setahun_compound = pinjaman * (1 + bunga_per_bulan) ** tenor  # compound interest

print(f"Cicilan pokok: Rp {cicilan_pokok_int:,}")
print(f"Bunga bulan pertama: Rp {bunga_bulan_ini:,.0f}")
```

**Tabel Operator Aritmatika:**

| Operator | Nama | Contoh | Hasil |
|---|---|---|---|
| `+` | Penjumlahan | `1000 + 500` | `1500` |
| `-` | Pengurangan | `5000 - 1200` | `3800` |
| `*` | Perkalian | `500 * 12` | `6000` |
| `/` | Pembagian (float) | `7 / 2` | `3.5` |
| `//` | Pembagian bulat | `7 // 2` | `3` |
| `%` | Modulo (sisa bagi) | `7 % 2` | `1` |
| `**` | Pangkat | `2 ** 8` | `256` |

### Operator Perbandingan

```python
saldo = 500000
nominal_transfer = 750000
limit_harian = 5000000
total_hari_ini = 2000000

print(saldo >= nominal_transfer)                    # False → saldo tidak cukup
print(nominal_transfer > 0)                         # True → nominal valid
print(total_hari_ini + nominal_transfer <= limit_harian)  # True → masih dalam limit
print(saldo == 0)                                   # False → saldo tidak nol
print(saldo != 0)                                   # True → saldo ada isinya
```

### Operator Logika

```python
# and → kedua kondisi harus True
saldo_cukup = saldo >= nominal_transfer    # False
dalam_limit = total_hari_ini + nominal_transfer <= limit_harian  # True
akun_aktif = True

bisa_transfer = saldo_cukup and dalam_limit and akun_aktif
print(bisa_transfer)  # False (karena saldo_cukup = False)

# or → minimal satu kondisi True
bisa_promo = total_hari_ini > 1000000 or akun_aktif
print(bisa_promo)  # True (karena akun_aktif = True)

# not → membalik nilai boolean
print(not akun_aktif)  # False
print(not (saldo == 0))  # True (saldo bukan nol)
```

### Operator Precedence (Prioritas Operator)

Python mengikuti aturan matematika: PEMDAS (Pangkat, perkalian/pembagian, penjumlahan/pengurangan).

```python
# Tanpa kurung — bisa membingungkan
hasil1 = 2 + 3 * 4      # 14, bukan 20! (perkalian dulu)
hasil2 = 10 - 2 ** 2    # 6, bukan 64! (pangkat dulu)

# Dengan kurung — lebih jelas dan aman
hasil3 = (2 + 3) * 4    # 20 (penjumlahan dulu karena dalam kurung)
hasil4 = (10 - 2) ** 2  # 64

# Contoh nyata: hitung total pembayaran dengan pajak dan diskon
harga = 1000000
diskon_persen = 10
pajak_persen = 11

# Salah (tanpa kurung yang tepat):
total_salah = harga - harga * diskon_persen / 100 + harga * pajak_persen / 100

# Benar (explicit):
harga_setelah_diskon = harga * (1 - diskon_persen / 100)  # 900000
total_bayar = harga_setelah_diskon * (1 + pajak_persen / 100)  # 999000
print(f"Total bayar: Rp {total_bayar:,.0f}")  # Rp 999,000
```

**Urutan Prioritas (dari tertinggi ke terendah):**

| Prioritas | Operator | Contoh |
|---|---|---|
| 1 (tertinggi) | `()` | `(2 + 3)` |
| 2 | `**` | `2 ** 3` |
| 3 | `*`, `/`, `//`, `%` | `10 / 2` |
| 4 | `+`, `-` | `5 + 3` |
| 5 | `==`, `!=`, `<`, `>`, `<=`, `>=` | `x > 0` |
| 6 | `not` | `not True` |
| 7 | `and` | `a and b` |
| 8 (terendah) | `or` | `a or b` |

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Kasus: Kalkulasi Bunga Flat & Validasi Data Nasabah

**Latar Belakang:**
Di platform pinjaman online (P2P Lending), sebelum sistem memproses pengajuan pinjaman, data nasabah harus divalidasi dan simulasi cicilan harus dihitung secara otomatis untuk ditampilkan ke calon peminjam.

**Masalah Bisnis:**
Data yang masuk dari formulir pendaftaran selalu berupa string (teks). Jika tim developer tidak memahami konversi tipe data, kalkulasi bisa salah atau crash — yang berarti pengalaman buruk bagi nasabah dan potensi kerugian bisnis.

**Solusi Teknis:**

```python
# ============================================================
# SIMULASI PINJAMAN - P2P Lending Platform
# ============================================================

print("=" * 50)
print("  SIMULASI PINJAMAN - KreditKu App")
print("=" * 50)

# Data dari form (semuanya masuk sebagai string)
nama_raw = "Ahmad Fauzi"
penghasilan_raw = "8500000"     # dari input form, string!
pinjaman_raw = "15000000"       # dari input form, string!
tenor_raw = "24"                # dari input form, string!
usia_raw = "29"                 # dari input form, string!
punya_tunggakan_raw = "tidak"   # dari input form, string!

# ---- STEP 1: Konversi Tipe Data ----
nama = str(nama_raw).strip().title()         # "Ahmad Fauzi"
penghasilan = int(penghasilan_raw)           # 8500000
pinjaman = int(pinjaman_raw)                 # 15000000
tenor = int(tenor_raw)                       # 24
usia = int(usia_raw)                         # 29
punya_tunggakan = punya_tunggakan_raw.lower() == "ya"  # False (bool)

# ---- STEP 2: Validasi Data ----
PENGHASILAN_MINIMUM = 3000000
USIA_MINIMUM = 21
USIA_MAKSIMUM = 55
RASIO_CICILAN_MAKSIMUM = 0.40   # maksimal 40% penghasilan

penghasilan_valid = penghasilan >= PENGHASILAN_MINIMUM
usia_valid = USIA_MINIMUM <= usia <= USIA_MAKSIMUM
tidak_ada_tunggakan = not punya_tunggakan

print(f"\nNama Nasabah   : {nama}")
print(f"Penghasilan    : Rp {penghasilan:,}")
print(f"Usia           : {usia} tahun")
print(f"Jumlah Pinjaman: Rp {pinjaman:,}")
print(f"Tenor          : {tenor} bulan")

# ---- STEP 3: Kalkulasi Cicilan (Metode Flat) ----
BUNGA_TAHUNAN = 0.18           # 18% per tahun (flat rate umum P2P Lending)
bunga_per_bulan = BUNGA_TAHUNAN / 12   # 0.015 = 1.5% per bulan

pokok_per_bulan = pinjaman / tenor                      # 625000.0
bunga_per_bulan_nominal = pinjaman * bunga_per_bulan    # 225000.0
cicilan_per_bulan = pokok_per_bulan + bunga_per_bulan_nominal  # 850000.0
total_bayar = cicilan_per_bulan * tenor                 # 20400000.0
total_bunga = total_bayar - pinjaman                    # 5400000.0

# ---- STEP 4: Cek Rasio Cicilan ----
rasio_cicilan = cicilan_per_bulan / penghasilan         # 0.1 = 10%
cicilan_dalam_batas = rasio_cicilan <= RASIO_CICILAN_MAKSIMUM

# ---- STEP 5: Keputusan Kelayakan ----
layak = penghasilan_valid and usia_valid and tidak_ada_tunggakan and cicilan_dalam_batas

print("\n---- Hasil Validasi ----")
print(f"Penghasilan cukup    : {'✅' if penghasilan_valid else '❌'}")
print(f"Usia sesuai          : {'✅' if usia_valid else '❌'}")
print(f"Tidak ada tunggakan  : {'✅' if tidak_ada_tunggakan else '❌'}")
print(f"Rasio cicilan ({rasio_cicilan:.1%}): {'✅' if cicilan_dalam_batas else '❌'}")

print("\n---- Simulasi Cicilan ----")
print(f"Cicilan per bulan    : Rp {cicilan_per_bulan:,.0f}")
print(f"Total bunga          : Rp {total_bunga:,.0f}")
print(f"Total pembayaran     : Rp {total_bayar:,.0f}")
print(f"Efektif bunga/tahun  : {BUNGA_TAHUNAN:.0%}")

print("\n---- Keputusan ----")
if layak:
    print("✅ PENGAJUAN DISETUJUI - Proses lanjut ke verifikasi dokumen")
else:
    print("❌ PENGAJUAN DITOLAK - Tidak memenuhi kriteria kelayakan")
print("=" * 50)
```

**Output:**
```
==================================================
  SIMULASI PINJAMAN - KreditKu App
==================================================

Nama Nasabah   : Ahmad Fauzi
Penghasilan    : Rp 8,500,000
Usia           : 29 tahun
Jumlah Pinjaman: Rp 15,000,000
Tenor          : 24 bulan

---- Hasil Validasi ----
Penghasilan cukup    : ✅
Usia sesuai          : ✅
Tidak ada tunggakan  : ✅
Rasio cicilan (10.0%): ✅

---- Simulasi Cicilan ----
Cicilan per bulan    : Rp 850,000
Total bunga          : Rp 5,400,000
Total pembayaran     : Rp 20,400,000
Efektif bunga/tahun  : 18%

---- Keputusan ----
✅ PENGAJUAN DISETUJUI - Proses lanjut ke verifikasi dokumen
==================================================
```

**Pelajaran dari studi kasus ini:**
- Data dari form web SELALU berupa `str` — konversi wajib dilakukan sebelum kalkulasi
- Gunakan konstanta (UPPER_CASE) untuk nilai bisnis yang tidak berubah seperti bunga dan limit
- `bool` dari string "ya"/"tidak" dilakukan dengan ekspresi perbandingan, bukan langsung casting
- Operator logika `and` menggabungkan semua syarat kelayakan dalam satu ekspresi yang bersih

---

## 📊 Visualisasi

### Alur Penanganan Tipe Data dalam Sistem Fintech

```
Data Masuk dari Form Web
         │
         ▼
   [Semua bertipe str]
    "15000000", "24", "ya"
         │
         ▼
  Langkah 1: KONVERSI
   int("15000000") → 15000000
   int("24")       → 24
   "ya" == "ya"    → True
         │
         ▼
  Langkah 2: VALIDASI
   pinjaman > 0 → True/False
   tenor > 0    → True/False
         │
         ▼
  Langkah 3: KALKULASI
   cicilan = pinjaman / tenor + bunga
         │
         ▼
  Langkah 4: OUTPUT
   str(cicilan) + " per bulan"
   f"Rp {cicilan:,}"
```

### Cheatsheet Konversi Tipe Data

| Dari | Ke | Fungsi | Contoh | Hasil |
|---|---|---|---|---|
| `str` | `int` | `int()` | `int("500")` | `500` |
| `str` | `float` | `float()` | `float("3.14")` | `3.14` |
| `int` | `str` | `str()` | `str(500)` | `"500"` |
| `float` | `int` | `int()` | `int(3.9)` | `3` (bukan 4!) |
| `int` | `float` | `float()` | `float(5)` | `5.0` |
| `str` | `bool` | perbandingan | `"ya" == "ya"` | `True` |
| angka | `bool` | `bool()` | `bool(0)` | `False` |
| angka | `bool` | `bool()` | `bool(100)` | `True` |

---

## ⚠️ Kesalahan Umum

### 1. Lupa Konversi Tipe Setelah `input()`
```python
# SALAH — input() selalu mengembalikan str!
nominal = input("Masukkan nominal: ")
pajak = nominal * 0.11   # TypeError!

# BENAR
nominal = float(input("Masukkan nominal: "))
pajak = nominal * 0.11   # Sekarang works
```

### 2. Membandingkan String dengan Integer
```python
# SALAH
umur = input("Umur kamu: ")   # umur = "25" (string)
if umur >= 21:                 # TypeError! Tidak bisa bandingkan str dengan int
    print("Boleh daftar")

# BENAR
umur = int(input("Umur kamu: "))
if umur >= 21:
    print("Boleh daftar")
```

### 3. Asumsi `int()` Melakukan Rounding
```python
# int() memotong (truncate), bukan membulatkan!
print(int(9.9))    # 9, bukan 10!
print(int(-3.7))   # -3, bukan -4!

# Untuk pembulatan biasa, gunakan round()
print(round(9.9))  # 10 ✅
print(round(9.5))  # 10 ✅
print(round(9.4))  # 9 ✅
```

### 4. Floating Point Tidak Akurat untuk Uang
```python
# MASALAH
print(0.1 + 0.2 == 0.3)   # False! Ini membingungkan

# SOLUSI 1: round() untuk perbandingan
print(round(0.1 + 0.2, 10) == round(0.3, 10))   # True

# SOLUSI 2: Simpan uang dalam satuan terkecil (sen/rupiah)
# Gunakan int, bukan float untuk nilai uang!
saldo_rupiah = 150000    # bukan 150000.00
komisi = saldo_rupiah * 15 // 1000  # integer math, aman
```

### 5. Salah Paham `=` vs `==`
```python
# = adalah ASSIGNMENT (memberi nilai)
saldo = 500000     # saldo sekarang bernilai 500000

# == adalah COMPARISON (membandingkan nilai, hasilnya bool)
print(saldo == 500000)   # True
print(saldo == 0)        # False

# Kesalahan umum: pakai = di dalam kondisi
if saldo = 0:      # SyntaxError!
    pass
if saldo == 0:     # BENAR
    pass
```

### 6. `None` Bukan String Kosong
```python
nama = None
nama_kosong = ""

print(nama == None)        # True
print(nama_kosong == None) # False — "" bukan None!
print(nama == "")          # False — None bukan ""!

# Cara cek None yang Pythonic:
if nama is None:
    print("Nama belum diisi")
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

Tanpa menjalankan di komputer, prediksi output dari kode berikut:

```python
x = 10
y = 3

print(x / y)
print(x // y)
print(x % y)
print(x ** y)
print(type(x / y))
print(type(x // y))

a = True
b = False
print(a and b)
print(a or b)
print(not a)
print(int(a) + int(b))
```

Setelah prediksi, jalankan dan bandingkan hasilnya!

### Soal 2 — Praktik

Buat program yang menghitung **komisi agen penjualan** berdasarkan aturan berikut:
- Total penjualan diinput oleh user (dalam ribuan rupiah)
- Komisi dihitung berdasarkan persentase dari total penjualan
- Komisi 3% untuk penjualan di bawah Rp 10.000.000
- Komisi 5% untuk penjualan Rp 10.000.000 - Rp 50.000.000
- Komisi 7.5% untuk penjualan di atas Rp 50.000.000
- Pajak penghasilan 5% dipotong dari komisi

Contoh output:
```
Total penjualan: Rp 25.000.000
Komisi (5%)    : Rp 1.250.000
PPh (5%)       : Rp 62.500
Komisi bersih  : Rp 1.187.500
```

### Soal 3 — Studi Kasus Fintech

**Skenario:** Kamu diminta membuat fitur **kalkulator cashback** untuk aplikasi e-commerce dengan aturan:

- Transaksi di atas Rp 500.000 mendapat cashback 5%
- Maksimal cashback Rp 50.000 per transaksi
- Ada biaya admin Rp 2.500 untuk setiap transaksi
- Tampilkan: harga asli, cashback yang diterima, biaya admin, dan total yang benar-benar dibayar

Input: nominal transaksi dari user (sebagai `input()`, jadi perlu dikonversi)

**Challenge tambahan:** Bagaimana cara memastikan user tidak bisa input nominal negatif atau 0?

---

## 📌 Ringkasan

**Tipe Data Dasar:**
- `int` → bilangan bulat: `42`, `-100`, `0`
- `float` → bilangan desimal: `3.14`, `0.01`, `-2.5`
- `str` → teks: `"halo"`, `'123'`
- `bool` → benar/salah: `True`, `False`
- `NoneType` → tidak ada nilai: `None`

**Konvensi Variabel:**
- Gunakan `snake_case`: `bunga_tahunan`, `nama_nasabah`
- Nama deskriptif: `cicilan_per_bulan` lebih baik dari `c`
- Konstanta pakai UPPER_CASE: `BUNGA_MINIMUM = 0.12`

**Konversi Tipe:**
- `int("500")` → `500`
- `float("3.5")` → `3.5`
- `str(1000)` → `"1000"`
- `int(3.9)` → `3` (truncate, bukan round!)

**Operator Penting:**
- `/` selalu menghasilkan float; `//` menghasilkan int
- `%` sisa bagi; `**` pangkat
- `==` perbandingan (bukan `=` yang assignment)
- `and`, `or`, `not` untuk logika boolean

**Ingat Selalu:**
- `input()` selalu mengembalikan `str` — konversi sebelum kalkulasi!
- Untuk uang, hindari `float` langsung — gunakan `round()` atau simpan sebagai integer (satuan terkecil)
- `None` berbeda dari `""` (string kosong) dan `0`
