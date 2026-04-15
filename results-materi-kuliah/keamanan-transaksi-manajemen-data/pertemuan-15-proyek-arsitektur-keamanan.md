# Pertemuan 15: Proyek Final — Arsitektur Keamanan E-Commerce

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Merancang arsitektur keamanan end-to-end untuk sistem e-commerce
* Mengintegrasikan semua komponen: HTTPS, autentikasi, enkripsi DB, audit log
* Melakukan threat modeling dan menunjukkan kontrol mitigasi
* Mendemonstrasikan kepatuhan terhadap UU PDP dan PCI-DSS
* Mempresentasikan rancangan secara profesional dengan Q&A

---

## 📖 Pengantar (Hook)

Ini adalah puncak dari semua yang telah kita pelajari.

Satu per satu, kita telah belajar:
* CIA Triad dan mengapa keamanan penting
* AES-256 untuk enkripsi data sensitif
* RSA dan ECDHE untuk keamanan komunikasi
* bcrypt/Argon2 untuk melindungi password
* Digital Signature untuk non-repudiation
* MySQL security dan Row-Level Security
* SQL Injection prevention
* Audit log dan backup terenkripsi
* HTTPS/TLS dan certificate management
* Payment Gateway dan PCI-DSS
* 2FA dan TOTP
* UU PDP dan GDPR
* Risk Assessment dan STRIDE

Sekarang saatnya menyatukan semua ini dalam satu rancangan arsitektur yang komprehensif — seperti yang dilakukan arsitek keamanan di perusahaan nyata.

---

## 🧩 Panduan Proyek Final

### Deskripsi Proyek

Rancang arsitektur keamanan untuk **"ShopSecure"** — platform e-commerce fiktif yang menjual produk digital (software license, e-book, kursus online).

**Karakteristik sistem:**
* 100.000+ pengguna terdaftar
* 10.000 transaksi per hari (peak: 50.000 saat promo)
* Menerima pembayaran: kartu kredit, transfer bank, e-wallet
* Data sensitif: data kartu, NIK (untuk verifikasi), email, riwayat pembelian

### Deliverables Proyek (Kelompok 3-4 Orang)

#### 1. Threat Model (STRIDE) — 20%

```
TEMPLATE THREAT MODEL:

Sistem: ShopSecure E-Commerce

Komponen yang dianalisis:
  □ Frontend (React/Vue)
  □ API Gateway
  □ Backend Services
  □ Database Layer
  □ Payment Integration
  □ User Authentication

Untuk setiap komponen, identifikasi threat STRIDE dan kontrol mitigasi.

Format:
┌─────────────────────────────────────────────────────────────────┐
│ Komponen: API Gateway                                           │
├─────────────────────────────────────────────────────────────────┤
│ S - Spoofing:                                                   │
│   Threat: Attacker forge JWT token untuk akses admin API        │
│   Kontrol: JWT signature verification, short expiry (15 min)   │
│            Refresh token rotation, IP binding                   │
├─────────────────────────────────────────────────────────────────┤
│ T - Tampering:                                                  │
│   Threat: MITM modify request payload (ubah harga checkout)    │
│   Kontrol: HTTPS/TLS 1.3, request body signing (HMAC)          │
├─────────────────────────────────────────────────────────────────┤
│ ... (lengkapi semua 6 STRIDE category)                          │
└─────────────────────────────────────────────────────────────────┘
```

#### 2. Risk Matrix — 15%

```
Identifikasi minimal 10 risiko dengan:
- Risk ID, Deskripsi, STRIDE category
- Likelihood (1-5), Impact (1-5), Score
- Current controls, Planned controls
- Risk treatment (Accept/Mitigate/Transfer/Avoid)
- Residual risk
- Owner dan target date

Top 3 critical risks harus dilengkapi dengan detailed mitigation plan.
```

#### 3. Security Architecture Diagram — 25%

