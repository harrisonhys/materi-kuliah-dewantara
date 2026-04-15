# Pertemuan 6: Fungsi (Function) — Parameter, Return, Scope

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Mendefinisikan fungsi menggunakan `def` dengan parameter dan `return` value
- Membedakan antara parameter positional, keyword argument, dan default value
- Memahami scope variabel: kapan variabel bersifat lokal vs global
- Menggunakan fungsi built-in Python yang paling sering dipakai (`len`, `max`, `min`, `abs`, `round`, `sorted`)
- Membuat fungsi rekursif sederhana untuk masalah seperti faktorial dan fibonacci
- Memecah kode kompleks menjadi fungsi-fungsi kecil yang mudah di-maintain (seperti di sistem transfer uang)

---

## 📖 Pengantar (Hook)

Bayangkan kamu bekerja di tim engineering sebuah perusahaan fintech. Ada fitur baru: transfer uang antar rekening. Kamu mulai menulis kodenya, dan tanpa sadar kodenya sudah 300 baris panjang — semua tercampur: validasi saldo, perhitungan biaya admin, formatting notifikasi, logging transaksi, semuanya dalam satu blok besar.

Tiga minggu kemudian, ada bug di perhitungan biaya admin. Untuk mencarinya, kamu harus membaca 300 baris kode yang saling tumpang tindih. Mimpi buruk, kan?

Solusinya: **fungsi**. Pisahkan setiap tanggung jawab menjadi fungsi kecil yang punya satu tugas jelas. `validasi_saldo()`, `hitung_biaya_admin()`, `format_notifikasi()` — masing-masing independen, mudah diuji, mudah diperbaiki.

Konsep ini dinamakan **modularisasi** — dan fungsi adalah alat utamanya. Ini bukan hanya teknis, tapi filosofi: *tulis kode seolah-olah orang lain yang harus memeliharanya, dan orang itu adalah kamu 6 bulan dari sekarang*.

---

## 🧩 Konsep Utama

### 1. Mendefinisikan Fungsi dengan `def`

```python
def nama_fungsi(parameter1, parameter2):
    # isi fungsi
    return hasil
```

- `def` — kata kunci untuk mendefinisikan fungsi
- `nama_fungsi` — nama yang kamu pilih, gunakan huruf kecil dan underscore (snake_case)
- `parameter` — input yang diterima fungsi (boleh tidak ada)
- `return` — nilai yang dikembalikan fungsi ke pemanggil (boleh tidak ada, default `None`)

### 2. Parameter dan Argumen

- **Parameter** = variabel di definisi fungsi (saat membuat fungsi)
- **Argumen** = nilai aktual yang dikirim saat memanggil fungsi

**Tiga jenis parameter:**

**a. Positional Parameter** — urutan sangat penting:
```python
def transfer(pengirim, penerima, nominal):
    pass

transfer("Budi", "Sari", 500000)  # urutan harus sama
```

**b. Keyword Argument** — urutan tidak penting, disebutkan nama parameternya:
```python
transfer(nominal=500000, penerima="Sari", pengirim="Budi")  # urutan bebas
```

**c. Default Value** — nilai default jika argumen tidak diberikan:
```python
def hitung_biaya_admin(nominal, rate=0.005):  # rate default 0.5%
    return nominal * rate

hitung_biaya_admin(1000000)         # rate pakai default: 5000
hitung_biaya_admin(1000000, 0.01)   # rate di-override: 10000
```

### 3. Return Value

- Fungsi bisa mengembalikan **satu nilai**, **beberapa nilai** (sebagai tuple), atau **tidak ada** (`None`)
- Setelah `return`, kode di bawahnya tidak dieksekusi

```python
# Return satu nilai
def kuadrat(n):
    return n * n

# Return beberapa nilai
def statistik(data):
    return min(data), max(data), sum(data) / len(data)

minimum, maksimum, rata = statistik([70, 85, 90, 60])
```

### 4. Scope: Lokal vs Global

**Scope** menentukan di mana variabel "terlihat" (bisa diakses).

- **Lokal**: variabel yang dibuat di dalam fungsi — hanya bisa diakses di dalam fungsi itu
- **Global**: variabel yang dibuat di luar fungsi — bisa diakses di mana saja

