# Pertemuan 10: Dictionary & Set

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Membuat dan mengakses Dictionary menggunakan key-value pair
- Menggunakan metode-metode Dictionary (get, keys, values, items, update, pop) dengan tepat
- Menulis Dictionary Comprehension untuk transformasi data yang efisien
- Membuat dan menavigasi Nested Dictionary untuk data terstruktur kompleks
- Memahami Set dan menggunakannya untuk operasi himpunan (union, intersection, difference)
- Menerapkan Dictionary dan Set dalam skenario sistem perbankan dan deteksi fraud

---

## 📖 Pengantar (Hook)

Bayangkan kamu bekerja di tim engineering sebuah bank digital. Suatu hari, ada laporan dari tim keamanan: sistem mendeteksi ribuan request login dalam waktu 10 menit dari berbagai IP address. Beberapa IP ternyata sudah pernah dilaporkan sebagai sumber serangan sebelumnya.

Pertanyaan: bagaimana kamu cepat cek apakah suatu IP sudah ada di daftar hitam? Bagaimana kamu simpan data profil nasabah dengan cepat bisa dicari berdasarkan nomor rekening?

Jawabannya ada dua struktur data yang harus kamu kuasai:
- **Dictionary** — untuk menyimpan data terstruktur yang dicari berdasarkan key (seperti kamus, cari kata → langsung ketemu artinya)
- **Set** — untuk menyimpan kumpulan nilai unik dan operasi himpunan (seperti filter duplikasi, cek keanggotaan dengan sangat cepat)

Dua struktur ini adalah senjata utama backend developer untuk mengelola data yang cepat, efisien, dan terstruktur.

---

## 🧩 Konsep Utama

### Dictionary: Data Key-Value

Dictionary adalah koleksi yang menyimpan pasangan **key → value**. Bayangkan seperti kamus beneran: kamu cari kata (key), langsung ketemu artinya (value).

Karakteristik Dictionary:
- **Key harus unik** — tidak boleh ada dua key yang sama
- **Value bisa apa saja** — string, int, list, bahkan dict lain
- **Terurut** — sejak Python 3.7+, urutan insert dijaga
- **Mutable** — bisa ditambah, diubah, dihapus

**Cara membuat Dictionary:**
```python
# Literal
nasabah = {
    "nama"    : "Budi Santoso",
    "rekening": "1234567890",
    "saldo"   : 5_250_000,
    "status"  : "aktif"
}

# Dict kosong
data = {}
data2 = dict()

# Dari pasangan key-value
data3 = dict(nama="Ani", saldo=3000000)

# Dari list of tuples
data4 = dict([("nama", "Cici"), ("saldo", 7500000)])
```

---

### Mengakses dan Memodifikasi Dictionary

```python
nasabah = {"nama": "Budi", "saldo": 5_250_000, "status": "aktif"}

# Akses nilai
print(nasabah["nama"])           # "Budi"
print(nasabah.get("saldo"))      # 5250000
print(nasabah.get("email", "Tidak ada"))  # "Tidak ada" (default jika key tidak ada)

# Tambah atau ubah nilai
nasabah["email"] = "budi@email.com"   # tambah key baru
nasabah["saldo"] = 6_000_000          # ubah nilai existing

# Cek keberadaan key
if "email" in nasabah:
    print("Email tersedia")

# Hapus
del nasabah["email"]              # hapus key tertentu
nilai = nasabah.pop("status")     # hapus & ambil nilainya
print(nilai)                      # "aktif"
```

---

### Metode-Metode Dictionary

| Metode | Fungsi | Contoh |
|---|---|---|
| `d.get(key)` | Ambil value, return None jika tidak ada | `d.get("nama")` |
| `d.get(key, default)` | Ambil value, return default jika tidak ada | `d.get("umur", 0)` |
| `d.keys()` | Return semua key | `list(d.keys())` |
| `d.values()` | Return semua value | `list(d.values())` |
| `d.items()` | Return semua pasangan (key, value) | `for k, v in d.items()` |
| `d.update(d2)` | Gabung/update dengan dict lain | `d.update({"saldo": 100})` |
| `d.pop(key)` | Hapus & return value | `d.pop("status")` |
| `d.pop(key, default)` | Hapus, return default jika tidak ada | `d.pop("x", None)` |
| `d.setdefault(key, val)` | Set value jika key belum ada | `d.setdefault("poin", 0)` |
| `d.clear()` | Hapus semua | `d.clear()` |
| `d.copy()` | Salinan dangkal | `d2 = d.copy()` |
| `len(d)` | Jumlah pasangan key-value | `len(d)` |

---

### Iterasi Dictionary

