# Pertemuan 4: Input/Output & Struktur Percabangan (if, elif, else)

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Menggunakan `input()` untuk menerima data dari user dan `print()` untuk menampilkan output
- Memformat output dengan f-string agar rapi dan informatif
- Membuat program dengan percabangan `if`, `elif`, dan `else`
- Membuat percabangan bersarang (*nested if*) untuk kondisi yang lebih kompleks
- Memvalidasi input user sebelum diproses
- Menggunakan *operator ternary* untuk ekspresi kondisional yang ringkas
- Membangun program interaktif sederhana seperti kalkulator dan penentu kategori

---

## 📖 Pengantar (Hook)

Bayangkan kamu sedang mengajukan pinjaman di aplikasi fintech. Kamu masukkan penghasilan, pekerjaan, dan jumlah pinjaman. Lalu dalam hitungan detik, muncul pesan:

> "Selamat! Pengajuan pinjaman Rp 10.000.000 kamu telah **DISETUJUI** dengan cicilan Rp 972.222/bulan selama 12 bulan."

Atau sebaliknya:
> "Maaf, pengajuan kamu **DITOLAK**. Rasio cicilan terhadap penghasilan melebihi batas yang ditentukan."

Dua kemungkinan output dari satu program. Sistem memutuskan berdasarkan kondisi yang kamu masukkan. Inilah inti dari **percabangan (branching)** dalam pemrograman.

Program tanpa percabangan itu seperti lampu yang tidak punya saklar — nyala terus atau mati terus. Dengan `if`, `elif`, dan `else`, program kamu bisa *berpikir* dan merespons secara berbeda tergantung situasi.

Di dunia fintech, percabangan ada di mana-mana: keputusan kredit, kategori risiko, tier cashback, deteksi fraud, verifikasi identitas — semuanya adalah if-elif-else yang kompleks.

---

## 🧩 Konsep Utama

### Input & Output

#### `input()` — Menerima Data dari User

```python
nama = input("Masukkan nama kamu: ")
```

Hal yang WAJIB diingat tentang `input()`:
- **Selalu mengembalikan `str`** — tanpa terkecuali
- User mengetik `25` → Python menyimpannya sebagai `"25"`, bukan `25`
- Selalu konversi sebelum dipakai untuk kalkulasi!

```python
# Pola standar input angka yang benar:
usia = int(input("Masukkan usia: "))
nominal = float(input("Masukkan nominal (Rp): "))
```

#### `print()` — Menampilkan Output

```python
# Cara 1: Concatenation (kurang direkomendasikan)
nama = "Budi"
saldo = 1500000
print("Halo " + nama + ", saldo kamu: Rp " + str(saldo))  # perlu str()

# Cara 2: f-string (DIREKOMENDASIKAN — Python 3.6+)
print(f"Halo {nama}, saldo kamu: Rp {saldo:,}")

# Cara 3: format()
print("Halo {}, saldo kamu: Rp {:,}".format(nama, saldo))
```

**F-string Formatting yang Sering Dipakai:**

| Format | Fungsi | Contoh | Output |
|---|---|---|---|
| `{nilai:,}` | Tambah separator ribuan | `f"{1500000:,}"` | `1,500,000` |
| `{nilai:.2f}` | 2 angka desimal | `f"{3.14159:.2f}"` | `3.14` |
| `{nilai:.0f}` | Bulatkan, tanpa desimal | `f"{3.9:.0f}"` | `4` |
| `{nilai:.1%}` | Format persen 1 desimal | `f"{0.125:.1%}"` | `12.5%` |
| `{nilai:>10}` | Rata kanan, lebar 10 | `f"{'Budi':>10}"` | `      Budi` |
| `{nilai:<10}` | Rata kiri, lebar 10 | `f"{'Budi':<10}"` | `Budi      ` |
| `{nilai:010}` | Pad dengan nol | `f"{42:010}"` | `0000000042` |

### Struktur Percabangan

#### `if` — Kondisi Tunggal

```python
if kondisi:
    # blok ini dijalankan hanya jika kondisi True
    lakukan_sesuatu()
```

#### `if ... else` — Dua Pilihan

```python
if kondisi:
    # jika kondisi True
    aksi_jika_benar()
else:
    # jika kondisi False
    aksi_jika_salah()
```

#### `if ... elif ... else` — Banyak Pilihan

