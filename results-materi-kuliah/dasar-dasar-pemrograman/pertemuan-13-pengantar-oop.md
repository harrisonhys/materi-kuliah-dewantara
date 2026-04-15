# Pertemuan 13: Pengantar OOP — Class, Object, Atribut, Metode

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Memahami **konsep dasar OOP**: Class, Object, Atribut, dan Metode — dan mengapa OOP ada
- Menulis **Class** dengan `__init__` (constructor) dan `self` untuk membuat blueprint objek
- Membuat **instance (objek)** dari class dan menggunakannya
- Mengakses **atribut** dan memanggil **metode** dari sebuah objek
- Menerapkan **enkapsulasi dasar**: membedakan atribut publik dan private (dengan konvensi `_`)
- Menggunakan `__str__` untuk representasi objek yang informatif
- Membangun class realistis seperti `BankAccount` dan `Mahasiswa` yang memiliki data dan perilaku

---

## 📖 Pengantar (Hook)

Bayangkan kamu diminta membangun sistem manajemen rekening bank. Dengan pendekatan **prosedural murni**, kode kamu akan terlihat seperti ini:

```python
# Pendekatan PROSEDURAL (tanpa OOP)
rekening1_nomor = "1234567890"
rekening1_pemilik = "Budi Santoso"
rekening1_saldo = 5_000_000

rekening2_nomor = "0987654321"
rekening2_pemilik = "Ani Wijaya"
rekening2_saldo = 2_500_000

def setor(saldo, jumlah):
    return saldo + jumlah

def tarik(saldo, jumlah):
    if jumlah > saldo:
        return saldo, False
    return saldo - jumlah, True

# Setor ke rekening Budi
rekening1_saldo = setor(rekening1_saldo, 500_000)

# Setor ke rekening Ani
rekening2_saldo = setor(rekening2_saldo, 200_000)
```

Tidak ada masalah untuk 2 rekening. Tapi bagaimana kalau ada **10.000 nasabah**? Kamu akan punya 30.000 variabel terpisah dan fungsi yang tidak "tahu" ke rekening mana mereka bekerja.

Dengan **OOP**, rekening adalah sebuah **objek** — dia punya data (nomor, pemilik, saldo) sekaligus kemampuan (setor, tarik, transfer). Kode jadi jauh lebih rapi, mudah dipahami, dan mudah dikembangkan.

---

## 🧩 Konsep Utama

### 1. Class dan Object

- **Class** = **blueprint / cetakan**. Mendefinisikan seperti apa sebuah objek — atribut apa yang dimiliki, aksi apa yang bisa dilakukan.
- **Object (Instance)** = **hasil cetakan**. Benda nyata yang dibuat dari class. Setiap objek punya data sendiri-sendiri, tapi berbagi metode yang sama.

Contoh sederhana:
- Class: `ManusiaBiasa` (blueprint)
- Object: `budi = ManusiaBiasa(...)`, `ani = ManusiaBiasa(...)` (individu nyata)

### 2. Atribut

**Atribut** adalah **data / variabel** yang dimiliki sebuah objek. Setiap objek menyimpan nilai atributnya sendiri.

Ada dua jenis atribut:
- **Instance attribute**: unik per objek (misalnya `saldo`, `nama`)
- **Class attribute**: sama untuk semua objek dari class itu (misalnya `bank_name = "BankXYZ"`)

### 3. Metode

**Metode** adalah **fungsi yang ada di dalam class** dan bekerja pada data objek tersebut. Metode selalu menerima `self` sebagai parameter pertama (meski kamu tidak perlu menyebutnya saat memanggil).

### 4. `__init__` — Constructor

`__init__` adalah metode khusus yang otomatis dipanggil ketika kamu membuat objek baru. Inilah tempat kamu mendefinisikan atribut-atribut awal objek.

### 5. `self`

`self` merujuk ke **objek itu sendiri**. Lewat `self`, sebuah metode bisa mengakses dan mengubah atribut objeknya sendiri. Ketika kamu menulis `self.saldo`, kamu mengakses atribut `saldo` dari objek saat ini.

### 6. Enkapsulasi Dasar

**Enkapsulasi** = menyembunyikan detail internal dan hanya mengekspos antarmuka (interface) yang diperlukan.

Di Python, konvensinya:
- `nama_atribut` (tanpa underscore) = **publik** — bebas diakses dari luar
- `_nama_atribut` (satu underscore) = **protected** — konvensi "jangan diakses langsung dari luar", tapi masih bisa
- `__nama_atribut` (dua underscore) = **private** — Python me-mangle nama ini agar sulit diakses dari luar