```python
nasabah = {"nama": "Budi", "saldo": 5250000, "status": "aktif"}

# Iterasi key
for key in nasabah:
    print(key)

# Iterasi value
for value in nasabah.values():
    print(value)

# Iterasi key dan value sekaligus (paling umum)
for key, value in nasabah.items():
    print(f"{key}: {value}")
```

---

### Dictionary Comprehension

Sintaks: `{key_expr: value_expr for item in iterable if kondisi}`

```python
# Dari list ke dict
nama_list    = ["Budi", "Ani", "Cici"]
saldo_list   = [5000000, 3000000, 7500000]

# Zip dua list menjadi dict
nasabah_dict = {nama: saldo for nama, saldo in zip(nama_list, saldo_list)}
# {'Budi': 5000000, 'Ani': 3000000, 'Cici': 7500000}

# Filter dan transformasi
saldo_tinggi = {
    nama: saldo
    for nama, saldo in nasabah_dict.items()
    if saldo >= 5_000_000
}
# {'Budi': 5000000, 'Cici': 7500000}

# Transformasi value
saldo_juta = {nama: saldo / 1_000_000 for nama, saldo in nasabah_dict.items()}
# {'Budi': 5.0, 'Ani': 3.0, 'Cici': 7.5}
```

---

### Nested Dictionary

Dictionary yang valuenya juga Dictionary — sangat umum untuk data terstruktur kompleks.

```python
database_nasabah = {
    "USR001": {
        "nama"     : "Budi Santoso",
        "rekening" : "1234567890",
        "saldo"    : 5_250_000,
        "alamat"   : {
            "kota"     : "Jakarta",
            "provinsi" : "DKI Jakarta",
            "kodepos"  : "10110"
        },
        "transaksi_terakhir": ["TXN001", "TXN002", "TXN003"]
    },
    "USR002": {
        "nama"     : "Ani Wijaya",
        "rekening" : "0987654321",
        "saldo"    : 3_000_000,
        "alamat"   : {
            "kota"     : "Bandung",
            "provinsi" : "Jawa Barat",
            "kodepos"  : "40115"
        },
        "transaksi_terakhir": ["TXN004", "TXN005"]
    }
}

# Akses nested
print(database_nasabah["USR001"]["nama"])              # "Budi Santoso"
print(database_nasabah["USR001"]["alamat"]["kota"])    # "Jakarta"
print(database_nasabah["USR001"]["transaksi_terakhir"][0])  # "TXN001"

# Update nested value
database_nasabah["USR001"]["saldo"] += 500_000
database_nasabah["USR001"]["alamat"]["kota"] = "Depok"

# Akses aman dengan .get() bertingkat
saldo = database_nasabah.get("USR999", {}).get("saldo", 0)
print(saldo)  # 0 — tidak error walau USR999 tidak ada
```

---

### Set: Koleksi Nilai Unik

Set adalah koleksi yang:
- **Tidak memiliki duplikat** — otomatis hapus nilai yang sama
- **Tidak terurut** (unordered) — tidak ada jaminan urutan
- **Tidak bisa diakses dengan indeks**
- **Mutable** (set biasa) atau **Immutable** (frozenset)
- **Pengecekan keanggotaan sangat cepat** — O(1) vs O(n) pada list

```python
# Membuat Set
ip_blacklist = {"192.168.1.1", "10.0.0.5", "172.16.0.100"}
ip_blacklist2 = set(["192.168.1.1", "10.0.0.5", "10.0.0.5"])  # duplikat otomatis hilang

# Set kosong — HARUS pakai set(), bukan {}
kosong = set()  # {} membuat dict kosong, bukan set!

# Tambah dan hapus
ip_blacklist.add("203.0.113.50")
ip_blacklist.remove("10.0.0.5")      # Error jika tidak ada
ip_blacklist.discard("10.0.0.5")     # Aman — tidak error jika tidak ada

# Cek keanggotaan (sangat cepat!)
if "192.168.1.1" in ip_blacklist:
    print("IP ini diblokir!")
```

---

### Operasi Himpunan pada Set

```python
set_A = {1, 2, 3, 4, 5}
set_B = {4, 5, 6, 7, 8}

# Union (gabungan): semua elemen dari A dan B
print(set_A | set_B)            # {1, 2, 3, 4, 5, 6, 7, 8}
print(set_A.union(set_B))       # sama

# Intersection (irisan): elemen yang ada di A DAN B
print(set_A & set_B)            # {4, 5}
print(set_A.intersection(set_B))  # sama

# Difference (selisih): elemen di A tapi TIDAK di B
print(set_A - set_B)            # {1, 2, 3}
print(set_A.difference(set_B))  # sama

# Symmetric Difference: elemen di A atau B tapi TIDAK keduanya
print(set_A ^ set_B)                      # {1, 2, 3, 6, 7, 8}
print(set_A.symmetric_difference(set_B))  # sama

# Subset & Superset
print({4, 5}.issubset(set_A))    # True — {4,5} ada semua di set_A
print(set_A.issuperset({4, 5}))  # True — set_A mengandung {4,5}
print(set_A.isdisjoint({9, 10})) # True — tidak ada irisan
```

