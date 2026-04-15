# Pertemuan 7: Review & Persiapan UTS

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Merangkum dan menghubungkan semua konsep dari Pertemuan 1–6 secara sistematis
- Mengidentifikasi area yang perlu diperkuat sebelum UTS
- Mengerjakan soal-soal yang mengintegrasikan beberapa konsep sekaligus
- Menerapkan strategi dan tips untuk mengerjakan UTS dengan efektif
- Membangun program lengkap yang menggabungkan algoritma, tipe data, percabangan, perulangan, dan fungsi

---

## 📖 Pengantar (Hook)

Kamu sudah melewati 6 pertemuan penuh materi. Sekarang saatnya melangkah mundur sejenak dan melihat gambaran besarnya.

Bayangkan kamu sedang membangun sebuah rumah. Pertemuan 1–6 adalah seperti mempelajari masing-masing bahan bangunan: bata, semen, besi, kayu, kaca, cat. Sekarang di pertemuan ini, kamu akan belajar **cara merakitnya menjadi rumah yang utuh**.

Program Python yang sesungguhnya tidak hanya terdiri dari satu konsep — sebuah program ATM misalnya, menggunakan **tipe data** untuk menyimpan saldo, **percabangan** untuk validasi PIN, **perulangan** untuk menu utama, dan **fungsi** untuk memisahkan setiap fitur. Semuanya bekerja bersama.

Pertemuan ini adalah kesempatanmu untuk:
1. Memastikan tidak ada konsep yang tertinggal
2. Berlatih soal-soal yang mirip dengan UTS
3. Membangun kepercayaan diri sebelum ujian

Mari kita mulai!

---

## 🧩 Ringkasan Komprehensif: Pertemuan 1–6

---

### 📘 Pertemuan 1: Algoritma & Flowchart

**Inti materi:**
- **Algoritma** = urutan langkah-langkah logis untuk menyelesaikan masalah
- Syarat algoritma: jelas (unambiguous), terbatas (finite), efektif, memiliki input/output
- **Flowchart** = representasi visual algoritma menggunakan simbol standar

**Simbol flowchart yang wajib diingat:**

| Simbol | Bentuk | Kegunaan |
|--------|--------|----------|
| Terminal | Oval/Elips | Mulai / Selesai |
| Proses | Persegi panjang | Perhitungan / Aksi |
| Keputusan | Belah ketupat | Kondisi Ya/Tidak |
| Input/Output | Jajar genjang | Membaca input / Menampilkan output |
| Aliran | Panah | Arah eksekusi |

**Konsep kunci:**
- Algoritma bisa ditulis dalam bahasa natural, pseudocode, atau flowchart — sebelum ditulis dalam kode
- Urutan langkah sangat penting: langkah yang salah urutan menghasilkan hasil yang salah

---

### 📘 Pertemuan 2: Tipe Data & Variabel

**Tipe data dasar Python:**

| Tipe | Contoh | Keterangan |
|------|--------|------------|
| `int` | `42`, `-10`, `0` | Bilangan bulat |
| `float` | `3.14`, `-0.5` | Bilangan desimal |
| `str` | `"halo"`, `'OVO'` | Teks, diapit kutip |
| `bool` | `True`, `False` | Nilai benar/salah |
| `list` | `[1, 2, 3]` | Koleksi berurutan, mutable |
| `tuple` | `(1, 2, 3)` | Koleksi berurutan, immutable |
| `dict` | `{"key": "value"}` | Pasangan key-value |

**Operasi penting:**
```python
# Konversi tipe
int("42")       # "42" → 42
float("3.14")   # "3.14" → 3.14
str(100)        # 100 → "100"

# F-string (cara format string terbaik)
nama = "Budi"
saldo = 1500000
print(f"Halo {nama}, saldo kamu: Rp{saldo:,}")

# Operasi string
"hello".upper()         # "HELLO"
"  spasi  ".strip()     # "spasi"
"a,b,c".split(",")      # ["a", "b", "c"]
",".join(["a","b","c"]) # "a,b,c"

# Operasi list
data = [3, 1, 4, 1, 5]
data.append(9)      # tambah di akhir
data.sort()         # sort in-place
len(data)           # panjang list
data[0]             # akses index pertama
data[-1]            # akses index terakhir
data[1:3]           # slicing: elemen index 1 dan 2
```

