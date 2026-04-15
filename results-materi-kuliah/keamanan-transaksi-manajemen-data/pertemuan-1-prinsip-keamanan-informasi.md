# Pertemuan 1: Prinsip Keamanan Informasi — CIA Triad, AAA & OWASP Top 10

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan CIA Triad dan memberikan contoh nyata pelanggaran setiap elemen
* Mendeskripsikan AAA Framework (Authentication, Authorization, Accounting) beserta contoh implementasinya
* Mengidentifikasi dan mengklasifikasikan ancaman OWASP Top 10 (2021)
* Menganalisis kasus pelanggaran data dan mengidentifikasi CIA element yang dilanggar

---

## 📖 Pengantar (Hook)

September 2021: 91 juta data pengguna Tokopedia bocor dan dijual di dark web seharga $5,000. Data yang bocor: nama lengkap, email, nomor telepon, tanggal lahir, dan hash password.

Bagaimana ini bisa terjadi? Siapa yang bertanggung jawab? Apa yang seharusnya dilakukan untuk mencegahnya?

Jawabannya ada di fondasi keamanan informasi — prinsip-prinsip dasar yang, jika diabaikan, bisa mengakibatkan kerugian miliaran rupiah dan hilangnya kepercayaan jutaan pengguna.

---

## 🧩 Konsep Utama

### CIA Triad: Tiga Pilar Keamanan Informasi

CIA Triad adalah framework fundamental yang mendefinisikan tiga tujuan utama keamanan informasi:

```
        Confidentiality (C)
               ▲
              / \
             /   \
            /     \
           /  CIA  \
          /  TRIAD  \
         /           \
        ▼_____________▼
Integrity (I)    Availability (A)
```

#### 1. Confidentiality (Kerahasiaan)
Data hanya dapat diakses oleh pihak yang berwenang.

| Ancaman | Contoh | Kontrol |
|---|---|---|
| Unauthorized Access | Hacker membaca database user | Enkripsi, Access Control |
| Data Leakage | Karyawan kirim data ke kompetitor | DLP, NDA, Monitoring |
| Eavesdropping | Intercept data di jaringan publik | HTTPS/TLS, VPN |

**Pelanggaran nyata:** Data 91 juta user Tokopedia → Confidentiality dilanggar karena data pribadi user diakses oleh pihak tidak berwenang.

#### 2. Integrity (Integritas)
Data tidak dapat dimodifikasi oleh pihak yang tidak berwenang, dan modifikasi terdeteksi.

| Ancaman | Contoh | Kontrol |
|---|---|---|
| Data Tampering | Mengubah nominal transaksi | Hash, Digital Signature |
| Man-in-the-Middle | Memodifikasi data saat transmisi | HTTPS/TLS, HMAC |
| SQL Injection | Mengubah record di database | Input Validation, WAF |

**Pelanggaran nyata:** Seorang pengguna e-banking memodifikasi request HTTP untuk mengubah nominal transfer dari Rp 1 juta menjadi Rp 1.000 → Integrity dilanggar.

#### 3. Availability (Ketersediaan)
Sistem dan data dapat diakses oleh pihak yang berwenang kapan pun dibutuhkan.

| Ancaman | Contoh | Kontrol |
|---|---|---|
| DDoS Attack | Server e-commerce lumpuh saat 12.12 | Load Balancer, CDN, Anti-DDoS |
| Ransomware | Database dikunci, minta tebusan | Backup, Incident Response |
| Hardware Failure | Server mati tanpa backup | Redundancy, HA Architecture |

**Pelanggaran nyata:** GoTo (GoJek-Tokopedia) mengalami gangguan layanan saat Harbolnas 11.11 → Availability terganggu.

### Hubungan dan Trade-off dalam CIA Triad

```
Confidentiality tinggi ←→ Availability rendah
(Enkripsi ketat dan multi-layer auth → akses lebih lambat/sulit)

Contoh: Bank dengan 3-factor auth + enkripsi berlapis:
  Sangat aman (C tinggi) tapi antrian di ATM panjang (A berkurang)
```

---

### AAA Framework: Authentication, Authorization, Accounting

AAA adalah protokol kontrol akses yang mendefinisikan **siapa** yang boleh masuk, **apa** yang boleh dilakukan, dan **mencatat** apa yang mereka lakukan.