---

### frozenset: Set yang Immutable

```python
# frozenset tidak bisa diubah setelah dibuat
ip_whitelist = frozenset({"10.0.0.1", "127.0.0.1", "192.168.0.1"})

# Bisa dipakai sebagai key dictionary (karena hashable)
aturan = {
    ip_whitelist: "IZINKAN",
}

# ip_whitelist.add("x")  ← AttributeError! Tidak bisa diubah
```

---

## 🧠 Ilustrasi / Analogi

### Dictionary = Kamus / Buku Telepon

| Konsep | Analogi Kamus/Buku Telepon |
|---|---|
| Dictionary | Buku telepon: cari nama → langsung dapat nomor |
| Key | Nama orang yang dicari |
| Value | Nomor teleponnya |
| `d["Budi"]` | Buka halaman "B", cari "Budi" |
| `d.get("Budi", "tidak ada")` | Cari "Budi", kalau tidak ada bilang "tidak ada" |
| `d["Budi"] = "081234"` | Tambahkan atau update nomor Budi di buku |
| `del d["Budi"]` | Coret Budi dari buku telepon |
| Nested Dict | Buku telepon dengan tab per kota, per nama |

### Set = Kantong Kelereng Tanpa Duplikat

| Konsep | Analogi Kelereng |
|---|---|
| Set | Kantong kelereng — tidak bisa ada dua kelereng identik |
| `add()` | Masukkan kelereng baru |
| `remove()` | Ambil kelereng tertentu |
| `A | B` (Union) | Gabung dua kantong, singkirkan duplikat |
| `A & B` (Intersection) | Ambil hanya kelereng yang ada di KEDUA kantong |
| `A - B` (Difference) | Kantong A dikurangi isi kantong B |
| `in` | "Apakah kelereng merah ini ada di kantong?" |
| frozenset | Kantong yang dikunci — tidak bisa tambah/kurangi |

---

## 💻 Contoh Teknis

### Contoh 1: Profil Nasabah Lengkap

```python
def buat_profil_nasabah(nama, rekening, saldo_awal):
    """Buat profil nasabah baru."""
    return {
        "nama"          : nama,
        "rekening"      : rekening,
        "saldo"         : saldo_awal,
        "status"        : "aktif",
        "jumlah_transaksi": 0,
        "riwayat"       : []
    }

def transfer(profil_pengirim, profil_penerima, jumlah, keterangan=""):
    """
    Proses transfer antar nasabah.
    Return: (sukses, pesan)
    """
    if profil_pengirim["status"] != "aktif":
        return False, "Akun pengirim tidak aktif"
    if profil_pengirim["saldo"] < jumlah:
        return False, f"Saldo tidak cukup. Saldo: Rp {profil_pengirim['saldo']:,}"
    if jumlah <= 0:
        return False, "Jumlah transfer harus positif"

    # Proses transfer
    profil_pengirim["saldo"] -= jumlah
    profil_penerima["saldo"] += jumlah

    # Catat riwayat
    from datetime import datetime
    waktu = datetime.now().strftime("%d/%m/%Y %H:%M")

    profil_pengirim["riwayat"].append({
        "waktu"  : waktu,
        "tipe"   : "debet",
        "jumlah" : jumlah,
        "ke"     : profil_penerima["nama"],
        "ket"    : keterangan
    })
    profil_penerima["riwayat"].append({
        "waktu"  : waktu,
        "tipe"   : "kredit",
        "jumlah" : jumlah,
        "dari"   : profil_pengirim["nama"],
        "ket"    : keterangan
    })

    profil_pengirim["jumlah_transaksi"] += 1
    profil_penerima["jumlah_transaksi"] += 1

    return True, f"Transfer Rp {jumlah:,} berhasil"


# Simulasi
budi = buat_profil_nasabah("Budi Santoso", "1234567890", 10_000_000)
ani  = buat_profil_nasabah("Ani Wijaya",   "0987654321",  5_000_000)

sukses, pesan = transfer(budi, ani, 2_000_000, "Bayar utang")
print(pesan)

print(f"\nSaldo Budi: Rp {budi['saldo']:,}")
print(f"Saldo Ani : Rp {ani['saldo']:,}")

print(f"\nRiwayat Budi:")
for r in budi["riwayat"]:
    print(f"  [{r['waktu']}] {r['tipe'].upper()} Rp {r['jumlah']:,} → {r.get('ke', r.get('dari'))}")
```