```
Diagram harus mencakup:

NETWORK LAYER:
  □ WAF (Web Application Firewall)
  □ DDoS protection (Cloudflare/AWS Shield)
  □ Load Balancer dengan TLS termination
  □ DMZ untuk public-facing services
  □ Private network untuk database

APPLICATION LAYER:
  □ HTTPS/TLS 1.3 untuk semua komunikasi
  □ API Gateway dengan rate limiting
  □ Authentication service (JWT + 2FA)
  □ Session management

DATA LAYER:
  □ Encrypted database (AES-256 untuk kolom sensitif)
  □ Least privilege DB users
  □ Read replica untuk analytics (bukan production DB)
  □ Encrypted backup (offsite)

MONITORING LAYER:
  □ Audit log centralized
  □ SIEM (Security Information and Event Management)
  □ Alert untuk anomali
  □ Certificate expiry monitoring
```

**Contoh diagram (text representation):**

```
ARSITEKTUR SHOPSECURE:

Internet
    │
    ▼
┌─────────────────────────────────────────────────────────────────┐
│  Cloudflare (DDoS Protection + WAF)                             │
│  • Rules: SQL injection, XSS, RCE patterns                      │
│  • Rate limiting: 100 req/s per IP                              │
└─────────────────────┬───────────────────────────────────────────┘
                      │ HTTPS only
                      ▼
┌──────────────────────────────────────────────────────────────────┐
│  Load Balancer (AWS ALB)                                         │
│  • TLS termination (certificate dari ACM)                        │
│  • HTTPS redirect (HTTP → HTTPS 301)                             │
│  • Health checks                                                 │
└──────────┬──────────────────────┬────────────────────────────────┘
           │                      │
           ▼                      ▼
    ┌────────────┐          ┌────────────┐
    │ API Server │          │ API Server │
    │ (ECS/EKS)  │          │ (ECS/EKS)  │
    │            │          │            │
    │ • JWT auth │          │ • JWT auth │
    │ • 2FA      │          │ • 2FA      │
    │ • Rate limit│         │ • Rate limit│
    └──────┬─────┘          └──────┬─────┘
           │                      │
           ▼                      │
┌─────────────────────────────────────────────────────────────────┐
│  VPC Private Subnet                                             │
├──────────────────────────┬──────────────────────────────────────┤
│ MySQL Primary (RDS)      │ Audit Log DB (separate instance)     │
│ • AES-256 column encrypt │ • Write-only from app                │
│ • App user: SELECT,      │ • Separate credentials               │
│   INSERT, UPDATE         │ • 90-day retention → S3 archive      │
│ • No DDL from app        │                                      │
├──────────────────────────┴──────────────────────────────────────┤
│ AWS KMS (Key Management)                                        │
│ • AES-256 keys untuk enkripsi DB                                │
│ • Audit log setiap penggunaan key                               │
│ • Rotation otomatis setiap 90 hari                              │
└─────────────────────────────────────────────────────────────────┘
                      │ Payment
                      ▼
              ┌─────────────┐
              │   Midtrans  │ ← PCI-DSS Level 1
              │   (Payment  │   Kartu tidak menyentuh
              │   Gateway)  │   server ShopSecure!
              └─────────────┘
```

#### 4. Implementasi Minimal 3 Kontrol Keamanan — 30%

