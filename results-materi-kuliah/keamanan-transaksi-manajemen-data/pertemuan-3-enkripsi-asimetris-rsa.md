# Pertemuan 3: Enkripsi Asimetris — RSA, ECC & Diffie-Hellman

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan konsep pasangan kunci publik-privat dan one-way function
* Menjelaskan cara kerja RSA: key generation, enkripsi, dan dekripsi
* Membandingkan RSA vs ECC dari sisi keamanan dan efisiensi
* Menjelaskan Diffie-Hellman Key Exchange untuk secure key sharing
* Mengimplementasikan RSA encryption menggunakan Python

---

## 📖 Pengantar (Hook)

Bayangkan kamu ingin menerima pesan rahasia dari siapa saja, tanpa harus bertemu dulu untuk bertukar kunci.

Solusinya: kamu pasang **gembok terbuka** di depan rumahmu. Siapa pun bisa memasukkan pesan ke kotak, lalu menutupnya dengan gembok itu. Tapi **hanya kamu yang punya kunci** untuk membukanya.

Itulah enkripsi asimetris. Gembok terbuka = public key. Kunci = private key.

Inilah teknologi yang memungkinkan kamu berbelanja di Shopee dengan aman, mengirim email ke seseorang yang belum pernah kamu temui, dan memverifikasi bahwa aplikasi yang kamu download benar-benar dari developer asli — semua tanpa perlu bertukar rahasia lebih dulu.

---

## 🧩 Konsep Utama

### Public Key Cryptography: Konsep Dasar

```
ENKRIPSI SIMETRIS (masalah distribusi kunci):
  Alice ──[kunci rahasia bersama]──► Bob
  Pertanyaan: Bagaimana mereka berbagi kunci tanpa ketahuan Eve?

ENKRIPSI ASIMETRIS (solusi):
  Bob generate dua kunci:
    🔑 Private Key → HANYA Bob yang tahu, tidak pernah dibagikan
    🔓 Public Key  → Siapapun boleh tahu, disebarkan secara publik
  
  Alice ingin kirim pesan rahasia ke Bob:
    1. Alice ambil Public Key Bob (dari website, email, dll)
    2. Alice enkripsi pesan dengan Public Key Bob
    3. Alice kirim ciphertext ke Bob
    4. BOB DEKRIPSI dengan Private Key-nya
    
  Eve melihat ciphertext dan Public Key Bob:
    → Tanpa Private Key Bob, Eve tidak bisa dekripsi
    → Aman!
```

### One-Way Function: Pondasi Matematika

Keamanan enkripsi asimetris bergantung pada **one-way function** — fungsi yang mudah dihitung ke satu arah tapi sangat sulit dibalik:

```
Contoh one-way function yang digunakan:

RSA: Factoring Problem
  Mudah: 61 × 53 = 3233 (perkalian dua prima)
  Sulit: 3233 = ? × ? (faktorisasi bilangan besar)
  
  RSA-2048 bit: Faktorisasi bilangan 617-digit
  → Komputer terkuat di dunia butuh 300+ juta tahun

ECC: Elliptic Curve Discrete Logarithm Problem
  Mudah: k × P = Q (multiplikasi titik pada kurva eliptik)
  Sulit: Q ÷ P = k (menemukan k dari Q dan P)
  → Lebih sulit dari factoring dengan kunci lebih pendek
```

### RSA (Rivest-Shamir-Adleman)

#### Cara Kerja RSA (Sederhana)

```
KEY GENERATION:
1. Pilih dua prima besar: p = 61, q = 53
2. n = p × q = 3233  (modulus, bagian dari public & private key)
3. φ(n) = (p-1)(q-1) = 60 × 52 = 3120
4. Pilih e: gcd(e, φ(n)) = 1, biasanya e = 65537
5. Hitung d: e × d ≡ 1 (mod φ(n))  → d = 2753

Public Key:  (e=65537, n=3233)  ← dibagikan ke siapapun
Private Key: (d=2753, n=3233)   ← JAGA KERAHASIAAN

ENKRIPSI (Alice menggunakan Public Key Bob):
  c = m^e mod n

DEKRIPSI (Bob menggunakan Private Key):
  m = c^d mod n
```

