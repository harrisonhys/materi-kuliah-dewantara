# Pertemuan 5: Struktur Perulangan (for, while, break, continue)

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Menggunakan `for` loop dengan `range()` untuk mengulang sesuatu sebanyak N kali
- Menggunakan `while` loop ketika jumlah pengulangan tidak diketahui dari awal
- Menghentikan atau melewati iterasi menggunakan `break`, `continue`, dan `pass`
- Membuat nested loop (loop di dalam loop) untuk pola atau tabel data
- Menerapkan perulangan dalam skenario nyata seperti retry logic, batch processing, dan pengiriman notifikasi massal

---

## 📖 Pengantar (Hook)

Bayangkan kamu punya tugas mengirim SMS notifikasi ke 10.000 nasabah setelah dana mereka berhasil dikreditkan. Kamu tidak mungkin mengetik dan mengirim satu per satu secara manual — itu akan butuh berminggu-minggu!

Atau bayangkan sistem payment gateway yang mencoba mengirim transaksi ke bank tujuan. Kadang jaringan sedang tidak stabil, jadi sistemnya perlu "mencoba lagi" sampai 3 kali sebelum menyerah dan menandai transaksi sebagai gagal.

Kedua situasi itu punya satu kesamaan: **pengulangan**. Dan di sinilah loop masuk sebagai senjata utama programmer. Dengan beberapa baris kode loop, kamu bisa memproses jutaan data, mengirim ribuan notifikasi, atau mencoba ulang koneksi — semua secara otomatis.

Loop adalah salah satu konsep terpenting dalam pemrograman. Hampir tidak ada program nyata yang bisa berjalan tanpa loop.

---

## 🧩 Konsep Utama

### 1. `for` Loop — Iterasi dengan Jumlah yang Sudah Diketahui

`for` loop digunakan ketika kamu **tahu berapa kali** ingin mengulang sesuatu, atau ketika kamu ingin memproses setiap elemen dalam sebuah koleksi (list, string, dll.).

**Struktur dasar:**
```python
for variabel in sekuens:
    # kode yang diulang
```

**Dengan `range()`:**
- `range(5)` → menghasilkan angka 0, 1, 2, 3, 4
- `range(1, 6)` → menghasilkan angka 1, 2, 3, 4, 5
- `range(0, 10, 2)` → menghasilkan angka 0, 2, 4, 6, 8 (dengan step 2)
- `range(10, 0, -1)` → mundur dari 10 ke 1

### 2. `while` Loop — Iterasi Berdasarkan Kondisi

`while` loop digunakan ketika kamu **tidak tahu pasti berapa kali** perulangan akan terjadi, tapi kamu tahu **kapan harus berhenti** (berdasarkan kondisi tertentu).

**Struktur dasar:**
```python
while kondisi:
    # kode yang diulang
    # pastikan kondisi suatu saat menjadi False!
```

> ⚠️ Hati-hati: jika kondisi tidak pernah menjadi `False`, kamu akan membuat **infinite loop** — program tidak akan pernah berhenti!

### 3. `break` — Keluar dari Loop

`break` digunakan untuk **langsung keluar** dari loop, meskipun kondisinya masih `True` atau iterasinya belum selesai.

Contoh: Cari transaksi mencurigakan — begitu ketemu satu, langsung berhenti cari.

### 4. `continue` — Lewati Iterasi Ini, Lanjut ke Berikutnya

`continue` digunakan untuk **melewati sisa kode** di iterasi saat ini dan langsung lanjut ke iterasi berikutnya.

Contoh: Proses daftar transaksi, tapi lewati transaksi yang statusnya "pending".

### 5. `pass` — Placeholder Kosong

`pass` adalah perintah yang **tidak melakukan apa-apa**. Biasanya digunakan sebagai placeholder saat kamu belum menulis implementasi tapi ingin kode tetap valid secara sintaks.

### 6. Nested Loop — Loop di Dalam Loop

Loop bisa diletakkan di dalam loop lain. Ini berguna untuk memproses data dua dimensi (tabel, matriks, pola bintang, dll.).

```python
for baris in range(3):
    for kolom in range(3):
        print(f"({baris}, {kolom})", end=" ")
    print()  # pindah baris
```

