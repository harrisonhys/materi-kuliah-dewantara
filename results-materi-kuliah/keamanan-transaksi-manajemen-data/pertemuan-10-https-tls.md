# Pertemuan 10: Protokol HTTPS/TLS & Certificate Management

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan perbedaan HTTP vs HTTPS dan risiko man-in-the-middle
* Mendeskripsikan TLS Handshake step-by-step
* Menganalisis SSL/TLS certificate menggunakan browser developer tools
* Menjelaskan konsep HSTS, certificate pinning, dan OCSP
* Mengonfigurasi HTTPS sederhana menggunakan self-signed certificate

---

## 📖 Pengantar (Hook)

Kamu sedang di kafe, menggunakan WiFi gratis untuk login ke aplikasi mobile banking. Seseorang dengan laptop di meja sebelah menjalankan Wireshark. Jika aplikasimu menggunakan HTTP (bukan HTTPS), orang itu bisa melihat username, password, dan nomor rekening kamu dalam plaintext — persis seperti membaca buku terbuka.

Itulah alasan mengapa HTTPS bukan lagi "nice to have" — ini adalah keharusan absolut untuk setiap aplikasi yang menangani data sensitif. Dan mulai 2017, Google Chrome menandai semua situs HTTP sebagai "Not Secure" di address bar.

Tapi bagaimana sebenarnya HTTPS melindungimu? Itulah yang akan kita pelajari hari ini.

---

## 🧩 Konsep Utama

### HTTP vs HTTPS: Perbedaan Fundamental

```
HTTP (Hypertext Transfer Protocol):
  Browser ──[DATA PLAINTEXT]──► Server
  
  Siapapun di jaringan yang sama bisa membaca:
  GET /login HTTP/1.1
  Host: bank.com
  
  POST /login HTTP/1.1
  username=budi&password=rahasia123  ← Terlihat jelas!

HTTPS (HTTP Secure = HTTP over TLS):
  Browser ──[DATA TERENKRIPSI]──► Server
  
  Yang terlihat di jaringan:
  \x17\x03\x03\x01\x7f\x4d\x8b\x3a...  ← Tidak terbaca!
  
  TLS memastikan:
  ✓ Confidentiality: data terenkripsi
  ✓ Integrity: data tidak bisa dimodifikasi
  ✓ Authentication: server adalah benar-benar server yang diklaim
```

### Ancaman Tanpa HTTPS

#### Man-in-the-Middle (MITM) Attack

```
TANPA HTTPS:
  Alice ──────────────────────────► Bank Server
         ↑
         Eve (hacker di network yang sama)
         Bisa: baca, modifikasi, inject data

CONTOH SERANGAN:
1. ARP Spoofing: Eve membuat traffic Alice lewat laptopnya
2. Eve lihat semua HTTP traffic dalam plaintext
3. Eve bisa inject malicious JavaScript ke halaman
4. Eve bisa capture credentials

DENGAN HTTPS:
  Alice ──[🔒 Encrypted + Authenticated]──► Bank Server
         ↑
         Eve hanya lihat: encrypted noise
         Tidak bisa baca, tidak bisa modifikasi
```

### TLS Handshake: Step by Step

TLS (Transport Layer Security) adalah protokol yang membangun "terowongan aman" sebelum data dikirim.

