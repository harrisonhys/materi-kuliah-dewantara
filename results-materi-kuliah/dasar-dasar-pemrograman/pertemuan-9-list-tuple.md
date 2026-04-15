# Pertemuan 9: List & Tuple — Indexing, Slicing, Comprehension

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Membuat dan memanipulasi List dengan berbagai metode bawaan Python
- Mengakses elemen List menggunakan indexing (positif dan negatif) dan slicing
- Menggunakan List Comprehension untuk menulis kode yang lebih ringkas dan efisien
- Bekerja dengan Nested List untuk merepresentasikan data dua dimensi (matriks)
- Memahami Tuple — perbedaannya dengan List dan kapan harus menggunakannya
- Menerapkan List dan Tuple untuk menyimpan dan memproses data transaksi keuangan

---

## 📖 Pengantar (Hook)

Coba bayangkan kamu adalah backend engineer di sebuah dompet digital. Setiap hari ada jutaan transaksi masuk — transfer, top-up, belanja, tarik tunai. Semua data ini harus disimpan, difilter, diurutkan, dan dianalisis.

Pertanyaannya: bagaimana Python menyimpan data sebanyak itu secara terurut?

Jawabannya adalah **List**.

List di Python bukan sekadar "array" biasa. Dia bisa menampung tipe data campur-campur, bisa diubah kapan saja, punya puluhan metode siap pakai, dan punya syntax spesial (comprehension) yang bikin kode jadi elegan.

Dan kembarannya yang lebih kaku tapi lebih aman? **Tuple** — data yang sekali ditetapkan, tidak bisa diubah. Cocok untuk data yang memang tidak boleh berubah, seperti koordinat kantor pusat atau kode bank.

---

## 🧩 Konsep Utama

### List: Kontainer Data yang Fleksibel

List adalah koleksi data yang:
- **Terurut** (ordered) — urutan elemen dijaga
- **Bisa diubah** (mutable) — bisa tambah, hapus, ubah elemen
- **Mengizinkan duplikat** — nilai yang sama boleh muncul lebih dari sekali
- **Bisa campur tipe data** — `[1, "hello", True, 3.14]`

**Cara membuat List:**
```python
# Literal
transaksi = [500000, 1200000, 750000, 300000]

# List kosong
riwayat = []
riwayat_2 = list()

# List dari iterable
angka = list(range(1, 6))  # [1, 2, 3, 4, 5]
```

---

### Indexing

Setiap elemen punya **indeks** (nomor urut), dimulai dari 0.

```
transaksi = [500000, 1200000, 750000, 300000, 900000]
index        0       1        2       3       4
index (neg) -5      -4       -3      -2      -1
```

```python
print(transaksi[0])   # 500000  (pertama)
print(transaksi[-1])  # 900000  (terakhir)
print(transaksi[-2])  # 300000  (kedua dari belakang)
```

---

### Slicing

Ambil sebagian elemen dengan sintaks `list[start:stop:step]`.
- `start` = indeks mulai (inklusif), default 0
- `stop` = indeks berhenti (eksklusif), default akhir list
- `step` = langkah, default 1

```python
transaksi = [500000, 1200000, 750000, 300000, 900000, 650000]

print(transaksi[1:4])    # [1200000, 750000, 300000] — indeks 1,2,3
print(transaksi[:3])     # [500000, 1200000, 750000] — 3 pertama
print(transaksi[3:])     # [300000, 900000, 650000]  — dari indeks 3 ke akhir
print(transaksi[::2])    # [500000, 750000, 900000]  — loncat 2
print(transaksi[::-1])   # [650000, 900000, 300000, 750000, 1200000, 500000] — balik urutan
```

---

### Metode-Metode List