---

### Contoh 2: Dictionary Comprehension untuk Laporan

```python
# Data transaksi per kategori
transaksi = [
    {"kategori": "transfer", "jumlah": 2_000_000},
    {"kategori": "topup",    "jumlah":   500_000},
    {"kategori": "transfer", "jumlah": 1_500_000},
    {"kategori": "belanja",  "jumlah":   750_000},
    {"kategori": "topup",    "jumlah": 1_000_000},
    {"kategori": "belanja",  "jumlah":   300_000},
    {"kategori": "transfer", "jumlah": 3_000_000},
]

# Hitung total per kategori dengan Dictionary
total_per_kategori = {}
for t in transaksi:
    kategori = t["kategori"]
    total_per_kategori[kategori] = total_per_kategori.get(kategori, 0) + t["jumlah"]

print("Total per kategori:")
for kat, total in sorted(total_per_kategori.items(), key=lambda x: x[1], reverse=True):
    print(f"  {kat:<12}: Rp {total:,}")

# Dict comprehension: hitung rata-rata per kategori
jumlah_per_kat = {}
count_per_kat  = {}
for t in transaksi:
    k = t["kategori"]
    jumlah_per_kat[k] = jumlah_per_kat.get(k, 0) + t["jumlah"]
    count_per_kat[k]  = count_per_kat.get(k, 0) + 1

rata_per_kat = {
    kat: jumlah_per_kat[kat] / count_per_kat[kat]
    for kat in jumlah_per_kat
}

print("\nRata-rata per kategori:")
for kat, rata in rata_per_kat.items():
    print(f"  {kat:<12}: Rp {rata:,.0f}")
```

---

### Contoh 3: Set untuk Deteksi Duplikasi

```python
# Deteksi transaksi duplikat berdasarkan ID
def cek_duplikasi(transaksi_baru, transaksi_tersimpan_set):
    """
    Cek apakah transaksi sudah pernah diproses.
    Menggunakan Set untuk pengecekan O(1).
    """
    return transaksi_baru in transaksi_tersimpan_set

# Simulasi
transaksi_tersimpan = {"TXN001", "TXN002", "TXN003", "TXN004"}

txn_baru = ["TXN005", "TXN002", "TXN006", "TXN003", "TXN007"]

for txn in txn_baru:
    if cek_duplikasi(txn, transaksi_tersimpan):
        print(f"[DITOLAK] {txn} sudah diproses sebelumnya — kemungkinan duplikat!")
    else:
        print(f"[DITERIMA] {txn} diproses...")
        transaksi_tersimpan.add(txn)
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Sistem Keamanan Anti-Fraud di Bank Digital

**Perusahaan**: "NeoBank" — bank digital dengan 3 juta nasabah

**Masalah Bisnis**:
Tim keamanan NeoBank mengidentifikasi tiga jenis ancaman:
1. **Credential Stuffing**: Hacker mencoba login dengan kombinasi username-password yang bocor dari breach di tempat lain. Mereka pakai ribuan IP berbeda, tapi banyak yang sudah ada di daftar hitam.
2. **Replay Attack**: Request transaksi yang sama dikirimkan berulang kali (misalnya lewat bug di app) — menyebabkan double debit yang merugikan nasabah.
3. **Data Enrichment**: Tim analis butuh profil nasabah yang lengkap dan terstruktur untuk scoring kredit, termasuk melihat riwayat transaksi dan kategori pengeluaran.

Semuanya butuh kombinasi Dictionary dan Set.

**Dampak Bisnis**:
- Replay Attack tanpa proteksi bisa menyebabkan transfer ganda, kerugian langsung ke nasabah dan reputasi bank
- Tanpa blacklist IP yang cepat dicek, bot bisa lakukan 10.000 percobaan login per menit
- Data nasabah yang tidak terstruktur memperlambat proses kredit scoring → nasabah frustrasi, konversi turun

**Solusi Teknis**:

```python
# ──────────────────────────────────────────────────────────
# security_system.py — Sistem Keamanan NeoBank
# ──────────────────────────────────────────────────────────

from datetime import datetime, timedelta

# ────────────────────────────────────────
# 1. IP BLACKLIST MANAGEMENT (menggunakan Set)
# ────────────────────────────────────────

