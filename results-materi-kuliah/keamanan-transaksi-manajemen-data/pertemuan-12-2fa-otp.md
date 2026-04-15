# Pertemuan 12: Two-Factor Authentication (2FA) & OTP Implementation

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan faktor autentikasi: something you know/have/are
* Membandingkan SMS OTP, TOTP (Google Authenticator), HOTP, dan FIDO2
* Mengimplementasikan TOTP menggunakan Python (library pyotp)
* Mengintegrasikan 2FA ke dalam sistem login sederhana
* Menjelaskan kelemahan SMS OTP dan alternatif yang lebih aman

---

## 📖 Pengantar (Hook)

2019: Akun GoPay pengguna dibobol, saldo Rp 8 juta lenyap. Modus operandi: hacker menelepon Telkomsel dengan berpura-pura sebagai korban, melakukan SIM swap — nomor telepon korban dipindahkan ke SIM card hacker.

Hasil: SMS OTP sekarang masuk ke HP hacker, bukan korban. Login dua langkah pun dilangkahi.

Ini bukan kelemahan GoPay — tapi kelemahan SMS OTP secara fundamental. Itulah mengapa NIST (standar keamanan AS) merekomendasikan untuk TIDAK menggunakan SMS OTP untuk autentikasi sensitif, dan mendorong TOTP atau FIDO2.

---

## 🧩 Konsep Utama

### Faktor Autentikasi: Tiga Kategori

```
SESUATU YANG KAMU TAHU (Knowledge Factor):
  • Password
  • PIN
  • Security question ("nama ibu kandung?")
  
  Kelemahan: Bisa ditebak, dicuri via phishing, bocor di database breach

SESUATU YANG KAMU PUNYA (Possession Factor):
  • HP dengan SMS OTP
  • Authenticator app (TOTP)
  • Hardware token (YubiKey)
  • Smart card
  
  Kelemahan: Bisa hilang/dicuri, SIM swap (SMS), supply chain

SESUATU YANG MERUPAKAN DIRIMU (Inherence Factor):
  • Sidik jari
  • Pengenalan wajah
  • Suara
  • Iris mata
  
  Kelemahan: Tidak bisa "reset" jika compromise, privasi concern

MULTI-FACTOR AUTHENTICATION (MFA):
  Minimal 2 faktor dari kategori BERBEDA
  
  SMS OTP = knowledge (password) + possession (HP)  → 2FA ✓
  PIN + OTP via app = knowledge + possession → 2FA ✓
  Password + security question = knowledge + knowledge → BUKAN 2FA
```

### Jenis OTP dan Perbandingan

#### SMS OTP

```
CARA KERJA:
  1. User input username + password
  2. Server generate random 6 digit OTP
  3. OTP dikirim via SMS ke nomor terdaftar
  4. User input OTP dalam window 5 menit
  5. Server verifikasi

KELEMAHAN KRITIS:
  ① SIM Swapping:
     Hacker telepon operator → "Nomor saya rusak, tolong pindahkan ke SIM baru"
     → SMS sekarang ke HP hacker
  
  ② SS7 Network Vulnerability:
     Jaringan SS7 (2G/3G signaling) memiliki kerentanan
     → SMS bisa diintercepted
  
  ③ Phishing OTP:
     Website palsu meminta OTP "untuk verifikasi"
     → User masukkan OTP ke hacker secara real-time
  
  ④ Malware:
     Android malware bisa forward SMS ke hacker

Rekomendasi: Gunakan untuk use case low-risk (login biasa).
JANGAN untuk transfer besar atau aksi sensitif tanpa alternatif.
```

#### TOTP (Time-based One-Time Password) — RFC 6238