| Metode | Fungsi | Contoh |
|---|---|---|
| `append(x)` | Tambah x di akhir | `lst.append(100)` |
| `insert(i, x)` | Sisip x di indeks i | `lst.insert(0, 100)` |
| `remove(x)` | Hapus kemunculan pertama x | `lst.remove(100)` |
| `pop(i)` | Hapus & return elemen di indeks i | `lst.pop(-1)` |
| `sort()` | Urutkan ascending (in-place) | `lst.sort()` |
| `sort(reverse=True)` | Urutkan descending | `lst.sort(reverse=True)` |
| `sorted(lst)` | Return list baru yang terurut | `sorted(lst)` |
| `reverse()` | Balik urutan (in-place) | `lst.reverse()` |
| `index(x)` | Return indeks pertama x | `lst.index(100)` |
| `count(x)` | Hitung kemunculan x | `lst.count(100)` |
| `len(lst)` | Jumlah elemen | `len(lst)` |
| `sum(lst)` | Total jumlah | `sum(lst)` |
| `min(lst)` | Nilai terkecil | `min(lst)` |
| `max(lst)` | Nilai terbesar | `max(lst)` |
| `extend(lst2)` | Gabung dengan list lain | `lst.extend([4,5])` |
| `clear()` | Hapus semua elemen | `lst.clear()` |
| `copy()` | Buat salinan dangkal | `lst2 = lst.copy()` |

---

### List Comprehension

List Comprehension adalah cara ringkas membuat list baru dari list/iterable lain.

**Sintaks**: `[ekspresi for item in iterable if kondisi]`

```python
# Tanpa comprehension (cara lama)
hasil = []
for x in range(10):
    if x % 2 == 0:
        hasil.append(x ** 2)

# Dengan comprehension (cara elegan)
hasil = [x**2 for x in range(10) if x % 2 == 0]
# [0, 4, 16, 36, 64]
```

---

### Nested List (List 2D / Matriks)

```python
# Representasi tabel transaksi (baris = transaksi, kolom = [id, jumlah, status])
transaksi_db = [
    ["TXN001", 500000,  "sukses"],
    ["TXN002", 1200000, "sukses"],
    ["TXN003", 750000,  "gagal"],
    ["TXN004", 300000,  "sukses"],
]

# Akses elemen
print(transaksi_db[0])      # ["TXN001", 500000, "sukses"]
print(transaksi_db[0][1])   # 500000 (jumlah transaksi pertama)
print(transaksi_db[2][2])   # "gagal" (status transaksi ketiga)

# Iterasi nested list
for txn in transaksi_db:
    if txn[2] == "sukses":
        print(f"{txn[0]}: Rp {txn[1]:,}")
```

---

### Tuple: List yang Tidak Bisa Diubah

Tuple adalah koleksi yang:
- **Terurut** (ordered)
- **Tidak bisa diubah** (immutable) — tidak bisa tambah/hapus/ubah elemen setelah dibuat
- **Mengizinkan duplikat**
- **Lebih cepat** dari list untuk operasi read

```python
# Membuat tuple
koordinat_kantor = (-6.2088, 106.8456)   # (latitude, longitude)
kode_bank = ("014", "BCA")               # kode dan nama bank
record = ("USR001", "Budi", 25000000)    # immutable user record

# Akses (sama seperti list)
print(koordinat_kantor[0])    # -6.2088
print(kode_bank[-1])          # "BCA"

# Tuple unpacking — fitur keren!
lat, lon = koordinat_kantor
id_user, nama, saldo = record
print(f"{nama} punya saldo Rp {saldo:,}")

# Tuple tidak bisa diubah:
# koordinat_kantor[0] = 0  ← TypeError!
```

---

### Perbedaan List vs Tuple

| Aspek | List | Tuple |
|---|---|---|
| Sintaks | `[1, 2, 3]` | `(1, 2, 3)` |
| Mutable | Ya — bisa diubah | Tidak — immutable |
| Performa | Lebih lambat | Lebih cepat |
| Penggunaan memori | Lebih besar | Lebih kecil |
| Bisa jadi key dict | Tidak | Ya |
| Cocok untuk | Data yang berubah (riwayat transaksi) | Data tetap (koordinat, konfigurasi) |
| Metode bawaan | Banyak (append, sort, dll.) | Sedikit (count, index) |

---

## 🧠 Ilustrasi / Analogi

Bayangkan sistem kasir di minimarket:

| Konsep | Analogi Kasir |
|---|---|
| **List** | Struk belanja yang bisa dicoret dan ditambah itemnya kapan saja |
| **Tuple** | Nomor antrian yang sudah dicetak — tidak bisa diganti |
| **Indexing** | "Barang ke-3 di struk itu apa?" |
| **Slicing** | "Ambil 5 transaksi terakhir dari daftar" |
| **append()** | Tambahkan item baru ke struk |
| **sort()** | Urutkan item dari yang paling mahal |
| **List Comprehension** | "Dari 100 transaksi, berikan saya semua yang nilainya di atas Rp 500rb" — tanpa baca satu per satu |
| **Nested List** | Rak minimarket: baris 2, kolom 3 = Indomie |

---

## 💻 Contoh Teknis

### Contoh 1: Operasi Dasar List Transaksi

```python
# Simulasi riwayat transaksi (dalam Rupiah)
riwayat = [500000, 1200000, 750000, 300000, 900000]

# Statistik dasar
print(f"Jumlah transaksi : {len(riwayat)}")
print(f"Total            : Rp {sum(riwayat):,}")
print(f"Rata-rata        : Rp {sum(riwayat)/len(riwayat):,.0f}")
print(f"Terbesar         : Rp {max(riwayat):,}")
print(f"Terkecil         : Rp {min(riwayat):,}")

# Tambah transaksi baru
riwayat.append(650000)

# Hapus transaksi terakhir (misal dibatalkan)
dibatalkan = riwayat.pop()
print(f"\nTransaksi dibatalkan: Rp {dibatalkan:,}")

# Urutkan dari terbesar
riwayat_sorted = sorted(riwayat, reverse=True)
print(f"\nTop 3 transaksi terbesar:")
for i, nilai in enumerate(riwayat_sorted[:3], start=1):
    print(f"  {i}. Rp {nilai:,}")
```

---

### Contoh 2: List Comprehension untuk Filter

```python
transaksi = [
    {"id": "T001", "jumlah": 500000,  "tipe": "transfer"},
    {"id": "T002", "jumlah": 1500000, "tipe": "transfer"},
    {"id": "T003", "jumlah": 200000,  "tipe": "topup"},
    {"id": "T004", "jumlah": 800000,  "tipe": "transfer"},
    {"id": "T005", "jumlah": 50000,   "tipe": "topup"},
    {"id": "T006", "jumlah": 2000000, "tipe": "transfer"},
]

THRESHOLD = 1_000_000

# Filter transfer di atas threshold (mencurigakan untuk monitoring)
txn_besar = [
    t for t in transaksi
    if t["tipe"] == "transfer" and t["jumlah"] >= THRESHOLD
]

print("Transaksi transfer >= Rp 1.000.000:")
for t in txn_besar:
    print(f"  {t['id']}: Rp {t['jumlah']:,}")

# Ambil hanya nilai jumlahnya
nilai_saja = [t["jumlah"] for t in transaksi]
print(f"\nSemua nilai: {nilai_saja}")

# Konversi nilai ke dalam satuan ribu
nilai_ribu = [t["jumlah"] // 1000 for t in transaksi]
print(f"Dalam ribuan: {nilai_ribu}")
```

---

### Contoh 3: Nested List sebagai Matriks Data

```python
# Tabel: [bulan, pemasukan, pengeluaran] dalam jutaan
laporan_keuangan = [
    ["Januari",  85, 72],
    ["Februari", 90, 68],
    ["Maret",    78, 81],
    ["April",    95, 70],
    ["Mei",      88, 75],
]

print(f"{'Bulan':<12} {'Pemasukan':>12} {'Pengeluaran':>12} {'Profit':>10}")
print("-" * 50)

total_profit = 0
for baris in laporan_keuangan:
    bulan, masuk, keluar = baris  # tuple unpacking
    profit = masuk - keluar
    total_profit += profit
    status = "+" if profit > 0 else "-"
    print(f"{bulan:<12} Rp {masuk:>7} jt  Rp {keluar:>7} jt  {status}Rp {abs(profit)} jt")

print("-" * 50)
print(f"{'Total Profit':<35} Rp {total_profit} jt")

# Slicing: ambil data Q1 (3 bulan pertama)
q1 = laporan_keuangan[:3]
profit_q1 = sum(b[1] - b[2] for b in q1)
print(f"\nProfit Q1: Rp {profit_q1} jt")
```