```python
saldo_global = 1000000  # variabel global

def cek_saldo():
    pesan = "Saldo kamu:"  # variabel lokal
    print(f"{pesan} Rp{saldo_global:,}")  # bisa baca global
    
cek_saldo()
print(pesan)  # ❌ ERROR: pesan tidak dikenal di luar fungsi
```

> **Aturan penting:** Fungsi bisa **membaca** variabel global, tapi tidak bisa **mengubahnya** tanpa kata kunci `global` (dan ini sebaiknya dihindari dalam praktik nyata).

### 5. Built-in Functions Python

Python sudah menyediakan banyak fungsi siap pakai:

| Fungsi | Kegunaan | Contoh |
|--------|---------|--------|
| `len(x)` | Panjang list/string | `len([1,2,3])` → `3` |
| `max(x)` | Nilai terbesar | `max([5, 2, 8])` → `8` |
| `min(x)` | Nilai terkecil | `min([5, 2, 8])` → `2` |
| `sum(x)` | Total penjumlahan | `sum([1, 2, 3])` → `6` |
| `abs(x)` | Nilai absolut | `abs(-500)` → `500` |
| `round(x, n)` | Pembulatan | `round(3.14159, 2)` → `3.14` |
| `sorted(x)` | Mengurutkan | `sorted([3,1,2])` → `[1,2,3]` |
| `type(x)` | Tipe data | `type(42)` → `<class 'int'>` |
| `int(x)` | Konversi ke integer | `int("42")` → `42` |
| `str(x)` | Konversi ke string | `str(42)` → `"42"` |

### 6. Rekursi — Fungsi yang Memanggil Dirinya Sendiri

Rekursi adalah teknik di mana sebuah fungsi memanggil dirinya sendiri untuk memecahkan masalah yang lebih kecil dari masalah asal.

Setiap fungsi rekursif wajib punya:
- **Base case** — kondisi berhenti (tanpa ini: stack overflow!)
- **Recursive case** — pemanggilan diri sendiri dengan input yang mengecil

---

## 🧠 Ilustrasi / Analogi

### Analogi: Fungsi seperti Mesin/Alat Khusus

| Konsep | Analogi |
|--------|---------|
| Fungsi | Mesin kopi — kamu masukkan biji kopi, keluar kopi jadi |
| Parameter | Bahan masukan (biji kopi, air, suhu) |
| Return | Hasil keluaran (secangkir kopi) |
| Scope lokal | Bahan di dalam mesin — kamu tidak bisa ambil dari luar |
| Scope global | Stopkontak listrik — semua mesin di dapur bisa pakai |
| Default parameter | Suhu default 90°C — bisa diganti, tapi kalau tidak disebutkan, pakai default |

### Analogi Scope: Ruangan dan Lemari

Bayangkan program adalah sebuah rumah:
- **Variabel global** = barang di ruang tamu (semua orang bisa lihat dan pakai)
- **Variabel lokal** = barang di dalam kamar tidur (hanya pemilik kamar yang bisa akses)
- Fungsi = kamar tidur yang punya "pintu" (parameter = pintu masuk, return = pintu keluar)

### Visualisasi Rekursi: Faktorial

`faktorial(4)` = 4 × 3 × 2 × 1 = 24

```
faktorial(4)
  └── 4 × faktorial(3)
           └── 3 × faktorial(2)
                    └── 2 × faktorial(1)
                             └── 1  ← BASE CASE, berhenti di sini
```

Hasilnya "dikembalikan ke atas":
```
faktorial(1) = 1
faktorial(2) = 2 × 1 = 2
faktorial(3) = 3 × 2 = 6
faktorial(4) = 4 × 6 = 24
```

---

## 💻 Contoh Teknis

### Contoh 1: Fungsi Dasar

```python
def sapa_nasabah(nama):
    """Menampilkan pesan sambutan untuk nasabah."""
    pesan = f"Selamat datang, {nama}! Selamat menggunakan layanan kami."
    return pesan

# Memanggil fungsi
sambutan = sapa_nasabah("Budi")
print(sambutan)
# Output: Selamat datang, Budi! Selamat menggunakan layanan kami.

# Bisa langsung dipakai tanpa simpan ke variabel
print(sapa_nasabah("Sari"))
```