```
CARA KERJA:
  1. Saat setup 2FA: Server dan user app share SECRET KEY
     (disimpan di authenticator app — Google Authenticator, Authy)
  
  2. Saat login: KEDUANYA (server dan app) menghitung:
     TOTP = TRUNCATE(HMAC-SHA1(secret, floor(unix_time / 30)))
     
  3. Kode berubah setiap 30 detik
  4. User input 6-digit kode dari app
  5. Server hitung sendiri → bandingkan

MENGAPA AMAN:
  ✓ Tidak ada yang dikirim via SMS → tidak bisa SIM swap
  ✓ Kode berlaku hanya 30 detik → replay attack tidak efektif
  ✓ Rahasia (secret key) tidak pernah ditransmisikan setelah setup
  ✓ Bekerja offline (tidak perlu internet/sinyal)
  ✓ Phishing lebih sulit (hacker harus real-time dan dalam 30 detik)

SETUP:
  Server generate random secret → encode ke Base32 → tampilkan QR Code
  User scan dengan authenticator app → secret tersimpan di app
```

#### HOTP (HMAC-based One-Time Password) — RFC 4226

```
HOTP = OTP berdasarkan COUNTER (bukan waktu)

TOTP = TRUNCATE(HMAC-SHA1(secret, counter))
  counter di-increment setiap kali digunakan (bukan waktu)

Digunakan untuk:
  • Hardware token (tanpa jam internal yang akurat)
  • Recovery codes (counter-based, single use)

Kekurangan vs TOTP:
  • Counter desync bisa terjadi → perlu re-sync
  • Kode tidak expire sampai digunakan (kurang aman jika dicuri)
```

#### FIDO2 / WebAuthn: Standard Modern Terkuat

```
FIDO2 = Fast Identity Online 2 (protokol industri 2018)
WebAuthn = W3C standard, browser API untuk FIDO2

CARA KERJA:
  1. Registrasi: Device generate key pair RSA/ECC
     Public key → server
     Private key → TERSIMPAN DI DEVICE (tidak bisa diekstrak)
  
  2. Autentikasi:
     Server kirim "challenge" (random nonce)
     Device SIGN challenge dengan private key
     Server verifikasi signature dengan public key

KEUNGGULAN:
  ✓ Private key tidak pernah meninggalkan device → phishing proof
  ✓ Public key per-origin → phishing dari domain berbeda GAGAL
  ✓ Tidak ada shared secret → tidak bisa di-phish
  ✓ Biometrik di device (Face ID, fingerprint) sebagai authorization
  ✓ Didukung Chrome, Firefox, Safari, Edge

IMPLEMENTASI:
  Hardware key: YubiKey (physical USB/NFC key)
  Platform authenticator: Passkey (Apple, Google, Microsoft)
  
  Catatan: Passkeys (2022+) adalah implementasi FIDO2 consumer-friendly
  → "Login tanpa password" dengan Face ID/fingerprint/PIN perangkat
```

---

## 🧠 Ilustrasi / Analogi

**2FA seperti ATM:**
* Kartu ATM = something you have (possession)
* PIN = something you know (knowledge)
* Satu faktor saja tidak cukup — butuh keduanya

**TOTP seperti password yang berubah setiap 30 detik:**
* Bayangkan kamu dan bank punya jam sinkron + buku kode rahasia yang sama
* Setiap 30 detik: lihat halaman buku → ada 6 digit → itu kode untuk login menit ini
* 30 detik kemudian: halaman berubah → kode baru → kode lama tidak berlaku

**FIDO2/Passkey seperti kunci pintu fisik:**
* Duplikat kunci tidak bisa dibuat tanpa original
* Kunci hanya cocok untuk satu pintu tertentu (domain binding — anti-phishing)
* Kalau pintu berubah (domain berbeda), kunci tidak muat

---

## 💻 Contoh Teknis

### Implementasi TOTP dengan pyotp

