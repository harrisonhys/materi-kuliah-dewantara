# Pertemuan 12: Exception Handling (try, except, finally)

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Mengenali dan membedakan jenis **exception umum** di Python: `TypeError`, `ValueError`, `ZeroDivisionError`, `FileNotFoundError`, `IndexError`, `KeyError`
- Menggunakan blok **`try / except / else / finally`** untuk menangani error dengan tepat
- Menangkap **multiple exception** sekaligus dalam satu blok atau terpisah
- Menggunakan **`raise`** untuk melempar exception secara sengaja
- Membuat **Custom Exception** sendiri dengan membuat class turunan dari `Exception`
- Menerapkan **defensive programming** — menulis kode yang tidak mudah crash
- Menyimpan **error log** ke file agar masalah bisa ditelusuri (debug) di kemudian hari

---

## 📖 Pengantar (Hook)

Jam 2 pagi. Sistem transfer antarbank sedang ramai karena banyak nasabah yang bayar tagihan menjelang jatuh tempo. Tiba-tiba, seorang nasabah memasukkan nominal transfer: **"satu juta"** (bukan angka, tapi teks).

Program tidak punya exception handling. Hasilnya? **Traceback error Python muncul langsung di layar nasabah.**

```
ValueError: invalid literal for int() with base 10: 'satu juta'
  File "transfer.py", line 47, in proses_transfer
    nominal = int(input_nominal)
```

Nasabah panik. Mereka tidak tahu apakah uangnya sudah terpotong atau belum. Mereka menelepon call center. Call center kebanjiran telepon. Reputasi perusahaan rusak.

Ini bukan kejadian fiksi — ini skenario nyata yang terjadi di banyak sistem keuangan yang dibangun tanpa exception handling yang proper.

Inilah mengapa **exception handling** adalah skill yang membedakan programmer junior dan programmer profesional.

---

## 🧩 Konsep Utama

### 1. Apa Itu Exception?

**Exception** adalah kondisi tidak normal yang terjadi saat program berjalan. Ini berbeda dari **syntax error** (kesalahan penulisan kode) — exception baru muncul saat program dieksekusi, bukan saat ditulis.

Ketika exception tidak ditangani, Python menghentikan program dan menampilkan **traceback** — pesan error panjang yang membingungkan pengguna awam.

### 2. Exception Umum di Python

| Exception | Penyebab | Contoh |
|-----------|----------|--------|
| `ValueError` | Nilai tidak valid untuk operasi | `int("abc")` |
| `TypeError` | Tipe data salah | `"5" + 5` |
| `ZeroDivisionError` | Pembagian dengan nol | `10 / 0` |
| `FileNotFoundError` | File tidak ditemukan | `open("tidak_ada.txt")` |
| `IndexError` | Indeks list di luar batas | `[1,2,3][10]` |
| `KeyError` | Key tidak ada di dictionary | `{"a":1}["b"]` |
| `AttributeError` | Atribut tidak ada di objek | `None.split()` |
| `NameError` | Variabel belum didefinisikan | `print(belum_ada)` |

### 3. Struktur try / except / else / finally

```
try:
    # Kode yang mungkin error
except TipeError:
    # Dijalankan JIKA ada error
else:
    # Dijalankan JIKA TIDAK ada error (opsional)
finally:
    # SELALU dijalankan, error atau tidak (opsional)
```

Alur eksekusi:
- **try** → Python mencoba menjalankan kode di sini
- **except** → Kalau ada error yang cocok, kode di sini dijalankan
- **else** → Kalau try berhasil tanpa error, kode di sini dijalankan
- **finally** → Selalu dijalankan apapun yang terjadi (cocok untuk cleanup seperti menutup file/koneksi)

### 4. `raise` — Melempar Exception Sendiri

Kadang kamu ingin membuat error sendiri ketika kondisi bisnis tidak terpenuhi. Misalnya: saldo tidak cukup, nominal negatif, atau format nomor rekening salah. Gunakan `raise` untuk ini.