class IPBlacklistManager:
    def __init__(self):
        # Set untuk O(1) lookup
        self._blacklist    = set()
        self._whitelist    = frozenset({"127.0.0.1", "10.0.0.1"})  # internal IPs
        self._hit_counter  = {}  # dict: ip → jumlah hit

    def tambah_ke_blacklist(self, ip):
        if ip not in self._whitelist:
            self._blacklist.add(ip)
            print(f"[BLACKLIST] IP {ip} ditambahkan ke daftar hitam")

    def is_blocked(self, ip):
        return ip in self._blacklist

    def catat_gagal_login(self, ip, maks_gagal=5):
        """
        Otomatis blacklist IP jika gagal login >= maks_gagal kali.
        """
        self._hit_counter[ip] = self._hit_counter.get(ip, 0) + 1
        if self._hit_counter[ip] >= maks_gagal:
            self.tambah_ke_blacklist(ip)
            return True  # diblokir
        return False

    def hapus_dari_blacklist(self, ip):
        self._blacklist.discard(ip)

    def statistik(self):
        return {
            "total_blocked_ip"  : len(self._blacklist),
            "total_ip_monitored": len(self._hit_counter),
            "top_offenders"     : sorted(
                self._hit_counter.items(),
                key=lambda x: x[1],
                reverse=True
            )[:5]
        }


# ────────────────────────────────────────
# 2. REPLAY ATTACK PREVENTION (menggunakan Set)
# ────────────────────────────────────────

class ReplayProtector:
    def __init__(self, window_menit=10):
        self._processed_txn   = set()    # ID transaksi yang sudah diproses
        self._txn_timestamp   = {}       # dict: txn_id → waktu proses
        self._window          = timedelta(minutes=window_menit)

    def _bersihkan_expired(self):
        """Hapus transaksi yang sudah lewat window time."""
        sekarang = datetime.now()
        expired  = {
            txn_id
            for txn_id, waktu in self._txn_timestamp.items()
            if sekarang - waktu > self._window
        }
        self._processed_txn  -= expired
        for txn_id in expired:
            del self._txn_timestamp[txn_id]

    def cek_dan_tandai(self, txn_id):
        """
        Return True jika transaksi BARU (aman diproses).
        Return False jika transaksi DUPLIKAT (tolak!).
        """
        self._bersihkan_expired()

        if txn_id in self._processed_txn:
            return False  # duplikat!

        self._processed_txn.add(txn_id)
        self._txn_timestamp[txn_id] = datetime.now()
        return True


# ────────────────────────────────────────
# 3. PROFIL NASABAH (menggunakan Nested Dictionary)
# ────────────────────────────────────────

class ProfilNasabah:
    def __init__(self):
        self._db = {}  # dict: user_id → profil

    def daftarkan(self, user_id, nama, rekening, saldo_awal=0):
        self._db[user_id] = {
            "identitas": {
                "user_id"  : user_id,
                "nama"     : nama,
                "rekening" : rekening,
                "status"   : "aktif"
            },
            "keuangan": {
                "saldo"        : saldo_awal,
                "total_kredit" : 0,
                "total_debet"  : 0,
                "limit_harian" : 20_000_000
            },
            "aktivitas": {
                "login_terakhir"    : None,
                "jumlah_transaksi"  : 0,
                "kategori_pengeluaran": {}  # dict: kategori → total
            },
            "keamanan": {
                "ip_login_dikenal" : set(),
                "gagal_login"      : 0
            }
        }

    def get_nasabah(self, user_id):
        return self._db.get(user_id)

    def catat_transaksi(self, user_id, jumlah, tipe, kategori="umum"):
        """
        tipe: 'kredit' (masuk) atau 'debet' (keluar)
        """
        profil = self._db.get(user_id)
        if not profil:
            return False, "User tidak ditemukan"

        keu = profil["keuangan"]
        akt = profil["aktivitas"]

        if tipe == "debet":
            if keu["saldo"] < jumlah:
                return False, "Saldo tidak cukup"
            if jumlah > keu["limit_harian"]:
                return False, f"Melebihi limit harian Rp {keu['limit_harian']:,}"
            keu["saldo"]       -= jumlah
            keu["total_debet"] += jumlah
            # Update pengeluaran per kategori
            akt["kategori_pengeluaran"][kategori] = (
                akt["kategori_pengeluaran"].get(kategori, 0) + jumlah
            )
        else:  # kredit
            keu["saldo"]        += jumlah
            keu["total_kredit"] += jumlah

        akt["jumlah_transaksi"] += 1
        return True, "Berhasil"

    def laporan_nasabah(self, user_id):
        profil = self._db.get(user_id)
        if not profil:
            return "User tidak ditemukan"

        ident = profil["identitas"]
        keu   = profil["keuangan"]
        akt   = profil["aktivitas"]

        lines = [
            f"\n{'='*50}",
            f" PROFIL NASABAH",
            f"{'='*50}",
            f" Nama       : {ident['nama']}",
            f" Rekening   : {ident['rekening']}",
            f" Status     : {ident['status'].upper()}",
            f"\n Keuangan:",
            f"   Saldo       : Rp {keu['saldo']:,}",
            f"   Total Masuk : Rp {keu['total_kredit']:,}",
            f"   Total Keluar: Rp {keu['total_debet']:,}",
            f"   Jml Transaksi: {akt['jumlah_transaksi']}",
        ]

        if akt["kategori_pengeluaran"]:
            lines.append("\n Pengeluaran per Kategori:")
            sorted_kat = sorted(
                akt["kategori_pengeluaran"].items(),
                key=lambda x: x[1],
                reverse=True
            )
            for kat, total in sorted_kat:
                lines.append(f"   {kat:<15}: Rp {total:,}")

        lines.append(f"{'='*50}")
        return "\n".join(lines)


