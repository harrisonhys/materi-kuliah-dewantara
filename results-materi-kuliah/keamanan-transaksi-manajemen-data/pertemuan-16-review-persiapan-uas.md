# Pertemuan 16: Review & Persiapan UAS

---

## 🎯 Learning Outcomes

Setelah pertemuan review ini, kamu akan bisa:

* Merangkum semua materi pasca-UTS (Pertemuan 8–14) dalam satu referensi cepat
* Menjawab soal UAS bertipe teori, analisis kasus, dan implementasi
* Mengintegrasikan semua konsep keamanan dalam framework Defense in Depth
* Menghindari kesalahan konseptual yang paling sering muncul di ujian

---

## 📖 Pengantar (Hook)

UAS adalah kesempatan untuk membuktikan bahwa kamu tidak sekadar hafal definisi SQL Injection atau rumus SUS Score — tapi benar-benar mengerti **mengapa** Prepared Statements aman, **bagaimana** audit log membantu forensik, dan **apa** yang dimaksud UU PDP dengan hak subjek data.

Security adalah ilmu yang sangat praktis. Teori tanpa pemahaman konteks nyata tidak berguna. Tapi pemahaman tanpa dasar teori yang kuat juga mudah tergoyahkan.

---

## 🧩 Ringkasan Materi Pertemuan 8–14

### Pertemuan 8: SQL Injection Prevention

* **Root cause:** String concatenation dalam query SQL
* **Jenis:** Classic (UNION-based), Blind Boolean, Blind Time-based
* **Solusi utama:** Prepared Statements / Parameterized Queries
* **Defense in Depth:** Input validation + Prepared Statements + Error handling aman + Least privilege DB user
* OWASP A03 — masih penyebab terbesar breach database

### Pertemuan 9: Audit Log & Backup

* **Audit log:** SIAPA + APA + KAPAN + DARI MANA + SEBELUM → SESUDAH
* MySQL Triggers: otomatis catat INSERT/UPDATE/DELETE tanpa ubah kode aplikasi
* **Backup strategy:** Full (mingguan) + Differential (harian) + Incremental (per jam)
* **3-2-1 Rule:** 3 salinan, 2 media berbeda, 1 offsite
* SELALU enkripsi backup — file backup = salinan seluruh database
* **RTO** (waktu recovery maksimal) dan **RPO** (data loss maksimal) menentukan strategi
* SELALU test restore — backup tidak pernah dicoba = tidak ada artinya

### Pertemuan 10: HTTPS/TLS

* **HTTP:** plaintext, siapapun di network bisa baca
* **HTTPS** = HTTP over TLS: enkripsi + integrity + server authentication
* **TLS Handshake:** Cipher negotiation → Certificate → ECDHE key exchange → Session key
* **HSTS:** Browser paksa HTTPS bahkan kunjungan pertama — cegah downgrade attack
* **Certificate Pinning:** App "ingat" public key server — cegah rogue CA attack
* TLS 1.0 dan 1.1 deprecated — hanya gunakan TLS 1.2 dan 1.3

### Pertemuan 11: Payment Gateway & PCI-DSS

* **Payment Gateway:** Perantara aman merchant ↔ jaringan kartu ↔ bank
* **Tokenisasi:** PAN (nomor kartu) → token acak. Merchant hanya simpan token
* CVV tidak boleh disimpan sama sekali setelah otorisasi (PCI-DSS req 3)
* **12 PCI-DSS Requirements:** keamanan jaringan, proteksi data, access control, monitoring, kebijakan
* Menggunakan gateway = scope PCI minimal (SAQ-A). SELALU verifikasi HMAC signature webhook

### Pertemuan 12: 2FA & OTP

* **Tiga faktor:** Knowledge (password), Possession (HP/token), Inherence (biometrik)
* **SMS OTP:** mudah tapi rentan SIM swap, SS7 vulnerability, phishing OTP
* **TOTP (RFC 6238):** `HMAC-SHA1(secret, floor(time/30))` — berubah tiap 30 detik, offline, lebih aman
* **Backup codes:** 8-10 kode single-use untuk recovery, di-hash di database
* **FIDO2/Passkey:** terkuat — private key di device, domain binding cegah phishing
* Rate limiting pada 2FA verification sama pentingnya dengan pada password login

### Pertemuan 13: UU PDP & GDPR

* **UU PDP No. 27/2022:** berlaku Oktober 2024, wajib untuk semua entitas proses data WNI
* **Data umum:** nama, email, dll. **Data sensitif:** NIK, biometrik, finansial, kesehatan
* **8 hak subjek data:** akses, koreksi, hapus, portabilitas, tarik consent, keberatan, informasi, non-automated decision
* **Breach notification:** 14 hari kerja ke BSSN + notify subjek terdampak
* **Privacy by Design:** privasi diintegrasikan sejak desain, bukan afterthought
* Denda: 2% pendapatan tahunan global + sanksi pidana (berbeda dari GDPR yang tidak ada pidana)