```python
if kondisi_1:
    aksi_1()
elif kondisi_2:
    aksi_2()
elif kondisi_3:
    aksi_3()
else:
    aksi_default()
```

Python mengecek kondisi dari **atas ke bawah** dan mengeksekusi blok **pertama** yang kondisinya `True`, lalu **langsung loncat** ke setelah seluruh struktur `if`.

#### Nested `if` — Percabangan Bersarang

```python
if kondisi_luar:
    if kondisi_dalam:
        aksi_keduanya_benar()
    else:
        aksi_luar_benar_dalam_salah()
else:
    aksi_luar_salah()
```

#### Operator Ternary — Satu Baris

```python
# Syntax: nilai_jika_true if kondisi else nilai_jika_false
status = "aktif" if saldo > 0 else "tidak aktif"
label = "LULUS" if nilai >= 75 else "TIDAK LULUS"
```

---

## 🧠 Ilustrasi / Analogi

### Analogi: Percabangan = Persimpangan Jalan

Bayangkan kamu mengemudi dan tiba di persimpangan:

```
                    [Mulai perjalanan]
                           │
                           ▼
                  Apakah lampu hijau?
                    /           \
                  YA            TIDAK
                  │               │
              Melaju           Berhenti
                  │               │
                  └───────┬───────┘
                          │
                    [Lanjut jalan]
```

`if` adalah titik persimpangan. Program "berkendara" sepanjang alur kode, dan di setiap `if` ia memutuskan harus belok ke mana berdasarkan kondisi yang ada.

### Analogi: elif = Menu Restoran

```
Kamu masuk restoran dan pilih:
  - Kalau mood makan berat  → Pesan nasi goreng
  - Kalau mood makan ringan → Pesan salad
  - Kalau mood minum saja   → Pesan jus
  - Kalau tidak tahu        → Pesan paket rekomendasi chef
```

Itulah `if ... elif ... elif ... else`. Setiap kondisi dicek berurutan, dan begitu satu cocok, yang lain diabaikan.

### Tabel Perbandingan Struktur Percabangan

| Struktur | Kapan Digunakan | Contoh Kasus |
|---|---|---|
| `if` saja | Lakukan sesuatu hanya jika kondisi terpenuhi, tidak perlu alternatif | Log error jika ada masalah |
| `if ... else` | Dua kemungkinan: kondisi terpenuhi atau tidak | Pinjaman disetujui / ditolak |
| `if ... elif ... else` | Tiga atau lebih kemungkinan yang saling eksklusif | Kategori risiko: rendah/sedang/tinggi |
| `nested if` | Kondisi dalam kondisi | Cek saldo dulu, baru cek limit transaksi |
| Ternary | Dua pilihan, ekspresi singkat | `status = "aktif" if saldo > 0 else "nonaktif"` |

---

## 💻 Contoh Teknis

### Contoh 1: Penentuan Nilai Huruf

```python
nilai = float(input("Masukkan nilai ujian (0-100): "))

if nilai >= 90:
    huruf = "A"
    predikat = "Sangat Baik"
elif nilai >= 80:
    huruf = "B"
    predikat = "Baik"
elif nilai >= 70:
    huruf = "C"
    predikat = "Cukup"
elif nilai >= 60:
    huruf = "D"
    predikat = "Kurang"
else:
    huruf = "E"
    predikat = "Tidak Lulus"

print(f"\nNilai kamu : {nilai}")
print(f"Huruf      : {huruf}")
print(f"Predikat   : {predikat}")
```

**Output contoh:**
```
Masukkan nilai ujian (0-100): 83

Nilai kamu : 83.0
Huruf      : B
Predikat   : Baik
```

### Contoh 2: Kalkulator Sederhana

```python
print("=== Kalkulator Sederhana ===")
print("Operasi yang tersedia: +, -, *, /")

angka1 = float(input("\nMasukkan angka pertama: "))
operator = input("Masukkan operator (+, -, *, /): ").strip()
angka2 = float(input("Masukkan angka kedua  : "))

if operator == "+":
    hasil = angka1 + angka2
    simbol = "+"
elif operator == "-":
    hasil = angka1 - angka2
    simbol = "-"
elif operator == "*":
    hasil = angka1 * angka2
    simbol = "×"
elif operator == "/":
    if angka2 == 0:
        print("\n❌ Error: Tidak bisa membagi dengan nol!")
        hasil = None
    else:
        hasil = angka1 / angka2
        simbol = "÷"
else:
    print(f"\n❌ Error: Operator '{operator}' tidak dikenali!")
    hasil = None

if hasil is not None:
    print(f"\n{angka1} {simbol} {angka2} = {hasil:,.4f}")
```

