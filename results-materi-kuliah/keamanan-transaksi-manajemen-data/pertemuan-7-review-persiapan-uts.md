# Pertemuan 7: Review & Persiapan UTS

---

## 🎯 Learning Outcomes

Setelah pertemuan review ini, kamu akan bisa:

* Merangkum semua materi Pertemuan 1–6 dalam satu referensi cepat
* Menjawab soal UTS bertipe teori, analisis kasus, dan implementasi
* Mengintegrasikan konsep CIA Triad, kriptografi, dan keamanan database
* Menghindari kesalahan konseptual yang paling sering muncul di ujian

---

## 📖 Pengantar (Hook)

UTS adalah kesempatan untuk membuktikan bahwa kamu bukan hanya hafal algoritma — tapi memahami **mengapa** AES digunakan untuk enkripsi data besar, **mengapa** bcrypt lebih aman dari SHA-256 untuk password, dan **bagaimana** semua konsep ini bekerja bersama dalam sistem yang benar-benar aman.

---

## 🧩 Ringkasan Materi Pertemuan 1–6

### Pertemuan 1: Prinsip Keamanan Informasi

* **CIA Triad:** Confidentiality (kerahasiaan) + Integrity (integritas) + Availability (ketersediaan)
* **AAA Framework:** Authentication (siapa?) → Authorization (boleh apa?) → Accounting (catat apa?)
* **OWASP Top 10 (2021):** A01 Broken Access Control, A02 Cryptographic Failures, A03 Injection paling kritis
* Kasus Tokopedia: 91 juta data bocor → CIA Confidentiality dilanggar

### Pertemuan 2: Enkripsi Simetris

* **Satu kunci** untuk enkripsi dan dekripsi
* **DES (56-bit):** BROKEN sejak 1998. **AES:** standar industri (128/256-bit)
* Mode operasi: **ECB** (JANGAN, pola terlihat) → **CBC** (aman, perlu IV) → **GCM** (AEAD, rekomendasi)
* **GCM:** enkripsi + authentication tag → mendeteksi tampering
* Key management: JANGAN hardcode. Gunakan env variable atau KMS.

### Pertemuan 3: Enkripsi Asimetris

* **Dua kunci berbeda:** public key (enkripsi/verifikasi) + private key (dekripsi/sign)
* **RSA:** berdasarkan kesulitan faktorisasi prima. Minimum 2048-bit
* **ECC:** lebih efisien. ECC-256 ≈ RSA-3072. Digunakan di TLS modern, Bitcoin
* RSA **hanya untuk data kecil** (~190 bytes) → gunakan **Hybrid Encryption** untuk data besar
* **Diffie-Hellman:** cara aman menukar kunci melalui kanal publik
* **Perfect Forward Secrecy (ECDHE):** session key baru tiap koneksi → bocornya private key tidak kompromikan sesi lama

### Pertemuan 4: Hash & Password Security

* **Hash:** one-way, tidak bisa dibalik. Berbeda dari enkripsi!
* **MD5/SHA-1:** BROKEN. **SHA-256:** aman untuk integritas, BUKAN untuk password
* Masalah SHA-256 untuk password: **terlalu cepat** (GPU: miliaran hash/detik)
* **Salt:** random nilai per user → mencegah rainbow table
* **bcrypt:** built-in salt + configurable cost → sengaja lambat
* **Argon2id:** PHC winner, memory-hard → GPU tidak efisien
* **Rate limiting:** lockout setelah N gagal → cegah online brute force

### Pertemuan 5: Digital Signature & PKI

* **Penandatangan dengan private key, verifikasi dengan public key** (kebalikan enkripsi!)
* Jaminan: **Authentication + Integrity + Non-Repudiation**
* **Hash dulu** dokumen → sign hash (efisien untuk data besar)
* **PKI:** Certificate Authority (CA) sebagai trusted third party
* **Chain of Trust:** Browser → Root CA → Intermediate CA → Server Certificate
* **X.509:** format standar sertifikat. DV < OV < EV validation level