```
TLS 1.3 HANDSHAKE (simplifikasi):

STEP 1: CLIENT HELLO
  Browser → Server:
  "Halo! Saya browser X, TLS 1.3, 
   cipher suites yang saya support: TLS_AES_256_GCM_SHA384, ...
   Ini adalah random nonce: client_random"

STEP 2: SERVER HELLO + CERTIFICATE + KEY_SHARE
  Server → Browser:
  "Halo! Kita pakai TLS_AES_256_GCM_SHA384.
   Ini server nonce: server_random.
   Ini certificate saya: [X.509 Certificate]
   Ini key_share ECDHE saya: [public value]"

STEP 3: KEY DERIVATION (keduanya hitung sendiri)
  Browser dan Server masing-masing hitung:
  master_secret = HKDF(ECDHE shared secret + client_random + server_random)
  session_key = derive_key(master_secret)
  → Keduanya mendapat session_key AES yang SAMA
  → Tanpa Eve bisa tahu (Diffie-Hellman)

STEP 4: BROWSER VERIFIKASI CERTIFICATE
  Browser:
  ✓ Certificate expired? Tidak
  ✓ Hostname match? api.bank.com == api.bank.com ✓
  ✓ Chain of Trust ke Root CA yang dipercaya? ✓
  ✓ Certificate revoked? (OCSP check) Tidak
  → Server terverifikasi!

STEP 5: HANDSHAKE FINISHED + ENCRYPTED DATA
  Browser ──[AES-256-GCM encrypted data]──► Server
```

### HSTS: HTTP Strict Transport Security

```
MASALAH:
  User pertama kali akses: http://bank.com (HTTP!)
  Browser terima redirect 301 ke https://bank.com
  
  Tapi ANTARA http request dan redirect — ada window untuk MITM!
  HSTS Stripping attack: Eve intercept redirect → ubah ke HTTP

SOLUSI: HSTS Header
  Server kirim: Strict-Transport-Security: max-age=31536000; includeSubDomains; preload

  Setelah browser menerima header ini:
  → Browser INGAT: "bank.com harus selalu HTTPS"
  → Kalau user ketik http://bank.com → browser langsung paksa ke https
  → TIDAK ada lagi request HTTP ke server!
  → max-age=31536000 = ingat selama 1 tahun

HSTS Preload List:
  Browser punya built-in list domain yang HARUS HTTPS
  → Bahkan kunjungan pertama pun langsung HTTPS
  → Submit di: hstspreload.org
  → Semua domain fintech Indonesia seharusnya di sini
```

### Certificate Pinning

```
MASALAH YANG LEBIH CANGGIH:
  Meskipun HTTPS, jika CA yang dipercaya bisa dikompromikan:
  → "Rogue CA" bisa terbitkan sertifikat palsu untuk bank.com
  → MITM attack tetap mungkin
  
  Contoh: Tahun 2011, CA DigiNotar di-hack → 
  sertifikat palsu google.com diterbitkan untuk MITM pengguna Iran

SOLUSI: Certificate Pinning
  Aplikasi mobile "ingat" public key atau certificate server yang benar.
  Jika certificate berubah (bahkan dengan CA valid) → TOLAK koneksi!

  Implementasi di Android/iOS:
  • Certificate Pinning: pin hash sertifikat spesifik (hard)
  • Public Key Pinning: pin public key (lebih fleksibel)
  
  Konfigurasi di Android (network_security_config.xml):
  <domain-config>
    <domain includeSubdomains="true">api.bankxyz.com</domain>
    <pin-set>
      <pin digest="SHA-256">AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=</pin>
      <pin digest="SHA-256">BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB=</pin><!-- backup -->
    </pin-set>
  </domain-config>

RISIKO:
  Jika certificate expire dan tidak ada backup pin → aplikasi tidak bisa connect!
  → Selalu PIN 2 kunci: current + backup (next rotation)
```

---

## 🧠 Ilustrasi / Analogi

**HTTPS seperti kurir surat bersegel:**
* HTTP = surat terbuka, siapapun di jalan bisa baca isinya
* HTTPS = surat dalam amplop tertutup + disegel + kurir terverifikasi identitasnya
* TLS Handshake = proses kurir dan penerima menyepakati kode rahasia sebelum bertukar surat

**TLS Handshake seperti buka rekening bank:**
* Step 1: Nasabah datang ke bank (client hello)
* Step 2: Bank tunjukkan izin usaha + identitas (certificate)
* Step 3: Nasabah verifikasi bank asli, bukan bank palsu (chain of trust)
* Step 4: Keduanya sepakati "password rahasia" untuk transaksi hari ini (session key)
* Step 5: Semua komunikasi pakai "bahasa sandi" yang hanya keduanya tahu