---

## 🧠 Ilustrasi / Analogi

### Analogi: Loop seperti Ban Berjalan di Pabrik

| Konsep | Analogi Dunia Nyata |
|--------|---------------------|
| `for` loop | Ban berjalan yang memproses tepat 100 paket lalu berhenti |
| `while` loop | Pintu otomatis yang terus terbuka selama ada orang yang terdeteksi |
| `break` | Tombol darurat yang menghentikan seluruh ban berjalan seketika |
| `continue` | Sensor yang melewati paket rusak — paket dibuang, ban tetap jalan |
| `pass` | Tombol dummy yang belum dipasang kabelnya |
| Nested loop | Mesin sortir yang memproses setiap rak (loop luar) dan setiap laci di rak itu (loop dalam) |

### Analogi: Retry di Kehidupan Nyata

Bayangkan kamu menghubungi customer service bank via telepon:
1. Pertama kali: "Semua agen sedang sibuk" → coba lagi
2. Kedua kali: "Semua agen sedang sibuk" → coba lagi
3. Ketiga kali: Tersambung! → selesai

Ini adalah **while loop dengan break**: terus coba sampai berhasil atau batas percobaan tercapai.

---

## 💻 Contoh Teknis

### Contoh 1: `for` Loop dengan `range()`

```python
# Cetak tabel perkalian 5
print("Tabel Perkalian 5:")
for i in range(1, 11):
    hasil = 5 * i
    print(f"5 x {i} = {hasil}")
```

**Output:**
```
Tabel Perkalian 5:
5 x 1 = 5
5 x 2 = 10
...
5 x 10 = 50
```

### Contoh 2: `for` Loop pada List

```python
daftar_rekening = ["BCA-1234", "Mandiri-5678", "BNI-9012", "BRI-3456"]

print("Memproses rekening:")
for rekening in daftar_rekening:
    print(f"  -> Memverifikasi {rekening}...")
print("Semua rekening selesai diverifikasi.")
```

### Contoh 3: `while` Loop

```python
# Simulasi input PIN ATM (maksimal 3 percobaan)
PIN_BENAR = "1234"
percobaan = 0
maksimal = 3

while percobaan < maksimal:
    pin = input("Masukkan PIN: ")
    percobaan += 1
    
    if pin == PIN_BENAR:
        print("PIN benar! Selamat datang.")
        break
    else:
        sisa = maksimal - percobaan
        if sisa > 0:
            print(f"PIN salah. Sisa percobaan: {sisa}")
        else:
            print("Kartu diblokir karena 3x salah PIN.")
```

### Contoh 4: `break` dan `continue`

```python
transaksi = [500000, -100000, 250000, -50000, 1000000]

print("Memproses transaksi kredit:")
for nominal in transaksi:
    if nominal < 0:
        print(f"  [SKIP] Transaksi debit {nominal} dilewati")
        continue  # lewati transaksi negatif
    
    if nominal > 900000:
        print(f"  [ALERT] Transaksi {nominal} melebihi limit, proses dihentikan!")
        break  # hentikan proses
    
    print(f"  [OK] Memproses transaksi kredit: Rp{nominal:,}")
```

**Output:**
```
Memproses transaksi kredit:
  [OK] Memproses transaksi kredit: Rp500,000
  [SKIP] Transaksi debit -100000 dilewati
  [OK] Memproses transaksi kredit: Rp250,000
  [SKIP] Transaksi debit -50000 dilewati
  [ALERT] Transaksi 1000000 melebihi limit, proses dihentikan!
```

### Contoh 5: FizzBuzz (Studi Kasus Klasik)

```python
# FizzBuzz: cetak angka 1-30
# Jika habis dibagi 3 -> "Fizz"
# Jika habis dibagi 5 -> "Buzz"
# Jika habis dibagi keduanya -> "FizzBuzz"

for i in range(1, 31):
    if i % 15 == 0:
        print("FizzBuzz")
    elif i % 3 == 0:
        print("Fizz")
    elif i % 5 == 0:
        print("Buzz")
    else:
        print(i)
```

### Contoh 6: Bilangan Prima