### Pertemuan 6: Keamanan Database

* **Least Privilege:** setiap user/service hanya dapat permission minimum
* Jangan gunakan `root` untuk koneksi aplikasi
* **RBAC di MySQL:** GRANT SELECT/INSERT/UPDATE per tabel per user
* **Enkripsi kolom:** AES_ENCRYPT MySQL (ECB, kurang ideal) vs application-level AES-256-GCM
* **Row-Level Security:** user hanya lihat data miliknya → filter berdasarkan user_id dari session

---

## 🧠 Framework Terintegrasi: Defense in Depth

```
SISTEM TRANSAKSI DIGITAL YANG AMAN (layered security):

Layer 1 — TRANSPORT SECURITY (Pertemuan 3, 5):
  ┌────────────────────────────────────────────────────┐
  │  HTTPS/TLS 1.3 + ECDHE                             │
  │  • Enkripsi all traffic (AES-256-GCM session key)  │
  │  • Server certificate (X.509, Chain of Trust)       │
  │  • Perfect Forward Secrecy                          │
  └────────────────────────────────────────────────────┘

Layer 2 — AUTHENTICATION & AUTHORIZATION (Pertemuan 1, 4):
  ┌────────────────────────────────────────────────────┐
  │  AAA Framework                                      │
  │  • Password: bcrypt cost=12 atau Argon2id          │
  │  • 2FA (akan dipelajari di Pertemuan 12)           │
  │  • JWT/Session dengan proper expiry                 │
  │  • Rate limiting: lockout setelah 5 gagal          │
  └────────────────────────────────────────────────────┘

Layer 3 — DATA SECURITY (Pertemuan 2, 6):
  ┌────────────────────────────────────────────────────┐
  │  Database Security                                  │
  │  • Kolom sensitif: AES-256-GCM (app layer)         │
  │  • User DB: least privilege per service            │
  │  • Row-Level Security: user hanya lihat datanya    │
  │  • Key Management: KMS / environment variable      │
  └────────────────────────────────────────────────────┘

Layer 4 — INTEGRITY VERIFICATION (Pertemuan 4, 5):
  ┌────────────────────────────────────────────────────┐
  │  Verifikasi Integritas                              │
  │  • API request signing: HMAC-SHA256                │
  │  • Critical transactions: RSA/ECDSA digital sig    │
  │  • File checksum: SHA-256                          │
  │  • Non-repudiation untuk bukti transaksi           │
  └────────────────────────────────────────────────────┘

Layer 5 — AUDIT & MONITORING (Pertemuan 1, 6):
  ┌────────────────────────────────────────────────────┐
  │  AAA - Accounting                                   │
  │  • Audit log: siapa, apa, kapan, dari mana         │
  │  • Monitoring anomali: alert jika ada pattern asing│
  │  • SIEM: Security Information and Event Management │
  └────────────────────────────────────────────────────┘
```

---

## 💻 Quick Reference: Rumus & Aturan Penting

```
PEMILIHAN ALGORITMA:
┌─────────────────────────────────────────────────────────┐
│ Kebutuhan              │ Algoritma yang Tepat            │
├─────────────────────────────────────────────────────────┤
│ Enkripsi data at rest  │ AES-256-GCM                    │
│ Enkripsi komunikasi    │ TLS 1.3 (ECDHE + AES-GCM)     │
│ Enkripsi kunci kecil   │ RSA-2048 (OAEP padding)        │
│ Password hashing       │ Argon2id atau bcrypt cost≥12   │
│ Integritas file/data   │ SHA-256 atau SHA-3             │
│ API request signing    │ HMAC-SHA256                    │
│ Tanda tangan transaksi │ ECDSA (secp256k1 atau P-256)   │
└─────────────────────────────────────────────────────────┘

YANG TIDAK BOLEH DIGUNAKAN:
• DES / 3DES → deprecated
• MD5 untuk security → broken collision
• SHA-1 untuk certificate → broken
• SHA-256 untuk password → terlalu cepat
• ECB mode untuk AES → pola terlihat
• RSA < 2048-bit → tidak aman
• Password disimpan plaintext → TIDAK PERNAH
• Database user root untuk aplikasi → TIDAK PERNAH

KEY SIZES (minimum keamanan 128-bit):
• AES: 256-bit (gunakan AES-256)
• RSA: 2048-bit (rekomendasi 3072+)
• ECC: 256-bit (NIST P-256)
• bcrypt: cost factor ≥ 10 (produksi: ≥ 12)
• Argon2: memory ≥ 64MB, time ≥ 2
```