### 5. Custom Exception

Python memungkinkan kamu membuat tipe exception sendiri dengan membuat class yang mewarisi `Exception`. Ini berguna untuk membedakan error domain bisnis (seperti `SaldoTidakCukupError`) dari error teknis generik.

### 6. Defensive Programming

Prinsip menulis kode yang mengantisipasi kemungkinan terburuk:
- Validasi input sebelum diproses
- Jangan asumsikan data selalu bersih dan valid
- Selalu tangani kasus edge (nilai 0, string kosong, None)

### 7. Logging Error ke File

Daripada hanya `print()` error ke console (yang hilang begitu program ditutup), kita bisa menyimpan error ke file log. Ini penting untuk sistem produksi karena kamu bisa audit masalah yang terjadi di masa lalu.

---

## 🧠 Ilustrasi / Analogi

Bayangkan kamu adalah kasir di bank:

| Situasi | Analogi Dunia Nyata | Exception Handling |
|---------|--------------------|--------------------|
| Nasabah datang minta tarik tunai | `try` — kita proses dulu | Menjalankan kode yang mungkin gagal |
| Saldo tidak cukup | `except` — tolak dengan sopan | Tangkap error, beri respons yang tepat |
| Proses berhasil | `else` — cetak struk | Jalankan kode sukses |
| Apapun terjadi, tutup laci kasir | `finally` — selalu tutup laci | Cleanup: tutup file/koneksi database |

Dan bayangkan error tanpa handling seperti kasir yang langsung berteriak "SALDO TIDAK CUKUP! KODE ERROR 0x4F5A!" — bukan "Maaf Bapak/Ibu, saldo Anda tidak mencukupi untuk transaksi ini. Saldo tersedia: Rp50.000."

**Pelanggan butuh pesan yang manusiawi, bukan traceback Python.**

---

## 💻 Contoh Teknis

### Exception Dasar

```python
# ========== TANPA EXCEPTION HANDLING ==========
# Langsung crash jika input bukan angka
nominal = int(input("Masukkan nominal transfer: "))  # Error jika user ketik "abc"
print(f"Transfer Rp{nominal:,} berhasil!")

# ========== DENGAN EXCEPTION HANDLING ==========
try:
    nominal_input = input("Masukkan nominal transfer: ")
    nominal = int(nominal_input)
    print(f"Transfer Rp{nominal:,} berhasil!")
except ValueError:
    print("❌ Nominal tidak valid. Harap masukkan angka saja.")
```

### Menangkap Multiple Exception

```python
def ambil_data_transaksi(data_list, index):
    """Mengambil transaksi berdasarkan indeks"""
    try:
        transaksi = data_list[index]
        nominal = int(transaksi["nominal"])
        hasil = 1_000_000 / nominal  # bisa ZeroDivisionError
        return hasil
    except IndexError:
        print(f"❌ Indeks {index} tidak ditemukan dalam data.")
    except KeyError as e:
        print(f"❌ Key {e} tidak ada dalam data transaksi.")
    except ValueError:
        print("❌ Format nominal tidak valid — bukan angka.")
    except ZeroDivisionError:
        print("❌ Nominal tidak boleh nol.")
    return None

# Bisa juga tangkap multiple dalam satu except
try:
    nilai = int("abc")
except (ValueError, TypeError) as e:
    print(f"❌ Error konversi: {e}")
```

### try / except / else / finally

```python
def baca_file_transaksi(nama_file):
    """Membaca file CSV transaksi dengan penanganan error lengkap"""
    f = None
    try:
        f = open(nama_file, "r", encoding="utf-8")
        isi = f.read()
        data = isi.split("\n")
    except FileNotFoundError:
        print(f"❌ File '{nama_file}' tidak ditemukan.")
        return None
    except PermissionError:
        print(f"❌ Tidak punya izin membaca '{nama_file}'.")
        return None
    else:
        # Hanya dijalankan jika tidak ada exception
        print(f"✅ File '{nama_file}' berhasil dibaca ({len(data)} baris).")
        return data
    finally:
        # Selalu dijalankan — pastikan file tertutup
        if f:
            f.close()
            print("🔒 File ditutup.")

# Test
hasil = baca_file_transaksi("transaksi.csv")
hasil_gagal = baca_file_transaksi("tidak_ada.csv")
```