# ────────────────────────────────────────
# SIMULASI SISTEM
# ────────────────────────────────────────

ip_manager   = IPBlacklistManager()
replay_guard = ReplayProtector(window_menit=10)
profil_db    = ProfilNasabah()

# Setup nasabah
profil_db.daftarkan("USR001", "Budi Santoso", "1234567890", saldo_awal=15_000_000)

# Simulasi serangan brute force dari satu IP
print("=== Simulasi Brute Force ===")
ip_penyerang = "185.220.101.5"
for percobaan in range(7):
    diblokir = ip_manager.catat_gagal_login(ip_penyerang)
    if diblokir:
        print(f"  Percobaan {percobaan+1}: IP {ip_penyerang} DIBLOKIR otomatis!")
        break
    else:
        print(f"  Percobaan {percobaan+1}: Login gagal ({ip_manager._hit_counter[ip_penyerang]}/5)")

# Simulasi replay attack
print("\n=== Simulasi Replay Attack ===")
txn_requests = ["TXN-2026001", "TXN-2026002", "TXN-2026001", "TXN-2026003", "TXN-2026002"]
for txn_id in txn_requests:
    if replay_guard.cek_dan_tandai(txn_id):
        print(f"  [PROSES] {txn_id} — valid, diproses")
    else:
        print(f"  [TOLAK]  {txn_id} — DUPLIKAT, ditolak!")

# Simulasi transaksi nasabah
print("\n=== Simulasi Transaksi Nasabah ===")
transaksi_budi = [
    (3_000_000, "debet",  "transfer"),
    (  500_000, "debet",  "makan"),
    (  200_000, "debet",  "transport"),
    (8_000_000, "kredit", "gaji"),
    (1_500_000, "debet",  "belanja"),
    (  100_000, "debet",  "makan"),
]

for jumlah, tipe, kat in transaksi_budi:
    sukses, pesan = profil_db.catat_transaksi("USR001", jumlah, tipe, kat)
    tanda = "OK" if sukses else "GAGAL"
    print(f"  [{tanda}] {tipe.upper()} Rp {jumlah:,} ({kat}): {pesan}")

print(profil_db.laporan_nasabah("USR001"))
```

**Hasil Sistem**:
- IP yang gagal login 5x otomatis masuk blacklist — bot tersaring tanpa intervensi manual
- Transaksi duplikat (replay) ditolak real-time dengan lookup O(1) via Set
- Profil nasabah terstruktur nested dict memungkinkan query cepat untuk kredit scoring

---

## 📊 Visualisasi

### Struktur Dictionary vs List

```
List (urutan penting):
  riwayat = [500000, 750000, 1200000]
  akses    → riwayat[0] = 500000

Dictionary (key penting):
  nasabah = {"nama": "Budi", "saldo": 5250000}
  akses   → nasabah["nama"] = "Budi"
```

### Operasi Set Secara Visual

```
Set A = {transfer, topup, belanja}
Set B = {belanja, tarik_tunai, transfer}

Union (A | B)        = {transfer, topup, belanja, tarik_tunai}
Intersection (A & B) = {transfer, belanja}
Difference (A - B)   = {topup}
Sym. Diff (A ^ B)    = {topup, tarik_tunai}
```

### Kapan Pakai Apa?

| Kebutuhan | Struktur | Alasan |
|---|---|---|
| Profil nasabah (nama, saldo, rekening) | Dictionary | Key unik, akses cepat by key |
| Riwayat transaksi berurutan | List | Urutan penting, bisa duplikat |
| Daftar IP yang diblokir | Set | Tidak perlu urutan, cek cepat O(1) |
| Kategori yang tidak boleh berubah | frozenset | Immutable, bisa jadi dict key |
| Laporan keuangan per bulan | Nested Dict | Data terstruktur multi-level |
| Koordinat/konstanta | Tuple | Immutable, ringan |

### Lookup Performance

```
Cari elemen dalam 1.000.000 data:

List  → O(n) — harus cek satu per satu → bisa ~0.5 detik
Dict  → O(1) — langsung ke key         → ~0.000001 detik
Set   → O(1) — langsung cek hash       → ~0.000001 detik
```

---

## ⚠️ Kesalahan Umum

**1. Akses key yang tidak ada → KeyError**
```python
nasabah = {"nama": "Budi", "saldo": 5000000}

# SALAH — bisa KeyError!
print(nasabah["email"])    # KeyError: 'email'

# BENAR — gunakan .get()
print(nasabah.get("email"))          # None
print(nasabah.get("email", "N/A"))   # "N/A"
```

**2. Set kosong dengan `{}` membuat Dictionary, bukan Set**
```python
# SALAH
koleksi = {}
print(type(koleksi))  # <class 'dict'> — bukan set!

# BENAR
koleksi = set()
print(type(koleksi))  # <class 'set'>
```

**3. Memodifikasi dictionary saat iterasi**
```python
data = {"a": 1, "b": 2, "c": 3}

# SALAH — RuntimeError!
for key in data:
    if data[key] < 2:
        del data[key]

# BENAR — iterasi salinan keynya
for key in list(data.keys()):
    if data[key] < 2:
        del data[key]

# Atau gunakan dict comprehension
data = {k: v for k, v in data.items() if v >= 2}
```

**4. Key dictionary harus hashable (immutable)**
```python
# List tidak bisa jadi key (mutable = tidak hashable)
d = {[1, 2]: "nilai"}  # TypeError: unhashable type: 'list'

# Tuple bisa jadi key
d = {(1, 2): "nilai"}  # OK!
d = {frozenset({1,2}): "nilai"}  # frozenset juga bisa
```

**5. Copy dictionary yang dangkal (shallow copy) pada nested dict**
```python
asli = {"profil": {"nama": "Budi", "saldo": 5000000}}
salinan = asli.copy()  # shallow copy!

# Nested dict MASIH referensi yang sama
salinan["profil"]["saldo"] = 9999999
print(asli["profil"]["saldo"])  # 9999999 — asli ikut berubah!

# Solusi: gunakan deep copy
import copy
salinan = copy.deepcopy(asli)
salinan["profil"]["saldo"] = 9999999
print(asli["profil"]["saldo"])  # 5000000 — aman!
```

**6. Lupa bahwa Set tidak terurut**
```python
angka = {3, 1, 4, 1, 5, 9, 2, 6}
print(angka)  # {1, 2, 3, 4, 5, 6, 9} — urutan tidak dijamin!

# Jika perlu urutan, konversi ke list dulu
print(sorted(angka))  # [1, 2, 3, 4, 5, 6, 9]
```

**7. `dict.update()` menimpa key yang sudah ada**
```python
profil = {"nama": "Budi", "saldo": 5000000}
update = {"saldo": 6000000, "email": "budi@email.com"}

profil.update(update)
# saldo DITIMPA, bukan ditambah!
print(profil)  # {"nama": "Budi", "saldo": 6000000, "email": "budi@email.com"}
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

Perhatikan kode berikut:
```python
set_A = {1, 2, 3, 4, 5}
set_B = {3, 4, 5, 6, 7}

d = {"x": 10, "y": 20, "z": 30}
```

**Pertanyaan (jawab tanpa run kode):**
- a) Apa hasil `set_A & set_B`?
- b) Apa hasil `set_A - set_B`?
- c) Apa hasil `set_A ^ set_B`?
- d) Apa output `d.get("w", 0)`?
- e) Setelah `d.update({"y": 99, "a": 100})`, apa isi `d`?
- f) Apakah `{} == set()` bernilai True?

---

### Soal 2 — Praktik: Inventaris Toko

Buat program manajemen inventaris sederhana menggunakan Dictionary:

```python
inventaris = {
    "Indomie Goreng"  : {"stok": 150, "harga": 3500,  "kategori": "makanan"},
    "Aqua 600ml"      : {"stok": 80,  "harga": 4000,  "kategori": "minuman"},
    "Tisu Paseo"      : {"stok": 45,  "harga": 12000, "kategori": "kebersihan"},
    "Rinso 800gr"     : {"stok": 30,  "harga": 28000, "kategori": "kebersihan"},
    "Pocari Sweat"    : {"stok": 60,  "harga": 7500,  "kategori": "minuman"},
    "Chitato"         : {"stok": 90,  "harga": 8500,  "kategori": "makanan"},
}
```

