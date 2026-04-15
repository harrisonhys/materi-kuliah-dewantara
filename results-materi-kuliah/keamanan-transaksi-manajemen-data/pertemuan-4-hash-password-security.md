# Pertemuan 4: Fungsi Hash & Password Security — SHA-256, bcrypt, Argon2

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan sifat-sifat kriptografis fungsi hash dan membedakannya dari enkripsi
* Membandingkan MD5/SHA-1 (deprecated) dengan SHA-256/SHA-3 (aman)
* Menjelaskan serangan rainbow table dan peran salt dalam pencegahannya
* Mengimplementasikan penyimpanan password yang aman menggunakan bcrypt dan Argon2

---

## 📖 Pengantar (Hook)

Pada 2012, 6.5 juta password LinkedIn bocor ke internet. Yang mengejutkan: password disimpan sebagai hash SHA-1 **tanpa salt**. Dalam hitungan jam, 60% password sudah ter-crack menggunakan rainbow table.

Pada 2019, database 500 juta akun Facebook bocor. Password mereka? Disimpan dalam **plaintext**. Tidak ada enkripsi, tidak ada hash.

Dua insiden ini bisa dicegah dengan pengetahuan yang akan kita pelajari hari ini: cara menyimpan password dengan benar menggunakan bcrypt atau Argon2.

---

## 🧩 Konsep Utama

### Fungsi Hash vs Enkripsi: Perbedaan Fundamental

```
ENKRIPSI:
  Plaintext → [Key + Algoritma] → Ciphertext
  Ciphertext → [Key + Algoritma] → Plaintext (REVERSIBLE)

HASH:
  Plaintext → [Hash Function] → Hash/Digest
  Hash → ??? (TIDAK BISA DIKEMBALIKAN — one-way)

Analogi:
  Enkripsi = memasukkan surat ke amplop terkunci (bisa dibuka)
  Hash = membakar kertas menjadi abu (tidak bisa dikembalikan)
```

**Kapan menggunakan mana:**
* **Enkripsi:** Data yang perlu dipulihkan (NIK, nomor kartu, isi pesan)
* **Hash:** Data yang perlu diverifikasi tapi tidak perlu dipulihkan (password, integritas file)

### Sifat Kriptografis Fungsi Hash

Fungsi hash kriptografis harus memiliki 4 sifat:

```
1. DETERMINISTIC
   Input sama → Output SELALU sama
   SHA-256("hello") = "2cf24dba..." (selalu!)

2. ONE-WAY (Pre-image Resistance)
   Dari hash, tidak bisa rekonstruksi input
   Mengetahui "2cf24dba..." → tidak bisa temukan "hello"

3. COLLISION RESISTANCE
   Sangat sulit menemukan dua input berbeda dengan hash yang sama
   SHA-256("hello") ≠ SHA-256("world") (hampir tidak mungkin sama)

4. AVALANCHE EFFECT
   Perubahan kecil pada input → perubahan besar pada output
   SHA-256("hello") = "2cf24dba5fb0a30e26e83b2ac5b9e29e..."
   SHA-256("Hello") = "185f8db32921bd46d35cc83bff0b46..."
   ← Berbeda satu huruf, output SANGAT berbeda
```

### Perbandingan Algoritma Hash

```
┌──────────────┬────────────┬──────────────┬────────────────────────────┐
│ Algoritma    │ Output Size│ Status       │ Keterangan                 │
├──────────────┼────────────┼──────────────┼────────────────────────────┤
│ MD5          │ 128-bit    │ BROKEN       │ Collision attacks found    │
│              │            │ (2004)       │ Jangan untuk security      │
├──────────────┼────────────┼──────────────┼────────────────────────────┤
│ SHA-1        │ 160-bit    │ BROKEN       │ Google SHAttered (2017)    │
│              │            │ (2017)       │ Collision ditemukan        │
├──────────────┼────────────┼──────────────┼────────────────────────────┤
│ SHA-256      │ 256-bit    │ SECURE       │ Bagian SHA-2 family        │
│              │            │              │ Bitcoin, SSL certificates  │
├──────────────┼────────────┼──────────────┼────────────────────────────┤
│ SHA-3        │ 256/512-bit│ SECURE       │ Arsitektur berbeda (Keccak)│
│              │            │              │ Alternatif SHA-2           │
├──────────────┼────────────┼──────────────┼────────────────────────────┤
│ bcrypt       │ 60 chars   │ SECURE       │ Password hashing           │
│              │            │              │ Intentionally SLOW         │
├──────────────┼────────────┼──────────────┼────────────────────────────┤
│ Argon2       │ Variable   │ SECURE       │ Password hashing           │
│              │            │              │ Winner PHC 2015, NIST rec  │
└──────────────┴────────────┴──────────────┴────────────────────────────┘
```