### Contoh 2: Parameter, Default Value, dan Keyword Argument

```python
def hitung_bunga(pokok, rate_per_tahun=0.05, tahun=1):
    """
    Hitung bunga tabungan sederhana.
    
    Args:
        pokok: Modal awal
        rate_per_tahun: Suku bunga per tahun (default 5%)
        tahun: Jangka waktu dalam tahun (default 1 tahun)
    
    Returns:
        Total uang setelah bunga
    """
    bunga = pokok * rate_per_tahun * tahun
    total = pokok + bunga
    return total

# Berbagai cara memanggil
print(hitung_bunga(10000000))                          # pakai semua default
print(hitung_bunga(10000000, 0.07))                   # rate di-override
print(hitung_bunga(10000000, tahun=3))                # skip rate, override tahun
print(hitung_bunga(pokok=5000000, tahun=2, rate_per_tahun=0.06))  # semua keyword
```

**Output:**
```
10500000.0
10700000.0
11500000.0
5600000.0
```

### Contoh 3: Return Beberapa Nilai

```python
def analisis_transaksi(daftar_nominal):
    """Mengembalikan statistik dari daftar transaksi."""
    if not daftar_nominal:
        return None, None, None, None
    
    total = sum(daftar_nominal)
    terbesar = max(daftar_nominal)
    terkecil = min(daftar_nominal)
    rata_rata = round(total / len(daftar_nominal), 2)
    
    return total, terbesar, terkecil, rata_rata

# Unpack return values
transaksi_hari_ini = [500000, 1200000, 300000, 750000, 2000000]
total, maks, mini, avg = analisis_transaksi(transaksi_hari_ini)

print(f"Total transaksi : Rp{total:,}")
print(f"Tertinggi       : Rp{maks:,}")
print(f"Terendah        : Rp{mini:,}")
print(f"Rata-rata       : Rp{avg:,}")
```

### Contoh 4: Scope Lokal vs Global

```python
saldo = 2000000  # variabel global

def tarik_tunai(nominal):
    # Baca global saldo
    print(f"Saldo sebelum: Rp{saldo:,}")
    
    sisa = saldo - nominal  # 'sisa' adalah variabel LOKAL
    print(f"Saldo setelah: Rp{sisa:,}")
    
    return sisa

saldo_baru = tarik_tunai(500000)
# print(sisa)  # ❌ ERROR - 'sisa' tidak ada di scope global

# Cara yang benar: update saldo via return value
saldo = tarik_tunai(500000)
print(f"Saldo terkini: Rp{saldo:,}")
```

### Contoh 5: Built-in Functions

```python
nilai_ujian = [75, 88, 62, 95, 71, 83, 90, 55, 78, 85]

print(f"Jumlah mahasiswa : {len(nilai_ujian)}")
print(f"Nilai tertinggi  : {max(nilai_ujian)}")
print(f"Nilai terendah   : {min(nilai_ujian)}")
print(f"Total nilai      : {sum(nilai_ujian)}")
print(f"Rata-rata        : {round(sum(nilai_ujian) / len(nilai_ujian), 2)}")
print(f"Urutan naik      : {sorted(nilai_ujian)}")
print(f"Urutan turun     : {sorted(nilai_ujian, reverse=True)}")

# abs() berguna untuk selisih
selisih = abs(nilai_ujian[0] - nilai_ujian[1])
print(f"Selisih nilai[0] dan nilai[1]: {selisih}")
```

### Contoh 6: Rekursi — Faktorial

```python
def faktorial(n):
    """
    Hitung n! secara rekursif.
    Contoh: faktorial(5) = 5 × 4 × 3 × 2 × 1 = 120
    """
    # Base case
    if n == 0 or n == 1:
        return 1
    
    # Recursive case
    return n * faktorial(n - 1)

# Test
for i in range(8):
    print(f"{i}! = {faktorial(i)}")
```

**Output:**
```
0! = 1
1! = 1
2! = 2
3! = 6
4! = 24
5! = 120
6! = 720
7! = 5040
```

