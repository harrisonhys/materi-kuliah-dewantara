# Pertemuan 8: Modul & Library Python (math, random, datetime)

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Memahami konsep modul dan mengapa modul itu penting dalam pengembangan software
- Menggunakan sintaks `import`, `from...import`, dan alias `as` dengan benar
- Memanfaatkan modul `math` untuk kalkulasi matematika (sqrt, ceil, floor, pi, pow)
- Memanfaatkan modul `random` untuk generate angka/pilihan acak secara terkontrol
- Memanfaatkan modul `datetime` untuk manipulasi tanggal, waktu, dan selisih waktu
- Membuat modul Python sendiri (file `.py`) yang bisa diimpor oleh program lain
- Menerapkan modul-modul tersebut dalam skenario sistem keuangan dan backend

---

## 📖 Pengantar (Hook)

Bayangkan kamu bekerja di sebuah startup fintech. Setiap hari ada ribuan transaksi yang masuk. Tiba-tiba atasanmu bilang: "Kita butuh fitur OTP untuk keamanan transfer, fitur cicilan yang otomatis hitung tanggal jatuh tempo, dan semua fungsi utility-nya harus bisa dipakai ulang di seluruh sistem."

Kamu panik. Masa harus nulis semua dari nol?

Tenang. Python sudah menyiapkan "gudang peralatan" bernama **modul**. Kamu tidak perlu reinvent the wheel — cukup ambil alat yang sudah ada, atau buat alat sendiri dan simpan di tempat yang bisa diakses semua bagian program.

Itulah esensi dari modul: **kode yang terorganisir, reusable, dan siap pakai.**

---

## 🧩 Konsep Utama

### Apa itu Modul?

Modul adalah file Python (`.py`) yang berisi definisi fungsi, variabel, atau kelas yang bisa digunakan di file Python lain. Bayangkan modul seperti **buku resep** — kamu tulis sekali, bisa dipakai berkali-kali di restoran mana pun.

Python punya dua jenis modul:
- **Built-in modules**: sudah terinstall bersama Python (math, random, datetime, os, dll.)
- **Third-party modules**: diinstall via `pip` (numpy, pandas, requests, dll.)
- **Custom modules**: buatan kamu sendiri

---

### Cara Import Modul

#### 1. `import nama_modul`
Import seluruh modul. Akses fungsinya dengan `nama_modul.fungsi()`.

```python
import math
print(math.sqrt(16))  # Output: 4.0
```

#### 2. `from nama_modul import fungsi`
Import fungsi spesifik saja. Bisa langsung dipanggil tanpa prefix.

```python
from math import sqrt, pi
print(sqrt(25))  # Output: 5.0
print(pi)        # Output: 3.141592653589793
```

#### 3. `import nama_modul as alias`
Import dengan nama pendek/alias agar lebih praktis.

```python
import datetime as dt
hari_ini = dt.date.today()
```

#### 4. `from nama_modul import *`
Import semua — **hindari ini** di proyek besar karena bisa menyebabkan konflik nama.

---

### Modul `math`

| Fungsi/Konstanta | Keterangan | Contoh |
|---|---|---|
| `math.sqrt(x)` | Akar kuadrat | `sqrt(9)` → `3.0` |
| `math.pow(x, y)` | Pangkat (x^y), return float | `pow(2, 8)` → `256.0` |
| `math.ceil(x)` | Bulatkan ke atas | `ceil(4.1)` → `5` |
| `math.floor(x)` | Bulatkan ke bawah | `floor(4.9)` → `4` |
| `math.pi` | Nilai Pi | `3.141592...` |
| `math.log(x)` | Logaritma natural | `log(math.e)` → `1.0` |
| `math.log10(x)` | Logaritma basis 10 | `log10(1000)` → `3.0` |
| `math.fabs(x)` | Nilai absolut (float) | `fabs(-5)` → `5.0` |

---

### Modul `random`