---

### Contoh 4: Tuple untuk Data Tetap

```python
# Konfigurasi bank yang tidak boleh berubah
KODE_BANK = {
    "014": ("BCA", "Bank Central Asia"),
    "009": ("BNI", "Bank Negara Indonesia"),
    "002": ("BRI", "Bank Rakyat Indonesia"),
    "008": ("Mandiri", "Bank Mandiri"),
}

# Koordinat kantor cabang (tidak boleh diubah runtime)
CABANG_JAKARTA = (
    (-6.2088, 106.8456, "Kantor Pusat Jakarta"),
    (-6.1751, 106.8272, "Cabang Gambir"),
    (-6.2615, 106.7810, "Cabang Kebayoran"),
)

# Iterasi tuple of tuples
for lat, lon, nama in CABANG_JAKARTA:
    print(f"{nama}: ({lat}, {lon})")

# Tuple sebagai return value — praktik umum di Python
def info_rekening(nomor_rek):
    """Return (nama_pemilik, saldo, status) — immutable result."""
    # simulasi query database
    return ("Budi Santoso", 5_250_000, "aktif")

nama, saldo, status = info_rekening("1234567890")
print(f"\n{nama}: Rp {saldo:,} [{status}]")
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Sistem Monitoring Transaksi di Dompet Digital

**Perusahaan**: "PayNow" — aplikasi dompet digital dengan 2 juta pengguna aktif

**Masalah Bisnis**:
Tim fraud detection PayNow menerima laporan bahwa beberapa akun melakukan transaksi mencurigakan: nilai sangat besar dalam waktu singkat, atau melakukan puluhan transaksi kecil dalam satu menit (smurfing — teknik untuk menghindari deteksi AML).

Tim butuh sistem yang bisa:
1. Menyimpan riwayat transaksi per user dalam satu sesi
2. Menghitung statistik transaksi: rata-rata, total, min, max
3. Filter transaksi di atas threshold untuk monitoring
4. Deteksi pola smurfing: transaksi kecil yang jumlahnya banyak dalam satu sesi

**Dampak Bisnis**:
Tanpa monitoring ini, PayNow bisa kena sanksi dari OJK (Otoritas Jasa Keuangan) karena tidak memenuhi ketentuan AML (Anti-Money Laundering). Denda bisa mencapai miliaran rupiah, belum lagi reputasi rusak.

**Solusi Teknis**:

```python
# ──────────────────────────────────────────────────
# fraud_monitor.py — Sistem Monitoring Transaksi
# ──────────────────────────────────────────────────

THRESHOLD_BESAR     = 10_000_000   # Rp 10 juta
THRESHOLD_KECIL     = 500_000      # Rp 500 ribu
BATAS_TRANSAKSI_KECIL = 5          # max 5 transaksi kecil per sesi
BATAS_TOTAL_SESI    = 50_000_000   # Rp 50 juta per sesi

def analisis_transaksi(user_id, transaksi_list):
    """
    Analisis riwayat transaksi untuk deteksi fraud.
    transaksi_list: list of dict {id, jumlah, waktu}
    Return: dict berisi hasil analisis dan flag fraud
    """
    if not transaksi_list:
        return {"error": "Tidak ada transaksi"}

    # Ekstrak nilai jumlah saja menggunakan list comprehension
    nilai = [t["jumlah"] for t in transaksi_list]

    # Hitung statistik dasar
    total       = sum(nilai)
    rata_rata   = total / len(nilai)
    nilai_max   = max(nilai)
    nilai_min   = min(nilai)
    jumlah_txn  = len(nilai)

    # Filter transaksi besar (untuk laporan ke OJK)
    txn_besar = [
        t for t in transaksi_list
        if t["jumlah"] >= THRESHOLD_BESAR
    ]

    # Deteksi smurfing: banyak transaksi kecil
    txn_kecil = [
        t for t in transaksi_list
        if t["jumlah"] < THRESHOLD_KECIL
    ]

    # Urutkan transaksi dari terbesar
    transaksi_sorted = sorted(transaksi_list, key=lambda t: t["jumlah"], reverse=True)
    top_3 = transaksi_sorted[:3]

    # Tentukan flag fraud
    flags = []
    if total > BATAS_TOTAL_SESI:
        flags.append(f"TOTAL_MELEBIHI_BATAS (Rp {total:,})")
    if len(txn_kecil) >= BATAS_TRANSAKSI_KECIL:
        flags.append(f"POTENSI_SMURFING ({len(txn_kecil)} transaksi < Rp {THRESHOLD_KECIL:,})")
    if txn_besar:
        flags.append(f"TRANSAKSI_BESAR ({len(txn_besar)} transaksi >= Rp {THRESHOLD_BESAR:,})")

    return {
        "user_id"       : user_id,
        "jumlah_txn"    : jumlah_txn,
        "total"         : total,
        "rata_rata"     : rata_rata,
        "nilai_max"     : nilai_max,
        "nilai_min"     : nilai_min,
        "txn_besar"     : txn_besar,
        "txn_kecil"     : txn_kecil,
        "top_3"         : top_3,
        "fraud_flags"   : flags,
        "status"        : "SUSPECT" if flags else "AMAN",
    }