### raise — Melempar Exception

```python
def validasi_nominal(nominal):
    """Validasi nominal transfer sesuai aturan bisnis"""
    if not isinstance(nominal, (int, float)):
        raise TypeError(f"Nominal harus berupa angka, bukan {type(nominal).__name__}")
    if nominal <= 0:
        raise ValueError("Nominal transfer harus lebih dari 0")
    if nominal < 10_000:
        raise ValueError("Nominal transfer minimum adalah Rp10.000")
    if nominal > 25_000_000:
        raise ValueError("Nominal transfer maksimum per transaksi adalah Rp25.000.000")
    return True

# Penggunaan
try:
    validasi_nominal(-50_000)
except ValueError as e:
    print(f"❌ Validasi gagal: {e}")
```

### Custom Exception

```python
# ========== DEFINISI CUSTOM EXCEPTION ==========
class TransferError(Exception):
    """Base exception untuk semua error transfer"""
    pass

class SaldoTidakCukupError(TransferError):
    """Saldo rekening tidak mencukupi untuk transaksi"""
    def __init__(self, saldo_tersedia, nominal_dibutuhkan):
        self.saldo_tersedia = saldo_tersedia
        self.nominal_dibutuhkan = nominal_dibutuhkan
        super().__init__(
            f"Saldo tidak cukup. "
            f"Tersedia: Rp{saldo_tersedia:,}, "
            f"Dibutuhkan: Rp{nominal_dibutuhkan:,}"
        )

class RekeningTidakAktifError(TransferError):
    """Rekening tujuan tidak aktif atau tidak ditemukan"""
    def __init__(self, nomor_rekening):
        self.nomor_rekening = nomor_rekening
        super().__init__(f"Rekening {nomor_rekening} tidak aktif atau tidak ditemukan")

class LimitTransferError(TransferError):
    """Transfer melebihi limit harian"""
    def __init__(self, limit_harian, sudah_digunakan, nominal_baru):
        sisa_limit = limit_harian - sudah_digunakan
        super().__init__(
            f"Melebihi limit transfer harian. "
            f"Sisa limit: Rp{sisa_limit:,}"
        )

# ========== PENGGUNAAN ==========
def proses_transfer(saldo, rekening_tujuan, nominal, limit_harian, sudah_digunakan):
    # Cek rekening tujuan
    rekening_valid = {"1234567890", "0987654321", "1122334455"}
    if rekening_tujuan not in rekening_valid:
        raise RekeningTidakAktifError(rekening_tujuan)

    # Cek limit harian
    if sudah_digunakan + nominal > limit_harian:
        raise LimitTransferError(limit_harian, sudah_digunakan, nominal)

    # Cek saldo
    if saldo < nominal:
        raise SaldoTidakCukupError(saldo, nominal)

    return saldo - nominal

# Contoh penggunaan dengan error handling
try:
    saldo_baru = proses_transfer(
        saldo=500_000,
        rekening_tujuan="1234567890",
        nominal=750_000,
        limit_harian=10_000_000,
        sudah_digunakan=0
    )
    print(f"✅ Transfer berhasil! Saldo baru: Rp{saldo_baru:,}")

except SaldoTidakCukupError as e:
    print(f"❌ {e}")

except RekeningTidakAktifError as e:
    print(f"❌ {e}")

except LimitTransferError as e:
    print(f"❌ {e}")

except TransferError as e:
    # Tangkap semua error transfer yang tidak terduga
    print(f"❌ Error transfer: {e}")
```

### Logging Error ke File