**Program harus:**
1. Hitung total nilai inventaris (stok × harga) untuk semua produk
2. Gunakan Dictionary Comprehension untuk membuat dict baru berisi produk dengan **stok < 50** (perlu restock)
3. Kelompokkan produk per kategori — buat dict `{kategori: [daftar produk]}`
4. Cari produk dengan harga tertinggi dan terendah
5. Tambahkan produk baru dan update stok produk yang sudah ada

---

### Soal 3 — Studi Kasus Fintech

**Skenario**: Kamu adalah engineer di tim Data & Analytics "PayGo" — platform pembayaran digital.

Tim bisnis butuh **sistem analitik transaksi real-time** yang bisa menjawab:
1. Berapa total transaksi per kota asal?
2. Merchant mana saja yang muncul lebih dari sekali dalam 1 jam terakhir dari IP yang berbeda (potensi card testing)?
3. User mana yang melakukan transaksi di lebih dari 3 kota berbeda dalam satu hari?

**Data yang tersedia:**
```python
transaksi_hari_ini = [
    {"id": "T001", "user": "U01", "merchant": "Tokopedia", "kota": "Jakarta",  "ip": "192.168.1.1", "jumlah": 250000},
    {"id": "T002", "user": "U02", "merchant": "Grab",      "kota": "Bandung",  "ip": "10.0.0.5",   "jumlah": 45000},
    {"id": "T003", "user": "U01", "merchant": "Shopee",    "kota": "Surabaya", "ip": "192.168.1.1", "jumlah": 180000},
    {"id": "T004", "user": "U03", "merchant": "Tokopedia", "kota": "Jakarta",  "ip": "172.16.0.1", "jumlah": 99000},
    {"id": "T005", "user": "U01", "merchant": "Indomaret", "kota": "Medan",    "ip": "192.168.1.1", "jumlah": 35000},
    {"id": "T006", "user": "U02", "merchant": "Grab",      "kota": "Jakarta",  "ip": "10.0.0.6",   "jumlah": 55000},
    {"id": "T007", "user": "U01", "merchant": "Alfamart",  "kota": "Bali",     "ip": "192.168.1.1", "jumlah": 75000},
    {"id": "T008", "user": "U04", "merchant": "Shopee",    "kota": "Bandung",  "ip": "172.16.0.2", "jumlah": 320000},
    {"id": "T009", "user": "U03", "merchant": "Grab",      "kota": "Jakarta",  "ip": "172.16.0.3", "jumlah": 30000},
    {"id": "T010", "user": "U02", "merchant": "Tokopedia", "kota": "Surabaya", "ip": "10.0.0.5",   "jumlah": 150000},
]
```

**Tugas (gunakan Dictionary dan Set):**
1. Hitung total transaksi per kota (gunakan dict)
2. Cari merchant yang diakses dari lebih dari 1 IP berbeda (gunakan dict of sets)
3. Identifikasi user yang transaksi di lebih dari 3 kota berbeda (gunakan dict of sets)
4. Tampilkan semua kota unik yang terlibat dalam transaksi (gunakan set)
5. Cek apakah ada IP yang muncul di lebih dari 5 transaksi (monitoring keamanan)

---

## 📌 Ringkasan

- **Dictionary** `{key: value}` — ordered (Python 3.7+), mutable, key harus unik dan hashable
- **Akses aman**: selalu gunakan `.get(key, default)` daripada `d[key]` untuk menghindari KeyError
- **Metode penting Dict**: `keys()`, `values()`, `items()`, `get()`, `update()`, `pop()`, `setdefault()`
- **Dict Comprehension**: `{k: v for k, v in iterable if kondisi}` — transformasi data ringkas
- **Nested Dict**: akses dengan chaining `d["level1"]["level2"]`, gunakan `.get()` berantai untuk keamanan
- **Shallow vs Deep Copy**: `.copy()` hanya menyalin satu level — gunakan `copy.deepcopy()` untuk nested dict
- **Set** `{a, b, c}` — unordered, elemen unik, pengecekan keanggotaan O(1)
- **Set kosong**: gunakan `set()`, bukan `{}` (itu dict!)
- **Operasi Set**: `|` union, `&` intersection, `-` difference, `^` symmetric difference
- **frozenset**: Set immutable — bisa jadi key dictionary
- **Set vs List untuk `in` check**: Set jauh lebih cepat O(1) vs O(n)
- **Jangan** modifikasi dict saat diiterasi — iterasi `list(d.keys())` atau buat dict baru
- **Key dict** harus immutable/hashable: string, int, tuple — **bukan list atau dict**