#### RSA Key Sizes dan Keamanan

```
┌────────────────┬──────────────┬────────────────────────────┐
│ RSA Key Size   │ Security Level│ Keterangan                 │
├────────────────┼──────────────┼────────────────────────────┤
│ 1024-bit       │ ~80-bit      │ TIDAK AMAN, jangan gunakan  │
│ 2048-bit       │ ~112-bit     │ Minimum untuk use case baru │
│ 3072-bit       │ ~128-bit     │ Rekomendasi NIST 2030+      │
│ 4096-bit       │ ~140-bit     │ High security, tapi lambat  │
└────────────────┴──────────────┴────────────────────────────┘

⚠️ RSA LAMBAT untuk data besar:
  RSA enkripsi 1KB data: ~1ms
  AES enkripsi 1MB data: ~0.1ms
  AES enkripsi 1GB data: ~100ms

→ RSA TIDAK digunakan untuk enkripsi data besar!
→ Gunakan RSA untuk mengenkripsi KUNCI AES, bukan data itu sendiri
→ Ini disebut "hybrid encryption" (TLS melakukan ini)
```

### ECC (Elliptic Curve Cryptography)

```
KEUNGGULAN ECC vs RSA:
  ECC-256 bit ≈ RSA-3072 bit dalam hal keamanan

┌────────────────┬──────────────┬──────────────┐
│ Security Level │ RSA Key Size │ ECC Key Size  │
├────────────────┼──────────────┼──────────────┤
│ 128-bit        │ 3072-bit     │ 256-bit       │
│ 192-bit        │ 7680-bit     │ 384-bit       │
│ 256-bit        │ 15360-bit    │ 521-bit       │
└────────────────┴──────────────┴──────────────┘

Keunggulan ECC:
✓ Kunci lebih pendek → data lebih kecil, proses lebih cepat
✓ Sangat cocok untuk perangkat mobile dan IoT (resource terbatas)
✓ TLS 1.3 menggunakan ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)
✓ Bitcoin dan Ethereum menggunakan secp256k1 (ECC)
```

### Diffie-Hellman Key Exchange

DH bukan enkripsi data — ini adalah protokol untuk **menukar kunci rahasia secara aman melalui kanal publik**.

```
MASALAH: Alice dan Bob ingin kunci rahasia bersama untuk AES
         tanpa Eve yang mendengar bisa tahu kunci itu

SOLUSI DIFFIE-HELLMAN:
1. Sepakati bilangan publik: g=5, p=23 (siapapun boleh tahu)

2. Alice pilih secret a=6 (rahasia)
   Alice hitung: A = g^a mod p = 5^6 mod 23 = 8
   Alice kirim A=8 ke Bob (Eve bisa lihat, tidak apa-apa)

3. Bob pilih secret b=15 (rahasia)
   Bob hitung: B = g^b mod p = 5^15 mod 23 = 19
   Bob kirim B=19 ke Alice (Eve bisa lihat, tidak apa-apa)

4. Alice hitung shared secret: s = B^a mod p = 19^6 mod 23 = 2
5. Bob hitung shared secret:   s = A^b mod p = 8^15 mod 23 = 2

→ Alice dan Bob SAMA-SAMA mendapatkan s=2!
→ Eve melihat: g=5, p=23, A=8, B=19 → TIDAK BISA hitung s tanpa a atau b!

Shared secret ini kemudian digunakan sebagai kunci AES!
```

---

## 🧠 Ilustrasi / Analogi

**RSA seperti kotak surat dengan kunci gembok publik:**
* Kamu pasang gembok terbuka di depan rumah (public key)
* Siapa pun bisa memasukkan surat dan mengunci gembok
* Hanya kamu yang punya kunci (private key) untuk membuka