```python
import datetime

def log_error(pesan_error, level="ERROR", nama_file="app_error.log"):
    """Menulis error ke file log dengan timestamp"""
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    with open(nama_file, "a", encoding="utf-8") as f:
        f.write(f"[{timestamp}] [{level}] {pesan_error}\n")

def proses_input_transfer_nasabah(input_nominal, input_rekening, saldo_nasabah):
    """
    Memproses input transfer dari nasabah dengan full exception handling
    dan logging ke file
    """
    try:
        # Validasi input nominal
        nominal = int(input_nominal.replace(".", "").replace(",", ""))

        if nominal <= 0:
            raise ValueError("Nominal harus lebih dari 0")

        # Validasi format rekening (10 digit angka)
        if not input_rekening.isdigit() or len(input_rekening) != 10:
            raise ValueError("Nomor rekening harus 10 digit angka")

        # Validasi saldo
        if saldo_nasabah < nominal:
            raise SaldoTidakCukupError(saldo_nasabah, nominal)

        # Semua valid — proses transfer
        log_error(
            f"Transfer berhasil: Rp{nominal:,} ke {input_rekening}",
            level="INFO"
        )
        return {
            "status": "SUCCESS",
            "pesan": f"Transfer Rp{nominal:,} ke rekening {input_rekening} berhasil!",
            "nominal": nominal
        }

    except ValueError as e:
        pesan = f"Input tidak valid: {e}"
        log_error(f"ValueError - {pesan} | Input: nominal='{input_nominal}', rek='{input_rekening}'")
        return {
            "status": "FAILED",
            "pesan": f"❌ {pesan}. Silakan periksa kembali input Anda.",
            "nominal": 0
        }

    except SaldoTidakCukupError as e:
        log_error(f"SaldoTidakCukup - {e}")
        return {
            "status": "FAILED",
            "pesan": f"❌ Saldo tidak mencukupi. Saldo Anda: Rp{saldo_nasabah:,}",
            "nominal": 0
        }

    except Exception as e:
        # Tangkap error tak terduga — jangan expose detail ke user!
        log_error(f"UnexpectedError - {type(e).__name__}: {e}")
        return {
            "status": "ERROR",
            "pesan": "❌ Terjadi kesalahan sistem. Silakan coba lagi atau hubungi CS.",
            "nominal": 0
        }

# Test berbagai skenario
skenario = [
    ("500000", "1234567890", 1_000_000),   # Normal
    ("satu juta", "1234567890", 1_000_000), # Input non-numerik
    ("0", "1234567890", 1_000_000),          # Nominal nol
    ("2000000", "1234567890", 1_000_000),   # Saldo kurang
    ("500000", "123", 1_000_000),           # Rekening tidak valid
]

for nominal_input, rek, saldo in skenario:
    hasil = proses_input_transfer_nasabah(nominal_input, rek, saldo)
    print(f"Input: '{nominal_input}' → {hasil['status']}: {hasil['pesan']}")
    print()
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Sistem Transfer yang Crash karena Input Nasabah

**Masalah:**

Aplikasi mobile banking "QuickPay" mengalami incident pada hari pertama promo cashback. Volume transaksi naik 10x lipat, dan banyak nasabah mencoba berbagai input:

1. Mengetik nominal `"1.000.000"` dengan titik sebagai pemisah ribuan (bukan karakter angka murni)
2. Copy-paste rekening tujuan yang terselip spasi `"1234 567890"`
3. Transfer dengan saldo 0 karena baru saja bayar tagihan lain
4. Koneksi internet putus di tengah transaksi

Sistem tidak punya exception handling → **program crash**, transaksi tidak jelas statusnya, nasabah panik.

**Dampak Bisnis:**
- 2.300 nasabah mengalami error dalam 1 jam
- Call center kewalahan: 800 tiket masuk dalam 30 menit
- Beberapa nasabah posting screenshot error Python di media sosial
- Tim engineering harus lembur malam untuk rollback

**Solusi Teknis: Sistem Transfer yang Robust**

```python
import datetime
import json

