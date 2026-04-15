# Pertemuan 5: Digital Signature & PKI (Public Key Infrastructure)

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan cara kerja digital signature dan membedakannya dari enkripsi
* Memahami konsep non-repudiation dan mengapa ini penting dalam transaksi digital
* Menjelaskan peran Certificate Authority (CA), Root CA, dan Chain of Trust
* Menganalisis struktur sertifikat X.509 dan memverifikasi sertifikat SSL di browser
* Membuat dan memverifikasi digital signature sederhana di Python

---

## 📖 Pengantar (Hook)

Kamu transfer Rp 50 juta ke rekening lain melalui internet banking. Bagaimana bank yakin bahwa perintah transfer itu benar-benar dari kamu — bukan dari seseorang yang mencuri sesi loginmu?

Bagaimana browser yakin bahwa website "bankbca.co.id" yang kamu buka benar-benar server BCA — bukan server hacker yang punya nama domain mirip?

Kedua pertanyaan ini dijawab oleh **Digital Signature** dan **Public Key Infrastructure (PKI)** — teknologi yang memungkinkan dunia digital untuk memverifikasi identitas dan memastikan bahwa data tidak diubah oleh pihak ketiga.

---

## 🧩 Konsep Utama

### Digital Signature: Tanda Tangan Elektronik yang Tidak Bisa Dipalsukan

Digital signature menggunakan **private key untuk menandatangani** dan **public key untuk memverifikasi** — kebalikan dari enkripsi!

```
ENKRIPSI:          Public Key → Enkripsi,   Private Key → Dekripsi
DIGITAL SIGNATURE: Private Key → Sign,      Public Key → Verify

CARA KERJA DIGITAL SIGNATURE:

PENGIRIM (Alice):
  1. Buat hash dokumen: hash = SHA-256(dokumen)
  2. Enkripsi hash dengan PRIVATE KEY Alice: signature = RSA_sign(hash, private_key)
  3. Kirim: dokumen + signature

PENERIMA (Bob):
  1. Hitung hash dokumen yang diterima: hash_received = SHA-256(dokumen)
  2. Dekripsi signature dengan PUBLIC KEY Alice: hash_from_sig = RSA_verify(signature, public_key)
  3. Bandingkan: hash_received == hash_from_sig?
     → SAMA: signature valid, dokumen asli, pengirim benar-benar Alice
     → BEDA: dokumen dimodifikasi ATAU signature bukan dari Alice

KENAPA MENGGUNAKAN HASH (bukan sign dokumen langsung)?
  • RSA hanya bisa sign data kecil (~190 bytes untuk RSA-2048)
  • Dokumen bisa berukuran GB
  • Hash selalu 256-bit — bisa di-sign dengan RSA
```

### Tiga Jaminan Digital Signature

```
1. AUTHENTICATION (Siapa yang menandatangani?)
   → Hanya pemilik private key yang bisa membuat signature
   → Verifikasi dengan public key yang sesuai

2. INTEGRITY (Apakah dokumen diubah?)
   → Jika satu bit dokumen berubah → hash berbeda → signature tidak match
   → Setiap modifikasi TERDETEKSI

3. NON-REPUDIATION (Tidak bisa menyangkal)
   → Alice tidak bisa bilang "Bukan saya yang tanda tangan"
   → Hanya private key Alice yang bisa buat signature itu
   → Krusial untuk transaksi keuangan dan kontrak digital
```

### Public Key Infrastructure (PKI)

**Masalah:** Bagaimana Bob tahu bahwa public key yang dia gunakan memang benar milik Alice — bukan public key palsu yang disebarkan hacker?

**Solusi: PKI** — sistem kepercayaan terhierarkis menggunakan Certificate Authority (CA).

```
HIERARKI PKI:

Root CA (Comodo, DigiCert, GlobalSign, Sectigo)
     │ "Kami adalah authority tertinggi. Percayakan pada kami."
     │ Root certificate di-install di OS/browser kamu
     │
     ├── Intermediate CA
     │        │ "Kami mendapat kepercayaan dari Root CA"
     │        │
     │        └── End-Entity Certificate
     │                 "Ini certificate untuk api.tokopedia.com"
     │                 signed by Intermediate CA
     │
     └── Intermediate CA 2
              └── End-Entity Certificate
                       "Ini certificate untuk bankbca.co.id"

Chain of Trust:
  Browser percaya Root CA (built-in)
  → Root CA bertanda tangan di Intermediate CA cert (percaya)
  → Intermediate CA bertanda tangan di Server cert (percaya)
  → Browser percaya Server cert api.tokopedia.com ✓
```

### Struktur Sertifikat X.509