### Pertemuan 14: Risk Assessment

* **STRIDE:** Spoofing, Tampering, Repudiation, Information Disclosure, DoS, Elevation of Privilege
* **Risk Score = Likelihood (1-5) × Impact (1-5)** → 15+ = Critical, 10-14 = High
* **Risk Treatment:** Accept / Mitigate / Transfer / Avoid
* Residual risk = risk setelah kontrol baru diterapkan
* **Incident Response:** Preparation → Detection → Containment → Eradication → Recovery → Review

---

## 🧠 Framework Terintegrasi: Defense in Depth

```
SHIELDED E-COMMERCE (Defense in Depth):

LAYER 1 — PERIMETER DEFENSE:
  ┌──────────────────────────────────────────────────────┐
  │  WAF + DDoS Protection + Rate Limiting               │
  │  (OWASP Rule Set, SQL Injection patterns)            │
  │  → Pertemuan 8 (SQL Injection), 14 (DoS risk)       │
  └──────────────────────────────────────────────────────┘

LAYER 2 — TRANSPORT SECURITY:
  ┌──────────────────────────────────────────────────────┐
  │  HTTPS/TLS 1.3 + HSTS + Certificate Pinning         │
  │  ECDHE (Perfect Forward Secrecy)                    │
  │  → Pertemuan 10                                      │
  └──────────────────────────────────────────────────────┘

LAYER 3 — AUTHENTICATION & AUTHORIZATION:
  ┌──────────────────────────────────────────────────────┐
  │  Argon2/bcrypt password + TOTP 2FA                   │
  │  JWT with short expiry + rotation                    │
  │  Role-Based Access Control                          │
  │  → Pertemuan 12 (2FA), Pertemuan 6 (RBAC)           │
  └──────────────────────────────────────────────────────┘

LAYER 4 — APPLICATION SECURITY:
  ┌──────────────────────────────────────────────────────┐
  │  Prepared Statements (SQL Injection prevention)     │
  │  Input validation + Output encoding                 │
  │  Security headers (HSTS, CSP, X-Frame-Options)     │
  │  → Pertemuan 8                                       │
  └──────────────────────────────────────────────────────┘

LAYER 5 — DATA SECURITY:
  ┌──────────────────────────────────────────────────────┐
  │  AES-256-GCM untuk kolom sensitif                   │
  │  Tokenisasi kartu (Midtrans/Xendit, bukan simpan PAN)│
  │  Least privilege DB users                           │
  │  Row-Level Security                                 │
  │  → Pertemuan 2 (AES), 6 (DB), 11 (Payment)         │
  └──────────────────────────────────────────────────────┘

LAYER 6 — MONITORING & COMPLIANCE:
  ┌──────────────────────────────────────────────────────┐
  │  Audit log (MySQL triggers) — siapa, apa, kapan     │
  │  Encrypted backup (3-2-1 rule)                      │
  │  STRIDE threat model + Risk matrix                  │
  │  UU PDP compliance (consent, hak subjek data)       │
  │  PCI-DSS compliance (tidak simpan CVV)              │
  │  → Pertemuan 9 (Audit/Backup), 13 (UU PDP), 14 (Risk│
  └──────────────────────────────────────────────────────┘
```

---

## 💻 Quick Reference: Rumus & Angka Penting

```
ALGORITMA & MINIMUM STANDARDS:
  AES:     256-bit key, GCM mode (AEAD)
  RSA:     2048-bit minimum, 3072-bit recommended post-2030
  ECC:     256-bit (NIST P-256 atau secp256k1)
  Hash:    SHA-256 atau SHA-3 (bukan MD5, bukan SHA-1)
  Password: bcrypt cost≥12 ATAU Argon2id (memory≥64MB)
  TLS:     1.2 minimum, 1.3 preferred

YANG PASTI TIDAK BOLEH:
  ✗ DES, 3DES (deprecated)
  ✗ MD5 untuk security (collision broken)
  ✗ SHA-1 untuk certificates (broken 2017)
  ✗ SHA-256 untuk password (terlalu cepat)
  ✗ ECB mode untuk AES (pola terlihat)
  ✗ TLS 1.0 / TLS 1.1 (deprecated 2021)
  ✗ RSA-1024 (deprecated 2010)
  ✗ Plaintext password di database (TIDAK PERNAH)
  ✗ Simpan CVV/CVC setelah otorisasi (PCI-DSS violation)
  ✗ User root untuk koneksi aplikasi

UU PDP KEY NUMBERS:
  Breach notification: 14 hari kerja ke BSSN
  Denda: 2% pendapatan tahunan global
  Pidana: 4-6 tahun penjara untuk pelanggaran tertentu
  Berlaku: Oktober 2024

PCI-DSS KEY NUMBERS:
  12 requirements
  Level 1: > 6 juta transaksi/tahun → annual QSA audit
  SAQ-A: Merchant yang fully outsource ke gateway
```