### Use Case SHA-256 (Bukan Password)

SHA-256 bagus untuk **integritas data**, tapi TIDAK untuk password:

```python
import hashlib

# ✅ USE CASE YANG TEPAT: Verifikasi integritas file
def verify_file_integrity(filename: str, expected_hash: str) -> bool:
    with open(filename, 'rb') as f:
        file_hash = hashlib.sha256(f.read()).hexdigest()
    return file_hash == expected_hash

# ✅ USE CASE YANG TEPAT: HMAC untuk API request signing
import hmac
def sign_api_request(payload: str, secret: str) -> str:
    return hmac.new(
        secret.encode(),
        payload.encode(),
        hashlib.sha256
    ).hexdigest()

# ❌ JANGAN: Menggunakan SHA-256 langsung untuk password
# → Terlalu cepat, rentan rainbow table & brute force
password_hash = hashlib.sha256("password123".encode()).hexdigest()
```

### Mengapa SHA-256 TIDAK Aman untuk Password

```
Masalah SHA-256 untuk password:

1. TERLALU CEPAT:
   GPU modern (RTX 4090): ~22 MILIAR SHA-256/detik
   Brute force 8-char password (a-z, 0-9): 36^8 = ~2.8 triliun kombinasi
   → Selesai dalam 2 menit!

2. RAINBOW TABLE ATTACK:
   Pre-compute SHA-256 dari jutaan password umum
   Tabel rainbow: SHA-256("password") = "5e884898..."
   Jika database menggunakan SHA-256 tanpa salt:
   → Tinggal lookup di tabel → crack instan!

3. IDENTICAL PASSWORD = IDENTICAL HASH:
   User A: password = "qwerty" → hash = "65e84be..."
   User B: password = "qwerty" → hash = "65e84be..." (SAMA!)
   → Hacker tahu siapa yang punya password sama
```

### Salt: Mencegah Rainbow Table

```
SALT = nilai random yang ditambahkan sebelum hashing

TANPA SALT:
  hash("password123") = "ef92b..."
  hash("password123") = "ef92b..." (sama!)
  → Rainbow table bekerja

DENGAN SALT:
  salt1 = "x7k2m9"
  hash("password123" + salt1) = "3a8f1..." (berbeda!)
  
  salt2 = "p4n3q1"  
  hash("password123" + salt2) = "9c2e7..." (berbeda meskipun password sama!)
  → Rainbow table TIDAK bekerja karena setiap hash unik

Bagaimana menyimpan salt?
  Database: {username, password_hash, salt}
  Saat verifikasi: hash(input + salt) == stored_hash? 
  Salt tidak perlu rahasia — harus unik per user
```

### bcrypt: Password Hashing yang Benar

```
bcrypt adalah algoritma yang DIRANCANG untuk password:

1. BUILT-IN SALT: Otomatis generate random salt
2. CONFIGURABLE COST (work factor):
   cost=10 → ~100ms per hash (aman)
   cost=12 → ~400ms per hash (lebih aman, tapi lebih lambat)
   cost=14 → ~1.5s per hash (sangat aman)
   → GPU tidak bisa parallelkan seperti SHA-256!

Format output bcrypt:
  $2b$12$LQv3c1yqBWVHxkd0LHAkCOYz6TtxMQJqhN8/lewdq0Rl...
   │   │  └─────── 22 chars salt ──── 31 chars hash ──┘
   │   └─ cost factor (12)
   └─ version (2b)
```

### Argon2: Standar Modern (PHC Winner)