| Fungsi | Keterangan | Contoh |
|---|---|---|
| `random.random()` | Float acak antara 0.0–1.0 | `0.7423...` |
| `random.randint(a, b)` | Integer acak antara a dan b (inklusif) | `randint(1, 6)` → `4` |
| `random.choice(seq)` | Pilih satu elemen acak dari list | `choice(['A','B','C'])` |
| `random.choices(seq, k=n)` | Pilih n elemen (dengan pengulangan) | `choices('0123456789', k=6)` |
| `random.shuffle(list)` | Acak urutan list (in-place) | `shuffle([1,2,3,4,5])` |
| `random.sample(seq, k)` | Pilih k elemen unik (tanpa pengulangan) | `sample(range(100), 6)` |
| `random.seed(n)` | Set seed untuk hasil yang reproducible | `seed(42)` |

---

### Modul `datetime`

Modul ini punya beberapa kelas utama:

| Kelas | Keterangan |
|---|---|
| `datetime.date` | Hanya tanggal (tahun, bulan, hari) |
| `datetime.time` | Hanya waktu (jam, menit, detik) |
| `datetime.datetime` | Gabungan tanggal dan waktu |
| `datetime.timedelta` | Selisih/durasi antara dua waktu |

**Method penting:**

```python
from datetime import date, datetime, timedelta

# Tanggal hari ini
hari_ini = date.today()                    # 2026-04-15

# Datetime sekarang
sekarang = datetime.now()                  # 2026-04-15 10:30:00.123456

# Format tanggal ke string
formatted = hari_ini.strftime("%d/%m/%Y")  # "15/04/2026"

# Parse string ke datetime
tgl = datetime.strptime("20/04/2026", "%d/%m/%Y")

# Operasi dengan timedelta
besok = hari_ini + timedelta(days=1)
30_hari_lagi = hari_ini + timedelta(days=30)
selisih = date(2026, 12, 31) - hari_ini   # timedelta object
print(selisih.days)                        # jumlah hari
```

**Format kode `strftime`:**

| Kode | Arti | Contoh |
|---|---|---|
| `%d` | Hari (01–31) | `15` |
| `%m` | Bulan (01–12) | `04` |
| `%Y` | Tahun 4 digit | `2026` |
| `%B` | Nama bulan | `April` |
| `%H` | Jam (00–23) | `10` |
| `%M` | Menit (00–59) | `30` |
| `%S` | Detik (00–59) | `00` |

---

### Membuat Modul Sendiri

Buat file `utils.py`:

```python
# utils.py — modul helper buatan sendiri

def format_rupiah(nominal):
    """Mengubah angka menjadi format Rupiah."""
    return f"Rp {nominal:,.0f}".replace(",", ".")

def hitung_bunga(pokok, rate_per_tahun, bulan):
    """Hitung bunga sederhana."""
    return pokok * (rate_per_tahun / 12 / 100) * bulan

PI_CUSTOM = 3.14159
```

Kemudian di file lain:

```python
# main.py
import utils

print(utils.format_rupiah(1500000))         # Rp 1.500.000
print(utils.hitung_bunga(5000000, 12, 6))   # 300000.0
```

---

## 🧠 Ilustrasi / Analogi

Bayangkan Python seperti **dapur restoran besar**:

| Konsep | Analogi Dapur |
|---|---|
| Modul | Buku resep spesialis |
| `import math` | Ambil buku resep matematika |
| `from math import sqrt` | Ambil satu resep spesifik dari buku |
| `import math as m` | Panggil koki matematika dengan nama panggilan "m" |
| Custom module | Resep rahasia buatan chef sendiri |
| `random` | Mesin undian untuk menentukan menu surprise |
| `datetime` | Jam dinding & kalender dapur |
| `math.ceil` | Bulatkan porsi ke atas (tidak mungkin setengah piring) |

Tanpa buku resep (modul), kamu harus menghapal semua formula matematika, cara generate angka acak, dan cara baca kalender dari memori — tidak efisien dan rawan error.

---

## 💻 Contoh Teknis

### Contoh 1: Modul math untuk kalkulasi keuangan