```python
def adalah_prima(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

# Cetak semua bilangan prima dari 1 sampai 50
print("Bilangan prima antara 1-50:")
prima = []
for angka in range(1, 51):
    if adalah_prima(angka):
        prima.append(angka)

print(prima)
# Output: [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]
```

### Contoh 7: Pola Bintang (Nested Loop)

```python
# Pola segitiga bintang
n = 5
for baris in range(1, n + 1):
    for kolom in range(baris):
        print("*", end="")
    print()  # newline
```

**Output:**
```
*
**
***
****
*****
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Retry Logic pada Payment Gateway

**Masalah bisnis:**
Sistem pembayaran GoPay sedang memproses transfer dana ke rekening tujuan. Kadang server bank tujuan timeout atau sedang maintenance. Jika sistem langsung menyerah saat pertama gagal, transaksi akan ditandai sebagai "gagal" — padahal sebenarnya hanya masalah jaringan sementara. Ini akan menyebabkan keluhan nasabah dan reputasi buruk.

**Dampak bisnis:**
- Nasabah komplain karena transfer "gagal" padahal saldo sudah terpotong
- Tim CS kewalahan menangani tiket refund
- Reputasi platform menurun

**Solusi teknis:** Implementasi retry logic — coba kirim transaksi maksimal 3 kali sebelum benar-benar menyerah.

```python
import random
import time

def kirim_ke_bank_tujuan(nominal, rekening_tujuan):
    """
    Simulasi pengiriman ke bank tujuan.
    Kadang gagal karena timeout (simulasi dengan random).
    """
    # Simulasi: 60% kemungkinan berhasil
    if random.random() > 0.4:
        return True  # berhasil
    else:
        return False  # gagal (timeout/error)

def proses_transfer_dengan_retry(nominal, rekening_tujuan, maks_percobaan=3):
    """
    Proses transfer dengan retry logic.
    Akan mencoba hingga maks_percobaan kali.
    """
    percobaan = 0
    berhasil = False
    
    print(f"Memulai transfer Rp{nominal:,} ke {rekening_tujuan}")
    
    while percobaan < maks_percobaan:
        percobaan += 1
        print(f"  [Percobaan {percobaan}/{maks_percobaan}] Mengirim transaksi...")
        
        hasil = kirim_ke_bank_tujuan(nominal, rekening_tujuan)
        
        if hasil:
            berhasil = True
            print(f"  [SUCCESS] Transfer berhasil pada percobaan ke-{percobaan}!")
            break
        else:
            print(f"  [FAILED] Gagal, server tidak merespons.")
            if percobaan < maks_percobaan:
                print(f"  Menunggu 2 detik sebelum mencoba lagi...")
                # time.sleep(2)  # di produksi: uncomment ini
    
    if not berhasil:
        print(f"  [ABORT] Transfer GAGAL setelah {maks_percobaan} percobaan.")
        print(f"  Transaksi akan masuk antrian refund otomatis.")
    
    return berhasil

# Simulasi batch transfer
daftar_transfer = [
    (500000, "BCA-1234567"),
    (1500000, "Mandiri-7654321"),
    (250000, "BNI-1122334"),
]

print("=" * 50)
print("SISTEM TRANSFER BATCH - GoPay Payment Engine")
print("=" * 50)

berhasil_count = 0
gagal_count = 0

for nominal, rekening in daftar_transfer:
    print()
    sukses = proses_transfer_dengan_retry(nominal, rekening)
    if sukses:
        berhasil_count += 1
    else:
        gagal_count += 1
    print("-" * 40)

print(f"\nRingkasan:")
print(f"  Berhasil : {berhasil_count} transaksi")
print(f"  Gagal    : {gagal_count} transaksi")
```

**Contoh output:**
```
==================================================
SISTEM TRANSFER BATCH - GoPay Payment Engine
==================================================

Memulai transfer Rp500,000 ke BCA-1234567
  [Percobaan 1/3] Mengirim transaksi...
  [FAILED] Gagal, server tidak merespons.
  Menunggu 2 detik sebelum mencoba lagi...
  [Percobaan 2/3] Mengirim transaksi...
  [SUCCESS] Transfer berhasil pada percobaan ke-2!
----------------------------------------