**Output contoh:**
```
=== Kalkulator Sederhana ===
Operasi yang tersedia: +, -, *, /

Masukkan angka pertama: 15000000
Masukkan operator (+, -, *, /): /
Masukkan angka kedua  : 12

15000000.0 ÷ 12.0 = 1,250,000.0000
```

### Contoh 3: Validasi Input

```python
# Pola validasi input yang robust
usia_input = input("Masukkan usia (tahun): ").strip()

# Validasi: apakah bisa dikonversi ke int?
if usia_input.isdigit():
    usia = int(usia_input)

    # Validasi: apakah dalam rentang yang masuk akal?
    if 0 < usia <= 120:
        print(f"Usia valid: {usia} tahun")
    else:
        print("❌ Usia harus antara 1-120 tahun")
else:
    print("❌ Masukkan angka yang valid, bukan huruf atau simbol")
```

### Contoh 4: Operator Ternary

```python
saldo = 750000
limit = 500000

# Tanpa ternary (verbose)
if saldo >= limit:
    status_transfer = "BISA"
else:
    status_transfer = "TIDAK BISA"

# Dengan ternary (ringkas)
status_transfer = "BISA" if saldo >= limit else "TIDAK BISA"

# Nested ternary (hati-hati — bisa tidak terbaca!)
kategori = "Premium" if saldo > 5000000 else "Regular" if saldo > 1000000 else "Basic"
# Lebih baik gunakan if-elif-else biasa untuk kasus ini
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Kasus: Sistem Persetujuan Pinjaman Otomatis & Kategori Risiko Kredit

**Latar Belakang:**
Platform pinjaman digital (seperti Kredivo, Akulaku, atau Danamas) menggunakan sistem scoring otomatis untuk menentukan kelayakan pinjaman dan kategori risiko nasabah. Proses ini harus cepat, konsisten, dan terdokumentasi.

**Aturan Bisnis:**
1. Penghasilan bulanan minimum: Rp 3.000.000
2. Usia antara 21–60 tahun
3. Skor kredit internal (0–100): dihitung dari histori pembayaran, rasio utang, dll
4. Kategori risiko berdasarkan skor:
   - Skor 80–100 → Risiko RENDAH → bunga 12%/tahun, limit hingga 50% penghasilan × 12
   - Skor 60–79 → Risiko SEDANG → bunga 18%/tahun, limit hingga 30% penghasilan × 12
   - Skor 40–59 → Risiko TINGGI → bunga 24%/tahun, limit hingga 20% penghasilan × 12
   - Skor < 40 → DITOLAK otomatis
5. Jika ada riwayat gagal bayar aktif → DITOLAK otomatis, apapun skornya

**Solusi Teknis:**

```python
# ============================================================
# SISTEM SCORING PINJAMAN OTOMATIS
# Platform: LendTech Digital
# ============================================================

def tampilkan_header():
    print("\n" + "=" * 55)
    print("      SISTEM PENGAJUAN PINJAMAN - LendTech Digital")
    print("=" * 55)

tampilkan_header()

# ---- INPUT DATA NASABAH ----
print("\n📋 Masukkan Data Pengajuan:")
nama = input("Nama lengkap         : ").strip().title()
usia = int(input("Usia (tahun)         : "))
penghasilan = int(input("Penghasilan/bulan(Rp): "))
pinjaman_diminta = int(input("Jumlah pinjaman (Rp) : "))
tenor = int(input("Tenor (bulan)        : "))
skor_kredit = int(input("Skor kredit internal : "))
punya_gagal_bayar = input("Ada gagal bayar aktif? (ya/tidak): ").strip().lower()

# ---- KONVERSI ----
ada_gagal_bayar = punya_gagal_bayar == "ya"

# ---- KONSTANTA BISNIS ----
PENGHASILAN_MIN = 3_000_000
USIA_MIN = 21
USIA_MAX = 60

print(f"\n{'=' * 55}")
print(f"  HASIL EVALUASI UNTUK: {nama.upper()}")
print(f"{'=' * 55}")