```python
import math

# Hitung cicilan dengan compound interest
def hitung_cicilan_bulanan(pokok, rate_tahunan, tenor_bulan):
    """
    Formula: M = P * [r(1+r)^n] / [(1+r)^n - 1]
    P = pokok, r = rate bulanan, n = tenor
    """
    r = rate_tahunan / 12 / 100  # rate per bulan dalam desimal
    n = tenor_bulan

    if r == 0:
        return pokok / n

    cicilan = pokok * (r * math.pow(1 + r, n)) / (math.pow(1 + r, n) - 1)
    return math.ceil(cicilan)  # bulatkan ke atas (menguntungkan lender)

pokok = 10_000_000
rate  = 12          # 12% per tahun
tenor = 12          # 12 bulan

cicilan = hitung_cicilan_bulanan(pokok, rate, tenor)
print(f"Cicilan bulanan: Rp {cicilan:,}")
# Output: Cicilan bulanan: Rp 888,489
```

---

### Contoh 2: Modul random untuk OTP

```python
import random
import string

def generate_otp(panjang=6):
    """Generate OTP numerik sejumlah 'panjang' digit."""
    digit = string.digits  # '0123456789'
    otp = ''.join(random.choices(digit, k=panjang))
    return otp

def generate_otp_alphanumeric(panjang=8):
    """Generate OTP alfanumerik (huruf + angka, uppercase)."""
    karakter = string.ascii_uppercase + string.digits
    otp = ''.join(random.choices(karakter, k=panjang))
    return otp

# Test
print("OTP 6 digit:", generate_otp())             # e.g. "483920"
print("OTP 8 karakter:", generate_otp_alphanumeric())  # e.g. "A3K9P2X7"
```

---

### Contoh 3: Modul datetime untuk jatuh tempo

```python
from datetime import date, timedelta

def hitung_jatuh_tempo(tanggal_mulai_str, tenor_bulan):
    """
    Hitung tanggal jatuh tempo cicilan terakhir.
    tanggal_mulai_str: format 'YYYY-MM-DD'
    """
    from datetime import datetime
    tgl_mulai = datetime.strptime(tanggal_mulai_str, "%Y-%m-%d").date()

    # Hitung bulan jatuh tempo dengan pendekatan sederhana
    bulan = tgl_mulai.month + tenor_bulan
    tahun = tgl_mulai.year + (bulan - 1) // 12
    bulan = (bulan - 1) % 12 + 1

    try:
        jatuh_tempo = tgl_mulai.replace(year=tahun, month=bulan)
    except ValueError:
        # Handle kasus tanggal 31 di bulan pendek (e.g. Feb)
        import calendar
        last_day = calendar.monthrange(tahun, bulan)[1]
        jatuh_tempo = tgl_mulai.replace(year=tahun, month=bulan, day=last_day)

    return jatuh_tempo

tgl_mulai = "2026-04-15"
tenor = 12

jatuh_tempo = hitung_jatuh_tempo(tgl_mulai, tenor)
print(f"Pinjaman mulai   : {tgl_mulai}")
print(f"Jatuh tempo akhir: {jatuh_tempo.strftime('%d %B %Y')}")
# Output: Jatuh tempo akhir: 15 April 2027
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Sistem OTP & Jatuh Tempo di Aplikasi Pinjaman Online

**Perusahaan**: StartUp "KreditCepat" — platform pinjaman online (P2P Lending)

**Masalah Bisnis**:
KreditCepat memiliki dua kebutuhan kritis:
1. Setiap kali user melakukan transfer atau mengajukan pinjaman, sistem harus mengirim **OTP 6 digit** yang unik dan tidak mudah ditebak.
2. Setelah pinjaman disetujui, sistem harus **otomatis menghitung semua tanggal jatuh tempo cicilan** dan menyimpannya di database agar bisa mengirim notifikasi H-3 sebelum jatuh tempo.

Tanpa fitur ini, user bisa melakukan transaksi tanpa verifikasi (risiko fraud), dan tim operasional harus hitung jatuh tempo secara manual di spreadsheet (rawan salah dan lambat).

**Dampak Bisnis**:
- Fraud tanpa OTP bisa menyebabkan kerugian Rp miliaran
- Kesalahan hitung jatuh tempo manual menyebabkan keterlambatan notifikasi → user tidak bayar tepat waktu → NPL (Non-Performing Loan) naik

**Solusi Teknis**:

```python
import random
import string
from datetime import date, datetime, timedelta
import math