```
Argon2 adalah pemenang Password Hashing Competition 2015
→ Rekomendasi NIST terbaru untuk password hashing

Tiga varian:
• Argon2d: Tahan GPU attack, tapi rentan side-channel (jangan untuk password)
• Argon2i: Tahan side-channel, cocok untuk credential hashing
• Argon2id: HYBRID, rekomendasi untuk aplikasi umum

Parameter:
• time_cost: berapa kali iterasi (≥ 2)
• memory_cost: berapa MB RAM (≥ 64MB)
• parallelism: berapa thread
→ Memory-hard: GPU tidak efisien karena butuh RAM besar!
```

---

## 🧠 Ilustrasi / Analogi

**Fungsi hash seperti mesin penghancur kertas:**
* Dokumen → shredder → confetti (tidak bisa dipulihkan)
* Confetti berbeda untuk setiap dokumen (deterministic per input)
* Tidak bisa rekonstruksi dokumen asli dari confetti

**Salt seperti bumbu rahasia yang berbeda per masakan:**
* Tanpa garam: resep yang sama → rasa yang sama → mudah ditiru
* Dengan garam berbeda setiap masak: rasa berbeda meski bahan dasar sama
* Rainbow table = kamus rasa tanpa garam — tidak berguna kalau selalu pakai garam baru

**Mengapa bcrypt "lambat" itu bagus:**
* SHA-256: seperti mengunci pintu dengan kunci plastik — buka dalam detik
* bcrypt cost=12: seperti brankas bank — butuh waktu, tapi itulah poinnya
* Hacker yang mencoba 1 miliar password: SHA-256 = 45 detik, bcrypt = 400 TAHUN

---

## 💻 Contoh Teknis

### Password Hashing dengan bcrypt

```python
"""
Implementasi password security yang benar menggunakan bcrypt
Install: pip install bcrypt
"""
import bcrypt
import time

class PasswordManager:
    """
    Manajemen password yang aman dengan bcrypt.
    cost_factor: 10-14 (trade-off keamanan vs kecepatan)
    """
    
    def __init__(self, cost_factor: int = 12):
        self.cost_factor = cost_factor
    
    def hash_password(self, password: str) -> str:
        """
        Hash password dengan bcrypt (salt otomatis di-generate).
        Simpan hasil ini ke database — bukan password plaintext!
        """
        password_bytes = password.encode('utf-8')
        
        start = time.perf_counter()
        # bcrypt otomatis generate salt random
        hashed = bcrypt.hashpw(password_bytes, bcrypt.gensalt(rounds=self.cost_factor))
        elapsed = time.perf_counter() - start
        
        print(f"  Hashing time (cost={self.cost_factor}): {elapsed:.3f}s")
        return hashed.decode('utf-8')
    
    def verify_password(self, password: str, hashed: str) -> bool:
        """
        Verifikasi password saat login.
        JANGAN bandingkan hash langsung → gunakan bcrypt.checkpw()
        """
        password_bytes = password.encode('utf-8')
        hashed_bytes = hashed.encode('utf-8')
        return bcrypt.checkpw(password_bytes, hashed_bytes)
    
    def needs_rehash(self, hashed: str) -> bool:
        """
        Cek apakah hash perlu di-update (cost factor ditingkatkan).
        Panggil setelah user berhasil login.
        """
        current_rounds = bcrypt.checkpw.__module__
        return bcrypt.gensalt(self.cost_factor) != bcrypt.gensalt()


# ===== Simulasi Sistem Login Fintech =====

class UserAuth:
    """Sistem autentikasi dengan password hashing aman dan rate limiting"""
    
    def __init__(self):
        self.pm = PasswordManager(cost_factor=12)
        # Simulasi database: {username: {hash, failed_attempts, locked_until}}
        self.users_db = {}
        self.MAX_ATTEMPTS = 5
        self.LOCKOUT_SECONDS = 300  # 5 menit
    
    def register(self, username: str, password: str):
        """Register user baru dengan password hashing."""
        # Validasi strength password
        if len(password) < 8:
            raise ValueError("Password minimal 8 karakter")
        
        # Hash dan simpan
        password_hash = self.pm.hash_password(password)
        self.users_db[username] = {
            'password_hash': password_hash,
            'failed_attempts': 0,
            'locked_until': 0
        }
        print(f"User '{username}' berhasil terdaftar")
        print(f"Disimpan di DB: {password_hash}")  # hash, BUKAN password!
    
    def login(self, username: str, password: str) -> bool:
        """Login dengan rate limiting (account lockout)."""
        if username not in self.users_db:
            # Tetap hash meskipun user tidak ada → mencegah timing attack
            self.pm.hash_password(password)
            return False
        
        user = self.users_db[username]
        
        # Cek account lockout
        if time.time() < user['locked_until']:
            remaining = int(user['locked_until'] - time.time())
            raise Exception(f"Akun terkunci. Coba lagi dalam {remaining} detik")
        
        # Verifikasi password
        if self.pm.verify_password(password, user['password_hash']):
            user['failed_attempts'] = 0  # Reset counter
            print("Login berhasil!")
            return True
        else:
            user['failed_attempts'] += 1
            if user['failed_attempts'] >= self.MAX_ATTEMPTS:
                user['locked_until'] = time.time() + self.LOCKOUT_SECONDS
                print(f"Akun terkunci setelah {self.MAX_ATTEMPTS} percobaan gagal")
            else:
                remaining = self.MAX_ATTEMPTS - user['failed_attempts']
                print(f"Password salah. {remaining} percobaan tersisa")
            return False


# Demo
auth = UserAuth()
print("=== REGISTRASI ===")
auth.register("budi_santoso", "P@ssw0rd123!")

print("\n=== LOGIN ===")
auth.login("budi_santoso", "salah_password")
auth.login("budi_santoso", "P@ssw0rd123!")
```