### 7. `__str__` — Representasi String Objek

Ketika kamu `print(objek)`, Python mencari metode `__str__`. Tanpanya, output hanya alamat memori yang tidak berguna. Dengan `__str__`, kamu bisa mendefinisikan tampilan yang informatif.

---

## 🧠 Ilustrasi / Analogi

### Analogi 1: Class seperti Formulir Kosong, Object seperti Formulir yang Diisi

Bayangkan **formulir pembukaan rekening bank**:

| Aspek OOP | Analogi Formulir Bank | Contoh |
|-----------|----------------------|--------|
| **Class** | Formulir kosong (template) | Blanko "Pembukaan Rekening" |
| **Object** | Formulir yang sudah diisi | Formulir milik Budi, formulir milik Ani |
| **Atribut** | Kolom-kolom di formulir | Nama, NIK, nomor rekening, saldo awal |
| **Metode** | Operasi yang bisa dilakukan | Setor, tarik, transfer, cek saldo |
| `__init__` | Proses pengisian awal saat pertama buka rekening | Isi semua kolom wajib saat mendaftar |

### Analogi 2: Mesin ATM

Mesin ATM adalah contoh OOP di dunia nyata:

```
Class: MesinATM
├── Atribut
│   ├── lokasi         (data/state)
│   ├── saldo_tunai    (data/state)
│   └── status_aktif   (data/state)
└── Metode
    ├── tarik_tunai()   (aksi/behavior)
    ├── cek_saldo()     (aksi/behavior)
    └── ganti_pin()     (aksi/behavior)
```

Ribuan mesin ATM BCA di seluruh Indonesia adalah **objek** yang dibuat dari **class** yang sama. Mereka punya kemampuan yang sama (tarik, cek saldo) tapi data yang berbeda (lokasi, saldo tunai).

---

## 💻 Contoh Teknis

### Membuat Class Pertama

```python
# ========== CLASS SEDERHANA ==========
class Mahasiswa:
    """Blueprint untuk objek Mahasiswa"""

    # Class attribute — sama untuk semua mahasiswa
    universitas = "Universitas Teknologi Nusantara"

    def __init__(self, nim, nama, jurusan, ipk=0.0):
        """
        Constructor — dijalankan otomatis saat objek dibuat
        self = objek itu sendiri (seperti "aku" atau "saya")
        """
        # Instance attributes — unik per mahasiswa
        self.nim = nim          # publik
        self.nama = nama        # publik
        self.jurusan = jurusan  # publik
        self._ipk = ipk         # protected — konvensi: akses lewat metode saja

    def __str__(self):
        """Representasi string yang informatif saat di-print"""
        return f"Mahasiswa({self.nim} - {self.nama} [{self.jurusan}])"

    def perkenalan(self):
        """Metode biasa"""
        return (f"Halo! Saya {self.nama}, NIM {self.nim}, "
                f"jurusan {self.jurusan} di {self.universitas}.")

    def get_ipk(self):
        """Getter untuk atribut protected"""
        return self._ipk

    def update_ipk(self, ipk_baru):
        """Setter dengan validasi"""
        if not (0.0 <= ipk_baru <= 4.0):
            raise ValueError(f"IPK harus antara 0.0 dan 4.0, bukan {ipk_baru}")
        self._ipk = ipk_baru
        print(f"IPK {self.nama} diperbarui menjadi {self._ipk:.2f}")

    def status_kelulusan(self):
        """Logika bisnis berdasarkan state objek"""
        if self._ipk >= 3.5:
            return "Cum Laude"
        elif self._ipk >= 3.0:
            return "Sangat Memuaskan"
        elif self._ipk >= 2.75:
            return "Memuaskan"
        elif self._ipk >= 2.0:
            return "Cukup"
        else:
            return "Perlu Perbaikan"


# ========== MEMBUAT OBJEK ==========
mhs1 = Mahasiswa("2024001", "Budi Santoso", "Teknik Informatika", 3.75)
mhs2 = Mahasiswa("2024002", "Ani Wijaya", "Sistem Informasi", 3.20)
mhs3 = Mahasiswa("2024003", "Ciko Pratama", "Teknik Informatika")

# Akses atribut publik
print(mhs1.nama)       # Budi Santoso
print(mhs1.nim)        # 2024001
print(mhs1.universitas)  # Universitas Teknologi Nusantara (class attribute)

# Akses lewat class attribute
print(Mahasiswa.universitas)  # juga bisa diakses lewat class langsung

# Panggil metode
print(mhs1.perkenalan())
print(f"IPK: {mhs1.get_ipk()}")
print(f"Status: {mhs1.status_kelulusan()}")

# print() otomatis memanggil __str__
print(mhs1)  # Mahasiswa(2024001 - Budi Santoso [Teknik Informatika])

# Update IPK
mhs2.update_ipk(3.50)

# Error handling di dalam metode
try:
    mhs3.update_ipk(5.0)  # IPK tidak valid
except ValueError as e:
    print(f"Error: {e}")
```