---

### 📘 Pertemuan 3 & 4: Percabangan (if / elif / else)

**Struktur dasar:**
```python
if kondisi_1:
    # jika kondisi_1 benar
elif kondisi_2:
    # jika kondisi_1 salah dan kondisi_2 benar
else:
    # jika semua kondisi di atas salah
```

**Operator perbandingan:**

| Operator | Arti |
|----------|------|
| `==` | Sama dengan |
| `!=` | Tidak sama dengan |
| `>` | Lebih besar |
| `<` | Lebih kecil |
| `>=` | Lebih besar atau sama |
| `<=` | Lebih kecil atau sama |

**Operator logika:**
- `and` → kedua kondisi harus benar
- `or` → cukup satu kondisi benar
- `not` → membalik nilai boolean

**Hal-hal yang bernilai `False`:** `0`, `0.0`, `""`, `[]`, `{}`, `None`

**Nested if** — if di dalam if:
```python
if pengguna_login:
    if saldo >= nominal:
        print("Transfer berhasil")
    else:
        print("Saldo tidak cukup")
else:
    print("Silakan login terlebih dahulu")
```

---

### 📘 Pertemuan 5: Perulangan (for / while / break / continue)

**Cheatsheet perulangan:**

```python
# for dengan range
for i in range(1, 6):          # 1, 2, 3, 4, 5
    print(i)

for i in range(0, 10, 2):     # 0, 2, 4, 6, 8
    print(i)

for i in range(5, 0, -1):     # 5, 4, 3, 2, 1
    print(i)

# for pada koleksi
for item in ["a", "b", "c"]:
    print(item)

# for dengan enumerate (dapat index sekaligus)
for i, item in enumerate(["apel", "jeruk", "mangga"]):
    print(f"{i}: {item}")

# while
i = 0
while i < 5:
    print(i)
    i += 1         # JANGAN LUPA ini!

# while dengan break
while True:
    user_input = input("Ketik 'quit' untuk keluar: ")
    if user_input == "quit":
        break

# continue - lewati iterasi tertentu
for i in range(10):
    if i % 2 == 0:
        continue   # lewati bilangan genap
    print(i)       # hanya cetak ganjil: 1, 3, 5, 7, 9
```

**Kapan pakai `for` vs `while`:**
- `for` → jumlah iterasi diketahui, atau iterasi koleksi
- `while` → iterasi berdasarkan kondisi yang bisa berubah kapan saja

---

### 📘 Pertemuan 6: Fungsi (def / parameter / return / scope)

**Cheatsheet fungsi:**

```python
# Definisi fungsi lengkap
def nama_fungsi(param1, param2="default"):
    """Docstring: jelaskan fungsi ini."""
    hasil = param1 + param2
    return hasil

# Pemanggilan
x = nama_fungsi(10)            # param2 pakai default
y = nama_fungsi(10, 20)        # positional
z = nama_fungsi(param2=5, param1=3)  # keyword (urutan bebas)

# Return beberapa nilai
def bagi_dan_sisa(a, b):
    return a // b, a % b

hasil_bagi, sisa = bagi_dan_sisa(17, 5)

# Scope
x = 100  # global

def fungsi():
    x = 200  # lokal, tidak mengubah global
    print(x)  # 200

fungsi()
print(x)  # masih 100
```

**Built-in functions penting:**
```python
len([1,2,3])           # 3
max([5, 2, 8])         # 8
min([5, 2, 8])         # 2
sum([1, 2, 3])         # 6
abs(-42)               # 42
round(3.14159, 2)      # 3.14
sorted([3,1,2])        # [1, 2, 3]
sorted([3,1,2], reverse=True)  # [3, 2, 1]
```

**Rekursi (pola dasar):**
```python
def rekursif(n):
    if n == 0:        # BASE CASE - wajib ada!
        return 1
    return n * rekursif(n - 1)  # RECURSIVE CASE
```

---

## 🧠 Ilustrasi / Analogi

### Peta Konsep: Hubungan Antar Materi