# ---- VALIDASI DASAR ----
penghasilan_ok = penghasilan >= PENGHASILAN_MIN
usia_ok = USIA_MIN <= usia <= USIA_MAX
tidak_gagal_bayar = not ada_gagal_bayar
skor_ok = skor_kredit >= 40

print(f"\n[Validasi Awal]")
print(f"  Penghasilan min Rp {PENGHASILAN_MIN:,} : {'✅ LULUS' if penghasilan_ok else '❌ GAGAL'}")
print(f"  Usia {USIA_MIN}-{USIA_MAX} tahun          : {'✅ LULUS' if usia_ok else '❌ GAGAL'}")
print(f"  Tidak ada gagal bayar    : {'✅ LULUS' if tidak_gagal_bayar else '❌ GAGAL'}")
print(f"  Skor kredit >= 40        : {'✅ LULUS' if skor_ok else '❌ GAGAL'}")

# ---- KEPUTUSAN UTAMA ----
if not penghasilan_ok:
    print(f"\n❌ PENGAJUAN DITOLAK")
    print(f"   Alasan: Penghasilan Rp {penghasilan:,} di bawah minimum Rp {PENGHASILAN_MIN:,}")

elif not usia_ok:
    print(f"\n❌ PENGAJUAN DITOLAK")
    print(f"   Alasan: Usia {usia} tahun tidak memenuhi syarat ({USIA_MIN}–{USIA_MAX} tahun)")

elif ada_gagal_bayar:
    print(f"\n❌ PENGAJUAN DITOLAK OTOMATIS")
    print(f"   Alasan: Terdapat riwayat gagal bayar aktif")
    print(f"   Tindakan: Selesaikan tunggakan terlebih dahulu")

elif skor_kredit < 40:
    print(f"\n❌ PENGAJUAN DITOLAK")
    print(f"   Alasan: Skor kredit {skor_kredit} terlalu rendah (minimum 40)")

else:
    # ---- TENTUKAN KATEGORI RISIKO ----
    if skor_kredit >= 80:
        kategori_risiko = "RENDAH"
        bunga_tahunan = 0.12
        faktor_limit = 0.50
        emoji_risiko = "🟢"
    elif skor_kredit >= 60:
        kategori_risiko = "SEDANG"
        bunga_tahunan = 0.18
        faktor_limit = 0.30
        emoji_risiko = "🟡"
    else:  # 40-59
        kategori_risiko = "TINGGI"
        bunga_tahunan = 0.24
        faktor_limit = 0.20
        emoji_risiko = "🔴"

    # ---- KALKULASI LIMIT & CICILAN ----
    limit_disetujui = penghasilan * 12 * faktor_limit
    pinjaman_final = min(pinjaman_diminta, limit_disetujui)  # ambil yang lebih kecil
    bunga_per_bulan = bunga_tahunan / 12
    cicilan_pokok = pinjaman_final / tenor
    cicilan_bunga = pinjaman_final * bunga_per_bulan
    cicilan_total = cicilan_pokok + cicilan_bunga
    total_bayar = cicilan_total * tenor
    total_bunga = total_bayar - pinjaman_final
    rasio_cicilan = cicilan_total / penghasilan

    # ---- CEK CICILAN TIDAK MELEBIHI RASIO AMAN ----
    RASIO_MAX = 0.40
    cicilan_aman = rasio_cicilan <= RASIO_MAX

    print(f"\n[Kategori Risiko]")
    print(f"  {emoji_risiko} Risiko {kategori_risiko} — Skor kredit: {skor_kredit}/100")
    print(f"  Bunga tahunan flat : {bunga_tahunan:.0%}")

    if not cicilan_aman:
        print(f"\n⚠️  PERHATIAN: Cicilan ({rasio_cicilan:.1%} penghasilan) melebihi batas aman 40%")
        # Hitung tenor minimum agar cicilan aman
        tenor_min = int((pinjaman_final * (1 + bunga_per_bulan * tenor)) / (penghasilan * RASIO_MAX)) + 1
        print(f"   Saran: Naikkan tenor minimal ke {tenor_min} bulan")
        print(f"\n❌ PENGAJUAN DITOLAK — Cicilan tidak dalam batas kemampuan bayar")
    else:
        print(f"\n✅ PENGAJUAN DISETUJUI")
        print(f"\n[Detail Pinjaman yang Disetujui]")
        print(f"  Nama nasabah     : {nama}")
        print(f"  Jumlah pinjaman  : Rp {pinjaman_final:,.0f}", end="")
        if pinjaman_diminta > limit_disetujui:
            print(f" (disesuaikan dari Rp {pinjaman_diminta:,})")
        else:
            print()
        print(f"  Tenor            : {tenor} bulan")
        print(f"  Bunga flat/tahun : {bunga_tahunan:.0%}")
        print(f"  Cicilan/bulan    : Rp {cicilan_total:,.0f}")
        print(f"  Rasio cicilan    : {rasio_cicilan:.1%} penghasilan")
        print(f"  Total bunga      : Rp {total_bunga:,.0f}")
        print(f"  Total pembayaran : Rp {total_bayar:,.0f}")