### Class BankAccount — Contoh Lebih Lengkap

```python
import datetime

class BankAccount:
    """
    Representasi rekening bank sederhana.
    Contoh implementasi OOP untuk sistem perbankan.
    """

    # Class attribute — berlaku untuk semua rekening
    nama_bank = "PyBank"
    bunga_tabungan_tahunan = 0.035  # 3.5% per tahun

    def __init__(self, nomor_rekening, pemilik, saldo_awal=0):
        """
        Constructor: inisialisasi rekening baru

        Args:
            nomor_rekening (str): nomor rekening unik
            pemilik (str): nama pemilik rekening
            saldo_awal (float): saldo pembukaan rekening (default 0)
        """
        if saldo_awal < 0:
            raise ValueError("Saldo awal tidak boleh negatif")

        self.nomor_rekening = nomor_rekening    # publik
        self.pemilik = pemilik                  # publik
        self._saldo = saldo_awal               # protected
        self._riwayat = []                     # protected — list histori transaksi
        self._tanggal_buka = datetime.date.today()  # protected

        # Catat transaksi pembukaan
        if saldo_awal > 0:
            self._catat_transaksi("SETOR", saldo_awal, "Setoran awal pembukaan rekening")

    def __str__(self):
        return (f"[{self.nama_bank}] Rekening {self.nomor_rekening} "
                f"a.n. {self.pemilik} | Saldo: Rp{self._saldo:,.0f}")

    def __repr__(self):
        return f"BankAccount('{self.nomor_rekening}', '{self.pemilik}', {self._saldo})"

    # ========== PRIVATE HELPER ==========
    def _catat_transaksi(self, tipe, jumlah, keterangan=""):
        """Metode private: hanya dipakai internal class"""
        transaksi = {
            "tipe": tipe,
            "jumlah": jumlah,
            "keterangan": keterangan,
            "waktu": datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "saldo_setelah": self._saldo
        }
        self._riwayat.append(transaksi)

    # ========== PUBLIC METHODS ==========
    def cek_saldo(self):
        """Mengembalikan saldo saat ini"""
        return self._saldo

    def setor(self, jumlah, keterangan=""):
        """
        Menyetor uang ke rekening.

        Args:
            jumlah (float): jumlah yang disetor (harus > 0)
            keterangan (str): deskripsi transaksi
        Returns:
            float: saldo terbaru setelah setor
        """
        if not isinstance(jumlah, (int, float)):
            raise TypeError("Jumlah setor harus berupa angka")
        if jumlah <= 0:
            raise ValueError("Jumlah setor harus lebih dari 0")

        self._saldo += jumlah
        self._catat_transaksi("SETOR", jumlah, keterangan or "Setoran tunai")
        print(f"✅ Setor Rp{jumlah:,.0f} berhasil. Saldo: Rp{self._saldo:,.0f}")
        return self._saldo

    def tarik(self, jumlah, keterangan=""):
        """
        Menarik uang dari rekening.

        Args:
            jumlah (float): jumlah yang ditarik
            keterangan (str): deskripsi transaksi
        Returns:
            float: saldo terbaru setelah tarik
        Raises:
            ValueError: jika saldo tidak cukup atau jumlah tidak valid
        """
        if not isinstance(jumlah, (int, float)):
            raise TypeError("Jumlah tarik harus berupa angka")
        if jumlah <= 0:
            raise ValueError("Jumlah tarik harus lebih dari 0")
        if jumlah > self._saldo:
            raise ValueError(
                f"Saldo tidak cukup. Saldo tersedia: Rp{self._saldo:,.0f}, "
                f"Diminta: Rp{jumlah:,.0f}"
            )

        self._saldo -= jumlah
        self._catat_transaksi("TARIK", jumlah, keterangan or "Penarikan tunai")
        print(f"✅ Tarik Rp{jumlah:,.0f} berhasil. Saldo: Rp{self._saldo:,.0f}")
        return self._saldo

    def transfer(self, rekening_tujuan, jumlah, keterangan=""):
        """
        Transfer ke rekening lain.

        Args:
            rekening_tujuan (BankAccount): objek rekening tujuan
            jumlah (float): jumlah yang ditransfer
        """
        if not isinstance(rekening_tujuan, BankAccount):
            raise TypeError("rekening_tujuan harus berupa objek BankAccount")

        # Tarik dari rekening sumber
        self.tarik(jumlah, f"Transfer ke {rekening_tujuan.pemilik}")

        # Setor ke rekening tujuan
        rekening_tujuan.setor(jumlah, f"Transfer dari {self.pemilik}")

        print(f"💸 Transfer Rp{jumlah:,.0f} dari {self.pemilik} "
              f"ke {rekening_tujuan.pemilik} berhasil!")

    def lihat_riwayat(self, n_terakhir=5):
        """Menampilkan n transaksi terakhir"""
        print(f"\n{'='*55}")
        print(f"  MUTASI REKENING {self.nomor_rekening} a.n. {self.pemilik}")
        print(f"{'='*55}")

        riwayat_tampil = self._riwayat[-n_terakhir:] if self._riwayat else []
        if not riwayat_tampil:
            print("  Belum ada transaksi.")
        else:
            for trx in riwayat_tampil:
                tanda = "+" if trx["tipe"] == "SETOR" else "-"
                print(f"  [{trx['waktu']}] {trx['tipe']:<6} "
                      f"{tanda}Rp{trx['jumlah']:>12,.0f}  "
                      f"| {trx['keterangan']}")

        print(f"{'─'*55}")
        print(f"  Saldo Saat Ini: Rp{self._saldo:,.0f}")
        print(f"{'='*55}\n")

    def hitung_bunga_bulanan(self):
        """Hitung estimasi bunga tabungan bulanan"""
        bunga_bulanan = self._saldo * (self.bunga_tabungan_tahunan / 12)
        return bunga_bulanan


# ========== PENGGUNAAN LENGKAP ==========
print("=" * 60)
print("DEMO SISTEM REKENING PYBANK")
print("=" * 60)

# Membuat objek rekening
rek_budi = BankAccount("1234567890", "Budi Santoso", 5_000_000)
rek_ani = BankAccount("0987654321", "Ani Wijaya", 3_000_000)

# Print representasi objek
print(rek_budi)
print(rek_ani)
print()

# Transaksi
rek_budi.setor(1_000_000, "Gaji bulan ini")
rek_budi.tarik(250_000, "Belanja kebutuhan")
rek_budi.transfer(rek_ani, 500_000, "Bayar patungan makan")

# Lihat riwayat
rek_budi.lihat_riwayat()
rek_ani.lihat_riwayat()

# Informasi bunga
bunga = rek_budi.hitung_bunga_bulanan()
print(f"Estimasi bunga bulanan Budi: Rp{bunga:,.0f}")

# Error handling
try:
    rek_budi.tarik(10_000_000)  # Saldo tidak cukup
except ValueError as e:
    print(f"❌ {e}")
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Skenario: Refaktor Sistem Transfer dari Prosedural ke OOP

**Masalah:**

Startup fintech "CoinFlow" awalnya dibangun dengan kode prosedural murni oleh tim kecil yang bergerak cepat. Setelah 1 tahun, mereka punya 50.000 pengguna, dan kodenya jadi **nightmare**:

```python
# Kode lama — PROSEDURAL (bermasalah)
users = {}