### Contoh 7: Rekursi — Fibonacci

```python
def fibonacci(n):
    """
    Hitung bilangan Fibonacci ke-n.
    Urutan: 0, 1, 1, 2, 3, 5, 8, 13, 21, ...
    """
    # Base cases
    if n == 0:
        return 0
    if n == 1:
        return 1
    
    # Recursive case
    return fibonacci(n - 1) + fibonacci(n - 2)

# Cetak 10 bilangan Fibonacci pertama
print("Deret Fibonacci:")
for i in range(10):
    print(fibonacci(i), end=" ")
# Output: 0 1 1 2 3 5 8 13 21 34
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Modularisasi Sistem Transfer Uang

**Masalah bisnis:**

Tim engineering OVO sedang membangun fitur transfer uang. Versi pertama kodenya ditulis "monolitik" — semua logika dalam satu blok besar. Ini menyebabkan masalah:
- Sulit di-debug ketika ada bug perhitungan biaya
- Tidak bisa di-reuse: validasi saldo untuk fitur lain harus ditulis ulang
- Susah di-test: tidak bisa test validasi tanpa menjalankan seluruh proses transfer

**Solusi:** Pecah menjadi fungsi-fungsi kecil yang masing-masing punya satu tanggung jawab.

```python
# ============================================================
# MODUL TRANSFER UANG - OVO Payment System
# Setiap fungsi punya satu tanggung jawab (Single Responsibility)
# ============================================================

def validasi_saldo(saldo, nominal):
    """
    Validasi apakah saldo mencukupi untuk transaksi.
    
    Args:
        saldo: Saldo pengirim saat ini
        nominal: Nominal yang ingin ditransfer
    
    Returns:
        (bool, str): (valid, pesan_error)
    """
    if nominal <= 0:
        return False, "Nominal transfer harus lebih dari 0"
    
    if nominal > saldo:
        kekurangan = nominal - saldo
        return False, f"Saldo tidak cukup. Kekurangan: Rp{kekurangan:,}"
    
    saldo_minimum = 10000  # saldo minimum setelah transaksi
    if saldo - nominal < saldo_minimum:
        return False, f"Saldo setelah transfer tidak boleh kurang dari Rp{saldo_minimum:,}"
    
    return True, "Saldo mencukupi"


def hitung_biaya_admin(nominal, tipe_transfer="sesama"):
    """
    Hitung biaya admin berdasarkan nominal dan tipe transfer.
    
    Args:
        nominal: Nominal transfer
        tipe_transfer: "sesama" (sesama OVO) atau "beda_bank"
    
    Returns:
        int: biaya admin dalam rupiah
    """
    if tipe_transfer == "sesama":
        return 0  # gratis antar pengguna OVO
    elif tipe_transfer == "beda_bank":
        if nominal >= 1000000:
            biaya = 2500  # flat fee untuk nominal besar
        else:
            biaya = 5000  # flat fee untuk nominal kecil
        return biaya
    else:
        return 0


def format_notifikasi(pengirim, penerima, nominal, biaya, status):
    """
    Format pesan notifikasi transaksi.
    
    Args:
        pengirim: Nama pengirim
        penerima: Nama penerima  
        nominal: Nominal transfer
        biaya: Biaya admin
        status: "sukses" atau "gagal"
    
    Returns:
        str: Pesan notifikasi yang sudah diformat
    """
    if status == "sukses":
        total_dipotong = nominal + biaya
        pesan = (
            f"[OVO] Transfer Berhasil!\n"
            f"Dari        : {pengirim}\n"
            f"Ke          : {penerima}\n"
            f"Nominal     : Rp{nominal:,}\n"
            f"Biaya Admin : Rp{biaya:,}\n"
            f"Total Potong: Rp{total_dipotong:,}\n"
            f"Status      : SUKSES"
        )
    else:
        pesan = (
            f"[OVO] Transfer Gagal\n"
            f"Dari        : {pengirim}\n"
            f"Ke          : {penerima}\n"
            f"Nominal     : Rp{nominal:,}\n"
            f"Status      : GAGAL"
        )
    
    return pesan