```
=== CERTIFICATE ===
Subject: CN=api.tokopedia.com, O=PT Tokopedia, C=ID
Issuer:  CN=DigiCert TLS RSA SHA256 2020 CA1, O=DigiCert
Version: 3

Public Key:
  Algorithm: RSA (atau ECC)
  Key Size: 2048-bit
  Value: 30 82 01 0a 02 82 01 01 00 b3 ...

Validity:
  Not Before: 2024-01-15 00:00:00 UTC
  Not After:  2025-01-14 23:59:59 UTC

Extensions:
  Subject Alt Names (SAN): 
    DNS: api.tokopedia.com
    DNS: *.tokopedia.com
  Key Usage: Digital Signature, Key Encipherment
  Extended Key Usage: TLS Web Server Authentication
  Certificate Policies: DV (Domain Validation)
  OCSP URL: http://ocsp.digicert.com

Signature (by Issuer's Private Key):
  Algorithm: SHA256withRSA
  Value: 4d 3e 1b ...

=== VERIFY ===
Browser:
  1. Download certificate
  2. Check: Not expired? ✓
  3. Check: Hostname match? api.tokopedia.com ✓
  4. Verify signature menggunakan Issuer's Public Key ✓
  5. Trace chain ke Root CA yang dipercaya ✓
  → HTTPS padlock hijau ✓
```

### Jenis Validasi Sertifikat

```
DV (Domain Validation):
  • CA memverifikasi kepemilikan domain saja
  • Proses: tambahkan DNS record atau file ke website
  • Waktu: menit — jam
  • Padlock: hijau tapi tidak ada nama perusahaan
  • Cocok untuk: blog, website personal

OV (Organization Validation):
  • CA memverifikasi kepemilikan domain + identitas organisasi
  • Proses: kirim dokumen perusahaan ke CA
  • Waktu: 1-3 hari bisnis
  • Cocok untuk: website bisnis resmi

EV (Extended Validation):
  • Proses verifikasi paling ketat
  • CA verifikasi dokumen legal, alamat fisik, nama resmi
  • Waktu: 1-2 minggu
  • Browser menampilkan nama perusahaan di address bar
  • Cocok untuk: bank, fintech, e-commerce besar (BCA, DANA, Shopee)
```

---

## 🧠 Ilustrasi / Analogi

**Digital Signature seperti tanda tangan basah + notaris:**
* Tanda tangan basah = private key (hanya kamu yang punya)
* Notaris memverifikasi tanda tangan = public key (siapapun bisa verifikasi)
* Dokumen yang dinotariskan = non-repudiation (tidak bisa disangkal)

**Certificate Authority seperti KTP/Paspor:**
* KTP bukan kamu yang buat sendiri — diterbitkan oleh Dukcapil (CA) yang dipercaya
* Polisi (pihak ketiga) percaya pada KTP karena percaya pada Dukcapil
* Tanpa CA: seperti membuat "KTP" sendiri dan berharap orang percaya

**Chain of Trust seperti surat rekomendasi berantai:**
* Presiden merekomendasikan Menteri (Root CA → Intermediate CA)
* Menteri merekomendasikan Direktur (Intermediate CA → End-Entity)
* Kamu percaya Direktur karena rantai rekomendasi ke orang yang kamu percaya

---

## 💻 Contoh Teknis

### Membuat dan Memverifikasi Digital Signature