**Kenapa RSA tidak untuk enkripsi data besar?**
* RSA seperti mesin ketik manual — bisa mengetik, tapi sangat lambat untuk novel 500 halaman
* AES seperti printer laser — jauh lebih cepat untuk konten besar
* Solusi: gunakan RSA untuk mengenkripsi "kunci" AES, lalu gunakan AES untuk datanya

**Diffie-Hellman seperti pencampuran cat:**
* Alice dan Bob masing-masing punya warna rahasia (a dan b)
* Mereka campurkan warna rahasia mereka dengan warna publik (g)
* Kirim hasil campuran ke satu sama lain (A dan B) — Eve bisa lihat
* Masing-masing tambahkan warna rahasia mereka sendiri → dapat warna yang SAMA
* Eve tidak bisa memisahkan campuran cat untuk tahu warna rahasia masing-masing

---

## 💻 Contoh Teknis

### Implementasi RSA di Python

```python
"""
Implementasi RSA menggunakan library cryptography
Install: pip install cryptography
"""
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.hazmat.backends import default_backend
import base64
import time

class RSAEncryptor:
    """RSA-2048 encryption untuk pertukaran kunci (bukan data besar!)"""
    
    def __init__(self):
        self.private_key = None
        self.public_key = None
    
    def generate_key_pair(self, key_size: int = 2048):
        """Generate RSA key pair. key_size: 2048 (minimum) atau 4096."""
        print(f"Generating RSA-{key_size} key pair...")
        start = time.time()
        
        self.private_key = rsa.generate_private_key(
            public_exponent=65537,
            key_size=key_size,
            backend=default_backend()
        )
        self.public_key = self.private_key.public_key()
        
        elapsed = time.time() - start
        print(f"Key generation took: {elapsed:.3f} seconds")
    
    def get_public_key_pem(self) -> str:
        """Export public key dalam format PEM (bisa dibagikan)"""
        return self.public_key.public_bytes(
            encoding=serialization.Encoding.PEM,
            format=serialization.PublicFormat.SubjectPublicKeyInfo
        ).decode()
    
    def get_private_key_pem(self, password: bytes = None) -> str:
        """Export private key dalam format PEM (JAGA KERAHASIAAN, boleh dipassword)"""
        encryption = (
            serialization.BestAvailableEncryption(password)
            if password
            else serialization.NoEncryption()
        )
        return self.private_key.private_bytes(
            encoding=serialization.Encoding.PEM,
            format=serialization.PrivateFormat.PKCS8,
            encryption_algorithm=encryption
        ).decode()
    
    def encrypt(self, message: str) -> str:
        """
        Enkripsi pesan dengan public key.
        Gunakan OAEP padding (lebih aman dari PKCS1v15 untuk enkripsi).
        CATATAN: RSA-2048 hanya bisa enkripsi max ~190 bytes!
        → Dalam praktek, enkripsi KUNCI AES (32 bytes), bukan data besar
        """
        message_bytes = message.encode('utf-8')
        
        ciphertext = self.public_key.encrypt(
            message_bytes,
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        return base64.b64encode(ciphertext).decode()
    
    def decrypt(self, ciphertext_b64: str) -> str:
        """Dekripsi ciphertext dengan private key."""
        ciphertext = base64.b64decode(ciphertext_b64)
        
        plaintext = self.private_key.decrypt(
            ciphertext,
            padding.OAEP(
                mgf=padding.MGF1(algorithm=hashes.SHA256()),
                algorithm=hashes.SHA256(),
                label=None
            )
        )
        return plaintext.decode('utf-8')


# ===== Simulasi: Hybrid Encryption (seperti TLS) =====
# RSA untuk enkripsi AES key → AES untuk enkripsi data besar

from cryptography.hazmat.primitives.ciphers.aead import AESGCM
import os

def hybrid_encrypt(public_key_pem: str, large_data: str) -> dict:
    """
    Hybrid Encryption:
    1. Generate random AES-256 session key
    2. Enkripsi data dengan AES-256-GCM (cepat)
    3. Enkripsi AES key dengan RSA public key (aman)
    """
    from cryptography.hazmat.primitives.serialization import load_pem_public_key
    
    # Step 1: Generate AES session key
    aes_key = os.urandom(32)
    
    # Step 2: Enkripsi data besar dengan AES (cepat)
    aesgcm = AESGCM(aes_key)
    nonce = os.urandom(12)
    encrypted_data = aesgcm.encrypt(nonce, large_data.encode(), None)
    
    # Step 3: Enkripsi AES key dengan RSA (aman, tapi hanya 32 bytes)
    public_key = load_pem_public_key(public_key_pem.encode(), backend=default_backend())
    encrypted_aes_key = public_key.encrypt(
        aes_key,
        padding.OAEP(mgf=padding.MGF1(hashes.SHA256()), algorithm=hashes.SHA256(), label=None)
    )
    
    return {
        'encrypted_aes_key': base64.b64encode(encrypted_aes_key).decode(),
        'nonce': nonce.hex(),
        'encrypted_data': base64.b64encode(encrypted_data).decode()
    }


# Demo:
rsa = RSAEncryptor()
rsa.generate_key_pair(2048)

print("\n=== Public Key (bisa dibagikan) ===")
print(rsa.get_public_key_pem()[:200], "...")

print("\n=== Enkripsi & Dekripsi ===")
aes_key_plaintext = "AES-Session-Key-32bytes-random!!"
ciphertext = rsa.encrypt(aes_key_plaintext)
print(f"Plaintext : {aes_key_plaintext}")
print(f"Ciphertext: {ciphertext[:80]}...")
print(f"Decrypted : {rsa.decrypt(ciphertext)}")
```