def catat_log(pengirim, nominal, status, alasan=""):
    """
    Catat log transaksi untuk audit trail.
    
    Args:
        pengirim: ID atau nama pengirim
        nominal: Nominal transaksi
        status: Status transaksi
        alasan: Alasan jika gagal
    """
    log_entry = f"[LOG] User={pengirim} | Nominal=Rp{nominal:,} | Status={status}"
    if alasan:
        log_entry += f" | Alasan={alasan}"
    print(log_entry)


def proses_transfer(pengirim, saldo_pengirim, penerima, nominal, tipe="sesama"):
    """
    Fungsi utama: Orkestrasi seluruh proses transfer.
    Memanggil fungsi-fungsi lain secara berurutan.
    
    Returns:
        (bool, int): (berhasil, saldo_akhir)
    """
    print(f"\n{'='*50}")
    print(f"MEMULAI TRANSFER: {pengirim} → {penerima}")
    print(f"{'='*50}")
    
    # Langkah 1: Hitung biaya dulu
    biaya = hitung_biaya_admin(nominal, tipe)
    total = nominal + biaya
    
    print(f"Nominal transfer : Rp{nominal:,}")
    print(f"Biaya admin      : Rp{biaya:,}")
    print(f"Total pemotongan : Rp{total:,}")
    print()
    
    # Langkah 2: Validasi saldo
    valid, pesan_validasi = validasi_saldo(saldo_pengirim, total)
    
    if not valid:
        print(f"[DITOLAK] {pesan_validasi}")
        notif = format_notifikasi(pengirim, penerima, nominal, biaya, "gagal")
        print("\n" + notif)
        catat_log(pengirim, nominal, "GAGAL", pesan_validasi)
        return False, saldo_pengirim
    
    # Langkah 3: Eksekusi transfer (simulasi)
    saldo_akhir = saldo_pengirim - total
    print(f"[SUKSES] Transfer diproses!")
    print(f"Saldo sebelum: Rp{saldo_pengirim:,}")
    print(f"Saldo setelah: Rp{saldo_akhir:,}")
    
    # Langkah 4: Kirim notifikasi
    notif = format_notifikasi(pengirim, penerima, nominal, biaya, "sukses")
    print("\n" + notif)
    
    # Langkah 5: Catat log
    catat_log(pengirim, nominal, "SUKSES")
    
    return True, saldo_akhir


# ========================
# MAIN: Simulasi Penggunaan
# ========================

saldo_budi = 2000000

# Transfer 1: Berhasil (sesama OVO)
sukses, saldo_budi = proses_transfer(
    pengirim="Budi",
    saldo_pengirim=saldo_budi,
    penerima="Sari",
    nominal=500000,
    tipe="sesama"
)

# Transfer 2: Gagal (saldo tidak cukup)
sukses, saldo_budi = proses_transfer(
    pengirim="Budi",
    saldo_pengirim=saldo_budi,
    penerima="Ahmad",
    nominal=1800000,
    tipe="beda_bank"
)

# Transfer 3: Berhasil (beda bank, ada biaya admin)
sukses, saldo_budi = proses_transfer(
    pengirim="Budi",
    saldo_pengirim=saldo_budi,
    penerima="Dewi",
    nominal=300000,
    tipe="beda_bank"
)

print(f"\n{'='*50}")
print(f"SALDO AKHIR BUDI: Rp{saldo_budi:,}")
```

**Kenapa modularisasi ini lebih baik?**

| Aspek | Kode Monolitik | Kode Modular dengan Fungsi |
|-------|---------------|---------------------------|
| Debug biaya admin | Harus baca 100+ baris | Langsung ke `hitung_biaya_admin()` |
| Reuse validasi | Copy-paste di setiap fitur | Panggil `validasi_saldo()` dari mana saja |
| Testing | Harus jalankan seluruh flow | Test setiap fungsi secara independen |
| Kolaborasi tim | Konflik saat edit file yang sama | Setiap developer bisa kerjakan fungsi berbeda |
| Keterbacaan | Sulit dipahami dalam 6 bulan | `proses_transfer()` jelas seperti resep |

---

## 📊 Visualisasi

### Alur Eksekusi Fungsi

```
PEMANGGIL (main program)
        │
        │  proses_transfer("Budi", 2000000, "Sari", 500000)
        ▼