Memulai transfer Rp1,500,000 ke Mandiri-7654321
  [Percobaan 1/3] Mengirim transaksi...
  [SUCCESS] Transfer berhasil pada percobaan ke-1!
----------------------------------------
```

### Skenario 2: Pengiriman Notifikasi Massal

```python
# Simulasi pengiriman notifikasi ke ribuan nasabah
nasabah_aktif = [
    {"id": "U001", "nama": "Budi", "email": "budi@email.com", "notif": True},
    {"id": "U002", "nama": "Sari", "email": "sari@email.com", "notif": False},
    {"id": "U003", "nama": "Ahmad", "email": "ahmad@email.com", "notif": True},
    {"id": "U004", "nama": "Dewi", "email": "dewi@email.com", "notif": True},
]

def kirim_notifikasi(email, pesan):
    print(f"  Email terkirim ke {email}: '{pesan}'")
    return True

terkirim = 0
dilewati = 0

print("Mengirim notifikasi promo akhir tahun...\n")

for nasabah in nasabah_aktif:
    # Lewati nasabah yang nonaktifkan notifikasi
    if not nasabah["notif"]:
        print(f"[SKIP] {nasabah['nama']} - notifikasi dinonaktifkan")
        dilewati += 1
        continue
    
    pesan = f"Halo {nasabah['nama']}! Dapatkan cashback 10% untuk transaksi hari ini."
    kirim_notifikasi(nasabah["email"], pesan)
    terkirim += 1

print(f"\nHasil: {terkirim} terkirim, {dilewati} dilewati.")
```

---

## 📊 Visualisasi

### Flowchart: Cara Kerja `for` Loop

```
MULAI
  │
  ▼
Ambil elemen pertama dari sekuens
  │
  ▼
┌─────────────────────────────┐
│ Masih ada elemen berikutnya? │
└─────────────────────────────┘
  │ YA                │ TIDAK
  ▼                   ▼
Jalankan          SELESAI
kode di dalam
loop
  │
  ▼
Ambil elemen berikutnya
  │
  └──────────────────┘ (kembali ke pengecekan)
```

### Flowchart: Cara Kerja `while` Loop dengan `break`

```
MULAI
  │
  ▼
percobaan = 0
  │
  ▼
┌──────────────────────────┐
│ percobaan < maks? (kondisi)│
└──────────────────────────┘
  │ YA                 │ TIDAK
  ▼                    ▼
Coba kirim          GAGAL TOTAL
transaksi           (keluar loop)
  │
  ├── BERHASIL? ──► break ──► SUKSES (keluar loop)
  │
  └── GAGAL ──► percobaan += 1 ──► (kembali ke kondisi)
```

### Tabel Perbandingan `for` vs `while`

| Aspek | `for` Loop | `while` Loop |
|-------|-----------|--------------|
| Jumlah iterasi | Sudah diketahui | Tidak diketahui |
| Kapan digunakan | Iterasi list, range tertentu | Kondisi dinamis |
| Risiko infinite loop | Sangat kecil | Ada jika kondisi tidak berubah |
| Contoh use case | Proses 100 transaksi | Tunggu sampai koneksi berhasil |
| Kontrol variabel | Otomatis | Manual (harus update sendiri) |

---

## ⚠️ Kesalahan Umum

### 1. Lupa Update Variabel di `while` Loop → Infinite Loop

```python
# ❌ SALAH - ini akan berjalan selamanya!
i = 0
while i < 5:
    print(i)
    # lupa: i += 1

# ✅ BENAR
i = 0
while i < 5:
    print(i)
    i += 1  # harus ada!
```

### 2. Salah Memahami `range()`

```python
# ❌ SALAH - dikira menghasilkan 1 sampai 10
for i in range(10):
    print(i)  # menghasilkan 0-9, bukan 1-10!

# ✅ BENAR jika mau 1 sampai 10
for i in range(1, 11):
    print(i)
```

### 3. `break` Hanya Keluar dari Loop Terdalam

```python
# Jika ada nested loop, break hanya keluar dari loop yang mengandungnya
for i in range(3):
    for j in range(3):
        if j == 1:
            break  # hanya keluar dari loop j, loop i tetap jalan!
    print(f"i = {i}")  # ini tetap dicetak 3 kali