### Perbandingan Performa RSA vs AES

```python
import time
import os
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives.ciphers.aead import AESGCM

# Generate keys
private_key = rsa.generate_private_key(65537, 2048, default_backend())
public_key = private_key.public_key()
aes_key = AESGCM.generate_key(256)

# Data 1KB
data_1kb = os.urandom(190)  # Max RSA-2048 OAEP payload

# Benchmark RSA
start = time.perf_counter()
for _ in range(100):
    c = public_key.encrypt(data_1kb, padding.OAEP(
        mgf=padding.MGF1(hashes.SHA256()), algorithm=hashes.SHA256(), label=None))
rsa_time = (time.perf_counter() - start) / 100 * 1000

# Benchmark AES
data_1mb = os.urandom(1024 * 1024)
aesgcm = AESGCM(aes_key)
nonce = os.urandom(12)
start = time.perf_counter()
for _ in range(10):
    aesgcm.encrypt(nonce, data_1mb, None)
aes_time = (time.perf_counter() - start) / 10 * 1000

print(f"RSA-2048 enkripsi 190 bytes: {rsa_time:.2f} ms")
print(f"AES-256-GCM enkripsi 1MB:    {aes_time:.2f} ms")
print(f"→ Untuk data besar, AES ~{int(rsa_time/aes_time*1000)}x lebih cepat dari RSA")
```

---

## 🏢 Studi Kasus: HTTPS di Midtrans Payment Gateway

**Konteks:** Setiap kali merchant mengirim data kartu ke Midtrans, komunikasi harus aman — mencegah eavesdropping.

**Bagaimana TLS 1.3 menggunakan kriptografi asimetris:**