---

## 📊 Tabel Komprehensif: Serangan & Pertahanan

| Serangan | STRIDE | OWASP | Kontrol Utama | Kontrol Pendukung |
|---|---|---|---|---|
| SQL Injection | Tampering | A03 | Prepared Statements | Input validation, WAF, Least privilege |
| Credential Stuffing | Spoofing | A07 | 2FA (TOTP) | Rate limiting, breach DB check, CAPTCHA |
| Data Breach (DB dump) | Info Disclosure | A02 | Enkripsi kolom (AES-256) | Least privilege, audit log, monitoring |
| MITM Attack | Tampering/Info Disc | A02 | HTTPS/TLS 1.3 | HSTS, Cert Pinning, ECDHE |
| DDoS | DoS | A04 | Rate Limiting, CDN | Auto-scaling, Circuit Breaker, WAF |
| SIM Swap | Spoofing | A07 | TOTP (bukan SMS OTP) | FIDO2/Passkey, backup codes |
| Phishing Admin | Spoofing | A07 | FIDO2/Passkey | Security training, SPF/DKIM |
| Ransomware | DoS | — | Encrypted backup (3-2-1) | Test restore, offline backup copy |
| Insider Threat | Info Disc | A01 | Least privilege + Audit log | Row-Level Security, monitoring |
| Payment Fraud | Tampering | A01 | Midtrans/tokenisasi, HMAC | 3DS, velocity check, fraud ML |

---

## 🧪 Soal Latihan UAS

### Bagian A: Teori (30 poin)

**1.** (8 poin) Sebuah aplikasi e-commerce memiliki kerentanan berikut. Untuk setiap kerentanan:
* Identifikasi OWASP Top 10 dan STRIDE category
* Jelaskan cara exploit (bagaimana hacker memanfaatkannya)
* Berikan solusi teknis spesifik

a) `query = "SELECT * FROM orders WHERE user_id=" + user_id`
b) Password disimpan sebagai `MD5(password)` tanpa salt
c) Error response: `{"error": "MySQL: Column 'email' doesn't exist in table 'users'"}`
d) Endpoint `/api/admin/users` tidak memverifikasi role pengguna
e) Server memiliki TLS 1.0 masih aktif
f) CVV kartu disimpan di tabel transactions

**2.** (7 poin) Jelaskan perbedaan antara:
a) Enkripsi simetris (AES) vs asimetris (RSA) — kapan masing-masing digunakan?
b) Hash (SHA-256) vs Password Hash (bcrypt) — mengapa keduanya diperlukan?
c) Tokenisasi kartu vs Enkripsi kartu — mengapa fintech lebih suka tokenisasi?

**3.** (8 poin) Jelaskan TLS Handshake step-by-step. Khusus untuk:
a) Bagaimana browser dan server mendapatkan session key yang sama?
b) Mengapa ECDHE lebih direkomendasikan dari RSA key exchange?
c) Apa itu Perfect Forward Secrecy dan mengapa penting?

**4.** (7 poin) Sebuah aplikasi fintech ingin comply dengan UU PDP. Jelaskan:
a) 5 langkah teknis yang harus diimplementasikan
b) Bagaimana mengimplementasikan "Right to be Forgotten" tanpa melanggar kewajiban OJK untuk menyimpan data transaksi 10 tahun?
c) Apa yang dimaksud "Privacy by Design" dalam konteks pembangunan fitur baru?

### Bagian B: Analisis Kasus (35 poin)

**5.** (15 poin) Analisis insiden keamanan berikut:

Pada Jumat malam pukul 23:00, sebuah payment startup menerima laporan dari pengguna bahwa saldo mereka berkurang tanpa otorisasi. Investigasi awal menemukan:

* Log server menunjukkan 50.000 request ke `/api/transfer` dalam 1 jam terakhir
* Semua request menggunakan token JWT yang valid dari berbagai IP berbeda
* Database menunjukkan 8.000 transfer berhasil ke 3 rekening yang sama
* Rekening tujuan dibuat 2 hari lalu, masing-masing menerima < Rp 5 juta (di bawah pelaporan PPATK)

a) Identifikasi jenis serangan yang terjadi
b) Analisis menggunakan STRIDE — kategori apa yang dilanggar?
c) Langkah immediate response (15 menit pertama setelah terdeteksi)
d) Root cause analysis: kontrol apa yang seharusnya mencegah ini?
e) Kewajiban UU PDP: apa yang harus dilakukan dalam 14 hari kerja?