**HSTS seperti autopilot keselamatan:**
* Sekali browser tahu "domain ini harus HTTPS", dia tidak perlu tanya lagi
* Seperti sabuk pengaman otomatis — tidak perlu dipasang manual setiap kali masuk mobil

---

## 💻 Contoh Teknis

### Analisis SSL Certificate dengan Python

```python
"""
Analisis mendalam SSL/TLS certificate dan keamanan HTTPS
"""
import ssl
import socket
import json
from datetime import datetime

def comprehensive_ssl_analysis(hostname: str, port: int = 443) -> dict:
    """
    Analisis komprehensif SSL/TLS certificate dan konfigurasi.
    """
    context = ssl.create_default_context()
    
    results = {
        'hostname': hostname,
        'timestamp': datetime.utcnow().isoformat()
    }
    
    try:
        with socket.create_connection((hostname, port), timeout=10) as sock:
            with context.wrap_socket(sock, server_hostname=hostname) as ssock:
                cert = ssock.getpeercert()
                
                # TLS Version
                results['tls_version'] = ssock.version()
                
                # Cipher Suite
                cipher = ssock.cipher()
                results['cipher_suite'] = {
                    'name': cipher[0],
                    'protocol': cipher[1],
                    'key_bits': cipher[2]
                }
                
                # Certificate Details
                subject = dict(x[0] for x in cert.get('subject', []))
                issuer = dict(x[0] for x in cert.get('issuer', []))
                
                results['certificate'] = {
                    'subject': subject,
                    'issuer': issuer,
                    'common_name': subject.get('commonName', 'N/A'),
                    'organization': subject.get('organizationName', 'N/A')
                }
                
                # Validity
                not_after_str = cert.get('notAfter', '')
                not_after = datetime.strptime(not_after_str, '%b %d %H:%M:%S %Y %Z')
                days_left = (not_after - datetime.utcnow()).days
                
                results['validity'] = {
                    'not_before': cert.get('notBefore'),
                    'not_after': not_after_str,
                    'days_until_expiry': days_left,
                    'status': 'VALID' if days_left > 0 else 'EXPIRED',
                    'warning': days_left < 30
                }
                
                # Subject Alt Names
                san_list = [
                    name[1] for name in cert.get('subjectAltName', [])
                    if name[0] == 'DNS'
                ]
                results['subject_alt_names'] = san_list
                
                # Security Assessment
                assessment = []
                
                if results['tls_version'] in ('TLSv1', 'TLSv1.1', 'SSLv3'):
                    assessment.append({
                        'severity': 'CRITICAL',
                        'issue': f"Outdated TLS version: {results['tls_version']}",
                        'recommendation': 'Upgrade to TLS 1.2 or 1.3'
                    })
                
                if 'RC4' in cipher[0] or 'DES' in cipher[0] or 'MD5' in cipher[0]:
                    assessment.append({
                        'severity': 'HIGH',
                        'issue': f"Weak cipher suite: {cipher[0]}",
                        'recommendation': 'Use AES-256-GCM or ChaCha20-Poly1305'
                    })
                
                if days_left < 30:
                    assessment.append({
                        'severity': 'HIGH' if days_left < 7 else 'MEDIUM',
                        'issue': f"Certificate expires in {days_left} days",
                        'recommendation': 'Renew certificate immediately'
                    })
                
                if not assessment:
                    assessment.append({
                        'severity': 'OK',
                        'issue': 'No major issues found',
                        'recommendation': 'Continue monitoring'
                    })
                
                results['assessment'] = assessment
    
    except ssl.SSLCertVerificationError as e:
        results['error'] = f"Certificate verification failed: {e}"
    except Exception as e:
        results['error'] = f"Connection failed: {e}"
    
    return results


def print_ssl_report(analysis: dict):
    """Print SSL analysis dalam format yang mudah dibaca."""
    print(f"\n{'='*60}")
    print(f"SSL/TLS Analysis: {analysis['hostname']}")
    print(f"{'='*60}")
    
    if 'error' in analysis:
        print(f"❌ ERROR: {analysis['error']}")
        return
    
    # TLS Info
    print(f"\n📡 Protocol:")
    print(f"   TLS Version: {analysis.get('tls_version', 'N/A')}")
    print(f"   Cipher Suite: {analysis.get('cipher_suite', {}).get('name', 'N/A')}")
    
    # Certificate
    cert = analysis.get('certificate', {})
    print(f"\n📜 Certificate:")
    print(f"   Common Name: {cert.get('common_name', 'N/A')}")
    print(f"   Organization: {cert.get('organization', 'N/A')}")
    print(f"   Issuer: {analysis.get('certificate', {}).get('issuer', {})}")
    
    # Validity
    validity = analysis.get('validity', {})
    days = validity.get('days_until_expiry', 0)
    status_icon = '✅' if days > 30 else ('⚠️' if days > 0 else '❌')
    print(f"\n⏱️ Validity:")
    print(f"   {status_icon} Expires: {validity.get('not_after', 'N/A')} ({days} days)")
    
    # SAN
    san = analysis.get('subject_alt_names', [])
    print(f"\n🌐 Subject Alt Names: {', '.join(san[:5])}")
    if len(san) > 5:
        print(f"   ... and {len(san)-5} more")
    
    # Assessment
    print(f"\n🔍 Security Assessment:")
    for item in analysis.get('assessment', []):
        icon = {'CRITICAL': '🔴', 'HIGH': '🟠', 'MEDIUM': '🟡', 'OK': '🟢'}.get(item['severity'], '⚪')
        print(f"   {icon} [{item['severity']}] {item['issue']}")
        print(f"      → {item['recommendation']}")


# Analisis beberapa website fintech Indonesia
# (jalankan saat ada koneksi internet)
websites = ['api.xendit.co', 'app.midtrans.com']
for site in websites:
    analysis = comprehensive_ssl_analysis(site)
    print_ssl_report(analysis)
```