def buat_user(user_id, nama, saldo):
    users[user_id] = {"nama": nama, "saldo": saldo, "riwayat": []}

def setor_user(user_id, jumlah):
    users[user_id]["saldo"] += jumlah
    users[user_id]["riwayat"].append(f"SETOR {jumlah}")

def tarik_user(user_id, jumlah):
    if users[user_id]["saldo"] < jumlah:
        return False
    users[user_id]["saldo"] -= jumlah
    users[user_id]["riwayat"].append(f"TARIK {jumlah}")
    return True

def transfer_user(dari_id, ke_id, jumlah):
    if tarik_user(dari_id, jumlah):
        setor_user(ke_id, jumlah)
        return True
    return False

# Masalah: Tidak ada validasi konsisten, mudah corrupt state,
# tidak ada encapsulation, sulit di-test, sulit ditambah fitur baru
```

Masalahnya: setiap kali ada bug (misalnya saldo bisa jadi negatif), perlu melacak ke 15 fungsi berbeda. Tidak ada satu tempat yang "bertanggung jawab" untuk state rekening.

**Dampak Bisnis:**
- Bug yang memungkinkan saldo negatif merugikan perusahaan Rp45 juta sebelum ketahuan
- Developer baru butuh 2 minggu hanya untuk memahami alur transaksi
- Penambahan fitur baru (misalnya limit transfer) harus dimodifikasi di 8 tempat berbeda
- Unit testing hampir tidak mungkin dilakukan

**Solusi: Refaktor ke OOP**

```python
import datetime
import uuid