# ========== CUSTOM EXCEPTIONS ==========
class PaymentError(Exception):
    """Base exception untuk semua error payment"""
    def __init__(self, pesan_user, kode_error, detail_teknis=""):
        self.pesan_user = pesan_user      # Pesan untuk ditampilkan ke nasabah
        self.kode_error = kode_error      # Kode error untuk logging
        self.detail_teknis = detail_teknis
        super().__init__(pesan_user)

class InputTidakValidError(PaymentError):
    def __init__(self, field, detail=""):
        super().__init__(
            pesan_user=f"Format {field} tidak valid. Silakan periksa kembali.",
            kode_error="ERR_INVALID_INPUT",
            detail_teknis=detail
        )

class SaldoTidakCukupError(PaymentError):
    def __init__(self, saldo, nominal):
        super().__init__(
            pesan_user=f"Saldo tidak mencukupi. Saldo tersedia: Rp{saldo:,}",
            kode_error="ERR_INSUFFICIENT_BALANCE",
            detail_teknis=f"saldo={saldo}, nominal={nominal}"
        )

class TransaksiTidakAmanError(PaymentError):
    def __init__(self, alasan):
        super().__init__(
            pesan_user="Transaksi tidak dapat diproses karena alasan keamanan. "
                       "Hubungi CS kami di 1500-XXX.",
            kode_error="ERR_SECURITY",
            detail_teknis=alasan
        )

# ========== SISTEM LOGGING ==========
def log_transaksi(level, kode_error, user_id, detail, nama_file="payment.log"):
    timestamp = datetime.datetime.now().isoformat()
    log_entry = {
        "timestamp": timestamp,
        "level": level,
        "kode_error": kode_error,
        "user_id": user_id,
        "detail": detail
    }
    with open(nama_file, "a", encoding="utf-8") as f:
        f.write(json.dumps(log_entry, ensure_ascii=False) + "\n")

# ========== VALIDASI INPUT ==========
def parse_nominal(input_str):
    """
    Parse nominal dari berbagai format input user:
    - "1.000.000" → 1000000
    - "1,000,000" → 1000000
    - "1000000"   → 1000000
    Raise InputTidakValidError jika tidak bisa di-parse
    """
    try:
        # Hapus semua karakter non-digit kecuali koma dan titik
        bersih = input_str.strip().replace("Rp", "").replace(" ", "")
        # Hapus titik/koma sebagai pemisah ribuan
        bersih = bersih.replace(".", "").replace(",", "")
        nominal = int(bersih)
        if nominal <= 0:
            raise ValueError("Nominal negatif atau nol")
        return nominal
    except (ValueError, AttributeError) as e:
        raise InputTidakValidError("nominal", detail=str(e))

def parse_rekening(input_str):
    """Validasi dan bersihkan nomor rekening"""
    try:
        bersih = input_str.strip().replace(" ", "").replace("-", "")
        if not bersih.isdigit():
            raise ValueError("Bukan digit")
        if len(bersih) not in [10, 12, 16]:  # Berbagai panjang rekening
            raise ValueError(f"Panjang {len(bersih)} tidak valid")
        return bersih
    except AttributeError as e:
        raise InputTidakValidError("nomor rekening", detail=str(e))
    except ValueError as e:
        raise InputTidakValidError("nomor rekening", detail=str(e))