```
┌─────────────────────────────────────────────────────┐
│                   USER REQUEST                       │
└──────────────────────┬──────────────────────────────┘
                       ▼
┌─────────────────────────────────────────────────────┐
│            AUTHENTICATION (Siapa kamu?)             │
│  • Username + Password                              │
│  • Biometrik (sidik jari, wajah)                   │
│  • Token/OTP                                        │
│  Pertanyaan: "Apakah kamu benar-benar user X?"      │
└──────────────────────┬──────────────────────────────┘
                       │ Identitas terverifikasi
                       ▼
┌─────────────────────────────────────────────────────┐
│            AUTHORIZATION (Apa yang boleh?)          │
│  • Role-Based Access Control (RBAC)                 │
│  • Permission per resource                         │
│  Pertanyaan: "Apakah user X boleh akses resource Y?"│
└──────────────────────┬──────────────────────────────┘
                       │ Akses diizinkan
                       ▼
┌─────────────────────────────────────────────────────┐
│            ACCOUNTING (Apa yang dilakukan?)         │
│  • Audit Log: siapa, apa, kapan, dari mana          │
│  • Non-repudiation                                  │
│  Pertanyaan: "Apa yang dilakukan user X?"           │
└─────────────────────────────────────────────────────┘
```

**Contoh fintech:**
```
Authentication: Budi login dengan password + OTP SMS
Authorization:  Budi (role: nasabah) boleh: transfer (max 10jt/hari),
                cek saldo, bayar tagihan
                TIDAK boleh: akses data nasabah lain, ubah batas limit
Accounting:     Log: "2025-01-15 14:32:05 | Budi | Transfer | Rp 500.000 
                → 1234567890 | IP: 182.x.x.x | SUCCESS"
```

---

### OWASP Top 10 (2021): Ancaman Web Paling Berbahaya

OWASP (Open Web Application Security Project) merilis daftar 10 kerentanan web paling kritis yang diperbarui secara berkala.

| Rank | Kategori | Deskripsi Singkat | Contoh |
|---|---|---|---|
| A01 | Broken Access Control | User bisa akses data/fitur di luar haknya | Nasabah A akses data nasabah B |
| A02 | Cryptographic Failures | Data sensitif tidak dienkripsi atau enkripsi lemah | Password disimpan plaintext |
| A03 | Injection | Input user dieksekusi sebagai kode | SQL Injection, Command Injection |
| A04 | Insecure Design | Desain sistem tidak mempertimbangkan keamanan | Tidak ada rate limiting di login |
| A05 | Security Misconfiguration | Konfigurasi default tidak diubah, error terlalu verbose | Default admin:admin masih aktif |
| A06 | Vulnerable Components | Menggunakan library dengan CVE yang diketahui | Log4Shell (Log4j vulnerability) |
| A07 | Auth & Session Failures | Session management yang lemah | Session ID di URL, tidak expire |
| A08 | Software & Data Integrity | Build/deploy tanpa verifikasi integritas | Supply chain attack |
| A09 | Security Logging Failures | Log tidak cukup, tidak dipantau | Serangan tidak terdeteksi |
| A10 | SSRF | Server diminta ambil resource dari URL berbahaya | Akses internal metadata cloud |

**Top 3 yang paling sering di fintech Indonesia:**

```
A01 Broken Access Control — Contoh nyata:
  GET /api/transaction?userId=123  ← Budi punya akses ini
  GET /api/transaction?userId=124  ← Budi TIDAK boleh, tapi server tidak cek!
  → Hanya ubah angka userId, bisa lihat data orang lain
  → IDOR (Insecure Direct Object Reference)

A02 Cryptographic Failures — Contoh nyata:
  Database password: md5("password123") = 482c811da5d5b4bc6d497ffa98491e38
  → MD5 sudah tidak aman, bisa di-crack dengan rainbow table dalam detik

A03 Injection — Contoh nyata:
  Username: admin'-- 
  Password: apapun
  SQL yang dieksekusi: SELECT * FROM users WHERE username='admin'--' AND password='...'
  → '--' adalah komentar SQL, password check diskip!
```

---

## 🧠 Ilustrasi / Analogi

**CIA Triad seperti brankas bank:**
* **Confidentiality** = Kunci brankas — hanya pemegang kunci yang bisa buka
* **Integrity** = Segel tamper-evident — kalau ada yang coba membuka, bekasnya terlihat
* **Availability** = Jam operasional bank — brankas harus bisa dibuka saat jam kerja, bukan hanya 1x setahun