```

### 4. Modifikasi List saat Sedang Di-iterasi

```python
data = [1, 2, 3, 4, 5]

# ❌ SALAH - hasil tidak terduga
for item in data:
    if item % 2 == 0:
        data.remove(item)  # jangan modifikasi list yang sedang di-loop!

# ✅ BENAR - buat list baru
data_ganjil = [item for item in data if item % 2 != 0]
```

### 5. Salah Meletakkan `continue` vs `break`

```python
# ❌ SALAH: pakai break padahal mau lewatin, bukan berhenti
for i in range(5):
    if i == 2:
        break  # ini menghentikan seluruh loop!
    print(i)  # Output: 0, 1 (bukan 0, 1, 3, 4)

# ✅ BENAR: pakai continue untuk melewati
for i in range(5):
    if i == 2:
        continue  # lewati angka 2 saja
    print(i)  # Output: 0, 1, 3, 4
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep Dasar

**a)** Apa output dari kode berikut?
```python
for i in range(2, 12, 3):
    print(i)
```

**b)** Apa perbedaan antara `break` dan `continue`? Berikan masing-masing satu contoh situasi di mana kamu akan menggunakannya.

**c)** Kode berikut memiliki bug. Temukan dan perbaiki:
```python
total = 0
i = 1
while i <= 100:
    total += i
print(f"Total 1 sampai 100 = {total}")
```

---

### Soal 2 — Coding Mandiri

**Soal: Kalkulator Rata-Rata Nilai**

Buat program yang meminta pengguna memasukkan nilai ujian satu per satu. Program berhenti ketika pengguna mengetik `"selesai"`. Program kemudian menampilkan total nilai, jumlah data, dan rata-rata.

**Contoh output:**
```
Masukkan nilai (ketik 'selesai' untuk berhenti):
Nilai ke-1: 85
Nilai ke-2: 90
Nilai ke-3: 78
Nilai ke-4: selesai

Total nilai  : 253
Jumlah data  : 3
Rata-rata    : 84.33
```

---

### Soal 3 — Studi Kasus Fintech

**Skenario: Sistem Cek Saldo dan Penarikan ATM**

Buat program simulasi ATM sederhana dengan spesifikasi:

1. Saldo awal: Rp 1.500.000
2. Program terus berjalan sampai pengguna memilih "Keluar"
3. Menu:
   - `1` → Cek Saldo
   - `2` → Tarik Tunai (masukkan nominal, validasi saldo cukup)
   - `3` → Keluar
4. Jika nominal penarikan > saldo, tampilkan pesan error dan jangan kurangi saldo
5. Setelah keluar, tampilkan ringkasan: berapa kali transaksi dilakukan dan saldo akhir

**Petunjuk:** Gunakan `while True` dengan `break` untuk menu, dan `if-elif-else` di dalamnya.

---

### Soal 4 — Challenge: Segitiga Angka

Buat program yang mencetak pola berikut untuk n=5:
```
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
```

---

## 📌 Ringkasan

- **`for` loop** → gunakan saat jumlah iterasi sudah diketahui atau kamu mau proses setiap elemen koleksi
- **`while` loop** → gunakan saat perulangan bergantung pada kondisi yang bisa berubah
- **`range(start, stop, step)`** → `stop` tidak ikut masuk, `step` bisa negatif untuk mundur
- **`break`** → keluar total dari loop, dipakai untuk kondisi sukses/gagal kritis
- **`continue`** → lewati iterasi saat ini, lanjut ke berikutnya — cocok untuk filter data
- **`pass`** → placeholder kosong, tidak melakukan apa-apa
- **Nested loop** → loop di dalam loop, kompleksitas bisa naik cepat — gunakan dengan bijak
- **Infinite loop** → selalu pastikan kondisi `while` akan berubah menjadi `False` pada suatu titik
- **Retry logic** → pola `while + break + counter` adalah pola standar di sistem produksi
- **Batch processing** → `for` loop atas list data adalah cara paling umum untuk proses data massal

> **Ingat:** Loop adalah otomasi paling dasar dalam pemrograman. Kalau kamu menemukan dirinya copy-paste kode yang sama berkali-kali, itu tanda kamu butuh loop!