def cetak_laporan(hasil):
    """Cetak laporan analisis transaksi."""
    print(f"\n{'='*55}")
    print(f" LAPORAN MONITORING — User: {hasil['user_id']}")
    print(f"{'='*55}")
    print(f" Jumlah transaksi  : {hasil['jumlah_txn']}")
    print(f" Total             : Rp {hasil['total']:,}")
    print(f" Rata-rata         : Rp {hasil['rata_rata']:,.0f}")
    print(f" Terbesar          : Rp {hasil['nilai_max']:,}")
    print(f" Terkecil          : Rp {hasil['nilai_min']:,}")

    print(f"\n Top 3 Transaksi Terbesar:")
    for t in hasil['top_3']:
        print(f"   - {t['id']}: Rp {t['jumlah']:,}")

    if hasil['txn_besar']:
        print(f"\n Transaksi >= Rp 10 juta (wajib lapor OJK):")
        for t in hasil['txn_besar']:
            print(f"   - {t['id']}: Rp {t['jumlah']:,}")

    print(f"\n Status : [{hasil['status']}]")
    if hasil['fraud_flags']:
        print(" Flags  :")
        for flag in hasil['fraud_flags']:
            print(f"   ⚠  {flag}")
    print(f"{'='*55}")


# ────── Simulasi Data ──────

riwayat_user_A = [
    {"id": "T001", "jumlah": 450000,  "waktu": "10:01"},
    {"id": "T002", "jumlah": 480000,  "waktu": "10:03"},
    {"id": "T003", "jumlah": 420000,  "waktu": "10:05"},
    {"id": "T004", "jumlah": 490000,  "waktu": "10:07"},
    {"id": "T005", "jumlah": 460000,  "waktu": "10:09"},
    {"id": "T006", "jumlah": 470000,  "waktu": "10:11"},
]

riwayat_user_B = [
    {"id": "T101", "jumlah": 15000000, "waktu": "14:00"},
    {"id": "T102", "jumlah": 8000000,  "waktu": "14:05"},
    {"id": "T103", "jumlah": 20000000, "waktu": "14:10"},
]

hasil_A = analisis_transaksi("USR-001", riwayat_user_A)
hasil_B = analisis_transaksi("USR-002", riwayat_user_B)

cetak_laporan(hasil_A)
cetak_laporan(hasil_B)
```

**Output:**
```
=======================================================
 LAPORAN MONITORING — User: USR-001
=======================================================
 Jumlah transaksi  : 6
 Total             : Rp 2,770,000
 Rata-rata         : Rp 461,667
 Terbesar          : Rp 490,000
 Terkecil          : Rp 420,000

 Top 3 Transaksi Terbesar:
   - T005: Rp 490,000
   - T002: Rp 480,000
   - T006: Rp 470,000

 Status : [SUSPECT]
 Flags  :
   ⚠  POTENSI_SMURFING (6 transaksi < Rp 500,000)
=======================================================

=======================================================
 LAPORAN MONITORING — User: USR-002