### Password Hashing dengan Argon2 (Rekomendasi Modern)

```python
"""
Implementasi dengan Argon2id (PHC winner, NIST recommended)
Install: pip install argon2-cffi
"""
from argon2 import PasswordHasher, exceptions
import time

# Konfigurasi Argon2id yang direkomendasikan OWASP
ph = PasswordHasher(
    time_cost=2,        # Jumlah iterasi
    memory_cost=65536,  # 64MB RAM — menyulitkan GPU attack
    parallelism=2,      # Thread paralel
    hash_len=32,        # Output hash 32 bytes
    salt_len=16         # Salt 16 bytes (otomatis)
)

def hash_password_argon2(password: str) -> str:
    """Hash password dengan Argon2id. Salt otomatis di-generate."""
    return ph.hash(password)

def verify_password_argon2(password: str, hash_str: str) -> bool:
    """Verifikasi dan auto-rehash jika parameter sudah outdated."""
    try:
        ph.verify(hash_str, password)
        
        # Cek apakah perlu rehash (misal: parameter ditingkatkan)
        if ph.check_needs_rehash(hash_str):
            new_hash = hash_password_argon2(password)
            print(f"  [INFO] Password di-rehash dengan parameter baru")
            return True, new_hash
        return True, None
    except exceptions.VerifyMismatchError:
        return False, None

# Demo
password = "MySecureP@ss2025!"
hash_result = hash_password_argon2(password)
print(f"Argon2 hash: {hash_result}")
# Output: $argon2id$v=19$m=65536,t=2,p=2$...

success, new_hash = verify_password_argon2(password, hash_result)
print(f"Verification: {success}")
```

---

## 🏢 Studi Kasus: Insiden Password LinkedIn (2012) dan Pelajarannya

**Kronologi:**
* Juni 2012: Hacker mempublikasikan 6.5 juta hash password LinkedIn
* Password disimpan sebagai SHA-1 **tanpa salt**
* Dalam 3 hari: 90% hash ter-crack menggunakan rainbow table
* 2016: Ternyata 117 juta akun terdampak (bukan hanya 6.5 juta)

**Root cause:**

```
KESALAHAN LINKEDIN:
1. SHA-1 (deprecated) bukan bcrypt/Argon2
2. Tidak ada salt → rainbow table langsung bekerja
3. Tidak ada monitoring untuk deteksi breach

Illustrasi serangan:
  Database LinkedIn bocor:
  {email: "alice@gmail.com", hash: "5baa61e4..."}
  
  Hacker punya pre-computed rainbow table:
  {"password": "5baa61e4..."} → Match dalam 0.001 detik!
  
  Jika menggunakan bcrypt dengan salt:
  {email: "alice@gmail.com", hash: "$2b$12$LQv3c1yq..."}
  → Hacker harus brute-force SETIAP user secara individual
  → Dengan bcrypt cost=12: 1 hash/400ms → 117 juta hash = 1800 tahun
```