```python
"""
KONTROL KEAMANAN YANG HARUS DIIMPLEMENTASIKAN:
(Pilih minimal 3 dari 5 opsi berikut)

OPSI 1: Enkripsi data sensitif (AES-256-GCM)
  - Fungsi encrypt/decrypt untuk kolom NIK dan nomor kartu
  - Simpan ke database
  - Verifikasi tampering detection

OPSI 2: SQL Injection Prevention
  - Buat endpoint yang rentan
  - Perbaiki dengan prepared statements
  - Test dengan input berbahaya

OPSI 3: 2FA dengan TOTP
  - Register dengan setup TOTP
  - Login 2 langkah
  - Backup codes

OPSI 4: Audit Logging
  - MySQL trigger untuk tabel transaksi
  - Query forensik
  - Alert untuk anomali (DELETE masal)

OPSI 5: Security Headers & HTTPS Configuration
  - HSTS header
  - Content-Security-Policy
  - X-Frame-Options
  - X-Content-Type-Options
"""

# TEMPLATE IMPLEMENTASI — Integrasikan dalam satu Flask application

from flask import Flask, request, jsonify, session
import os

app = Flask(__name__)

# ============================================================
# SECURITY HEADERS (wajib ada di semua response)
# ============================================================
@app.after_request
def add_security_headers(response):
    """
    Tambahkan security headers ke semua response.
    Ini adalah kontrol keamanan yang sering terlupakan tapi mudah diimplementasikan.
    """
    # HSTS: paksa HTTPS untuk 1 tahun (termasuk subdomain)
    response.headers['Strict-Transport-Security'] = \
        'max-age=31536000; includeSubDomains; preload'
    
    # Prevent clickjacking
    response.headers['X-Frame-Options'] = 'DENY'
    
    # Prevent MIME sniffing
    response.headers['X-Content-Type-Options'] = 'nosniff'
    
    # Content Security Policy (basic)
    response.headers['Content-Security-Policy'] = \
        "default-src 'self'; script-src 'self' https://api.midtrans.com; " \
        "object-src 'none'; frame-ancestors 'none'"
    
    # Referrer Policy
    response.headers['Referrer-Policy'] = 'strict-origin-when-cross-origin'
    
    # Remove server version info
    response.headers.pop('X-Powered-By', None)
    response.headers.pop('Server', None)
    
    return response
```

#### 5. Analisis Kepatuhan UU PDP & PCI-DSS — 10%

```
CHECKLIST KEPATUHAN:

UU PDP Compliance:
  □ Privacy Policy tersedia dalam Bahasa Indonesia
  □ Consent granular untuk setiap purpose penggunaan data
  □ Fitur export data pengguna (data portability)
  □ Fitur hapus akun (right to erasure)
  □ Data sensitif (NIK) dienkripsi
  □ Retention policy didefinisikan
  □ DPO / contact person privasi tersedia
  □ Incident response plan untuk breach notification

PCI-DSS Compliance:
  □ Tidak menyimpan CVV sama sekali
  □ Nomor kartu tidak melewati server (Midtrans.js tokenization)
  □ HTTPS/TLS 1.2+ untuk semua komunikasi
  □ Akses ke cardholder data di-log (audit trail)
  □ Penggunaan payment gateway (reduce PCI scope ke SAQ-A)
  □ Dependency scanning untuk vulnerable libraries
  □ Penetration testing sebelum production launch

Format laporan:
  Untuk setiap item: ✓ Compliant / ✗ Non-compliant / ⚠️ Partial
  Untuk yang Non-compliant: jelaskan gap dan action plan
```

---

## 💻 Contoh Teknis: Integrated Security Application