# ─────────────────────────────────────────────
# MODULE: otp_service.py
# ─────────────────────────────────────────────

def generate_otp(panjang=6, expire_menit=5):
    """
    Generate OTP dan waktu kedaluwarsa.
    Return: (otp_string, waktu_expire)
    """
    digit = string.digits
    otp = ''.join(random.choices(digit, k=panjang))
    expire_time = datetime.now() + timedelta(minutes=expire_menit)
    return otp, expire_time

def verifikasi_otp(otp_input, otp_stored, expire_time):
    """
    Verifikasi OTP yang dimasukkan user.
    Return: (bool, pesan)
    """
    if datetime.now() > expire_time:
        return False, "OTP sudah kedaluwarsa. Minta OTP baru."
    if otp_input != otp_stored:
        return False, "OTP salah. Silakan coba lagi."
    return True, "OTP valid. Transaksi diizinkan."


# ─────────────────────────────────────────────
# MODULE: cicilan_service.py
# ─────────────────────────────────────────────

def generate_jadwal_cicilan(pokok, rate_tahunan, tenor_bulan, tgl_mulai_str):
    """
    Generate jadwal cicilan lengkap untuk pinjaman.
    Return: list of dict berisi detail per cicilan
    """
    tgl_mulai = datetime.strptime(tgl_mulai_str, "%Y-%m-%d").date()
    r = rate_tahunan / 12 / 100
    n = tenor_bulan

    # Hitung cicilan bulanan (formula anuitas)
    if r == 0:
        cicilan_pokok = pokok / n
        cicilan_bunga = 0
    else:
        cicilan_total = pokok * (r * math.pow(1 + r, n)) / (math.pow(1 + r, n) - 1)
        cicilan_total = math.ceil(cicilan_total)

    sisa_pokok = pokok
    jadwal = []

    for i in range(1, tenor_bulan + 1):
        # Hitung tanggal jatuh tempo untuk cicilan ke-i
        bulan_target = tgl_mulai.month + i
        tahun_target = tgl_mulai.year + (bulan_target - 1) // 12
        bulan_target = (bulan_target - 1) % 12 + 1

        try:
            tgl_jatuh_tempo = tgl_mulai.replace(year=tahun_target, month=bulan_target)
        except ValueError:
            import calendar
            last_day = calendar.monthrange(tahun_target, bulan_target)[1]
            tgl_jatuh_tempo = tgl_mulai.replace(
                year=tahun_target, month=bulan_target, day=last_day
            )

        bunga_bulan = math.ceil(sisa_pokok * r)
        pokok_bulan = cicilan_total - bunga_bulan if r != 0 else math.ceil(cicilan_pokok)

        if i == tenor_bulan:
            pokok_bulan = sisa_pokok  # lunasi sisa

        sisa_pokok -= pokok_bulan

        jadwal.append({
            "cicilan_ke"    : i,
            "jatuh_tempo"   : tgl_jatuh_tempo.strftime("%d/%m/%Y"),
            "pokok"         : pokok_bulan,
            "bunga"         : bunga_bulan,
            "total_bayar"   : pokok_bulan + bunga_bulan,
            "sisa_pokok"    : max(sisa_pokok, 0),
        })

    return jadwal


# ─────────────────────────────────────────────
# MAIN: simulasi transaksi
# ─────────────────────────────────────────────

# Step 1: User minta OTP untuk approve pinjaman
otp, expire = generate_otp()
print(f"OTP dikirim ke HP user: {otp}")
print(f"Berlaku hingga: {expire.strftime('%H:%M:%S')}")