### Self-Signed Certificate (untuk Development)

```bash
# Generate private key
openssl genrsa -out server.key 2048

# Generate Certificate Signing Request (CSR)
openssl req -new -key server.key -out server.csr \
  -subj "/C=ID/ST=Jakarta/L=Jakarta Selatan/O=MyApp Dev/CN=localhost"

# Generate self-signed certificate (valid 1 tahun)
openssl x509 -req -days 365 \
  -in server.csr \
  -signkey server.key \
  -out server.crt \
  -extfile <(printf "subjectAltName=DNS:localhost,IP:127.0.0.1")

# Gunakan di Flask/Python development server:
# app.run(ssl_context=('server.crt', 'server.key'))

# Catatan: Self-signed JANGAN digunakan di production!
# Gunakan Let's Encrypt (gratis) atau beli dari CA untuk production.
```

```python
# Flask dengan HTTPS (development)
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return {'message': 'HTTPS working!'}

if __name__ == '__main__':
    # Development: self-signed cert
    app.run(
        host='0.0.0.0',
        port=443,
        ssl_context=('server.crt', 'server.key'),
        debug=True
    )
```

---

## 🏢 Studi Kasus: Analisis HTTPS pada Payment Gateway Lokal

**Konteks:** Perbandingan keamanan TLS pada 3 payment gateway Indonesia.

**Pengujian menggunakan SSL Labs (ssllabs.com/ssltest):**

```
Midtrans (api.midtrans.com):
  Grade: A+
  TLS Versions: TLS 1.2, TLS 1.3
  Cipher Suites: TLS_AES_256_GCM_SHA384, ECDHE-RSA-AES256-GCM-SHA384
  HSTS: Yes (max-age=31536000; includeSubDomains; preload)
  Certificate: EV (Extended Validation)
  Key: RSA 2048-bit
  OCSP Stapling: Yes

Xendit (api.xendit.co):
  Grade: A
  TLS Versions: TLS 1.2, TLS 1.3
  HSTS: Yes
  Certificate: OV (Organization Validation)
  ECDHE: Yes (Perfect Forward Secrecy)

Contoh Buruk (website yang belum diupgrade):
  Grade: C atau F
  TLS Versions: TLS 1.0, TLS 1.1 (keduanya deprecated!)
  Cipher Suites: RC4, 3DES (broken!)
  HSTS: No
  → Rentan POODLE, BEAST, SWEET32 attacks
```