```python
"""
Contoh integrasi semua kontrol keamanan dalam satu aplikasi ShopSecure
"""
from flask import Flask, request, jsonify, session, g
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
from argon2 import PasswordHasher
import pyotp
import sqlite3
import os
import secrets
import time
import logging
import json
from functools import wraps

app = Flask(__name__)
app.secret_key = os.urandom(32)

# ===== SECURITY CONFIGURATION =====
ENCRYPTION_KEY = os.environ.get('ENCRYPTION_KEY', os.urandom(32).hex())
encryption_key = bytes.fromhex(ENCRYPTION_KEY) if isinstance(ENCRYPTION_KEY, str) else ENCRYPTION_KEY

ph = PasswordHasher(time_cost=2, memory_cost=65536, parallelism=2)

# Audit logger
audit_logger = logging.getLogger('audit')
audit_logger.setLevel(logging.INFO)
audit_handler = logging.FileHandler('audit.log')
audit_logger.addHandler(audit_handler)

def audit_log(user_id, action, details=""):
    """Catat semua aksi penting ke audit log."""
    entry = {
        'timestamp': time.time(),
        'user_id': user_id,
        'action': action,
        'ip': request.remote_addr,
        'user_agent': request.user_agent.string[:100],
        'details': details
    }
    audit_logger.info(json.dumps(entry))

# ===== ENCRYPTION UTILITIES =====
aesgcm = AESGCM(encryption_key)

def encrypt_sensitive(plaintext: str) -> str:
    nonce = os.urandom(12)
    ct = aesgcm.encrypt(nonce, plaintext.encode(), None)
    return (nonce + ct).hex()

def decrypt_sensitive(encrypted_hex: str) -> str:
    data = bytes.fromhex(encrypted_hex)
    nonce, ct = data[:12], data[12:]
    return aesgcm.decrypt(nonce, ct, None).decode()

# ===== RATE LIMITING =====
login_attempts = {}
def check_rate_limit(key: str, max_attempts: int = 5, window: int = 300) -> bool:
    now = time.time()
    attempts = login_attempts.get(key, [])
    attempts = [t for t in attempts if now - t < window]
    if len(attempts) >= max_attempts:
        return False
    attempts.append(now)
    login_attempts[key] = attempts
    return True

# ===== AUTHENTICATION =====
def require_auth(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        if 'user_id' not in session:
            return jsonify({'error': 'Authentication required'}), 401
        return f(*args, **kwargs)
    return decorated

@app.route('/api/register', methods=['POST'])
def register():
    data = request.get_json()
    username = data.get('username', '').strip()
    password = data.get('password', '')
    nik = data.get('nik', '')
    
    # Validasi
    if not username or len(username) < 3:
        return jsonify({'error': 'Username minimal 3 karakter'}), 400
    if len(password) < 8:
        return jsonify({'error': 'Password minimal 8 karakter'}), 400
    
    # Hash password + enkripsi NIK
    password_hash = ph.hash(password)
    nik_encrypted = encrypt_sensitive(nik) if nik else None
    
    # Generate TOTP secret
    totp_secret = pyotp.random_base32()
    
    # Simpan ke database (prepared statement)
    # db.execute("INSERT INTO users (username, password_hash, nik, totp_secret) VALUES (?,?,?,?)",
    #            (username, password_hash, nik_encrypted, totp_secret))
    
    audit_log(None, 'REGISTER', f'username: {username}')
    
    totp = pyotp.TOTP(totp_secret)
    return jsonify({
        'message': 'Registered successfully',
        'totp_setup_url': totp.provisioning_uri(username, 'ShopSecure'),
        'totp_secret': totp_secret
    })


@app.route('/api/login', methods=['POST'])
def login():
    data = request.get_json()
    username = data.get('username', '')
    password = data.get('password', '')
    totp_code = data.get('totp_code', '')
    
    # Rate limiting
    if not check_rate_limit(f"login:{username}"):
        audit_log(None, 'LOGIN_BLOCKED', f'username: {username}')
        return jsonify({'error': 'Terlalu banyak percobaan. Coba lagi dalam 5 menit.'}), 429
    
    # Verifikasi credential (gunakan prepared statement)
    # user = db.execute("SELECT * FROM users WHERE username=?", (username,)).fetchone()
    # Simulasi:
    stored_hash = ph.hash("correctpassword")
    
    try:
        ph.verify(stored_hash, password)
    except Exception:
        audit_log(None, 'LOGIN_FAILED', f'username: {username}')
        return jsonify({'error': 'Invalid credentials'}), 401
    
    # Verifikasi 2FA
    # totp = pyotp.TOTP(user['totp_secret'])
    totp = pyotp.TOTP(pyotp.random_base32())
    if totp_code and not totp.verify(totp_code, valid_window=1):
        audit_log(None, 'LOGIN_2FA_FAILED', f'username: {username}')
        return jsonify({'error': 'Invalid TOTP code'}), 401
    
    session['user_id'] = 1  # user['id']
    audit_log(1, 'LOGIN_SUCCESS', f'username: {username}')
    
    return jsonify({'message': 'Login successful'})


@app.route('/api/user/data-export', methods=['GET'])
@require_auth
def export_user_data():
    """HAK PORTABILITAS — ekspor semua data pengguna"""
    user_id = session['user_id']
    audit_log(user_id, 'DATA_EXPORT_REQUEST', 'User requested data export')
    
    # Return all user data in structured format
    # (query dari database dengan user_id filter)
    return jsonify({
        'export_date': time.time(),
        'user_data': 'All your data here',
        'note': 'Data ini sesuai dengan hak portabilitas UU PDP Pasal 9'
    })
```

---

## 🏢 Panduan Presentasi

### Alokasi Waktu (15 menit + Q&A)