class Wallet:
    """
    Digital wallet untuk platform CoinFlow.
    Menggantikan pendekatan prosedural dengan OOP yang terstruktur.
    """

    LIMIT_TRANSFER_HARIAN = 10_000_000  # Rp10 juta per hari
    SALDO_MINIMUM = 10_000              # Rp10 ribu saldo minimum

    def __init__(self, user_id, nama_pemilik, saldo_awal=0):
        self.user_id = user_id
        self.nama_pemilik = nama_pemilik
        self._saldo = saldo_awal
        self._riwayat_transaksi = []
        self._total_transfer_hari_ini = 0
        self._tanggal_terakhir_reset = datetime.date.today()
        self._aktif = True

    def __str__(self):
        status = "AKTIF" if self._aktif else "NONAKTIF"
        return (f"Wallet [{status}] {self.user_id} | "
                f"{self.nama_pemilik} | Saldo: Rp{self._saldo:,.0f}")

    def _reset_limit_harian_jika_perlu(self):
        """Auto-reset limit transfer ketika hari berganti"""
        hari_ini = datetime.date.today()
        if hari_ini > self._tanggal_terakhir_reset:
            self._total_transfer_hari_ini = 0
            self._tanggal_terakhir_reset = hari_ini

    def _buat_id_transaksi(self):
        """Generate ID transaksi unik"""
        return f"CF-{uuid.uuid4().hex[:8].upper()}"

    def _log_transaksi(self, tipe, jumlah, keterangan, trx_id):
        """Catat transaksi ke riwayat internal"""
        self._riwayat_transaksi.append({
            "trx_id": trx_id,
            "tipe": tipe,
            "jumlah": jumlah,
            "keterangan": keterangan,
            "waktu": datetime.datetime.now().isoformat(),
            "saldo_setelah": self._saldo
        })

    @property
    def saldo(self):
        """Property: akses saldo dengan sintaks seperti atribut tapi tetap terkontrol"""
        return self._saldo

    @property
    def aktif(self):
        return self._aktif

    def nonaktifkan(self, alasan=""):
        """Menonaktifkan wallet (misalnya karena pelanggaran)"""
        self._aktif = False
        print(f"⚠️ Wallet {self.user_id} dinonaktifkan. Alasan: {alasan}")

    def top_up(self, jumlah, keterangan="Top up saldo"):
        """Menambah saldo ke wallet"""
        if not self._aktif:
            raise PermissionError("Wallet tidak aktif. Hubungi CS.")
        if jumlah <= 0:
            raise ValueError("Jumlah top-up harus lebih dari 0")
        if jumlah > 10_000_000:
            raise ValueError("Maksimum top-up sekali transaksi: Rp10.000.000")

        trx_id = self._buat_id_transaksi()
        self._saldo += jumlah
        self._log_transaksi("TOP_UP", jumlah, keterangan, trx_id)
        return trx_id

    def bayar(self, jumlah, merchant, keterangan=""):
        """Pembayaran ke merchant"""
        self._reset_limit_harian_jika_perlu()

        if not self._aktif:
            raise PermissionError("Wallet tidak aktif. Hubungi CS.")
        if jumlah <= 0:
            raise ValueError("Jumlah pembayaran harus lebih dari 0")
        if self._saldo - jumlah < self.SALDO_MINIMUM:
            raise ValueError(
                f"Saldo tidak cukup. Saldo tersedia setelah biaya: "
                f"Rp{self._saldo - jumlah:,.0f} (min. Rp{self.SALDO_MINIMUM:,.0f})"
            )

        trx_id = self._buat_id_transaksi()
        self._saldo -= jumlah
        self._log_transaksi("BAYAR", jumlah, f"{merchant}: {keterangan}", trx_id)
        return trx_id

    def transfer(self, wallet_tujuan, jumlah, keterangan=""):
        """Transfer ke wallet lain dengan validasi limit harian"""
        self._reset_limit_harian_jika_perlu()

        if not self._aktif:
            raise PermissionError("Wallet pengirim tidak aktif.")
        if not wallet_tujuan.aktif:
            raise PermissionError("Wallet penerima tidak aktif.")
        if jumlah <= 0:
            raise ValueError("Jumlah transfer harus lebih dari 0")
        if self._total_transfer_hari_ini + jumlah > self.LIMIT_TRANSFER_HARIAN:
            sisa = self.LIMIT_TRANSFER_HARIAN - self._total_transfer_hari_ini
            raise ValueError(
                f"Melebihi limit transfer harian. Sisa limit hari ini: Rp{sisa:,.0f}"
            )
        if self._saldo - jumlah < self.SALDO_MINIMUM:
            raise ValueError(
                f"Saldo tidak cukup. Saldo tersedia: Rp{self._saldo - jumlah + self.SALDO_MINIMUM:,.0f}"
            )

        trx_id = self._buat_id_transaksi()

        # Eksekusi transfer
        self._saldo -= jumlah
        self._total_transfer_hari_ini += jumlah
        self._log_transaksi(
            "TRANSFER_KELUAR", jumlah,
            f"Transfer ke {wallet_tujuan.nama_pemilik}: {keterangan}", trx_id
        )

        wallet_tujuan._saldo += jumlah
        wallet_tujuan._log_transaksi(
            "TRANSFER_MASUK", jumlah,
            f"Transfer dari {self.nama_pemilik}: {keterangan}", trx_id
        )

        return trx_id

    def mutasi(self, n=10):
        """Tampilkan n transaksi terakhir"""
        print(f"\n{'─'*60}")
        print(f"  MUTASI: {self.nama_pemilik} ({self.user_id})")
        print(f"{'─'*60}")
        transaksi_tampil = self._riwayat_transaksi[-n:]
        for t in transaksi_tampil:
            arah = "↑" if "MASUK" in t["tipe"] or "TOP" in t["tipe"] else "↓"
            print(f"  {arah} [{t['trx_id']}] {t['tipe']:<18} "
                  f"Rp{t['jumlah']:>10,.0f}  | {t['keterangan'][:30]}")
        print(f"{'─'*60}")
        print(f"  Saldo: Rp{self._saldo:,.0f}")
        print(f"{'─'*60}\n")