```
ALGORITMA (Pertemuan 1)
    │
    │ "cara berpikir sebelum coding"
    ▼
TIPE DATA & VARIABEL (Pertemuan 2)
    │
    │ "bahan-bahan dasar program"
    ▼
PERCABANGAN (Pertemuan 3-4)          PERULANGAN (Pertemuan 5)
    │                                      │
    │ "program bisa memilih"               │ "program bisa mengulang"
    └─────────────────┬────────────────────┘
                      │
                      │ "semua disatukan dalam"
                      ▼
               FUNGSI (Pertemuan 6)
                      │
                      │ "modularisasi & reusability"
                      ▼
              PROGRAM LENGKAP
         (ATM, Transfer, dst.)
```

### Analogi: Program seperti Resep Masakan

| Komponen Program | Analogi Masakan |
|-----------------|-----------------|
| Algoritma | Resep (langkah-langkah) |
| Variabel | Mangkuk, piring, tempat bahan |
| Tipe data | Jenis bahan (tepung, gula, telur) |
| Percabangan | "Jika adonan terlalu kering, tambah air" |
| Perulangan | "Aduk selama 10 menit" |
| Fungsi | Sub-resep khusus (misal: cara membuat saus) |

---

## 💻 Contoh Teknis

### Contoh Integrasi: Program Lengkap Pengelola Tabungan

```python
# Program sederhana yang menggabungkan semua konsep

# === FUNGSI-FUNGSI ===

def tampilkan_menu():
    """Tampilkan menu utama."""
    print("\n" + "="*40)
    print("   APLIKASI TABUNGAN SEDERHANA")
    print("="*40)
    print("1. Lihat Saldo")
    print("2. Setor Dana")
    print("3. Tarik Dana")
    print("4. Riwayat Transaksi")
    print("5. Keluar")
    print("-"*40)

def format_rupiah(nominal):
    """Format angka menjadi string rupiah."""
    return f"Rp{nominal:,}"

def setor(saldo, nominal):
    """Tambah saldo. Return saldo baru."""
    if nominal <= 0:
        return saldo, "Nominal harus lebih dari 0"
    return saldo + nominal, "Berhasil"

def tarik(saldo, nominal, saldo_min=50000):
    """Kurangi saldo. Return saldo baru."""
    if nominal <= 0:
        return saldo, "Nominal harus lebih dari 0"
    if nominal > saldo:
        return saldo, "Saldo tidak mencukupi"
    if saldo - nominal < saldo_min:
        return saldo, f"Saldo tidak boleh kurang dari {format_rupiah(saldo_min)}"
    return saldo - nominal, "Berhasil"

# === PROGRAM UTAMA ===

saldo = 500000
riwayat = []  # list of dict
nama_nasabah = "Budi Santoso"

print(f"Selamat datang, {nama_nasabah}!")

while True:
    tampilkan_menu()
    pilihan = input("Pilih menu (1-5): ").strip()
    
    if pilihan == "1":
        print(f"\nSaldo kamu: {format_rupiah(saldo)}")
    
    elif pilihan == "2":
        try:
            nominal = int(input("Nominal setor: Rp"))
            saldo_lama = saldo
            saldo, pesan = setor(saldo, nominal)
            if pesan == "Berhasil":
                riwayat.append({"tipe": "SETOR", "nominal": nominal, "saldo": saldo})
                print(f"[OK] Setor {format_rupiah(nominal)} berhasil!")
                print(f"Saldo sekarang: {format_rupiah(saldo)}")
            else:
                print(f"[GAGAL] {pesan}")
        except ValueError:
            print("[ERROR] Masukkan angka yang valid!")
    
    elif pilihan == "3":
        try:
            nominal = int(input("Nominal tarik: Rp"))
            saldo_lama = saldo
            saldo, pesan = tarik(saldo, nominal)
            if pesan == "Berhasil":
                riwayat.append({"tipe": "TARIK", "nominal": nominal, "saldo": saldo})
                print(f"[OK] Penarikan {format_rupiah(nominal)} berhasil!")
                print(f"Saldo sekarang: {format_rupiah(saldo)}")
            else:
                print(f"[GAGAL] {pesan}")
        except ValueError:
            print("[ERROR] Masukkan angka yang valid!")
    
    elif pilihan == "4":
        if not riwayat:
            print("\nBelum ada transaksi.")
        else:
            print(f"\n{'='*40}")
            print("RIWAYAT TRANSAKSI")
            print(f"{'='*40}")
            for i, trx in enumerate(riwayat, 1):
                print(f"{i}. [{trx['tipe']}] {format_rupiah(trx['nominal'])} | Saldo: {format_rupiah(trx['saldo'])}")
    
    elif pilihan == "5":
        print(f"\nTerima kasih, {nama_nasabah}!")
        print(f"Saldo akhir: {format_rupiah(saldo)}")
        print(f"Total transaksi: {len(riwayat)}")
        break
    
    else:
        print("[ERROR] Pilihan tidak valid. Masukkan angka 1-5.")
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Simulasi ATM Lengkap — Soal Integrasi UTS

**Konteks bisnis:**

Bank Digital Nusantara sedang membangun fitur ATM virtual yang bisa diakses via aplikasi. Fitur ini harus bisa:
- Autentikasi nasabah dengan PIN (max 3 percobaan)
- Menampilkan saldo
- Melakukan penarikan dengan validasi
- Melakukan transfer ke sesama nasabah
- Mencatat semua transaksi

Berikut implementasi lengkapnya — ini adalah contoh program yang mengintegrasikan SEMUA konsep yang sudah dipelajari:

```python
# ============================================================
# SIMULASI ATM - Bank Digital Nusantara
# Mengintegrasikan: tipe data, percabangan, perulangan, fungsi
# ============================================================