```
00:00-01:00  Perkenalan sistem: ShopSecure, target pengguna, data sensitif
01:00-03:00  Threat Model: top 5 ancaman + STRIDE analysis
03:00-05:00  Security Architecture Diagram: walkthrough komponen
05:00-09:00  Demo implementasi (3 kontrol yang diimplementasikan)
09:00-11:00  Compliance: UU PDP + PCI-DSS checklist
11:00-13:00  Risk matrix dan risk treatment decisions
13:00-15:00  Lessons learned + Next steps jika ada waktu lebih
15:00+       Q&A
```

### Pertanyaan Q&A yang Mungkin Keluar

| Pertanyaan | Tips Menjawab |
|---|---|
| "Mengapa pilih AES-256-GCM bukan CBC?" | "GCM memberikan enkripsi + authentication tag — mendeteksi tampering, CBC tidak" |
| "Jika server key bocor, apa yang terjadi?" | "Data terenkripsi bisa didekripsi. Itulnya key harus di AWS KMS, dirotasi berkala" |
| "Bagaimana kalau gateway Midtrans down?" | "Implementasi circuit breaker + fallback ke payment method alternatif" |
| "Apakah 2FA wajib?" | "Best practice: wajibkan untuk aksi high-risk (transfer besar, ubah profil)" |
| "Bagaimana prove UU PDP compliant?" | "Tunjukkan: fitur data export, hapus akun, consent granular, DPO contact" |

---

## ⚠️ Checklist Presentasi

```
PERSIAPAN TEKNIS:
  □ Demo bisa dijalankan secara lokal (localhost)
  □ Dependencies sudah di-install (requirements.txt tersedia)
  □ Environment variables sudah diset (.env.example ada)
  □ Screenshot/video backup jika ada masalah teknis
  □ Kode sudah di-review: tidak ada hardcoded credentials
  □ SQL queries menggunakan prepared statements

KELENGKAPAN DELIVERABLES:
  □ Threat model mencakup semua 6 STRIDE untuk minimal 3 komponen
  □ Risk matrix minimal 10 risiko dengan score dan treatment
  □ Architecture diagram punya semua layer (network, app, data, monitoring)
  □ Minimal 3 kontrol keamanan di-implementasikan dan bisa di-demo
  □ UU PDP + PCI-DSS checklist lengkap dengan status per item

KUALITAS PRESENTASI:
  □ Setiap keputusan desain bisa dipertanggungjawabkan
  □ Tahu mengapa AES bukan DES, bcrypt bukan SHA-256, dll
  □ Bisa jelaskan bagaimana compliance UU PDP terpenuhi
  □ Ada lessons learned yang genuine (bukan copy dari materi)
  □ Q&A: tidak bluffing, kalau tidak tahu: "akan kami investigasi lebih lanjut"
```

---

## 📌 Ringkasan

Proyek ini adalah sintesis dari seluruh perjalanan belajar:

* **Kriptografi** (AES, RSA, Hash, Digital Signature) → Lindungi data
* **Database Security** (Least Privilege, Enkripsi, SQL Injection Prevention) → Secure storage
* **Transport Security** (TLS, HSTS, Certificate) → Secure komunikasi
* **Payment Security** (Gateway, Tokenisasi, PCI-DSS) → Secure transaksi
* **Identity Security** (bcrypt, 2FA/TOTP) → Secure autentikasi
* **Compliance** (UU PDP, GDPR) → Legal protection
* **Risk Management** (STRIDE, Risk Matrix, Incident Response) → Proactive defense

Security bukan tentang membuat sistem yang tidak bisa diserang — **tidak ada sistem yang 100% aman**. Security adalah tentang membuat sistem yang:
1. Sulit diserang (preventive controls)
2. Cepat mendeteksi jika ada serangan (detective controls)
3. Bisa pulih dengan cepat jika terjadi insiden (corrective controls)

---

*📚 Referensi: NIST Cybersecurity Framework | OWASP Top 10 | PCI-DSS v4.0 | UU PDP No. 27/2022 | Anderson, R. (2020). Security Engineering | Stallings, W. (2022). Cryptography and Network Security*