print("\n" + "=" * 55)
print("  Informasi ini bersifat simulasi. Keputusan final")
print("  tunduk pada verifikasi dokumen tim analis kredit.")
print("=" * 55 + "\n")
```

**Contoh Output (Nasabah Disetujui):**
```
===================================================
      SISTEM PENGAJUAN PINJAMAN - LendTech Digital
===================================================

📋 Masukkan Data Pengajuan:
Nama lengkap         : siti rahayu
Usia (tahun)         : 32
Penghasilan/bulan(Rp): 7500000
Jumlah pinjaman (Rp) : 20000000
Tenor (bulan)        : 24
Skor kredit internal : 75
Ada gagal bayar aktif? (ya/tidak): tidak

===================================================
  HASIL EVALUASI UNTUK: SITI RAHAYU
===================================================

[Validasi Awal]
  Penghasilan min Rp 3,000,000 : ✅ LULUS
  Usia 21-60 tahun              : ✅ LULUS
  Tidak ada gagal bayar         : ✅ LULUS
  Skor kredit >= 40             : ✅ LULUS

[Kategori Risiko]
  🟡 Risiko SEDANG — Skor kredit: 75/100
  Bunga tahunan flat : 18%

✅ PENGAJUAN DISETUJUI

[Detail Pinjaman yang Disetujui]
  Nama nasabah     : Siti Rahayu
  Jumlah pinjaman  : Rp 20,000,000 (disesuaikan dari Rp 20,000,000)
  Tenor            : 24 bulan
  Bunga flat/tahun : 18%
  Cicilan/bulan    : Rp 1,133,333
  Rasio cicilan    : 15.1% penghasilan
  Total bunga      : Rp 7,200,000
  Total pembayaran : Rp 27,200,000
===================================================
```

**Mengapa ini penting secara bisnis:**
- Proses scoring manual membutuhkan 1–3 hari kerja; otomatisasi membuatnya selesai dalam detik
- Konsistensi keputusan — tidak tergantung pada suasana hati analis
- Audit trail yang jelas: setiap keputusan bisa dijelaskan dengan aturan yang terdokumentasi
- Sistem seperti ini adalah inti dari credit engine di Kredivo, Julo, dan Modalku

---

## 📊 Visualisasi

### Flowchart Sistem Percabangan Pinjaman

```
[MULAI]
    │
    ▼
[Input data nasabah]
    │
    ▼
Penghasilan >= 3jt?
  Tidak → [TOLAK: Penghasilan kurang]
  Ya ↓
    │
    ▼
Usia 21-60 tahun?
  Tidak → [TOLAK: Usia tidak sesuai]
  Ya ↓
    │
    ▼
Ada gagal bayar aktif?
  Ya → [TOLAK: Ada gagal bayar]
  Tidak ↓
    │
    ▼
Skor kredit >= 40?
  Tidak → [TOLAK: Skor terlalu rendah]
  Ya ↓
    │
    ▼
Skor >= 80?
  Ya → Risiko RENDAH (bunga 12%, limit 50%)
  Tidak ↓
    │
Skor >= 60?
  Ya → Risiko SEDANG (bunga 18%, limit 30%)
  Tidak → Risiko TINGGI (bunga 24%, limit 20%)
    │
    ▼
Hitung cicilan, cek rasio <= 40%?
  Tidak → [TOLAK: Cicilan tidak terjangkau]
  Ya → [SETUJUI: Tampilkan detail pinjaman]
    │
    ▼