```python
"""
Implementasi Digital Signature menggunakan RSA-PSS dan ECDSA
Install: pip install cryptography
"""
from cryptography.hazmat.primitives.asymmetric import rsa, ec, padding
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.hazmat.backends import default_backend
from cryptography.exceptions import InvalidSignature
import hashlib
import json

class DigitalSigner:
    """RSA-PSS Digital Signature untuk dokumen/transaksi."""
    
    def __init__(self):
        # Generate RSA key pair
        self.private_key = rsa.generate_private_key(
            public_exponent=65537,
            key_size=2048,
            backend=default_backend()
        )
        self.public_key = self.private_key.public_key()
    
    def sign(self, data: bytes) -> bytes:
        """
        Tanda tangani data menggunakan private key.
        RSA-PSS (probabilistic) lebih aman dari PKCS1v15 untuk signing.
        Internally: SHA-256(data) lalu sign hash dengan private key.
        """
        signature = self.private_key.sign(
            data,
            padding.PSS(
                mgf=padding.MGF1(hashes.SHA256()),
                salt_length=padding.PSS.MAX_LENGTH
            ),
            hashes.SHA256()
        )
        return signature
    
    def verify(self, data: bytes, signature: bytes, public_key=None) -> bool:
        """Verifikasi signature menggunakan public key."""
        pub_key = public_key or self.public_key
        try:
            pub_key.verify(
                signature,
                data,
                padding.PSS(
                    mgf=padding.MGF1(hashes.SHA256()),
                    salt_length=padding.PSS.MAX_LENGTH
                ),
                hashes.SHA256()
            )
            return True
        except InvalidSignature:
            return False


# ===== Simulasi: Penandatanganan Instruksi Transfer =====

def sign_transfer_instruction(signer: DigitalSigner, transfer: dict) -> dict:
    """
    Sign instruksi transfer perbankan.
    Non-repudiation: bank bisa buktikan customer memang memerintahkan transfer ini.
    """
    # Canonical JSON (sorted keys) untuk konsistensi
    transfer_bytes = json.dumps(transfer, sort_keys=True).encode('utf-8')
    signature = signer.sign(transfer_bytes)
    
    return {
        'transfer': transfer,
        'signature': signature.hex(),
        'signing_algorithm': 'RSA-PSS-SHA256'
    }

def verify_transfer(signer: DigitalSigner, signed_transfer: dict) -> bool:
    """
    Bank memverifikasi instruksi transfer saat memproses.
    """
    transfer_bytes = json.dumps(
        signed_transfer['transfer'], 
        sort_keys=True
    ).encode('utf-8')
    signature = bytes.fromhex(signed_transfer['signature'])
    
    return signer.verify(transfer_bytes, signature)


# Demo
alice = DigitalSigner()
print("Alice's public key (dikirim ke bank):")
pub_pem = alice.public_key.public_bytes(
    serialization.Encoding.PEM, 
    serialization.PublicFormat.SubjectPublicKeyInfo
)
print(pub_pem.decode()[:200])

transfer_order = {
    'from_account': '1234567890',
    'to_account': '0987654321',
    'amount': 5000000,
    'currency': 'IDR',
    'timestamp': '2025-01-15T14:32:00Z',
    'reference': 'TRX-20250115-001'
}

print("\n=== Alice menandatangani instruksi transfer ===")
signed = sign_transfer_instruction(alice, transfer_order)
print(f"Signature (hex): {signed['signature'][:80]}...")

print("\n=== Bank memverifikasi ===")
is_valid = verify_transfer(alice, signed)
print(f"Signature valid: {is_valid}")

# Simulasi tampering
print("\n=== Hacker mengubah nominal ===")
tampered = signed.copy()
tampered['transfer'] = transfer_order.copy()
tampered['transfer']['amount'] = 50000000  # Ubah nominal!
is_tampered_valid = verify_transfer(alice, tampered)
print(f"Tampered signature valid: {is_tampered_valid}")  # False!
```

### Analisis Sertifikat SSL (Praktis)

```python
"""
Analisis sertifikat SSL website menggunakan Python
"""
import ssl
import socket
from datetime import datetime

def analyze_ssl_certificate(hostname: str, port: int = 443):
    """
    Ambil dan analisis sertifikat SSL dari hostname.
    """
    context = ssl.create_default_context()
    
    with socket.create_connection((hostname, port)) as sock:
        with context.wrap_socket(sock, server_hostname=hostname) as ssock:
            cert = ssock.getpeercert()
    
    print(f"\n=== SSL Certificate Analysis: {hostname} ===")
    print(f"Subject: {dict(x[0] for x in cert['subject'])}")
    print(f"Issuer: {dict(x[0] for x in cert['issuer'])}")
    
    # Validity
    not_before = datetime.strptime(cert['notBefore'], '%b %d %H:%M:%S %Y %Z')
    not_after = datetime.strptime(cert['notAfter'], '%b %d %H:%M:%S %Y %Z')
    days_left = (not_after - datetime.utcnow()).days
    
    print(f"\nValidity:")
    print(f"  Not Before: {not_before}")
    print(f"  Not After:  {not_after}")
    print(f"  Days Until Expiry: {days_left}")
    
    if days_left < 30:
        print(f"  ⚠️  WARNING: Certificate expires soon!")
    else:
        print(f"  ✓ Certificate valid")
    
    # Subject Alt Names
    san_list = []
    for ext in cert.get('subjectAltName', []):
        if ext[0] == 'DNS':
            san_list.append(ext[1])
    print(f"\nSubject Alt Names (SAN): {', '.join(san_list)}")
    
    # TLS version
    print(f"\nTLS Version: {ssock.version()}")
    print(f"Cipher Suite: {ssock.cipher()[0]}")

# Jalankan analisis (pastikan ada koneksi internet)
# analyze_ssl_certificate("api.xendit.co")
# analyze_ssl_certificate("app.midtrans.com")
```

---

## 🏢 Studi Kasus: Digital Signature di DANA e-Wallet

**Konteks:** DANA memproses jutaan transaksi per hari. Setiap instruksi (transfer, bayar) harus:
1. Benar-benar berasal dari pengguna yang sah (authentication)
2. Tidak bisa diubah di tengah jalan (integrity)
3. Tidak bisa disangkal oleh pengguna (non-repudiation)

**Arsitektur Digital Signature DANA (tipikal fintech):**