# ========== PROSES TRANSFER UTAMA ==========
def proses_transfer_v2(user_id, input_nominal, input_rekening, saldo, limit_harian=10_000_000):
    """
    Sistem transfer yang robust dengan full exception handling
    """
    nominal = None
    rekening = None

    try:
        # Step 1: Parse dan validasi input
        nominal = parse_nominal(input_nominal)
        rekening = parse_rekening(input_rekening)

        # Step 2: Validasi aturan bisnis
        if nominal < 10_000:
            raise InputTidakValidError("nominal", "Minimum transfer Rp10.000")

        if nominal > limit_harian:
            raise InputTidakValidError(
                "nominal",
                f"Melebihi limit harian Rp{limit_harian:,}"
            )

        # Simulasi cek saldo (di sistem nyata ini query ke database)
        if saldo < nominal:
            raise SaldoTidakCukupError(saldo, nominal)

        # Simulasi pengecekan rekening tujuan
        rekening_aktif = {"1234567890", "0987654321", "1122334455"}
        if rekening not in rekening_aktif:
            raise TransaksiTidakAmanError(f"Rekening {rekening} tidak ditemukan di sistem")

        # Step 3: Eksekusi transfer (simulasi)
        saldo_baru = saldo - nominal

        # Step 4: Catat ke log sukses
        log_transaksi(
            level="INFO",
            kode_error="SUCCESS",
            user_id=user_id,
            detail=f"Transfer Rp{nominal:,} ke {rekening} berhasil. Saldo baru: Rp{saldo_baru:,}"
        )

        return {
            "status": "SUCCESS",
            "pesan": f"Transfer Rp{nominal:,} ke rekening {rekening} berhasil diproses!",
            "saldo_baru": saldo_baru,
            "referensi": f"TRX-{datetime.datetime.now().strftime('%Y%m%d%H%M%S')}-{user_id}"
        }

    except PaymentError as e:
        # Error bisnis yang sudah diketahui — log dan return pesan user-friendly
        log_transaksi(
            level="WARNING",
            kode_error=e.kode_error,
            user_id=user_id,
            detail=f"nominal_input='{input_nominal}', rek='{input_rekening}' | {e.detail_teknis}"
        )
        return {
            "status": "FAILED",
            "pesan": e.pesan_user,
            "saldo_baru": saldo,  # Saldo tidak berubah
            "referensi": None
        }

    except Exception as e:
        # Error tidak terduga — jangan expose ke user, log saja
        log_transaksi(
            level="ERROR",
            kode_error="ERR_UNEXPECTED",
            user_id=user_id,
            detail=f"{type(e).__name__}: {e}"
        )
        return {
            "status": "ERROR",
            "pesan": "Sistem sedang mengalami gangguan. Transaksi tidak diproses. "
                     "Silakan coba lagi dalam beberapa menit.",
            "saldo_baru": saldo,
            "referensi": None
        }

# ========== SIMULASI BERBAGAI SKENARIO NASABAH ==========
print("=" * 60)
print("SIMULASI SISTEM TRANSFER QUICKPAY")
print("=" * 60)

skenario_nasabah = [
    # (user_id, input_nominal, input_rekening, saldo)
    ("USR001", "1.000.000", "1234567890", 5_000_000),  # Normal, titik pemisah ribuan
    ("USR002", "satu juta", "1234567890", 5_000_000),  # Input teks
    ("USR003", "500000", "1234 5678 90", 5_000_000),   # Rekening dengan spasi
    ("USR004", "3000000", "1234567890", 2_000_000),     # Saldo kurang
    ("USR005", "500000", "9999999999", 5_000_000),      # Rekening tidak ditemukan
    ("USR006", "5000", "1234567890", 5_000_000),        # Nominal di bawah minimum
]

for user_id, nominal_input, rek_input, saldo in skenario_nasabah:
    print(f"\n[{user_id}] Transfer '{nominal_input}' ke '{rek_input}' (saldo: Rp{saldo:,})")
    hasil = proses_transfer_v2(user_id, nominal_input, rek_input, saldo)
    print(f"  Status  : {hasil['status']}")
    print(f"  Pesan   : {hasil['pesan']}")
    if hasil['referensi']:
        print(f"  Referensi: {hasil['referensi']}")