[SELESAI]
```

### Tier Cashback — Step-by-Step Logic

```python
# Contoh: Sistem tier cashback e-commerce

nominal = 750000

# Pengecekan berurutan dari atas ke bawah
if nominal < 50000:
    cashback_persen = 0
    tier = "No Cashback"
elif nominal < 200000:
    cashback_persen = 2
    tier = "Bronze"
elif nominal < 500000:
    cashback_persen = 5
    tier = "Silver"
elif nominal < 2000000:
    cashback_persen = 8
    tier = "Gold"
else:
    cashback_persen = 10
    tier = "Platinum"

cashback = nominal * cashback_persen / 100
cashback = min(cashback, 100000)  # maksimal cashback Rp 100.000

print(f"Nominal transaksi : Rp {nominal:,}")
print(f"Tier              : {tier} ({cashback_persen}%)")
print(f"Cashback          : Rp {cashback:,.0f}")
print(f"Bayar setelah CB  : Rp {nominal - cashback:,.0f}")
```

---

## ⚠️ Kesalahan Umum

### 1. Lupa Indentasi (Indentation Error)

```python
# SALAH — blok if tidak diindentasi
if saldo > 0:
print("Saldo ada")   # IndentationError!

# BENAR — gunakan 4 spasi (atau 1 tab, konsisten)
if saldo > 0:
    print("Saldo ada")
```

### 2. Urutan elif yang Salah

```python
nilai = 85

# SALAH — kondisi pertama menangkap semua nilai >= 60
if nilai >= 60:
    print("C")
elif nilai >= 80:    # ini tidak akan pernah tercapai untuk nilai >= 80!
    print("B")
elif nilai >= 90:
    print("A")

# BENAR — urutkan dari yang paling spesifik (ketat) ke paling umum
if nilai >= 90:
    print("A")
elif nilai >= 80:
    print("B")
elif nilai >= 60:
    print("C")
else:
    print("D/E")
```

### 3. Menggunakan `=` Bukan `==` dalam Kondisi

```python
# SALAH
status = "aktif"
if status = "aktif":   # SyntaxError!
    print("Akun aktif")

# BENAR
if status == "aktif":
    print("Akun aktif")
```

### 4. Tidak Menangani Input yang Tidak Valid

```python
# BERBAHAYA — langsung konversi tanpa validasi
usia = int(input("Masukkan usia: "))  # crash jika user ketik "dua puluh"!

# LEBIH BAIK — validasi dulu
usia_str = input("Masukkan usia: ").strip()
if usia_str.isdigit():
    usia = int(usia_str)
    print(f"Usia: {usia} tahun")
else:
    print("❌ Input tidak valid! Masukkan angka saja.")
```

### 5. Nested if yang Terlalu Dalam

```python
# SULIT DIBACA — nested terlalu dalam (3+ level)
if penghasilan > 3000000:
    if usia >= 21:
        if not ada_tunggakan:
            if skor > 60:
                if tenor <= 36:
                    print("DISETUJUI")

# LEBIH BAIK — gunakan early return / kondisi gabungan
syarat_terpenuhi = (
    penghasilan > 3000000 and
    usia >= 21 and
    not ada_tunggakan and
    skor > 60 and
    tenor <= 36
)

if syarat_terpenuhi:
    print("DISETUJUI")
```

### 6. Membandingkan Float dengan `==`

```python
# BERBAHAYA — floating point tidak akurat
bunga = 0.1 + 0.2
if bunga == 0.3:       # False! Meskipun kelihatannya harus True
    print("Bunga 30%")

# BENAR — gunakan toleransi atau round()
if round(bunga, 10) == 0.3:
    print("Bunga 30%")
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep: Prediksi Output

Tanpa menjalankan kode, prediksi output dari program berikut:

```python
x = 15
y = 7

if x > 10 and y < 10:
    print("A")
    if x + y > 20:
        print("B")
    else:
        print("C")
elif x > 5 or y > 5:
    print("D")
else:
    print("E")

status = "besar" if x > y else "kecil"
print(f"x lebih {status} dari y")
```

Tuliskan urutan output yang dihasilkan dan jelaskan mengapa!

### Soal 2 — Sistem Kategori Risiko Kredit

Buat program Python untuk menentukan kategori risiko kredit nasabah berdasarkan tiga faktor:

**Aturan penilaian:**