┌─────────────────────────────────┐
│         proses_transfer()        │
│                                 │
│  ┌─────────────────────────┐    │
│  │  hitung_biaya_admin()   │ ◄──┤── langkah 1
│  │  return: 0              │    │
│  └─────────────────────────┘    │
│                                 │
│  ┌─────────────────────────┐    │
│  │   validasi_saldo()      │ ◄──┤── langkah 2
│  │   return: (True, "OK")  │    │
│  └─────────────────────────┘    │
│                                 │
│  ┌─────────────────────────┐    │
│  │  format_notifikasi()    │ ◄──┤── langkah 4
│  │  return: string         │    │
│  └─────────────────────────┘    │
│                                 │
│  ┌─────────────────────────┐    │
│  │      catat_log()        │ ◄──┤── langkah 5
│  └─────────────────────────┘    │
│                                 │
│  return (True, 1500000)         │
└─────────────────────────────────┘
        │
        ▼
PEMANGGIL menerima hasil
```

### Scope Variable Diagram

```python
saldo = 2000000          # ← GLOBAL (terlihat di mana saja)
RATE_ADMIN = 0.005       # ← GLOBAL (konstanta)

def hitung_biaya(nominal):
    biaya = nominal * RATE_ADMIN   # ← biaya LOKAL (hanya di sini)
    return biaya
    # setelah return, 'biaya' hilang

def proses():
    hasil = hitung_biaya(1000000)  # ← hasil LOKAL (hanya di sini)
    print(saldo)                   # ← bisa baca global
    
# Di luar fungsi:
# print(biaya)  ← ERROR, tidak ada
# print(hasil)  ← ERROR, tidak ada
```

---

## ⚠️ Kesalahan Umum

### 1. Lupa `return` — Fungsi Selalu Mengembalikan `None`

```python
# ❌ SALAH
def tambah(a, b):
    hasil = a + b
    # lupa return!

x = tambah(3, 5)
print(x)  # Output: None (bukan 8!)

# ✅ BENAR
def tambah(a, b):
    return a + b
```

### 2. Mengubah Variabel Global Tanpa `global` Keyword

```python
saldo = 1000000

# ❌ SALAH - membuat variabel lokal baru, bukan mengubah global
def kurangi_saldo(nominal):
    saldo = saldo - nominal  # ← ini sebenarnya membuat 'saldo' lokal
    # Python akan error: "local variable 'saldo' referenced before assignment"

# ✅ SOLUSI TERBAIK - pakai return value, jangan ubah global
def kurangi_saldo(saldo_saat_ini, nominal):
    return saldo_saat_ini - nominal

saldo = kurangi_saldo(saldo, 200000)  # update via assignment
```

### 3. Parameter Default yang Mutable (Jebakan Berbahaya!)

```python
# ❌ SALAH - list sebagai default value berbahaya!
def tambah_ke_riwayat(transaksi, riwayat=[]):  # jangan lakukan ini!
    riwayat.append(transaksi)
    return riwayat

print(tambah_ke_riwayat(100000))  # [100000]
print(tambah_ke_riwayat(200000))  # [100000, 200000] ← menggunakan list yang sama!

# ✅ BENAR - gunakan None sebagai default
def tambah_ke_riwayat(transaksi, riwayat=None):
    if riwayat is None:
        riwayat = []
    riwayat.append(transaksi)
    return riwayat
```

### 4. Rekursi Tanpa Base Case → Stack Overflow

```python
# ❌ SALAH - tidak ada base case, akan crash!
def countdown(n):
    print(n)
    countdown(n - 1)  # tidak pernah berhenti!

# ✅ BENAR
def countdown(n):
    if n <= 0:      # BASE CASE
        print("Selesai!")
        return
    print(n)
    countdown(n - 1)
```

### 5. Salah Urutan Argumen Positional

```python
def transfer(pengirim, penerima, nominal):
    print(f"{pengirim} mengirim Rp{nominal:,} ke {penerima}")

# ❌ SALAH - urutan tidak sesuai
transfer("Sari", 500000, "Budi")  # nominal jadi penerima!