**AAA seperti masuk gedung kantor:**
* **Authentication** = Scan kartu di pintu masuk (membuktikan kamu karyawan)
* **Authorization** = Kartu kamu hanya bisa buka lantai 3 dan 5 (bukan semua lantai)
* **Accounting** = Log "Budi masuk lantai 3 pukul 08:45, keluar 17:30" (audit trail)

**OWASP Top 10 seperti daftar penyakit paling berbahaya versi WHO:**
* Bukan daftar semua penyakit, tapi 10 yang paling sering menyebabkan kematian sistem
* Kalau aplikasimu bebas dari 10 ini, sudah jauh lebih sehat dari rata-rata

---

## 💻 Contoh Teknis

### Broken Access Control — Contoh dan Pencegahan (Python/Flask)

```python
from flask import Flask, request, jsonify, session

app = Flask(__name__)

# ❌ RENTAN: Tidak ada pengecekan kepemilikan resource
@app.route('/api/transaction/<int:transaction_id>')
def get_transaction_vulnerable(transaction_id):
    # Siapapun bisa akses transaksi manapun hanya dengan ganti ID!
    transaction = db.query("SELECT * FROM transactions WHERE id = ?", transaction_id)
    return jsonify(transaction)

# ✅ AMAN: Selalu verifikasi kepemilikan resource
@app.route('/api/transaction/<int:transaction_id>')
def get_transaction_secure(transaction_id):
    user_id = session.get('user_id')
    if not user_id:
        return jsonify({'error': 'Unauthorized'}), 401
    
    # Tambahkan kondisi: transaksi harus milik user yang login
    transaction = db.query(
        "SELECT * FROM transactions WHERE id = ? AND user_id = ?",
        transaction_id, user_id
    )
    
    if not transaction:
        # Jangan bedakan "tidak ada" vs "bukan milikmu" — sama-sama 404
        return jsonify({'error': 'Not found'}), 404
    
    return jsonify(transaction)
```

### Audit Logging Sederhana (AAA - Accounting)

```python
import logging
from datetime import datetime
from flask import request, session

# Setup dedicated security logger
security_logger = logging.getLogger('security')
handler = logging.FileHandler('security_audit.log')
handler.setFormatter(logging.Formatter(
    '%(asctime)s | %(levelname)s | %(message)s'
))
security_logger.addHandler(handler)

def log_security_event(action, status, details=""):
    """
    Catat setiap event keamanan penting.
    Who: user_id | What: action | When: timestamp | Where: IP | Result: status
    """
    user_id = session.get('user_id', 'anonymous')
    ip_address = request.remote_addr
    
    log_entry = f"USER:{user_id} | ACTION:{action} | IP:{ip_address} | STATUS:{status}"
    if details:
        log_entry += f" | DETAIL:{details}"
    
    security_logger.info(log_entry)

# Contoh penggunaan:
# log_security_event("LOGIN", "SUCCESS")
# log_security_event("TRANSFER", "FAILED", "Saldo tidak cukup")
# log_security_event("LOGIN", "FAILED", "Wrong password attempt 3/5")
```

---

## 🏢 Studi Kasus: Pelanggaran Data Tokopedia 2021

**Kronologi:**
* Maret 2020: Tokopedia mengalami data breach besar
* Mei 2020: 91 juta data user dijual di forum hacker Raidforums
* Data yang bocor: nama, email, nomor HP, hash password (bcrypt), tanggal lahir

**Analisis CIA Triad:**

| CIA Element | Status | Detail |
|---|---|---|
| Confidentiality | ❌ VIOLATED | Data pribadi 91 juta user bocor ke publik |
| Integrity | ⚠️ UNCERTAIN | Tidak ada bukti data dimodifikasi, tapi tidak bisa dikonfirmasi |
| Availability | ✅ NOT AFFECTED | Layanan Tokopedia tetap berjalan normal |

**Analisis OWASP:**
* Kemungkinan: **A01 Broken Access Control** — penyerang berhasil mengekstrak seluruh tabel user
* Kemungkinan: **A05 Security Misconfiguration** — konfigurasi database atau API tidak cukup dibatasi
* Kemungkinan: **A09 Security Logging Failures** — breach berjalan lama tanpa terdeteksi

**Dampak Bisnis:**
* Reputasi: kepercayaan pengguna turun signifikan
* Regulasi: berpotensi kena sanksi UU PDP (jika berlaku saat itu)
* Operasional: wajib reset password massal, komunikasi ke semua user