**6.** (20 poin) Kamu diminta melakukan code review berikut dan memberikan assessment keamanan:

```python
@app.route('/api/transaction/report')
def transaction_report():
    start_date = request.args.get('start_date')
    end_date = request.args.get('end_date')
    category = request.args.get('category', 'all')
    sort_by = request.args.get('sort_by', 'date')
    
    query = f"""
        SELECT t.id, t.amount, t.status, u.email, u.nik
        FROM transactions t
        JOIN users u ON t.user_id = u.id
        WHERE t.created_at BETWEEN '{start_date}' AND '{end_date}'
        {'AND t.category = "' + category + '"' if category != 'all' else ''}
        ORDER BY {sort_by}
    """
    
    conn = get_db_connection()  # Uses root user
    cursor = conn.cursor()
    cursor.execute(query)
    results = cursor.fetchall()
    
    return jsonify({
        'data': results,
        'server_info': {
            'db_version': conn.get_server_info(),
            'db_host': DB_HOST
        }
    })
```

a) Identifikasi SEMUA masalah keamanan (berikan minimal 6 masalah)
b) Untuk setiap masalah: nama vulnerability, OWASP category, cara exploit
c) Tulis ulang kode yang aman dengan semua masalah diperbaiki

### Bagian C: Implementasi (35 poin)

**7.** (20 poin) Implementasikan sistem autentikasi yang aman:

```python
# Implement fungsi-fungsi berikut:

def register_user(username: str, password: str) -> dict:
    """
    Requirements:
    - Hash password menggunakan Argon2id
    - Validasi: username alphanumeric 3-50 char, password min 8 char
    - Generate TOTP secret
    - Generate 10 backup codes (di-hash sebelum disimpan)
    - Return: {'user_id': ..., 'totp_secret': ..., 'backup_codes': [...]}
    """
    pass

def login(username: str, password: str, totp_code: str) -> dict:
    """
    Requirements:
    - Verifikasi password dengan Argon2id
    - Rate limiting: max 5 attempts per 5 menit
    - Verifikasi TOTP dengan valid_window=1
    - Jika totp_code 8 karakter uppercase: verifikasi backup code (single use)
    - Return: {'success': bool, 'session_token': str} atau {'error': str}
    """
    pass

# Test case:
user = register_user("testuser", "SecureP@ss!")
assert 'totp_secret' in user
assert 'backup_codes' in user
assert len(user['backup_codes']) == 10

# Simulate login
totp = pyotp.TOTP(user['totp_secret'])
result = login("testuser", "SecureP@ss!", totp.now())
assert result['success'] == True
```

**8.** (15 poin) Tulis SQL script yang mengimplementasikan keamanan database untuk tabel `transactions` dan `users`:

a) Buat tabel dengan kolom yang tepat (termasuk kolom untuk NIK yang akan dienkripsi)
b) Buat 3 user dengan privilege berbeda: `app_api`, `report_service`, `backup_agent`
c) Buat VIEW `safe_transactions` yang menyembunyikan kolom sensitif untuk `report_service`
d) Buat trigger AFTER UPDATE untuk audit log perubahan `transactions.status`
e) Buat trigger BEFORE DELETE pada `users` yang mencegah penghapusan jika ada transaksi aktif

---

## 📌 Checklist Pre-UAS

- [ ] Bisa jelaskan SQL Injection dan mengapa Prepared Statements mencegahnya
- [ ] Paham perbedaan AES-CBC vs GCM (authentication tag)
- [ ] Tahu mengapa bcrypt lebih baik dari SHA-256 untuk password
- [ ] Bisa trace TLS Handshake dari awal sampai data terenkripsi
- [ ] Paham cara kerja tokenisasi kartu di payment gateway
- [ ] Bisa implementasikan TOTP dengan pyotp dari awal sampai login
- [ ] Tahu 8 hak subjek data menurut UU PDP
- [ ] Bisa lakukan STRIDE analysis untuk komponen sistem
- [ ] Paham 4 pilihan risk treatment dan kapan masing-masing digunakan
- [ ] Bisa implementasikan MySQL audit log menggunakan trigger
- [ ] Tahu 12 PCI-DSS requirements dan mana yang paling relevan untuk developer

---

*📚 Referensi: Stallings, W. (2022). Cryptography and Network Security | Anderson, R. (2020). Security Engineering | OWASP Top 10 (2021) | UU No. 27/2022 (UU PDP) | PCI-DSS v4.0 | NIST Cybersecurity Framework | Python cryptography.io, pyotp, argon2-cffi*