```

**Hasilnya:** Nasabah mendapat pesan yang jelas dan manusiawi, bukan traceback Python. Tim engineering bisa menganalisis log untuk mengetahui pola error yang sering terjadi dan memperbaiki UX aplikasi.

---

## 📊 Visualisasi

### Alur Eksekusi try / except / else / finally

```
        ┌─────────────────────┐
        │       try block      │
        └──────────┬──────────┘
                   │
            Error terjadi?
           ┌───────┴───────┐
         Tidak            Ya
           │               │
           ▼               ▼
    ┌──────────┐    ┌──────────────┐
    │   else   │    │    except    │
    │  (sukses)│    │  (tangkap)   │
    └────┬─────┘    └──────┬───────┘
         │                 │
         └────────┬────────┘
                  │
                  ▼
          ┌──────────────┐
          │   finally    │ ← Selalu dijalankan
          │  (cleanup)   │
          └──────────────┘
```

### Hierarki Exception Python (yang Relevan)

```
BaseException
└── Exception
    ├── ValueError          ← Input tidak sesuai tipe yang valid
    ├── TypeError           ← Tipe data salah
    ├── ArithmeticError
    │   └── ZeroDivisionError ← Bagi dengan nol
    ├── LookupError
    │   ├── IndexError      ← Index list out of range
    │   └── KeyError        ← Key dict tidak ada
    ├── OSError
    │   └── FileNotFoundError ← File tidak ada
    └── [Custom Exception kamu]
        └── PaymentError
            ├── SaldoTidakCukupError
            ├── InputTidakValidError
            └── TransaksiTidakAmanError
```

### Kapan Pakai `raise` vs Return Error?

| Situasi | Rekomendasi |
|---------|-------------|
| Fungsi kamu menemukan kondisi yang tidak bisa dilanjutkan | Gunakan `raise` |
| Kamu di layer UI/controller yang interaksi dengan user | Tangkap exception, return respons |
| Library/modul yang digunakan oleh banyak kode lain | Selalu `raise` — biarkan caller yang tangkap |
| Validasi input yang dipanggil dari banyak tempat | `raise` custom exception |

---

## ⚠️ Kesalahan Umum

**1. Menangkap semua exception secara buta (`except Exception` atau `except:`)**

```python
# ❌ SALAH — menyembunyikan semua error, termasuk bug nyata
try:
    proses_transfer()
except:
    print("Ada error")  # Kamu tidak tahu errornya apa!

# ✅ BENAR — tangkap exception yang spesifik
try:
    proses_transfer()
except ValueError as e:
    print(f"Input tidak valid: {e}")
except SaldoTidakCukupError as e:
    print(f"Saldo kurang: {e}")
```

**2. Exception handling digunakan sebagai control flow normal**

```python
# ❌ BURUK — menggunakan exception untuk logika biasa
try:
    index = data.index("nilai")
except ValueError:
    index = -1

# ✅ LEBIH BAIK — gunakan conditional untuk kasus yang diharapkan
if "nilai" in data:
    index = data.index("nilai")
else:
    index = -1
```

**3. Lupa `raise` di dalam `except` ketika hanya mau log saja**

```python
# ❌ SALAH — error "ditelan", kode berlanjut seolah tidak ada masalah
try:
    koneksi_database()
except ConnectionError as e:
    log_error(e)
    # ERROR: tidak ada raise! kode lanjut terus meskipun DB tidak terkoneksi

# ✅ BENAR
try:
    koneksi_database()
except ConnectionError as e:
    log_error(e)
    raise  # lempar ulang exception yang sama
```

**4. Menulis pesan error teknis langsung ke user**

```python
# ❌ SALAH — user tidak mengerti ini
except KeyError as e:
    return f"KeyError: {e}"

# ✅ BENAR — pesan yang manusiawi
except KeyError as e:
    log_error(f"KeyError: {e}")  # detail untuk developer
    return "Data tidak ditemukan. Silakan coba lagi."  # untuk user
```

**5. Menggunakan `finally` untuk hal yang seharusnya di `else`**

```python
# ❌ MEMBINGUNGKAN — finally juga jalan saat ada error
try:
    hasil = proses()
finally:
    print(f"Proses berhasil: {hasil}")  # ERROR jika ada exception!