| Faktor | Poin |
|---|---|
| Penghasilan >= 10 juta | +30 |
| Penghasilan 5–10 juta | +20 |
| Penghasilan < 5 juta | +10 |
| Usia 25–45 tahun | +20 |
| Usia di luar 25–45 | +10 |
| Tidak punya pinjaman aktif | +30 |
| Punya 1 pinjaman aktif | +20 |
| Punya 2+ pinjaman aktif | +10 |
| Rekening aktif > 2 tahun | +20 |
| Rekening aktif <= 2 tahun | +10 |

**Kategori berdasarkan total poin:**
- 80–100 → "Prime" → limit 100% penghasilan
- 60–79 → "Near Prime" → limit 60% penghasilan
- 40–59 → "Subprime" → limit 30% penghasilan
- < 40 → "Ditolak"

**Output yang diharapkan:**
```
=== Scoring Risiko Kredit ===
[... input nasabah ...]

Total skor   : 70 poin
Kategori     : Near Prime
Limit kredit : Rp 4.500.000 (60% dari Rp 7.500.000)
```

### Soal 3 — Studi Kasus Fintech: Cashback Tier & Biaya Layanan

**Skenario:** Kamu diminta membangun logika cashback untuk aplikasi e-wallet. Ketentuannya:

**Tier Member:**
- Member Basic (default)
- Member Silver: jika total transaksi bulan ini > Rp 1.000.000
- Member Gold: jika total transaksi bulan ini > Rp 5.000.000

**Cashback per transaksi:**
- Basic: 0% (tidak ada cashback)
- Silver: 3% per transaksi, maks Rp 25.000
- Gold: 5% per transaksi, maks Rp 75.000

**Biaya layanan:**
- Jika bayar ke merchant luar kota: + Rp 2.500
- Jika transaksi di atas Rp 5.000.000: gratis biaya layanan

**Program harus:**
1. Menerima input: total transaksi bulan ini, nominal transaksi saat ini, dan apakah merchant luar kota
2. Menentukan tier member otomatis
3. Menghitung cashback dan biaya layanan
4. Menampilkan rincian yang jelas

### Soal 4 — Tantangan: Kalkulator Konversi Mata Uang

Buat kalkulator konversi mata uang yang mendukung: IDR, USD, EUR, SGD.

Gunakan kurs hari ini (hardcode sebagai konstanta). Program harus:
- Tanya mata uang asal, mata uang tujuan, dan nominal
- Tampilkan hasil konversi dengan format yang rapi
- Tampilkan pesan error jika kode mata uang tidak dikenali
- Gunakan `if-elif-else` untuk menentukan kurs

---

## 📌 Ringkasan

**Input & Output:**
- `input()` selalu mengembalikan `str` — selalu konversi jika butuh angka
- f-string adalah cara terbaik format output: `f"Saldo: Rp {saldo:,}"`
- Format penting: `:,` (ribuan), `:.2f` (2 desimal), `:.1%` (persen)

**Percabangan:**
- `if kondisi:` → jalankan jika kondisi `True`
- `elif kondisi_lain:` → alternatif berikutnya (dicek berurutan)
- `else:` → dijalankan jika semua kondisi sebelumnya `False`
- Python eksekusi blok **pertama** yang kondisinya `True`, sisanya dilewati

**Urutan elif yang tepat:**
- Untuk range angka: urutan dari **paling ketat** (nilai tinggi/rendah) ke **paling umum**
- Untuk kategori: urutan dari **paling spesifik** ke **paling umum**

**Nested if:**
- Gunakan hanya jika logika memang berhierarki
- Maksimal 2–3 level; lebih dari itu pertimbangkan gabungkan kondisi dengan `and`/`or`

**Operator Ternary:**
- `nilai = "A" if skor >= 90 else "B"` — hanya untuk kasus dua pilihan sederhana
- Jangan nested ternary — susah dibaca

**Validasi Input:**
- Selalu validasi sebelum konversi: `if teks.isdigit(): angka = int(teks)`
- Jangan asumsikan user selalu input data yang valid

**Di Dunia Nyata Fintech:**
- Setiap keputusan kredit adalah `if-elif-else` yang kompleks
- Aturan bisnis berubah → logika percabangan berubah → kode harus modular
- Selalu tangani semua kemungkinan input, termasuk yang tidak terduga