# ✅ BENAR
transfer("Budi", "Sari", 500000)
# Atau pakai keyword argument agar jelas:
transfer(pengirim="Budi", penerima="Sari", nominal=500000)
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep Dasar

**a)** Apa perbedaan antara **parameter** dan **argumen**? Berikan contoh!

**b)** Apa yang dikembalikan fungsi ini?
```python
def cek_ganjil(n):
    if n % 2 != 0:
        return True
    
x = cek_ganjil(4)
print(x)
```

**c)** Jelaskan perbedaan variabel lokal dan global. Mengapa sebaiknya menghindari penggunaan `global` terlalu sering?

---

### Soal 2 — Coding Mandiri

**a) Fungsi Konversi Mata Uang**

Buat fungsi `konversi_rupiah(nominal, ke="USD")` yang:
- Mengkonversi rupiah ke mata uang lain
- Mendukung: `"USD"` (kurs: 1 USD = 16.000 IDR), `"EUR"` (1 EUR = 17.500 IDR), `"SGD"` (1 SGD = 12.000 IDR)
- Return hasil konversi dibulatkan 2 desimal
- Jika mata uang tidak dikenali, return `None` dan print pesan error

**b) Fungsi Rekursif: Pangkat**

Buat fungsi `pangkat(basis, eksponen)` menggunakan rekursi (tanpa menggunakan `**` operator).
- `pangkat(2, 10)` harus menghasilkan `1024`
- Tangani kasus `eksponen == 0` sebagai base case

---

### Soal 3 — Studi Kasus Fintech

**Skenario: Sistem Poin Reward GoPay**

Buat sistem poin reward dengan spesifikasi berikut menggunakan beberapa fungsi terpisah:

1. **`hitung_poin(nominal, tipe_merchant)`**
   - Tipe `"grocery"`: 1 poin per Rp1.000
   - Tipe `"food"`: 2 poin per Rp1.000
   - Tipe `"online"`: 3 poin per Rp1.000
   - Default: 1 poin per Rp1.000

2. **`konversi_poin_ke_rupiah(poin, rate=50)`**
   - Default: 1 poin = Rp50

3. **`cek_level_nasabah(total_poin)`**
   - 0–999 poin: `"Silver"`
   - 1000–4999 poin: `"Gold"`
   - 5000+: `"Platinum"`

4. **`ringkasan_reward(nama, poin_sebelum, transaksi_list)`**
   - `transaksi_list` berisi list of tuple: `[(nominal, tipe_merchant), ...]`
   - Hitung total poin baru dari semua transaksi
   - Print ringkasan: nama, level, total poin, nilai dalam rupiah

**Contoh pemanggilan:**
```python
transaksi = [
    (150000, "food"),
    (500000, "online"),
    (200000, "grocery"),
]
ringkasan_reward("Budi", 800, transaksi)
```

---

## 📌 Ringkasan

- **`def nama(param): return nilai`** — template dasar membuat fungsi
- **Positional** → urutan penting; **Keyword** → nama parameter disebutkan; **Default** → nilai dipakai jika tidak ada argumen
- **`return`** wajib ada jika fungsi harus mengeluarkan nilai; tanpa return, fungsi mengembalikan `None`
- **Scope Lokal** = hidup hanya di dalam fungsi; **Scope Global** = hidup sepanjang program
- Hindari mengubah variabel global di dalam fungsi — lebih baik pakai parameter dan return
- **Built-in functions**: `len`, `max`, `min`, `sum`, `abs`, `round`, `sorted` — hafal dan gunakan!
- **Rekursi** butuh dua hal wajib: base case (kondisi berhenti) dan recursive case (panggil diri sendiri dengan input lebih kecil)
- **Modularisasi** = satu fungsi satu tanggung jawab — kode lebih mudah debug, test, dan maintain
- Nama fungsi yang baik: gunakan kata kerja — `hitung_biaya()`, `validasi_saldo()`, `format_notifikasi()`
- **Docstring** (`"""..."""` setelah `def`) adalah dokumentasi fungsi — biasakan menulis ini!

> **Prinsip terpenting:** Kalau kamu menulis kode yang sama lebih dari 2 kali — itu sinyal bahwa kamu butuh fungsi!