# ✅ BENAR — gunakan else untuk hal yang hanya jalan jika sukses
try:
    hasil = proses()
except Exception as e:
    print(f"Error: {e}")
else:
    print(f"Proses berhasil: {hasil}")
finally:
    print("Cleanup selesai")  # ini untuk cleanup, bukan logika bisnis
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep: Identifikasi Exception

Untuk setiap kode berikut, tentukan: exception apa yang akan terjadi, dan tulis kode `try/except` yang tepat untuk menanganinya.

```python
# Kasus A
data = [10, 20, 30]
print(data[5])

# Kasus B
profil = {"nama": "Budi", "email": "budi@email.com"}
print(profil["nomor_hp"])

# Kasus C
nilai = "abc"
hasil = int(nilai) / 0

# Kasus D
with open("file_yang_tidak_ada.txt") as f:
    isi = f.read()
```

### Soal 2 — Studi Kasus: Kalkulator Cicilan KPR

Kamu diminta membangun fungsi kalkulator cicilan KPR sederhana untuk aplikasi fintech properti.

**Formula cicilan bulanan:**
```
M = P * [r(1+r)^n] / [(1+r)^n - 1]
M = cicilan bulanan
P = pokok pinjaman
r = bunga per bulan (bunga_tahunan / 12 / 100)
n = jumlah bulan
```

Buat fungsi `hitung_cicilan_kpr(pokok, bunga_tahunan, tahun)` yang:

1. Memvalidasi semua input (pokok minimal Rp100 juta, bunga antara 1%-30%, tenor antara 1-30 tahun)
2. Membuat Custom Exception `KPRError` dengan subclass `PokokTidakValidError`, `BungaTidakValidError`, `TenorTidakValidError`
3. Menangani `ZeroDivisionError` jika bunga = 0 (cicilan flat = pokok / bulan)
4. Mencatat setiap perhitungan ke file `kpr_history.log`
5. Mengembalikan dict berisi: `cicilan_bulanan`, `total_bayar`, `total_bunga`, `pesan_sukses`

**Uji fungsimu dengan skenario:**
- Pokok Rp500 juta, bunga 10%, 20 tahun → Harusnya sukses
- Pokok Rp50 juta (di bawah minimum) → PokokTidakValidError
- Bunga 0% → Harus ditangani sebagai kasus khusus
- Tenor 40 tahun → TenorTidakValidError

---

## 📌 Ringkasan

**Struktur Dasar:**
```python
try:
    # kode yang mungkin error
except TipeError as e:
    # tangkap error spesifik
except (Error1, Error2) as e:
    # tangkap multiple exception
else:
    # jalankan jika try BERHASIL
finally:
    # selalu jalankan (cleanup)
```

**Exception Umum — Cheatsheet:**

| Exception | Kapan Muncul |
|-----------|-------------|
| `ValueError` | `int("abc")`, nilai di luar range |
| `TypeError` | Operasi pada tipe yang salah |
| `ZeroDivisionError` | `x / 0` |
| `FileNotFoundError` | `open("tidak_ada.txt")` |
| `IndexError` | `list[100]` saat list cuma 3 elemen |
| `KeyError` | `dict["key_yang_tidak_ada"]` |

**Custom Exception — Template:**
```python
class NamaError(Exception):
    def __init__(self, pesan_user, detail_teknis=""):
        self.pesan_user = pesan_user
        self.detail_teknis = detail_teknis
        super().__init__(pesan_user)
```

**Prinsip Penting:**
- Tangkap exception **spesifik**, bukan `except:` buta
- Pesan untuk **user**: manusiawi, tidak teknis
- Pesan untuk **log**: detail, termasuk input dan konteks
- `finally` → untuk **cleanup** (tutup file, tutup koneksi)
- `else` → untuk **kode sukses**, bukan `finally`
- `raise` tanpa argumen di dalam `except` → melempar ulang exception yang sama
- Jangan gunakan exception sebagai **control flow normal** — itu untuk kondisi luar biasa