```
FLOW TRANSFER DANA:

1. User tap "Bayar" di app DANA
   → App generate transaction payload:
     {"amount": 150000, "to": "081234567890", "ts": 1705312321}

2. App sign payload dengan User's Private Key
   (disimpan di Secure Enclave HP — tidak bisa diekstrak)
   → signature = ECDSA(payload, user_private_key)

3. App kirim ke DANA server: {payload, signature, user_id}

4. DANA server verifikasi:
   → Ambil public key user dari database
   → Verify ECDSA signature
   → Jika valid: proses transaksi
   → Jika invalid: TOLAK (kemungkinan tampering atau replay attack)

5. DANA server sign response:
   → {"status": "success", "tx_id": "..."} + DANA's signature
   → User app bisa verifikasi response asli dari DANA

KEUNTUNGAN:
  • Hacker intercept dan modify payload → signature invalid → ditolak
  • User klaim "Saya tidak transfer" → server punya signature mereka
  • Non-repudiation terbukti di pengadilan digital
```

**Keamanan Tambahan:**
* ECDSA (Elliptic Curve) lebih efisien dari RSA untuk mobile
* Timestamp dalam payload mencegah replay attack
* Key disimpan di Secure Enclave (iPhone) / StrongBox (Android) — tidak bisa diekstrak bahkan oleh malware

---

## ⚠️ Kesalahan Umum

1. **Mengenkripsi dengan private key, bukan sign** → "Enkripsi dengan private key = signature" adalah simplifikasi yang salah secara teknis. Gunakan fungsi sign/verify yang tepat (RSA-PSS atau ECDSA), bukan encrypt/decrypt.

2. **Tidak memvalidasi certificate chain** → Sertifikat self-signed tidak dipercaya oleh browser. Pastikan sertifikat ditandatangani oleh CA yang diakui (diinstall di Root CA store OS/browser).

3. **Tidak memeriksa expiry date sertifikat** → Sertifikat yang expired = koneksi tidak aman. Setup monitoring/alerting untuk sertifikat yang akan expire dalam 30 hari.

4. **Tidak memverifikasi hostname dalam sertifikat** → Sertifikat valid untuk `*.tokopedia.com` tidak valid untuk `tokopedia.co.id` — ini berbeda domain. Browser melakukan ini otomatis, tapi library HTTP client dalam kode mungkin tidak (jika disabled verification).

5. **Private key di-hardcode atau disimpan tanpa enkripsi** → Private key yang bocor = semua signature bisa dipalsukan. Gunakan HSM, Secure Enclave, atau setidaknya enkripsi file private key dengan passphrase.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan antara enkripsi asimetris dan digital signature dalam hal penggunaan kunci. Mengapa digital signature menggunakan private key untuk "mengunci" (sign) dan public key untuk "membuka" (verify), berlawanan dengan enkripsi?

b) Apa itu Chain of Trust dalam PKI? Mengapa browser kamu bisa langsung percaya sertifikat `api.tokopedia.com` meskipun belum pernah mengenal DigiCert sebelumnya?

c) Seorang pengguna melakukan transfer online lalu mengklaim "Saya tidak pernah melakukan transfer itu." Jelaskan bagaimana digital signature memberikan non-repudiation.

### Soal 2 — Praktik

Buat program Python yang:
1. Generate RSA/ECDSA key pair
2. Sign string/file teks menggunakan private key
3. Verifikasi signature menggunakan public key
4. Demonstrasikan: modifikasi dokumen setelah ditandatangani → verification gagal
5. Tambahan: analisis sertifikat SSL dari 2 website fintech Indonesia (gunakan kode analisis di atas)

---

## 📌 Ringkasan

* **Digital Signature:** Private key untuk sign, Public key untuk verify — kebalikan dari enkripsi
* Jaminan: **Authentication** (siapa pengirim) + **Integrity** (tidak diubah) + **Non-repudiation** (tidak bisa menyangkal)
* Praktis: sign **hash** dokumen (bukan dokumen langsung) — efisien untuk data besar
* **PKI:** Certificate Authority (CA) sebagai pihak ketiga yang dipercaya untuk memverifikasi identitas
* **Chain of Trust:** Browser → Root CA → Intermediate CA → Server Certificate
* **X.509:** Struktur standar sertifikat digital (Subject, Issuer, Public Key, Validity, Extensions)
* Validasi sertifikat: DV (domain only) → OV (+ organization) → EV (paling ketat, nama perusahaan di browser)
* Di fintech: digital signature memastikan instruksi transaksi tidak bisa dimanipulasi dan tidak bisa disangkal

---

*📚 Referensi: Stallings, W. (2022). Cryptography and Network Security, 8th Ed. | RFC 5280 — X.509 Certificate | RFC 8446 — TLS 1.3 | Python cryptography.io | NIST SP 800-57 — Key Management*