```python
"""
Implementasi Two-Factor Authentication menggunakan TOTP
Install: pip install pyotp qrcode[pil] bcrypt flask
"""
import pyotp
import qrcode
import bcrypt
import os
import secrets
import time
from io import BytesIO
import base64

class TwoFactorAuth:
    """
    Sistem 2FA lengkap dengan TOTP, backup codes, dan rate limiting.
    """
    
    # Simulasi database (dalam produksi: gunakan database nyata)
    _users_db = {}
    _login_attempts = {}
    
    MAX_ATTEMPTS = 5
    LOCKOUT_DURATION = 300  # 5 menit
    
    @classmethod
    def register_user(cls, username: str, password: str, email: str) -> dict:
        """
        Register user baru dengan setup 2FA.
        Returns: secret key dan QR code image (base64)
        """
        # Hash password
        password_hash = bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12))
        
        # Generate TOTP secret (160-bit random)
        totp_secret = pyotp.random_base32()
        
        # Generate 8 backup codes (untuk recovery jika kehilangan phone)
        backup_codes = [secrets.token_hex(4).upper() for _ in range(8)]
        backup_codes_hashed = [
            bcrypt.hashpw(code.encode(), bcrypt.gensalt(rounds=10)).decode()
            for code in backup_codes
        ]
        
        cls._users_db[username] = {
            'email': email,
            'password_hash': password_hash.decode(),
            'totp_secret': totp_secret,
            'totp_enabled': False,  # Belum aktif sampai user verify setup
            'backup_codes': backup_codes_hashed,
            'backup_codes_used': [],
            'created_at': time.time()
        }
        
        # Generate QR code untuk Google Authenticator
        totp = pyotp.TOTP(totp_secret)
        otp_auth_url = totp.provisioning_uri(
            name=email,
            issuer_name="FinTechApp"
        )
        # Format: otpauth://totp/FinTechApp:email?secret=...&issuer=...
        
        qr = qrcode.QRCode(version=1, box_size=10, border=5)
        qr.add_data(otp_auth_url)
        qr.make(fit=True)
        img = qr.make_image(fill_color="black", back_color="white")
        
        buffer = BytesIO()
        img.save(buffer, format='PNG')
        qr_base64 = base64.b64encode(buffer.getvalue()).decode()
        
        return {
            'username': username,
            'totp_secret': totp_secret,  # Tampilkan ke user untuk manual entry
            'qr_code_base64': qr_base64,
            'backup_codes': backup_codes,  # Tampilkan SEKALI, minta user simpan!
            'setup_url': otp_auth_url
        }
    
    @classmethod
    def verify_totp_setup(cls, username: str, totp_code: str) -> bool:
        """
        Verifikasi bahwa user berhasil setup authenticator.
        User harus input kode dari app untuk konfirmasi setup berhasil.
        """
        user = cls._users_db.get(username)
        if not user:
            return False
        
        totp = pyotp.TOTP(user['totp_secret'])
        # valid_window=1: terima kode 30 detik sebelum dan sesudahnya (clock skew)
        if totp.verify(totp_code, valid_window=1):
            user['totp_enabled'] = True
            return True
        return False
    
    @classmethod
    def login_step1(cls, username: str, password: str) -> dict:
        """
        Login step 1: Verifikasi username + password.
        Returns 'requires_2fa' jika 2FA enabled.
        """
        user = cls._users_db.get(username)
        
        # Rate limiting
        attempt_key = username
        attempts = cls._login_attempts.get(attempt_key, {'count': 0, 'locked_until': 0})
        
        if time.time() < attempts['locked_until']:
            wait = int(attempts['locked_until'] - time.time())
            raise PermissionError(f"Akun terkunci. Coba lagi dalam {wait} detik.")
        
        # Selalu lakukan bcrypt check (constant time) — mencegah user enumeration
        if not user:
            bcrypt.checkpw(password.encode(), bcrypt.hashpw(b'dummy', bcrypt.gensalt()))
            return {'success': False, 'error': 'Invalid credentials'}
        
        if not bcrypt.checkpw(password.encode(), user['password_hash'].encode()):
            attempts['count'] = attempts.get('count', 0) + 1
            if attempts['count'] >= cls.MAX_ATTEMPTS:
                attempts['locked_until'] = time.time() + cls.LOCKOUT_DURATION
            cls._login_attempts[attempt_key] = attempts
            
            remaining = cls.MAX_ATTEMPTS - attempts['count']
            return {
                'success': False, 
                'error': f'Invalid credentials. {max(0, remaining)} attempts remaining'
            }
        
        # Reset attempts on successful password
        cls._login_attempts[attempt_key] = {'count': 0, 'locked_until': 0}
        
        if user.get('totp_enabled'):
            # Generate temporary token untuk 2FA step
            temp_token = secrets.token_urlsafe(32)
            user['pending_2fa_token'] = {
                'token': temp_token,
                'expires': time.time() + 300  # 5 menit untuk complete 2FA
            }
            return {
                'success': True,
                'requires_2fa': True,
                'temp_token': temp_token  # Kirim ke frontend untuk step 2
            }
        
        return {'success': True, 'requires_2fa': False, 'user': username}
    
    @classmethod
    def login_step2(cls, username: str, temp_token: str, totp_code: str) -> dict:
        """
        Login step 2: Verifikasi TOTP kode.
        """
        user = cls._users_db.get(username)
        if not user:
            return {'success': False, 'error': 'Invalid request'}
        
        # Verifikasi temp token
        pending = user.get('pending_2fa_token', {})
        if (pending.get('token') != temp_token or 
                time.time() > pending.get('expires', 0)):
            return {'success': False, 'error': 'Session expired. Please login again.'}
        
        # Cek apakah ini backup code (format: XXXXXXXX)
        if len(totp_code) == 8 and totp_code.isupper():
            return cls._verify_backup_code(username, totp_code)
        
        # Verifikasi TOTP
        totp = pyotp.TOTP(user['totp_secret'])
        if totp.verify(totp_code, valid_window=1):
            del user['pending_2fa_token']  # Clear temp token
            return {'success': True, 'user': username, 'session': 'created'}
        
        return {'success': False, 'error': 'Invalid TOTP code'}
    
    @classmethod
    def _verify_backup_code(cls, username: str, backup_code: str) -> dict:
        """Verifikasi dan invalidasi backup code (single use)."""
        user = cls._users_db[username]
        
        for i, hashed_code in enumerate(user['backup_codes']):
            if i in user['backup_codes_used']:
                continue  # Kode ini sudah digunakan
            
            if bcrypt.checkpw(backup_code.encode(), hashed_code.encode()):
                user['backup_codes_used'].append(i)  # Mark sebagai used
                remaining = len(user['backup_codes']) - len(user['backup_codes_used'])
                return {
                    'success': True,
                    'user': username,
                    'warning': f'Backup code used. {remaining} backup codes remaining.'
                }
        
        return {'success': False, 'error': 'Invalid backup code'}


# ===== DEMO =====
print("=== REGISTRASI ===")
result = TwoFactorAuth.register_user("budi", "P@ssw0rd123!", "budi@email.com")
print(f"TOTP Secret: {result['totp_secret']}")
print(f"Backup codes: {result['backup_codes']}")
print(f"QR Code URL tersedia untuk scan dengan Google Authenticator")

print("\n=== SIMULATE TOTP CODE (dalam production, user ambil dari app) ===")
secret = result['totp_secret']
totp = pyotp.TOTP(secret)
current_code = totp.now()
print(f"TOTP code sekarang: {current_code} (valid 30 detik)")

print("\n=== VERIFIKASI SETUP ===")
setup_ok = TwoFactorAuth.verify_totp_setup("budi", current_code)
print(f"Setup verified: {setup_ok}")

print("\n=== LOGIN 2FA ===")
step1 = TwoFactorAuth.login_step1("budi", "P@ssw0rd123!")
print(f"Step 1: {step1}")

if step1.get('requires_2fa'):
    # Ambil kode baru (mungkin sudah berganti dalam 30 detik)
    new_code = pyotp.TOTP(secret).now()
    step2 = TwoFactorAuth.login_step2("budi", step1['temp_token'], new_code)
    print(f"Step 2: {step2}")
```