```
TLS 1.3 Handshake (simplified):

1. CLIENT HELLO
   Browser → Midtrans: "Halo! Saya support cipher: AES-256-GCM-SHA384, ..."

2. SERVER HELLO + CERTIFICATE  
   Midtrans → Browser: "Pakai cipher ini. Ini sertifikatku (Public Key RSA/ECC)."

3. KEY EXCHANGE (ECDHE — Elliptic Curve Diffie-Hellman Ephemeral)
   Browser + Midtrans: Lakukan DH key exchange menggunakan ECC
   → Keduanya mendapatkan "session key" AES yang sama
   → Eve yang mendengar tidak bisa rekonstruksi session key

4. ENCRYPTED COMMUNICATION
   Semua data (nomor kartu, CVV) dienkripsi dengan AES session key
   → Aman karena hanya browser dan Midtrans yang punya kunci

Catatan ECDHE vs RSA key exchange:
   • RSA key exchange: session key dienkripsi dengan RSA public key server
     → Jika private key server bocor di masa depan → semua recording masa lalu terdekripsi
   • ECDHE: session key dibuat baru setiap sesi (ephemeral)
     → Perfect Forward Secrecy (PFS): bocornya private key tidak kompromikan masa lalu
```

---

## ⚠️ Kesalahan Umum

1. **Menggunakan RSA untuk enkripsi file besar** → RSA-2048 hanya bisa enkripsi ~190 bytes. Untuk data besar, gunakan hybrid encryption: RSA enkripsi kunci AES, AES enkripsi data.

2. **Menggunakan PKCS1v15 padding untuk enkripsi** → Rentan terhadap serangan Bleichenbacher's oracle. Selalu gunakan OAEP padding untuk RSA encryption.

3. **Reusing RSA key pair untuk bertahun-tahun tanpa rotasi** → Jika private key bocor (server breach), semua data historis bisa terdekripsi. Rotasi key secara berkala dan gunakan Perfect Forward Secrecy.

4. **Private key tersimpan dalam format unencrypted di server** → Gunakan HSM (Hardware Security Module) atau enkripsi private key dengan passphrase yang kuat.

5. **Menggunakan RSA-1024** → RSA-1024 sudah tidak aman (NIST deprecated 2010). Minimum RSA-2048, rekomendasinya RSA-3072 atau beralih ke ECC-256.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan antara enkripsi simetris dan asimetris. Dalam skenario TLS/HTTPS, keduanya digunakan bersamaan — jelaskan bagaimana dan mengapa.

b) Mengapa RSA tidak efisien untuk mengenkripsi file 100MB? Apa solusinya (hybrid encryption)?

c) Apa itu Perfect Forward Secrecy (PFS)? Mengapa TLS 1.3 menggunakan ECDHE (yang memiliki PFS) dibandingkan RSA key exchange?

### Soal 2 — Praktik

Buat program Python yang:
1. Generate RSA key pair 2048-bit
2. Simpan public key ke file `public_key.pem` dan private key ke file `private_key.pem` (dengan password proteksi)
3. Enkripsi string pendek (simulasi enkripsi AES session key) menggunakan public key
4. Dekripsi menggunakan private key
5. Bandingkan waktu generate key RSA-2048 vs RSA-4096 (gunakan `time.perf_counter()`)
6. Tunjukkan batasan ukuran data yang bisa dienkripsi RSA-2048 dengan OAEP padding

---

## 📌 Ringkasan

* **Enkripsi asimetris** = dua kunci berbeda: public key (siapapun boleh tahu) + private key (rahasia)
* **Enkripsi:** gunakan public key penerima. **Dekripsi:** gunakan private key penerima
* **RSA** = berdasarkan kesulitan faktorisasi bilangan prima besar. Minimum 2048-bit
* **ECC** = lebih efisien dari RSA: ECC-256 ≈ RSA-3072 dalam keamanan
* RSA hanya untuk data KECIL (max ~190 bytes untuk RSA-2048) → gunakan **hybrid encryption**
* **Diffie-Hellman** = cara aman menukar kunci melalui kanal publik tanpa pre-shared secret
* **TLS menggunakan ECDHE** untuk Perfect Forward Secrecy — bocornya private key tidak kompromikan sesi masa lalu

---

*📚 Referensi: Stallings, W. (2022). Cryptography and Network Security, 8th Ed. | NIST SP 800-56A — Key Establishment | Python cryptography.io | RFC 8446 — TLS 1.3 Specification*