# ========== SIMULASI TRANSAKSI ==========
print("=" * 60)
print("DEMO SISTEM COINFLOW WALLET")
print("=" * 60)

# Buat wallet
w_budi = Wallet("USR-001", "Budi Santoso", 500_000)
w_ani = Wallet("USR-002", "Ani Wijaya", 1_000_000)
w_merchant = Wallet("MCH-001", "Tokopedia", 0)

# Top up
trx1 = w_budi.top_up(2_000_000, "Top up via BCA Virtual Account")
print(f"✅ Top up berhasil. ID: {trx1}")

# Bayar
trx2 = w_budi.bayar(350_000, "Tokopedia", "Beli headphone")
print(f"✅ Bayar berhasil. ID: {trx2}")

# Transfer antar wallet
try:
    trx3 = w_budi.transfer(w_ani, 500_000, "Bayar patungan makan siang")
    print(f"✅ Transfer berhasil. ID: {trx3}")
except ValueError as e:
    print(f"❌ Transfer gagal: {e}")

# Tampilkan mutasi
w_budi.mutasi()
w_ani.mutasi()

# Coba transfer melebihi limit
try:
    w_budi.transfer(w_ani, 9_000_000, "Test limit")
except ValueError as e:
    print(f"❌ {e}")
```

**Keunggulan OOP dibanding Prosedural:**

| Aspek | Prosedural (Lama) | OOP (Baru) |
|-------|-------------------|------------|
| **Validasi saldo** | Tersebar di 8 fungsi | Terpusat di satu metode |
| **Riwayat transaksi** | Di-manage manual | Otomatis dalam objek |
| **Limit harian** | Harus pass variabel ke mana-mana | Auto-reset dalam objek |
| **Testing** | Sulit, banyak state global | Mudah: buat objek, jalankan, cek state |
| **Bug saldo negatif** | Bisa terjadi | Tidak mungkin (validasi di setter) |
| **Tambah fitur baru** | Edit banyak tempat | Tambah metode baru ke class |

---

## 📊 Visualisasi

### Anatomi Class Python

```python
class NamaClass:             # ← Definisi class
    class_attr = "nilai"     # ← Class attribute (shared)

    def __init__(self, param):   # ← Constructor (method khusus)
        self.instance_attr = param   # ← Instance attribute (unik per objek)
        self._protected = None       # ← Protected (konvensi: akses lewat metode)
        self.__private = None        # ← Private (name mangling Python)

    def __str__(self):       # ← Method khusus: representasi string
        return f"..."

    def method_publik(self): # ← Method yang bisa dipanggil dari luar
        return ...

    def _method_protected(self):  # ← Konvensi: hanya dipakai dalam class
        return ...