---

## 🏢 Studi Kasus: Migrasi GoPay dari SMS OTP ke TOTP

**Konteks:** Setelah serangkaian insiden SIM swap (2019-2020), GoPay meningkatkan keamanan autentikasi.

**Langkah migrasi:**

```
SEBELUM (SMS OTP only):
  Login → SMS OTP → Akses

MASALAH:
  • SIM swap attack: hacker pindahkan nomor ke SIM baru → SMS ke hacker
  • Phishing OTP: website palsu → user kirim OTP ke hacker
  • 2020: ratusan kasus pembobolan akun via SIM swap dilaporkan

STRATEGI MIGRASI (bertahap):
  Phase 1: Tambahkan TOTP sebagai OPSI (opt-in)
    → User bisa pilih: SMS atau TOTP
    → Insentif: limit transfer lebih tinggi dengan TOTP
  
  Phase 2: Wajibkan 2FA untuk transaksi > Rp 1 juta
    → SMS OTP masih boleh, tapi encourage TOTP
    → Tambahkan warning di UI tentang risiko SMS OTP
  
  Phase 3: TOTP sebagai default untuk akun business
    → Business account (merchant) wajib TOTP
    → Consumer: optional tapi sangat direkomendasikan

HASIL:
  • Insiden SIM swap turun >80% pada akun yang menggunakan TOTP
  • TOTP adoption: 35% user aktif dalam 6 bulan
  • Support ticket terkait account takeover turun signifikan
```