---

## 📊 Tabel Komprehensif: Ancaman & Kontrol

| Ancaman | CIA Element | Teknik Serangan | Kontrol |
|---|---|---|---|
| Data breach (database dump) | C | SQL Injection, insider | Enkripsi kolom, least privilege |
| Credential stuffing | C, I | Brute force API login | Rate limiting, 2FA, bcrypt |
| Man-in-the-Middle | C, I | ARP spoofing, rogue WiFi | HTTPS/TLS, cert pinning |
| Data tampering di transit | I | MITM, packet injection | HMAC, digital signature, TLS |
| Password database breach | C | Rainbow table | bcrypt/Argon2 + salt |
| Replay attack | I | Intercept & resend request | Nonce/timestamp, HMAC |
| DDoS | A | Botnet flood | CDN, rate limiting, WAF |
| SQL Injection | C, I, A | Malicious input | Prepared statements (Pertemuan 8) |
| Insider threat | C, I | Privilege abuse | Least privilege, audit log |

---

## 🧪 Soal Latihan UTS

### Bagian A: Teori (30 poin)

**1.** (6 poin) Sebuah e-commerce menyimpan data pengguna dengan kondisi:
* Password di-hash menggunakan SHA-256 tanpa salt
* Kolom nomor kartu disimpan plaintext
* User database menggunakan root dengan full privilege
* Tidak ada audit log

Untuk setiap poin di atas: (a) identifikasi ancaman keamanan spesifik, (b) rekomendasikan perbaikan.

**2.** (6 poin) Jelaskan perbedaan mendasar antara enkripsi simetris (AES) dan asimetris (RSA):
a) Bagaimana kunci digunakan
b) Kelebihan/kekurangan masing-masing
c) Mengapa TLS menggunakan keduanya (hybrid)

**3.** (4 poin) Apa itu "salt" dalam konteks password hashing? Mengapa salt tidak perlu dirahasiakan, tapi tetap membuat password lebih aman?

**4.** (7 poin) Jelaskan cara kerja Digital Signature end-to-end:
a) Proses signing (pengirim)
b) Proses verifikasi (penerima)
c) Tiga jaminan yang diberikan
d) Mengapa hash dokumen yang di-sign, bukan dokumen langsung?

**5.** (7 poin) Sebuah startup fintech memiliki tim: developer (5 orang), customer service (15 orang), data analyst (3 orang), dan admin database (2 orang). Rancang strategi user management database yang mengimplementasikan Least Privilege. Sebutkan privilege apa yang diberikan ke setiap peran.

### Bagian B: Analisis Kasus (30 poin)

**6.** (15 poin) Analisis kasus: Layanan streaming musik Indonesia mengalami breach. Investigasi menemukan:
* 10 juta email + password bocor ke dark web
* Password tersimpan sebagai `MD5(password)` tanpa salt
* API endpoint `/api/user?id=123` tidak memverifikasi kepemilikan
* Error message: "Database error: Table 'users' doesn't exist in 'prod_db'"
* Log server tidak ada karena "makan terlalu banyak disk space"