```

### Alur Pembuatan dan Penggunaan Objek

```
Definisi Class                    Pembuatan Objek             Penggunaan
─────────────                     ───────────────             ──────────
class BankAccount:                rek1 = BankAccount(         rek1.setor(500_000)
  def __init__(self, ...):    →     "123", "Budi", 1_000_000) rek1.tarik(200_000)
    self.saldo = saldo_awal   →   rek2 = BankAccount(     →   rek1.transfer(rek2, 300_000)
  def setor(self, jumlah):         "456", "Ani", 2_000_000)   print(rek1.cek_saldo())
    ...                                                        print(rek1)
```

### Perbedaan Class Attribute vs Instance Attribute

```python
class Rekening:
    bank = "PyBank"       # ← Class attribute: SAMA untuk semua

    def __init__(self, saldo):
        self.saldo = saldo  # ← Instance attribute: BEDA tiap objek

r1 = Rekening(1_000_000)
r2 = Rekening(5_000_000)

print(r1.bank)   # "PyBank"     ← dari class
print(r2.bank)   # "PyBank"     ← sama persis
print(r1.saldo)  # 1_000_000    ← dari objek r1
print(r2.saldo)  # 5_000_000    ← dari objek r2, berbeda!
```

---

## ⚠️ Kesalahan Umum

**1. Lupa `self` di parameter metode**

```python
# ❌ SALAH — Python tidak tahu ini metode objek
class Rekening:
    def cek_saldo():  # Lupa self!
        return self.saldo  # NameError: name 'self' is not defined

# ✅ BENAR
class Rekening:
    def cek_saldo(self):
        return self._saldo
```

**2. Lupa `self.` saat mengakses atribut di dalam metode**

```python
# ❌ SALAH — saldo ini variabel lokal, bukan atribut objek
class Rekening:
    def __init__(self, saldo_awal):
        saldo = saldo_awal  # Ini variabel lokal, hilang setelah __init__ selesai!

    def cek_saldo(self):
        return saldo  # NameError!

# ✅ BENAR
class Rekening:
    def __init__(self, saldo_awal):
        self._saldo = saldo_awal  # Ini atribut objek, persist

    def cek_saldo(self):
        return self._saldo
```

**3. Mengakses atribut `_protected` langsung dari luar class**

```python
rek = BankAccount("123", "Budi", 1_000_000)

# ❌ KURANG BAIK — bypass enkapsulasi
rek._saldo = 999_999_999  # Ini bisa dilakukan, tapi melanggar konvensi!

# ✅ BENAR — gunakan metode yang disediakan
rek.setor(999_000_000)  # Lewat metode dengan validasi
```

**4. Membingungkan class attribute dan instance attribute**

```python
class Akun:
    saldo = 0  # ← CLASS attribute

    def setor(self, jumlah):
        self.saldo += jumlah  # ← Ini sebenarnya membuat INSTANCE attribute baru!

a1 = Akun()
a2 = Akun()
a1.setor(100)

print(a1.saldo)  # 100    ← instance attribute milik a1
print(a2.saldo)  # 0      ← masih pakai class attribute
print(Akun.saldo)  # 0    ← class attribute tidak berubah
```

**5. Memanggil metode dengan tanda kurung yang terlupakan**

```python
rek = BankAccount("123", "Budi", 1_000_000)

# ❌ SALAH — tidak dipanggil, hanya mereferensi method object
saldo = rek.cek_saldo     # saldo = <bound method BankAccount.cek_saldo>
print(saldo)               # <bound method ...> — bukan angka!