=======================================================
 ...
 Status : [SUSPECT]
 Flags  :
   ⚠  TOTAL_MELEBIHI_BATAS (Rp 43,000,000)
   ⚠  TRANSAKSI_BESAR (2 transaksi >= Rp 10,000,000)
=======================================================
```

**Hasil Bisnis**: Sistem otomatis mendeteksi pola smurfing (User A) dan transaksi besar mencurigakan (User B), sehingga tim compliance bisa langsung follow-up tanpa review manual ratusan data.

---

## 📊 Visualisasi

### Cara Kerja Indexing & Slicing

```
List: transaksi = [500000, 1200000, 750000, 300000, 900000]

Indeks positif:    0        1        2       3       4
Indeks negatif:   -5       -4       -3      -2      -1

transaksi[0]     → 500000
transaksi[-1]    → 900000
transaksi[1:3]   → [1200000, 750000]  (stop=3 tidak ikut)
transaksi[::-1]  → [900000, 300000, 750000, 1200000, 500000]
```

### Alur List Comprehension

```
Transaksi biasa (for loop):                  Comprehension:
────────────────────────                     ──────────────
hasil = []                                   hasil = [
for t in transaksi:               ──────▶       t["jumlah"]
    if t["jumlah"] > 1_000_000:              for t in transaksi
        hasil.append(t["jumlah"])            if t["jumlah"] > 1_000_000
                                             ]
```

### Kapan Pakai List vs Tuple?

```
Data transaksi (berubah terus)    → List   [500000, 750000, 900000]
Koordinat kantor (tetap)          → Tuple  (-6.208, 106.845)
Riwayat login user                → List   ["10:01", "14:30", "20:15"]
Konfigurasi database              → Tuple  ("localhost", 5432, "mydb")
Daftar produk di cart             → List   ["Indomie", "Susu", "Roti"]
Return banyak nilai dari fungsi   → Tuple  (True, "Sukses", 200)
```

---

## ⚠️ Kesalahan Umum

**1. IndexError karena salah hitung indeks**
```python
data = [10, 20, 30]
print(data[3])   # IndexError! Indeks valid: 0, 1, 2

# Selalu cek panjang list dulu
if len(data) > 3:
    print(data[3])
# Atau pakai try-except
try:
    print(data[10])
except IndexError:
    print("Indeks tidak valid")
```

**2. Lupa bahwa `sort()` mengubah list asli (in-place)**
```python
transaksi = [500, 300, 700, 100]

# HATI-HATI: sort() mengubah list asli
transaksi.sort()
print(transaksi)  # [100, 300, 500, 700] — list ASLI berubah!

# Jika ingin list asli tetap aman, gunakan sorted()
transaksi_asli = [500, 300, 700, 100]
transaksi_sorted = sorted(transaksi_asli)  # return list BARU
print(transaksi_asli)   # [500, 300, 700, 100] — tidak berubah
print(transaksi_sorted) # [100, 300, 500, 700]
```

**3. Copy list dengan cara yang salah (shallow copy)**
```python
asli = [1, 2, 3, 4]

# SALAH: ini bukan copy, ini alias!
salinan = asli
salinan.append(999)
print(asli)      # [1, 2, 3, 4, 999] — asli ikut berubah!

# BENAR: gunakan .copy() atau slicing
salinan = asli.copy()   # atau: salinan = asli[:]
salinan.append(999)
print(asli)      # [1, 2, 3, 4] — aman
```

**4. Mencoba ubah elemen Tuple**
```python
koordinat = (-6.2088, 106.8456)
koordinat[0] = 0  # TypeError: 'tuple' object does not support item assignment

# Jika perlu ubah, konversi dulu ke list
koordinat_list = list(koordinat)
koordinat_list[0] = -6.3000
koordinat = tuple(koordinat_list)
```

**5. `remove()` hanya hapus kemunculan PERTAMA**
```python
data = [1, 2, 3, 2, 4, 2]
data.remove(2)
print(data)  # [1, 3, 2, 4, 2] — hanya 2 pertama yang dihapus!