# === DATA NASABAH (simulasi database) ===
DATABASE_NASABAH = {
    "1234567890": {
        "nama": "Budi Santoso",
        "pin": "4321",
        "saldo": 2500000,
        "riwayat": []
    },
    "0987654321": {
        "nama": "Sari Dewi",
        "pin": "1111",
        "saldo": 1800000,
        "riwayat": []
    }
}


# === FUNGSI AUTENTIKASI ===

def autentikasi(no_rekening, pin_input, maks_percobaan=3):
    """
    Autentikasi nasabah berdasarkan nomor rekening dan PIN.
    
    Returns:
        dict atau None: Data nasabah jika berhasil, None jika gagal
    """
    if no_rekening not in DATABASE_NASABAH:
        print("Nomor rekening tidak ditemukan.")
        return None
    
    nasabah = DATABASE_NASABAH[no_rekening]
    percobaan = 0
    
    while percobaan < maks_percobaan:
        percobaan += 1
        
        if pin_input == nasabah["pin"]:
            print(f"Selamat datang, {nasabah['nama']}!")
            return nasabah
        else:
            sisa = maks_percobaan - percobaan
            if sisa > 0:
                print(f"PIN salah. Sisa percobaan: {sisa}")
                pin_input = input("Masukkan PIN: ")
            else:
                print("Akun diblokir sementara karena 3x salah PIN.")
                return None
    
    return None


# === FUNGSI TRANSAKSI ===

def cek_saldo(nasabah):
    """Tampilkan saldo nasabah."""
    print(f"\nSaldo kamu: Rp{nasabah['saldo']:,}")


def tarik_tunai(nasabah, nominal):
    """
    Proses penarikan tunai.
    
    Returns:
        bool: True jika berhasil
    """
    # Validasi nominal
    if nominal <= 0:
        print("Nominal harus lebih dari 0.")
        return False
    
    # Validasi kelipatan
    if nominal % 50000 != 0:
        print("Nominal harus kelipatan Rp50.000.")
        return False
    
    # Validasi saldo
    if nominal > nasabah["saldo"]:
        print(f"Saldo tidak mencukupi. Saldo kamu: Rp{nasabah['saldo']:,}")
        return False
    
    # Saldo minimum
    if nasabah["saldo"] - nominal < 50000:
        print("Saldo minimum Rp50.000 harus tersisa.")
        return False
    
    # Eksekusi
    nasabah["saldo"] -= nominal
    nasabah["riwayat"].append(f"TARIK: Rp{nominal:,} | Sisa: Rp{nasabah['saldo']:,}")
    print(f"[OK] Penarikan Rp{nominal:,} berhasil!")
    print(f"Saldo sekarang: Rp{nasabah['saldo']:,}")
    return True