# Step 2: Verifikasi OTP (simulasi user input)
otp_input = otp  # asumsikan user input benar
valid, pesan = verifikasi_otp(otp_input, otp, expire)
print(f"\nVerifikasi: {pesan}")

# Step 3: Jika OTP valid, generate jadwal cicilan
if valid:
    jadwal = generate_jadwal_cicilan(
        pokok=10_000_000,
        rate_tahunan=18,     # 18% per tahun
        tenor_bulan=6,
        tgl_mulai_str="2026-04-15"
    )

    print("\n📋 Jadwal Cicilan:")
    print(f"{'No':>3} | {'Jatuh Tempo':>12} | {'Pokok':>12} | {'Bunga':>10} | {'Total':>12}")
    print("-" * 60)
    for c in jadwal:
        print(
            f"{c['cicilan_ke']:>3} | "
            f"{c['jatuh_tempo']:>12} | "
            f"Rp {c['pokok']:>9,} | "
            f"Rp {c['bunga']:>7,} | "
            f"Rp {c['total_bayar']:>9,}"
        )
```

**Output yang dihasilkan:**
```
OTP dikirim ke HP user: 483920
Berlaku hingga: 10:35:00

Verifikasi: OTP valid. Transaksi diizinkan.

📋 Jadwal Cicilan:
 No |  Jatuh Tempo |        Pokok |      Bunga |        Total
------------------------------------------------------------
  1 |   15/05/2026 | Rp 1,572,903 | Rp 150,000 | Rp 1,722,903
  2 |   15/06/2026 | Rp 1,596,497 | Rp 126,406 | Rp 1,722,903
  ...
```

**Hasil**: OTP ter-generate otomatis dan jadwal cicilan 6 bulan langsung tersimpan ke database tanpa ada perhitungan manual.

---

## 📊 Visualisasi

### Alur Penggunaan Modul di Sistem Fintech

```
User request transaksi
        │
        ▼
[import random]
generate_otp(6)
→ "483920", expire: 10:35
        │
        ▼
Kirim OTP via SMS
        │
        ▼
User input OTP
        │
        ▼
verifikasi_otp() ──── GAGAL ──→ Tolak transaksi
        │
      VALID
        │
        ▼
[import datetime]
hitung_jatuh_tempo()
→ List tanggal cicilan
        │
        ▼
[import math]
hitung_cicilan_bulanan()
→ Nominal per bulan
        │
        ▼
Simpan ke database
        │
        ▼
Kirim notifikasi H-3 sebelum jatuh tempo
```

### Perbedaan Import Method

| Cara Import | Sintaks | Akses | Kapan Digunakan |
|---|---|---|---|
| Full import | `import math` | `math.sqrt()` | Sering butuh banyak fungsi dari modul |
| Selective | `from math import sqrt` | `sqrt()` | Hanya butuh 1-2 fungsi |
| Alias | `import datetime as dt` | `dt.date.today()` | Nama modul panjang, sering dipakai |
| Wildcard | `from math import *` | `sqrt()` | **Hindari** di proyek besar |

---

## ⚠️ Kesalahan Umum

**1. Lupa bahwa `math.pow()` return float, bukan int**
```python
import math
x = math.pow(2, 3)
print(x)        # 8.0 bukan 8!
print(type(x))  # <class 'float'>

# Solusi: gunakan ** operator jika butuh int
y = 2 ** 3
print(y)        # 8
print(type(y))  # <class 'int'>
```

**2. Bingung `random.choice()` vs `random.choices()`**
```python
import random
data = [1, 2, 3, 4, 5]

# choice: pilih SATU elemen
print(random.choice(data))      # 3

# choices: pilih BANYAK elemen (boleh duplikat)
print(random.choices(data, k=3))  # [2, 2, 5] — bisa duplikat!