# ✅ BENAR — dipanggil dengan ()
saldo = rek.cek_saldo()   # saldo = 1000000
print(saldo)               # 1000000
```

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep: Class Mahasiswa yang Lebih Lengkap

Buat class `Mahasiswa` dengan spesifikasi berikut:

**Atribut:**
- `nim` (publik)
- `nama` (publik)
- `_nilai` (protected) — dictionary `{"matkul": nilai}`, awal kosong

**Metode:**
- `tambah_nilai(matkul, nilai)` — tambahkan nilai untuk mata kuliah. Validasi: nilai harus 0-100
- `get_ipk()` — hitung rata-rata semua nilai yang ada, return 0.0 jika belum ada nilai
- `get_predikat()` — return predikat berdasarkan IPK (A/B/C/D/E)
- `__str__` — tampilkan: NIM, nama, dan IPK saat ini
- `cetak_transkrip()` — tampilkan semua nilai per mata kuliah, rapi dengan formatting

**Uji dengan:**
- Tambahkan nilai: Matematika=85, Pemrograman=92, Basis Data=78, Jaringan=70
- Print objek mahasiswa
- Cetak transkrip lengkap

### Soal 2 — Studi Kasus: Sistem Pinjaman (Loan)

Desain class `Pinjaman` untuk platform fintech P2P lending "LoanFlow".

**Spesifikasi:**

Atribut (semua protected, akses lewat property/method):
- `id_pinjaman` (string unik, generate otomatis)
- `peminjam` (nama string)
- `pokok_pinjaman` (float)
- `bunga_per_bulan` (float, dalam %)
- `tenor_bulan` (int)
- `status` (string: "ACTIVE" / "PAID" / "DEFAULT")
- `cicilan_terbayar` (int, mulai dari 0)

Metode:
- `hitung_cicilan_bulanan()` → gunakan rumus anuitas atau flat sederhana (pilih salah satu, jelaskan)
- `bayar_cicilan()` → kurangi cicilan yang belum terbayar. Jika semua lunas, ubah status ke "PAID". Raise error jika status bukan "ACTIVE"
- `sisa_cicilan()` → return berapa cicilan yang belum dibayar
- `total_kewajiban()` → total yang harus dibayar (pokok + bunga keseluruhan)
- `tandai_macet()` → ubah status ke "DEFAULT" jika cicilan_terbayar < 3 dan sudah melewati bulan ke-4
- `__str__` → tampilkan ringkasan pinjaman yang informatif

**Uji dengan skenario:**
- Pinjaman Rp10 juta, bunga 1.5%/bulan, tenor 12 bulan
- Bayar 3 kali cicilan
- Cek sisa cicilan dan total yang sudah dibayar
- Bayar semua sisa cicilan → status harus jadi "PAID"
- Coba bayar cicilan lagi setelah lunas → harus raise error

---

## 📌 Ringkasan

**Terminologi Kunci:**

| Istilah | Artinya | Contoh |
|---------|---------|--------|
| **Class** | Blueprint/cetakan | `class BankAccount:` |
| **Object/Instance** | Hasil cetakan | `rek = BankAccount(...)` |
| **Atribut** | Data dalam objek | `self.saldo = 1000` |
| **Metode** | Fungsi dalam class | `def setor(self, jml):` |
| **Constructor** | Metode inisialisasi | `def __init__(self, ...):` |
| **self** | Referensi ke objek sendiri | `self.nama`, `self.setor()` |

**Pola Class — Template Dasar:**

```python
class NamaClass:
    class_attr = "nilai_bersama"  # opsional

    def __init__(self, param1, param2):
        self.atribut_publik = param1
        self._atribut_protected = param2

    def __str__(self):
        return f"NamaClass({self.atribut_publik})"

    def metode_publik(self):
        return self._atribut_protected

    def _metode_internal(self):
        # Hanya dipakai di dalam class
        pass
```

**Enkapsulasi — Konvensi Python:**
- `nama` → publik, bebas diakses dari mana saja
- `_nama` → protected, konvensi "jangan akses langsung dari luar"
- `__nama` → private, Python mangle namanya menjadi `_NamaClass__nama`

**Checklist Membuat Class yang Baik:**
- `__init__` mendefinisikan semua atribut yang dibutuhkan
- `__str__` memberikan representasi yang informatif
- Atribut penting dilindungi dengan `_` dan diakses lewat metode
- Validasi ada di dalam metode (bukan di luar)
- Setiap metode punya satu tanggung jawab yang jelas
- Nama class menggunakan `PascalCase`, atribut dan metode `snake_case`