def transfer(nasabah_pengirim, no_rekening_tujuan, nominal):
    """
    Proses transfer ke nasabah lain.
    
    Returns:
        bool: True jika berhasil
    """
    # Validasi rekening tujuan
    if no_rekening_tujuan not in DATABASE_NASABAH:
        print("Rekening tujuan tidak ditemukan.")
        return False
    
    # Tidak boleh transfer ke diri sendiri
    if DATABASE_NASABAH[no_rekening_tujuan] is nasabah_pengirim:
        print("Tidak bisa transfer ke rekening sendiri.")
        return False
    
    # Validasi nominal
    if nominal <= 0:
        print("Nominal harus lebih dari 0.")
        return False
    
    # Validasi saldo
    biaya_admin = 2500
    total = nominal + biaya_admin
    
    if total > nasabah_pengirim["saldo"]:
        print(f"Saldo tidak mencukupi (termasuk biaya admin Rp{biaya_admin:,}).")
        return False
    
    # Eksekusi
    nasabah_tujuan = DATABASE_NASABAH[no_rekening_tujuan]
    nasabah_pengirim["saldo"] -= total
    nasabah_tujuan["saldo"] += nominal
    
    nasabah_pengirim["riwayat"].append(
        f"TRANSFER KE {nasabah_tujuan['nama']}: Rp{nominal:,} | Sisa: Rp{nasabah_pengirim['saldo']:,}"
    )
    nasabah_tujuan["riwayat"].append(
        f"TERIMA DARI {nasabah_pengirim['nama']}: Rp{nominal:,}"
    )
    
    print(f"[OK] Transfer Rp{nominal:,} ke {nasabah_tujuan['nama']} berhasil!")
    print(f"Biaya admin: Rp{biaya_admin:,}")
    print(f"Saldo sekarang: Rp{nasabah_pengirim['saldo']:,}")
    return True


def lihat_riwayat(nasabah):
    """Tampilkan riwayat transaksi."""
    if not nasabah["riwayat"]:
        print("Belum ada riwayat transaksi.")
        return
    
    print(f"\n{'='*45}")
    print("RIWAYAT TRANSAKSI")
    print(f"{'='*45}")
    for i, trx in enumerate(nasabah["riwayat"], 1):
        print(f"{i}. {trx}")


# === MENU UTAMA ATM ===

def jalankan_atm():
    """Fungsi utama: menjalankan seluruh sesi ATM."""
    print("=" * 45)
    print("   SELAMAT DATANG DI ATM BANK DIGITAL NUSANTARA")
    print("=" * 45)
    
    # Login
    no_rekening = input("Nomor Rekening: ")
    pin = input("PIN: ")
    
    nasabah = autentikasi(no_rekening, pin)
    
    if nasabah is None:
        print("Sesi berakhir.")
        return
    
    # Menu utama
    while True:
        print("\n" + "-"*45)
        print("MENU UTAMA")
        print("-"*45)
        print("1. Cek Saldo")
        print("2. Tarik Tunai")
        print("3. Transfer")
        print("4. Riwayat Transaksi")
        print("5. Keluar")
        print("-"*45)
        
        pilihan = input("Pilih (1-5): ").strip()
        
        if pilihan == "1":
            cek_saldo(nasabah)
        
        elif pilihan == "2":
            try:
                nominal = int(input("Nominal tarik (kelipatan 50.000): Rp"))
                tarik_tunai(nasabah, nominal)
            except ValueError:
                print("Input tidak valid.")
        
        elif pilihan == "3":
            no_tujuan = input("Nomor rekening tujuan: ")
            try:
                nominal = int(input("Nominal transfer: Rp"))
                transfer(nasabah, no_tujuan, nominal)
            except ValueError:
                print("Input tidak valid.")
        
        elif pilihan == "4":
            lihat_riwayat(nasabah)
        
        elif pilihan == "5":
            print(f"\nTerima kasih telah menggunakan layanan kami, {nasabah['nama']}!")
            print(f"Saldo akhir: Rp{nasabah['saldo']:,}")
            break
        
        else:
            print("Pilihan tidak valid!")