**Dampak bisnis:**
* LinkedIn didenda $1.25 juta oleh FTC (Federal Trade Commission)
* Reputasi rusak besar-besaran
* Paksa reset password semua pengguna (gangguan layanan masif)

**Apa yang harus dilakukan:**
1. Gunakan bcrypt minimum cost=10, atau Argon2id
2. SELALU salt (bcrypt otomatis, Argon2 otomatis)
3. Jika upgrade dari hash lama: re-hash saat user login berikutnya
4. Monitor: alert jika ada > 100 failed login/menit (credential stuffing)

---

## ⚠️ Kesalahan Umum

1. **Menyimpan password dalam plaintext** → "Kami butuh password untuk reset" → SALAH. Gunakan secure password reset flow (email link token). TIDAK ADA alasan untuk menyimpan password plaintext.

2. **Menggunakan MD5 atau SHA-256 untuk password** → Terlalu cepat. GPU bisa coba miliaran kombinasi per detik. SELALU gunakan bcrypt atau Argon2.

3. **Menggunakan bcrypt cost terlalu rendah (< 10)** → Cost=4 mengambil < 1ms per hash. NIST merekomendasikan minimal 100ms. Cost=12 memberikan ~400ms.

4. **Membandingkan hash dengan `==` bukan fungsi constant-time** → `hash1 == hash2` di Python rentan timing attack. Gunakan `bcrypt.checkpw()` atau `hmac.compare_digest()`.

5. **Lupa increment cost factor seiring waktu** → Hardware semakin cepat. bcrypt cost=10 yang "aman" tahun 2010 mungkin perlu ditingkatkan ke cost=12 tahun 2025. Saat user login, cek apakah perlu rehash.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan antara fungsi hash dan enkripsi. Mengapa password harus di-hash bukan dienkripsi?

b) Apa itu rainbow table attack? Bagaimana salt mencegah serangan ini? Apakah salt perlu dirahasiakan?

c) Mengapa SHA-256 tidak cocok untuk menyimpan password, meskipun SHA-256 sendiri kriptografis aman?

### Soal 2 — Praktik

Buat program Python "Password Security Audit" yang:
1. Implementasikan hashing dengan bcrypt (cost=12) dan Argon2id
2. Tunjukkan demonstrasi "kelemahan" MD5 dengan brute-force dictionary attack (gunakan list 10 password umum)
3. Buat fungsi `password_strength_check(password)` yang memvalidasi:
   - Minimal 8 karakter
   - Ada huruf besar dan kecil
   - Ada angka
   - Ada karakter spesial
   - Tidak ada dalam daftar common passwords
4. Implementasikan rate limiting: lockout 5 menit setelah 5 percobaan gagal

---

## 📌 Ringkasan

* **Fungsi hash:** One-way, deterministic, collision-resistant — TIDAK bisa dibalik
* **MD5/SHA-1:** BROKEN (collision ditemukan). **SHA-256:** aman untuk integritas, TAPI terlalu cepat untuk password
* **Masalah password hashing:** SHA-256 terlalu cepat → brute force dan rainbow table efektif
* **Salt** = random string unik per user → mencegah rainbow table
* **bcrypt:** Built-in salt + configurable cost factor → semakin tinggi cost, semakin lambat (bagus!)
* **Argon2id:** PHC winner, NIST recommended, memory-hard → GPU tidak efisien
* **Rate limiting** = lockout setelah N percobaan gagal → mencegah online brute force
* Kasus LinkedIn: 117 juta hash SHA-1 tanpa salt → 90% ter-crack dalam hari

---

*📚 Referensi: NIST SP 800-63B — Digital Identity Guidelines | OWASP Password Storage Cheat Sheet | Python argon2-cffi docs | Python bcrypt docs | Stallings, W. (2022). Cryptography and Network Security, 8th Ed.*