Untuk setiap kerentanan:
a) Identifikasi mana OWASP Top 10 yang dilanggar
b) Identifikasi CIA element yang dilanggar
c) Jelaskan bagaimana serangan bisa terjadi
d) Rekomendasikan solusi teknis spesifik

**7.** (15 poin) Kamu diminta merancang sistem penyimpanan password untuk aplikasi fintech baru. Pilihlah satu dari dua opsi berikut dan justifikasikan:

**Opsi A:**
```python
import hashlib
def store_password(password, user_id):
    salt = str(user_id)  # Gunakan user_id sebagai salt
    return hashlib.sha256((password + salt).encode()).hexdigest()
```

**Opsi B:**
```python
from argon2 import PasswordHasher
ph = PasswordHasher(time_cost=2, memory_cost=65536, parallelism=2)
def store_password(password):
    return ph.hash(password)  # Salt otomatis random
```

a) Identifikasi semua kelemahan Opsi A
b) Jelaskan mengapa Opsi B lebih baik
c) Tambahkan rate limiting ke Opsi B

### Bagian C: Implementasi (40 poin)

**8.** (20 poin) Tulis fungsi Python yang mengimplementasikan sistem enkripsi data sensitif untuk kolom database:

```python
# Implement fungsi ini:
def encrypt_sensitive_data(plaintext: str, key: bytes) -> str:
    """
    Enkripsi menggunakan AES-256-GCM.
    Return format: "nonce_hex:ciphertext_hex"
    """
    pass

def decrypt_sensitive_data(encrypted: str, key: bytes) -> str:
    """
    Dekripsi data yang dienkripsi dengan fungsi di atas.
    Raise ValueError jika data dimodifikasi (tampering).
    """
    pass

# Test kamu:
key = os.urandom(32)
plaintext = "3273012501900001"  # NIK

encrypted = encrypt_sensitive_data(plaintext, key)
decrypted = decrypt_sensitive_data(encrypted, key)
assert decrypted == plaintext, "Dekripsi gagal!"

# Simulasikan tampering:
tampered = encrypted[:-4] + "0000"  # Ubah 4 char terakhir
try:
    decrypt_sensitive_data(tampered, key)
    print("ERROR: Tampering tidak terdeteksi!")
except ValueError as e:
    print(f"OK: Tampering terdeteksi: {e}")
```

**9.** (20 poin) Tulis SQL script yang:
a) Membuat database `fintech_db` dengan tabel `users` dan `transactions`
b) Membuat 3 user MySQL: `api_service` (SELECT, INSERT, UPDATE), `report_service` (SELECT only), `backup_agent` (SELECT, LOCK TABLES)
c) Mengenkripsi kolom `phone_number` di tabel `users` menggunakan AES_ENCRYPT
d) Membuat VIEW `user_public_info` yang menyembunyikan kolom `phone_number` untuk `report_service`
e) Verifikasi privilege dengan SHOW GRANTS

---

## 📌 Checklist Pre-UTS

- [ ] Bisa jelaskan CIA Triad dengan contoh pelanggaran nyata
- [ ] Paham perbedaan enkripsi simetris vs asimetris + kapan masing-masing digunakan
- [ ] Tahu mengapa ECB berbahaya dan kapan gunakan CBC vs GCM
- [ ] Bisa jelaskan mengapa SHA-256 tidak cocok untuk password
- [ ] Paham cara kerja salt dalam mencegah rainbow table
- [ ] Bisa tulis kode Python untuk AES-256-GCM encrypt/decrypt
- [ ] Bisa tulis SQL GRANT PRIVILEGE ke user MySQL
- [ ] Paham Chain of Trust PKI dari browser sampai server
- [ ] Bisa identifikasi OWASP Top 10 dari skenario kode yang diberikan

---

*📚 Referensi: Stallings, W. (2022). Cryptography and Network Security | Anderson, R. (2020). Security Engineering | OWASP Top 10 (2021) | NIST SP 800-57 | MySQL Security Documentation*