# Jalankan ATM
jalankan_atm()
```

---

## 📊 Visualisasi

### Peta Materi UTS — Apa yang Perlu Dikuasai

```
DASAR-DASAR PEMROGRAMAN PYTHON
│
├── KONSEP FUNDAMENTAL
│   ├── Apa itu algoritma?
│   ├── Simbol flowchart
│   └── Cara baca pseudocode
│
├── TIPE DATA & VARIABEL
│   ├── int, float, str, bool
│   ├── list, dict, tuple
│   ├── Konversi tipe (int(), str(), float())
│   └── F-string & operasi string
│
├── PERCABANGAN
│   ├── if / elif / else
│   ├── Operator perbandingan & logika
│   ├── Nested if
│   └── Truthy/Falsy values
│
├── PERULANGAN
│   ├── for + range()
│   ├── for iterasi koleksi
│   ├── while + kondisi
│   ├── break, continue, pass
│   └── Nested loop
│
└── FUNGSI
    ├── def + parameter + return
    ├── Default value & keyword argument
    ├── Scope lokal vs global
    ├── Built-in functions
    └── Rekursi (faktorial, fibonacci)
```

### Tabel Tingkat Kesulitan Topik

| Topik | Bobot di UTS | Tingkat Kesulitan | Tips |
|-------|-------------|-------------------|------|
| Algoritma & Flowchart | 15% | ★☆☆ | Pahami simbolnya, latih baca alur |
| Tipe Data & Operasi | 20% | ★★☆ | Banyak hafal operasi built-in |
| Percabangan | 20% | ★★☆ | Hati-hati nested if yang dalam |
| Perulangan | 25% | ★★★ | Latihan banyak soal trace code |
| Fungsi + Scope | 20% | ★★★ | Pahami return vs print, lokal vs global |

---

## ⚠️ Kesalahan Umum (Kumpulan dari Semua Pertemuan)

### 1. `=` vs `==`
```python
# ❌ SALAH - assignment bukan comparison
if saldo = 0:  # SyntaxError!

# ✅ BENAR
if saldo == 0:
```

### 2. Lupa `return` di Fungsi
```python
# ❌ Fungsi ini selalu return None
def hitung(a, b):
    hasil = a + b  # lupa return!

x = hitung(3, 5)
print(x + 10)  # TypeError: NoneType tidak bisa dijumlah
```

### 3. Infinite Loop di `while`
```python
# ❌ SALAH
i = 0
while i < 10:
    print(i)
    # lupa i += 1!
```

### 4. `range(10)` dimulai dari 0, bukan 1
```python
list(range(10))  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9] → 10 elemen dari 0
list(range(1, 11))  # [1, 2, ..., 10] → jika mau 1 sampai 10
```

### 5. IndexError — Akses Index di Luar Batas
```python
data = [1, 2, 3]
print(data[3])  # IndexError! Index 0-2 saja yang ada
print(data[-1])  # OK: index terakhir = 3
```

### 6. Membandingkan String dengan `==`, Bukan `is`
```python
nama = "budi"
# ✅ untuk perbandingan nilai
if nama == "budi":
    print("Match!")

# ❌ 'is' membandingkan identitas objek, bukan nilai
if nama is "budi":  # jangan pakai ini untuk string
    ...