**Lesson Learned:**
1. Enkripsi database sensitif (bukan hanya hash password)
2. Monitoring anomali akses: query yang mengambil jutaan record sekaligus → alert
3. Prinsip "least privilege": API/service tidak boleh akses semua tabel tanpa perlu
4. Regular penetration testing dan security audit

---

## 📊 Visualisasi: Threat Landscape Fintech Indonesia

```
THREAT LANDSCAPE FINTECH INDONESIA (2023-2024)
Based on BSSN & OJK Reports

Jenis Serangan:
┌─────────────────────────────────────────────────────┐
│ Phishing/Social Engineering  ████████████████  42%  │
│ SQL Injection               █████████████     35%   │
│ Credential Stuffing         ██████████        28%   │
│ API Security Issues         ████████          22%   │
│ Insider Threat              ██████            18%   │
└─────────────────────────────────────────────────────┘

Target Industri yang Paling Sering Diserang:
┌─────────────────────────────────────────────────────┐
│ Financial Services     ████████████████████  54%    │
│ E-Commerce             █████████████         38%    │
│ Healthcare             ████████              24%    │
│ Government             ███████               21%    │
└─────────────────────────────────────────────────────┘
```

---

## ⚠️ Kesalahan Umum

1. **Mengira CIA Triad hanya tentang Confidentiality** → Banyak developer fokus pada "kerahasiaan data" tapi mengabaikan Integrity (apakah data bisa dimanipulasi?) dan Availability (apakah sistem bisa di-DDoS?).

2. **Authentication tanpa Authorization** → "Sudah login, berarti boleh akses semua" → SALAH. Login hanya membuktikan identitas. Authorization yang menentukan apa yang boleh diakses.

3. **Tidak ada Accounting/Audit Log** → "Kami tidak tahu kapan dan bagaimana breach terjadi" → Tanpa audit log, forensik digital menjadi mustahil.

4. **Menganggap OWASP Top 10 adalah checklist yang harus dipenuhi satu kali** → OWASP diperbarui setiap 3-4 tahun. Versi 2021 berbeda dengan 2017. Security adalah proses berkelanjutan, bukan proyek sekali jalan.

5. **Mengecilkan insider threat** → "Yang berbahaya pasti hacker dari luar" → Statistik menunjukkan 20-30% breach melibatkan orang dalam (karyawan, kontraktor).

---

## 🧪 Latihan

### Soal 1 — Analisis CIA Triad

Untuk setiap skenario berikut, identifikasi CIA element apa yang dilanggar dan jelaskan mengapa:

a) Seorang admin database menggunakan akun root untuk semua operasi harian, termasuk SELECT sederhana.

b) Sistem e-banking tidak memiliki backup. Saat server mati, tidak ada yang bisa transaksi selama 8 jam.

c) Seorang karyawan mengirim spreadsheet data nasabah (nama, NIK, saldo) ke email pribadinya.

d) Hacker berhasil mengubah nominal transfer di database dari Rp 100.000 menjadi Rp 1.000.000, dan tidak ada yang mendeteksi perubahan ini.

### Soal 2 — OWASP Identification

Identifikasi kerentanan OWASP Top 10 yang ada di setiap kode/skenario berikut:

a) `SELECT * FROM users WHERE username='$username' AND password='$password'`

b) Error message yang ditampilkan ke user: "MySQL Error 1045: Access denied for user 'root'@'localhost'"

c) API endpoint `GET /api/v1/user/profile?id=456` yang tidak memverifikasi apakah user yang login memiliki id=456

d) Sistem menggunakan jQuery versi 1.9.1 yang dirilis tahun 2013

---

## 📌 Ringkasan

* **CIA Triad** = Confidentiality (kerahasiaan) + Integrity (integritas) + Availability (ketersediaan) — tiga pilar keamanan informasi
* Setiap keputusan keamanan adalah **trade-off** antara ketiga elemen ini
* **AAA Framework:** Authentication (siapa) → Authorization (boleh apa) → Accounting (log apa)
* **OWASP Top 10 (2021):** Broken Access Control adalah #1 — paling sering terjadi di fintech
* Kasus Tokopedia: bukti bahwa bahkan unicorn pun bisa dilanggar — CIA Triad diabaikan = konsekuensi besar
* Security bukan fitur yang ditambahkan belakangan — harus **built-in dari awal**

---

*📚 Referensi: Anderson, R. (2020). Security Engineering, 3rd Ed. | OWASP Top 10 (2021) — owasp.org | BSSN Annual Report 2023 | Stallings, W. (2022). Cryptography and Network Security, 8th Ed.*