# Hapus SEMUA nilai tertentu dengan comprehension:
data = [x for x in data if x != 2]
print(data)  # [1, 3, 4]
```

**6. Slicing tidak raise IndexError**
```python
data = [1, 2, 3]
print(data[10:20])  # [] — bukan error, hanya list kosong
print(data[:100])   # [1, 2, 3] — aman, hanya ambil yang ada
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

Perhatikan kode berikut:
```python
angka = [5, 3, 8, 1, 9, 2, 7]
```

**Tanpa menjalankan kode**, tentukan output dari:
- a) `angka[2]`
- b) `angka[-3]`
- c) `angka[1:5]`
- d) `angka[::2]`
- e) `angka[::-1]`
- f) Setelah `angka.sort()`, apa isi `angka`?
- g) `[x**2 for x in angka if x > 5]` (gunakan list setelah sort)

---

### Soal 2 — Praktik: Analisis Nilai Mahasiswa

Buat program untuk menganalisis nilai ujian mahasiswa:

```python
nilai_mahasiswa = [78, 85, 92, 67, 88, 55, 90, 73, 81, 95]
```

**Program harus:**
1. Tampilkan nilai tertinggi, terendah, dan rata-rata
2. Gunakan list comprehension untuk mendapatkan nilai yang **lulus** (>= 75)
3. Gunakan list comprehension untuk mendapatkan nilai yang **tidak lulus** (< 75)
4. Tampilkan persentase kelulusan
5. Urutkan nilai dari tertinggi dan tampilkan **ranking top 3**

---

### Soal 3 — Studi Kasus Fintech

**Skenario**: Kamu adalah backend developer di "CashFlow" — aplikasi manajemen keuangan personal. Fitur baru yang diminta: **Laporan Keuangan Bulanan**.

**Data yang tersedia** (simpan sebagai nested list):
```python
transaksi_april = [
    # [tanggal, kategori, deskripsi, jumlah, tipe]
    ["01/04", "makan",    "Warung Padang",     35000, "keluar"],
    ["02/04", "transport","Grab ke kantor",    25000, "keluar"],
    ["05/04", "gaji",     "Gaji bulan April", 8000000,"masuk"],
    ["07/04", "makan",    "KFC",               85000, "keluar"],
    ["10/04", "hiburan",  "Netflix",          186000, "keluar"],
    ["12/04", "makan",    "Mie Ayam",          20000, "keluar"],
    ["15/04", "transport","Bensin motor",      60000, "keluar"],
    ["20/04", "belanja",  "Indomaret",        150000, "keluar"],
    ["25/04", "freelance","Project website", 2500000, "masuk"],
    ["28/04", "makan",    "Pizza Hut",        200000, "keluar"],
]
```

**Tugas:**
1. Hitung total **pemasukan** dan total **pengeluaran** menggunakan list comprehension
2. Hitung **net cash flow** (pemasukan - pengeluaran)
3. Buat ringkasan pengeluaran per **kategori** (berapa total per kategori)
4. Tampilkan **5 transaksi pengeluaran terbesar**
5. Filter semua transaksi dengan tipe "keluar" yang nilainya di atas Rp 50.000

---

## 📌 Ringkasan

- **List** `[a, b, c]` — ordered, mutable, boleh duplikat
- **Tuple** `(a, b, c)` — ordered, immutable, cocok untuk data tetap
- **Indexing**: mulai dari `0`, negatif dari `-1` (akhir)
- **Slicing**: `lst[start:stop:step]` — stop tidak ikut
- **`lst[::-1]`** — membalik urutan list
- **Metode penting List**: `append`, `insert`, `remove`, `pop`, `sort`, `sorted`, `index`, `count`, `extend`, `copy`
- **`sort()` vs `sorted()`**: sort mengubah list asli, sorted return list baru
- **List Comprehension**: `[expr for x in lst if kondisi]` — lebih cepat dari for loop
- **Nested List**: akses dengan dua indeks `lst[i][j]`
- **Tuple unpacking**: `a, b, c = (1, 2, 3)` — elegan dan pythonic
- **Jangan** assign list dengan `=` jika mau copy — gunakan `.copy()` atau `[:]`
- **`remove()`** hanya hapus kemunculan pertama
- **Slicing** tidak akan raise IndexError walau indeks melebihi panjang list