```

### 7. Integer Division vs Float Division
```python
print(7 / 2)   # 3.5  (float division)
print(7 // 2)  # 3    (integer division, buang desimal)
print(7 % 2)   # 1    (modulus, sisa bagi)
```

---

## 🧪 Latihan / Studi Kasus — Soal UTS

---

### BAGIAN A: Soal Teori (Pilihan Ganda & Essay)

**Soal 1 — Pilihan Ganda**

Apa output dari kode berikut?
```python
x = 10
y = 3

if x > 5 and y < 5:
    print("A")
elif x > 5 or y > 5:
    print("B")
else:
    print("C")
```
a) A  
b) B  
c) C  
d) Error  

*Jawaban: a) A*

---

**Soal 2 — Pilihan Ganda**

Apa output dari kode berikut?
```python
for i in range(2, 10, 3):
    print(i, end=" ")
```
a) 2 5 8  
b) 2 4 6 8  
c) 2 3 5 8  
d) 2 5 8 11  

*Jawaban: a) 2 5 8*

---

**Soal 3 — Pilihan Ganda**

Manakah pernyataan yang BENAR tentang variabel lokal?

a) Variabel lokal bisa diakses dari seluruh program  
b) Variabel lokal hanya bisa diakses di dalam fungsi tempat ia dideklarasikan  
c) Variabel lokal sama dengan variabel global  
d) Variabel lokal harus dideklarasikan dengan kata kunci `local`  

*Jawaban: b)*

---

**Soal 4 — Pilihan Ganda**

Apa yang dikembalikan fungsi ini saat dipanggil dengan `hitung(10)`?
```python
def hitung(n):
    if n % 2 == 0:
        return "genap"
    return "ganjil"
```
a) `None`  
b) `"ganjil"`  
c) `"genap"`  
d) `10`  

*Jawaban: c) "genap"*

---

**Soal 5 — Essay**

Jelaskan perbedaan antara `break` dan `continue` dalam sebuah loop. Berikan masing-masing satu contoh skenario nyata (dari dunia fintech atau backend) di mana kamu akan menggunakan `break` dan di mana kamu akan menggunakan `continue`!

*Panduan jawaban:*
- `break`: menghentikan seluruh loop saat kondisi tertentu terpenuhi. Contoh: proses transfer batch, hentikan semua proses jika ditemukan transaksi mencurigakan (fraud detection)
- `continue`: melewati iterasi saat ini dan lanjut ke berikutnya. Contoh: kirim notifikasi ke semua nasabah aktif, lewati nasabah yang sudah menonaktifkan notifikasi

---

### BAGIAN B: Soal Coding Python

---

**Soal Coding 1 — Perulangan & Percabangan (Tingkat Dasar)**

Buat program Python yang:
1. Menerima input satu angka bulat positif `n` dari pengguna
2. Mencetak semua angka dari 1 sampai `n`
3. Untuk setiap angka:
   - Jika habis dibagi 3 DAN 5: cetak `"BankBRI"`
   - Jika habis dibagi 3 saja: cetak `"Bank"`
   - Jika habis dibagi 5 saja: cetak `"BRI"`
   - Selainnya: cetak angkanya

**Contoh output untuk n=15:**
```
1
2
Bank
4
BRI
Bank
7
8
Bank
BRI
11
Bank
13
14
BankBRI
```

---

**Soal Coding 2 — Fungsi & Tipe Data (Tingkat Menengah)**

Buat program untuk menganalisis nilai ujian mahasiswa dengan spesifikasi:

1. Fungsi `input_nilai(n)` — minta input `n` nilai dari pengguna, return list nilai
2. Fungsi `hitung_statistik(daftar_nilai)` — return tuple: (rata_rata, tertinggi, terendah)
3. Fungsi `tentukan_grade(nilai)`:
   - 90–100: `"A"`
   - 80–89: `"B"`
   - 70–79: `"C"`
   - 60–69: `"D"`
   - Di bawah 60: `"E"`
4. Fungsi `cetak_laporan(daftar_nilai)` — tampilkan tabel nilai + grade, lalu tampilkan statistik

**Contoh output:**
```
No | Nilai | Grade
---|-------|------
1  |  85   |   B
2  |  72   |   C
3  |  91   |   A
4  |  55   |   E

Rata-rata : 75.75
Tertinggi : 91
Terendah  : 55
```

---

**Soal Coding 3 — Integrasi Semua Konsep / Studi Kasus Fintech (Tingkat Lanjut)**

**Skenario: Mesin Kasir Digital Mini-Market**

Buat program simulasi kasir menggunakan fungsi dan perulangan dengan spesifikasi:

**Data produk (dict):**
```python
PRODUK = {
    "P001": {"nama": "Indomie Goreng", "harga": 3500},
    "P002": {"nama": "Aqua 600ml", "harga": 4000},
    "P003": {"nama": "Teh Botol", "harga": 5000},
    "P004": {"nama": "Roti Tawar", "harga": 12000},
    "P005": {"nama": "Susu Ultra 250ml", "harga": 6500},
}
```

**Fungsi yang harus dibuat:**
1. `tampilkan_produk(produk)` — tampilkan daftar produk dengan harga
2. `tambah_ke_keranjang(keranjang, kode_produk, qty, produk)` — tambahkan item ke keranjang (dict), validasi kode produk ada
3. `hitung_subtotal(keranjang, produk)` — hitung total belanja
4. `hitung_diskon(subtotal)`:
   - Subtotal ≥ Rp 50.000: diskon 10%
   - Subtotal ≥ Rp 30.000: diskon 5%
   - Di bawah itu: tidak ada diskon
5. `cetak_struk(keranjang, produk)` — cetak struk belanja lengkap dengan subtotal, diskon, dan total bayar

**Alur program:**
- Tampilkan produk
- Loop: pengguna memasukkan kode produk + qty (ketik `"selesai"` untuk stop)
- Cetak struk

**Contoh struk:**
```
==============================
     STRUK BELANJA DIGITAL
==============================
Indomie Goreng   x3  Rp10.500
Aqua 600ml       x2  Rp 8.000
Roti Tawar       x1  Rp12.000
------------------------------
Subtotal         :  Rp30.500
Diskon (5%)      :  Rp 1.525
Total Bayar      :  Rp28.975
==============================
Terima kasih sudah berbelanja!
```

---

## 📌 Ringkasan & Tips UTS

### Cheatsheet Sintaks Python (Bawa Ke Kepala!)

```python
# TIPE DATA
x = 10          # int
y = 3.14        # float
s = "hello"     # str
b = True        # bool
lst = [1,2,3]   # list (mutable)
tpl = (1,2,3)   # tuple (immutable)
dct = {"a":1}   # dict

# PERCABANGAN
if kondisi:
    ...
elif kondisi_lain:
    ...
else:
    ...

# FOR LOOP
for i in range(start, stop, step):
    ...

for item in koleksi:
    ...

# WHILE LOOP
while kondisi:
    ...
    # jangan lupa update kondisi!

# FUNGSI
def nama(param, default_param=nilai):
    """Docstring."""
    return hasil

# BUILT-IN
len(x)  max(x)  min(x)  sum(x)
abs(x)  round(x, n)  sorted(x)
int(x)  float(x)  str(x)
```

### 10 Tips Mengerjakan UTS

1. **Baca soal 2x** sebelum mulai nulis kode — pastikan kamu mengerti apa yang diminta
2. **Tulis pseudocode atau flowchart dulu** untuk soal coding yang kompleks
3. **Mulai dari yang kamu tahu** — jangan terpaku di soal yang sulit, kerjakan yang mudah dulu
4. **Perhatikan indentasi** — Python sangat sensitif dengan spasi/tab
5. **Trace code di kertas** — untuk soal "apa outputnya?", jalankan kode di kepala baris per baris
6. **Ingat: `print` ≠ `return`** — `print` menampilkan ke layar, `return` mengembalikan nilai ke pemanggil
7. **Cek tipe data** — operasi string dan integer berbeda. `"5" + "3" = "53"`, bukan `8`
8. **Untuk soal while: selalu cek apakah ada kondisi berhenti** yang jelas
9. **Untuk soal fungsi: cek apakah ada `return`** — fungsi tanpa return selalu `None`
10. **Manajemen waktu**: alokasikan ~30% waktu untuk soal teori, ~70% untuk coding

### Checklist Persiapan Terakhir

Sebelum UTS, pastikan kamu bisa mengerjakan ini tanpa melihat catatan:

- [ ] Tulis program "Halo, dunia!" dan program input nama
- [ ] Buat if-elif-else untuk grade nilai
- [ ] Buat for loop yang mencetak bilangan genap 1-20
- [ ] Buat while loop dengan kondisi berhenti
- [ ] Pakai break dan continue dengan tepat
- [ ] Definisikan fungsi dengan parameter dan return value
- [ ] Jelaskan perbedaan variabel lokal dan global
- [ ] Pakai min(), max(), len(), sorted() pada sebuah list
- [ ] Buat fungsi rekursif faktorial
- [ ] Buat program mini yang mengombinasikan semua konsep di atas

> **Pesan terakhir:** UTS bukan tentang hafalan sintaks — melainkan tentang **kemampuan berpikir algoritmik**. Kalau kamu mengerti *mengapa* kode bekerja, bukan hanya *bagaimana* menulisnya, kamu siap untuk UTS. Semangat!