**Lessons from scoring:**
* Grade A+ = HSTS preload + Qualys SSL Labs clean
* Grade B = usually weak cipher suite atau missing HSTS
* Grade F = certificate expired atau TLS 1.0 masih enabled
* Semua fintech yang ingin PCI-DSS compliance HARUS minimal grade A

---

## ⚠️ Kesalahan Umum

1. **Menggunakan HTTP untuk API internal** → "API internal aman, tidak perlu HTTPS" → Serangan lateral movement: jika attacker ada di internal network, traffic plaintext = exposed. HTTPS everywhere, bahkan internal.

2. **Tidak monitor certificate expiry** → Certificate expired = layanan down dan browser warning. Setup monitoring alerting 30/14/7 hari sebelum expire. Let's Encrypt menyediakan auto-renewal gratis.

3. **TLS 1.0 dan 1.1 masih diaktifkan** → Keduanya deprecated (IETF 2021). Disable dan hanya gunakan TLS 1.2 + TLS 1.3.

4. **Mengorbankan keamanan untuk kompatibilitas** → "Kami perlu support IE8" → IE8 tidak support TLS 1.2. Pilihan: keamanan atau IE8. Jawabannya: keamanan selalu menang.

5. **Certificate pinning tanpa backup pin** → Jika certificate expire dan tidak ada backup pin, semua user mobile tidak bisa connect sampai update app. Selalu pin 2+ kunci.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan TLS handshake step by step. Bagaimana keduanya (browser dan server) mendapatkan session key yang sama tanpa Eve bisa mengetahuinya?

b) Apa itu HSTS? Mengapa hanya redirect 301 ke HTTPS tidak cukup untuk mencegah HSTS stripping attack?

c) Kapan certificate pinning diperlukan? Sebutkan trade-off antara keamanan ekstra dari certificate pinning dan risiko operasionalnya.

### Soal 2 — Praktik (Lab)

1. Lakukan analisis SSL pada 3 website e-commerce Indonesia menggunakan:
   - Browser DevTools (Security tab)
   - Kode Python di atas
   - ssl-labs.com (opsional)
   
2. Untuk setiap website, dokumentasikan:
   - Versi TLS yang digunakan
   - Cipher suite
   - Tanggal expiry sertifikat
   - Apakah HSTS diaktifkan

3. Generate self-signed certificate dan setup Flask server dengan HTTPS di localhost

---

## 📌 Ringkasan

* **HTTP:** plaintext — siapapun di network bisa baca dan modifikasi data
* **HTTPS** = HTTP over TLS: enkripsi + integrity + server authentication
* **TLS Handshake:** cipher negotiation → certificate verification → ECDHE key exchange → session key → encrypted data
* **TLS 1.3:** lebih cepat (1-RTT) dan lebih aman dari TLS 1.2
* **HSTS:** browser paksa HTTPS bahkan untuk kunjungan pertama — cegah downgrade attack
* **Certificate Pinning:** aplikasi mobile "pin" public key server yang benar — cegah rogue CA
* **OCSP:** real-time check apakah sertifikat sudah direvoke
* Gunakan SSL Labs untuk scoring — target minimum Grade A untuk produksi
* Self-signed cert = hanya untuk development, JANGAN production

---

*📚 Referensi: Stallings, W. (2022). Cryptography and Network Security | RFC 8446 — TLS 1.3 | Mozilla SSL Configuration Generator | SSL Labs Best Practices | OWASP Transport Layer Security Cheat Sheet*