# sample: pilih BANYAK elemen (tanpa duplikat)
print(random.sample(data, k=3))   # [1, 4, 3] — tidak duplikat
```

**3. `datetime.now()` vs `datetime.today()` — hampir sama tapi ada perbedaan**
```python
from datetime import datetime
# Keduanya return waktu lokal, tapi:
# now() bisa menerima parameter timezone: datetime.now(tz=timezone.utc)
# today() tidak bisa
# Rekomendasi: gunakan now() untuk konsistensi
```

**4. Nama file modul sendiri konflik dengan nama modul bawaan**
```python
# JANGAN buat file bernama random.py, math.py, atau datetime.py
# Python akan import file kamu sendiri, bukan modul standar!
# Contoh: jika ada file random.py di folder kamu:
import random  # → import file random.py kamu, BUKAN modul standar!
```

**5. Lupa bahwa `timedelta` tidak bisa langsung tambah bulan**
```python
from datetime import date, timedelta

# SALAH: tidak ada 'months' di timedelta
# tgl + timedelta(months=1)  ← ERROR!

# BENAR: konversi ke hari atau pakai dateutil
tgl + timedelta(days=30)   # pendekatan: 30 hari ≈ 1 bulan
# Atau gunakan: from dateutil.relativedelta import relativedelta
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

Perhatikan kode berikut:
```python
import random
random.seed(42)
print(random.randint(1, 100))
print(random.randint(1, 100))

random.seed(42)
print(random.randint(1, 100))
```

**Pertanyaan:**
- a) Apa fungsi `random.seed(42)`?
- b) Apakah output baris ke-3 sama dengan output baris ke-1? Mengapa?
- c) Dalam konteks apa penggunaan `seed` berguna di dunia nyata?

---

### Soal 2 — Praktik: Kalkulator Bunga Majemuk

Buat program yang menghitung nilai investasi dengan bunga majemuk menggunakan modul `math`.

**Formula**: `A = P * (1 + r/n)^(n*t)`
- `P` = modal awal
- `r` = rate per tahun (desimal)
- `n` = berapa kali bunga dikompon per tahun
- `t` = lama investasi (tahun)

**Ketentuan:**
- Input: P, r (%), n, t dari user
- Output: nilai akhir investasi + keuntungan
- Gunakan `math.pow()` dan format angka ke Rupiah

---

### Soal 3 — Studi Kasus Fintech

**Skenario**: Kamu diminta membuat modul `security_helper.py` untuk sistem perbankan digital.

**Modul harus berisi:**
1. Fungsi `generate_transaction_id()` — buat ID transaksi unik format: `TXN-{tanggal_hari_ini}-{6_digit_random}`
   - Contoh output: `TXN-20260415-483920`
2. Fungsi `is_weekend(tanggal_str)` — return `True` jika tanggal tersebut adalah Sabtu/Minggu (transaksi besar biasanya diblokir saat weekend)
3. Fungsi `hitung_denda(pokok, hari_terlambat, persen_denda_per_hari=0.1)` — hitung denda keterlambatan bayar cicilan

**Kemudian** buat file `main.py` yang mengimport dan menggunakan ketiga fungsi tersebut dengan contoh data nyata.

---

## 📌 Ringkasan

- **Modul** = file `.py` berisi kode yang bisa diimpor dan dipakai ulang
- **`import x`** → akses via `x.fungsi()` | **`from x import f`** → langsung `f()`
- **`as`** → buat alias: `import datetime as dt`
- **`math`**: `sqrt`, `ceil`, `floor`, `pow`, `pi`, `log`, `fabs`
- **`random`**: `randint`, `choice`, `choices`, `sample`, `shuffle`, `seed`
- **`datetime`**: `date.today()`, `datetime.now()`, `timedelta`, `strftime`, `strptime`
- **Custom module**: buat file `.py`, import dengan `import nama_file` (tanpa `.py`)
- **Hindari**: nama file konflik dengan modul standar, `from x import *` di proyek besar
- **`timedelta`** tidak support `months` — hitung hari atau pakai `dateutil.relativedelta`
- **OTP best practice**: selalu set waktu expire, gunakan `random.choices` bukan `randint` untuk digit ganda