---

## ⚠️ Kesalahan Umum

1. **Menganggap SMS OTP = aman karena "sudah 2FA"** → SMS OTP rentan SIM swap dan SS7 attack. Untuk aplikasi keuangan dengan nilai transaksi tinggi, SMS OTP saja tidak cukup. Sediakan TOTP sebagai alternatif.

2. **Tidak ada backup codes** → Jika user kehilangan HP, tidak bisa login. SELALU berikan 8-10 backup codes saat setup 2FA. Backup codes harus single-use dan di-hash di database.

3. **TOTP window terlalu lebar** → `valid_window=10` berarti kode berlaku 5 menit sebelum dan sesudahnya. Ini melonggarkan keamanan. Gunakan `valid_window=1` (30 detik toleransi) kecuali ada masalah clock skew.

4. **Menyimpan TOTP secret dalam plaintext** → Secret TOTP adalah credential sensitif. Enkripsi dengan AES-256-GCM sebelum menyimpan ke database.

5. **Tidak ada rate limiting pada 2FA verification** → Hacker bisa brute force 6-digit TOTP (1 juta kombinasi / 30 detik = sangat mungkin). Rate limit: max 5 percobaan, lalu lockout.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan antara TOTP dan HOTP. Mana yang lebih aman untuk aplikasi mobile?

b) Mengapa SMS OTP rentan terhadap SIM swapping? Apa yang bisa dilakukan penyedia layanan untuk mengurangi risiko ini (selain menghapus SMS OTP)?

c) Apa itu FIDO2/Passkey? Mengapa Passkey dianggap lebih aman dari TOTP untuk mencegah phishing?

### Soal 2 — Praktik

Buat aplikasi login Python/Flask dengan 2FA:
1. Registrasi user: password (hashed bcrypt) + setup TOTP (tampilkan QR code)
2. Login 2 tahap: step 1 (password) → step 2 (TOTP code)
3. Implementasikan account lockout: 5 percobaan gagal → kunci 5 menit
4. Generate dan verifikasi backup codes (8 kode, single-use)
5. Tambahkan endpoint `/2fa/disable` dengan re-konfirmasi password + TOTP

---

## 📌 Ringkasan

* **Tiga faktor:** Knowledge (password), Possession (HP/token), Inherence (biometrik)
* **2FA** = dua faktor dari kategori BERBEDA
* **SMS OTP:** mudah tapi rentan SIM swap, SS7 vulnerability, phishing OTP
* **TOTP (RFC 6238):** HMAC-SHA1(secret, time/30) — berubah setiap 30 detik, offline, lebih aman
* **Backup codes:** 8-10 kode single-use untuk recovery — harus di-hash di database
* **FIDO2/Passkey:** standard terkuat — private key di device, domain binding mencegah phishing
* Rate limiting pada 2FA verification sama pentingnya dengan pada login password
* Sediakan TOTP sebagai alternatif SMS OTP, terutama untuk akun bisnis/nilai transaksi tinggi

---

*📚 Referensi: RFC 6238 — TOTP | RFC 4226 — HOTP | NIST SP 800-63B — Digital Identity | Python pyotp docs | FIDO Alliance — fidoalliance.org | Anderson, R. (2020). Security Engineering*
