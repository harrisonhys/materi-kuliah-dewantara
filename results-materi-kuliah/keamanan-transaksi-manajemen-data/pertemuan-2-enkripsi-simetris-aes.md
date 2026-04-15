# Pertemuan 2: Enkripsi Simetris — AES, DES & Mode Operasi

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan prinsip kerja enkripsi simetris dan perbedaannya dengan enkripsi asimetris
* Membandingkan DES (deprecated) vs AES (standar industri) dari sisi keamanan dan performa
* Menjelaskan mode operasi AES: ECB, CBC, GCM dan kapan masing-masing digunakan
* Mengimplementasikan enkripsi dan dekripsi AES-256-CBC menggunakan Python

---

## 📖 Pengantar (Hook)

Bayangkan kamu mengirim pesan rahasia ke temanmu. Kamu punya **satu kunci gembok**, dan temanmu punya **salinan kunci yang sama**. Kamu kunci pesan → kirim → teman buka dengan kunci yang sama.

Itulah enkripsi simetris. Satu kunci untuk enkripsi, kunci yang sama untuk dekripsi.

Sederhana — tapi ini yang melindungi database 270 juta transaksi DANA setiap harinya. Kolom NIK, nomor kartu, dan saldo dienkripsi dengan AES-256 sebelum disimpan ke disk. Bahkan jika hacker berhasil dump seluruh database, yang mereka dapatkan hanyalah noise — data terenkripsi yang tidak ada artinya tanpa kunci.

---

## 🧩 Konsep Utama

### Prinsip Kerja Enkripsi Simetris

```
ENKRIPSI:
Plaintext + Key → [Algoritma Enkripsi] → Ciphertext

DEKRIPSI:
Ciphertext + Key → [Algoritma Dekripsi] → Plaintext

Kunci SAMA digunakan untuk dua arah!

Contoh:
Plaintext:  "Saldo: Rp 5.000.000"
Key:        b"supersecretkey32"  (harus dijaga kerahasiaannya!)
Ciphertext: b"\x3f\xa2\xb1..." (tidak terbaca)
```

**Karakteristik enkripsi simetris:**
* **Cepat:** Jauh lebih cepat dari enkripsi asimetris (RSA) untuk data besar
* **Satu kunci:** Sama untuk enkripsi dan dekripsi → **masalah distribusi kunci**
* **Use case:** Enkripsi data at rest (database), enkripsi data besar

### DES vs AES: Sejarah dan Perbandingan

#### DES (Data Encryption Standard) — DEPRECATED

```
DES (1977):
• Ukuran kunci: 56-bit
• Ukuran blok: 64-bit
• 16 rounds Feistel cipher

Mengapa sudah tidak aman?
• 56-bit = hanya 2^56 = ~72 quadrillion kemungkinan kunci
• Tahun 1998: EFF "Deep Crack" memecahkan DES dalam 22 JAM
• Sekarang dengan GPU modern: beberapa JAM
• Status: TIDAK BOLEH digunakan untuk aplikasi baru
```

#### Triple DES (3DES) — Juga Deprecated

```
3DES: DES dijalankan 3x dengan kunci berbeda
Ukuran kunci efektif: 112-bit
Lebih aman dari DES, tapi 3x lebih lambat
NIST deprecated 3DES tahun 2023
```

#### AES (Advanced Encryption Standard) — Standar Saat Ini

```
AES (2001, by Rijndael):
• Ukuran kunci: 128-bit, 192-bit, atau 256-bit
• Ukuran blok: 128-bit (tetap)
• Rounds: 10 (128-bit), 12 (192-bit), 14 (256-bit)

Mengapa aman?
• AES-128: 2^128 kemungkinan kunci
• Dengan seluruh komputasi di bumi: butuh triliunan tahun
• NSA menggunakan AES-256 untuk Top Secret information

Perbandingan:
┌──────────────┬────────────┬─────────┬─────────────────┐
│ Algoritma    │ Key Size   │ Status  │ Use Case        │
├──────────────┼────────────┼─────────┼─────────────────┤
│ DES          │ 56-bit     │ BROKEN  │ Jangan gunakan  │
│ 3DES         │ 112-bit    │ DEPRECATED│ Legacy only   │
│ AES-128      │ 128-bit    │ SECURE  │ General use     │
│ AES-256      │ 256-bit    │ SECURE  │ High security   │
└──────────────┴────────────┴─────────┴─────────────────┘
```

### Mode Operasi AES

AES adalah **block cipher** — enkripsi data blok 128-bit sekaligus. Untuk data lebih panjang, dibutuhkan "mode operasi" yang menentukan bagaimana blok-blok dienkripsi.

#### ECB (Electronic Codebook) — JANGAN DIGUNAKAN

```
ECB: Setiap blok dienkripsi INDEPENDEN dengan kunci yang sama

Plaintext:  [Blok1][Blok2][Blok3]
              ↓K     ↓K     ↓K
Ciphertext: [Enc1][Enc2][Enc3]

MASALAH: Blok plaintext yang SAMA → ciphertext yang SAMA!

Contoh visual (terkenal): enkripsi gambar dengan ECB
→ Pola gambar asli masih terlihat di ciphertext!
→ TIDAK AMAN untuk data yang memiliki pola berulang
```

#### CBC (Cipher Block Chaining) — Standar Umum

```
CBC: Setiap blok di-XOR dengan ciphertext blok SEBELUMNYA

Plaintext:  [P1]     [P2]     [P3]
             ⊕        ⊕        ⊕
IV ────────→[IV]  [C1]─→   [C2]─→
             ↓K     ↓K       ↓K
Ciphertext: [C1]    [C2]    [C3]

IV = Initialization Vector (random, tidak perlu rahasia, tapi harus unik)

KEUNGGULAN:
✓ Blok identik → ciphertext berbeda (karena IV dan chaining)
✓ Aman untuk enkripsi file, database

KELEMAHAN:
✗ Tidak bisa diparalelkan (harus sequential)
✗ Tidak ada authentication (tidak mendeteksi tampering)
```

#### GCM (Galois/Counter Mode) — AEAD, Rekomendasi Modern

```
GCM = Enkripsi (CTR mode) + Authentication (GHASH)
→ Authenticated Encryption with Associated Data (AEAD)

KEUNGGULAN:
✓ Enkripsi + Integritas dalam satu operasi
✓ Menghasilkan Authentication Tag (16 byte)
✓ Bisa diparalelkan
✓ Mendeteksi jika ciphertext dimodifikasi (tampering)

KAPAN DIGUNAKAN:
✓ HTTPS/TLS 1.3 menggunakan AES-256-GCM
✓ API communication (enkripsi + verifikasi integritas)
✓ Rekomendasi NIST untuk aplikasi modern
```

---

## 🧠 Ilustrasi / Analogi

**Enkripsi simetris seperti koper dengan kunci TSA:**
* Koper = data, kunci TSA = symmetric key
* Kamu kunci koper (enkripsi) → bawa ke bandara → baggage handler buka (dekripsi) dengan kunci yang sama
* Masalah: bagaimana kamu dan baggage handler punya kunci yang sama tanpa ketahuan orang lain?
* → Inilah "key distribution problem" yang diselesaikan oleh enkripsi asimetris (pertemuan 3)

**Mode ECB vs CBC:**
* **ECB** = Setiap amplop dikirim dengan stempel yang sama. Kalau isi dua amplop sama, stempel luarnya juga sama → mudah ditebak polanya
* **CBC** = Setiap amplop dikombinasikan dengan isi amplop sebelumnya. Amplop identik → stempel berbeda karena histori berbeda

---

## 💻 Contoh Teknis

### Implementasi AES-256-CBC di Python

```python
"""
Implementasi AES-256-CBC menggunakan library cryptography
Install: pip install cryptography
"""
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend
import os
import base64

class AESEncryptor:
    """
    Enkripsi/dekripsi menggunakan AES-256-CBC.
    Key harus 32 bytes (256-bit).
    """
    
    def __init__(self, key: bytes):
        if len(key) != 32:
            raise ValueError("Key harus tepat 32 bytes (256-bit)")
        self.key = key
    
    def encrypt(self, plaintext: str) -> dict:
        """
        Enkripsi string dan kembalikan IV + ciphertext dalam base64.
        IV dibuat random baru setiap enkripsi (penting untuk keamanan CBC!).
        """
        # Generate IV random 16 bytes — HARUS unik untuk setiap enkripsi
        iv = os.urandom(16)
        
        # Padding manual menggunakan PKCS7
        plaintext_bytes = plaintext.encode('utf-8')
        pad_len = 16 - (len(plaintext_bytes) % 16)
        padded = plaintext_bytes + bytes([pad_len] * pad_len)
        
        # Enkripsi
        cipher = Cipher(
            algorithms.AES(self.key),
            modes.CBC(iv),
            backend=default_backend()
        )
        encryptor = cipher.encryptor()
        ciphertext = encryptor.update(padded) + encryptor.finalize()
        
        return {
            'iv': base64.b64encode(iv).decode(),
            'ciphertext': base64.b64encode(ciphertext).decode()
        }
    
    def decrypt(self, iv_b64: str, ciphertext_b64: str) -> str:
        """
        Dekripsi ciphertext menggunakan IV yang tersimpan.
        """
        iv = base64.b64decode(iv_b64)
        ciphertext = base64.b64decode(ciphertext_b64)
        
        cipher = Cipher(
            algorithms.AES(self.key),
            modes.CBC(iv),
            backend=default_backend()
        )
        decryptor = cipher.decryptor()
        padded = decryptor.update(ciphertext) + decryptor.finalize()
        
        # Unpad PKCS7
        pad_len = padded[-1]
        return padded[:-pad_len].decode('utf-8')


# ============ Simulasi Penggunaan di Fintech ============

def demo_database_column_encryption():
    """Simulasi enkripsi kolom database sensitif (seperti NIK, nomor kartu)"""
    
    # Key disimpan di environment variable atau Key Management Service
    # JANGAN hardcode key di source code!
    key = os.urandom(32)  # Dalam produksi: ambil dari AWS KMS / HashiCorp Vault
    
    encryptor = AESEncryptor(key)
    
    # Data sensitif yang akan disimpan ke database
    sensitive_data = {
        'nik': '3273012501900001',
        'nomor_kartu': '4111111111111111',
        'saldo': '5000000'
    }
    
    print("=== DATA SEBELUM ENKRIPSI ===")
    for field, value in sensitive_data.items():
        print(f"  {field}: {value}")
    
    print("\n=== DATA SETELAH ENKRIPSI (disimpan ke DB) ===")
    encrypted_data = {}
    for field, value in sensitive_data.items():
        result = encryptor.encrypt(value)
        # Simpan IV bersamaan dengan ciphertext (tidak perlu rahasia)
        encrypted_data[field] = f"{result['iv']}:{result['ciphertext']}"
        print(f"  {field}: {encrypted_data[field][:60]}...")
    
    print("\n=== DEKRIPSI SAAT DIBUTUHKAN ===")
    for field, encrypted_value in encrypted_data.items():
        iv_b64, ciphertext_b64 = encrypted_value.split(':', 1)
        decrypted = encryptor.decrypt(iv_b64, ciphertext_b64)
        print(f"  {field}: {decrypted}")

demo_database_column_encryption()
```

### AES-256-GCM (Rekomendasi Modern — dengan Authentication Tag)

```python
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
import os

def encrypt_aes_gcm(key: bytes, plaintext: str, associated_data: bytes = None) -> dict:
    """
    AES-256-GCM: Enkripsi + Authentication dalam satu operasi.
    associated_data: data yang di-authenticate tapi tidak dienkripsi
                     (misalnya: user_id, timestamp untuk mencegah replay attack)
    """
    aesgcm = AESGCM(key)
    nonce = os.urandom(12)  # GCM nonce: 12 bytes standar
    
    plaintext_bytes = plaintext.encode('utf-8')
    # ciphertext sudah include 16-byte authentication tag di akhir
    ciphertext = aesgcm.encrypt(nonce, plaintext_bytes, associated_data)
    
    return {
        'nonce': nonce.hex(),
        'ciphertext': ciphertext.hex()
    }

def decrypt_aes_gcm(key: bytes, nonce_hex: str, ciphertext_hex: str, 
                    associated_data: bytes = None) -> str:
    """
    Dekripsi + verifikasi integritas.
    Jika ciphertext dimodifikasi → InvalidTag exception (tampering terdeteksi!)
    """
    from cryptography.exceptions import InvalidTag
    
    aesgcm = AESGCM(key)
    nonce = bytes.fromhex(nonce_hex)
    ciphertext = bytes.fromhex(ciphertext_hex)
    
    try:
        plaintext = aesgcm.decrypt(nonce, ciphertext, associated_data)
        return plaintext.decode('utf-8')
    except InvalidTag:
        raise ValueError("PERINGATAN: Data telah dimodifikasi (tampering detected)!")

# Demo:
key = AESGCM.generate_key(bit_length=256)
user_id = b"user_123"  # associated data — tidak dienkripsi tapi di-authenticate

encrypted = encrypt_aes_gcm(key, "Transfer: Rp 500.000", associated_data=user_id)
print("Encrypted:", encrypted)

decrypted = decrypt_aes_gcm(key, encrypted['nonce'], encrypted['ciphertext'], user_id)
print("Decrypted:", decrypted)
```

---

## 🏢 Studi Kasus: Enkripsi Database DANA

**Konteks:** DANA (dompet digital Ant Group Indonesia) menyimpan data sensitif 130+ juta pengguna.

**Masalah:** Data NIK, nomor kartu tersimpan di database — bagaimana jika database di-dump?

**Solusi DANA (arsitektur tipikal fintech-grade):**

```
ARSITEKTUR ENKRIPSI DATA AT REST:

Aplikasi Layer:
  ┌─────────────────────────────────────────┐
  │  Data Masuk: NIK = "3273012501900001"   │
  │  ↓                                       │
  │  Enkripsi: AES-256-GCM                   │
  │  Key Source: AWS KMS (bukan hardcode)    │
  │  ↓                                       │
  │  Tersimpan: IV + Ciphertext + Auth Tag   │
  └─────────────────────────────────────────┘

Database Layer:
  ┌─────────────────────────────────────────┐
  │  Kolom: nik_encrypted (TEXT)            │
  │  Value: "8f2a...:3f9b...:2e1c..."       │
  │  (IV:Ciphertext:AuthTag dalam hex)      │
  └─────────────────────────────────────────┘

Key Management:
  ┌─────────────────────────────────────────┐
  │  AWS KMS / HashiCorp Vault              │
  │  • Key tidak pernah menyentuh disk app  │
  │  • Rotasi key otomatis setiap 90 hari   │
  │  • Audit log setiap penggunaan key      │
  └─────────────────────────────────────────┘
```

**Hasil:** Bahkan jika seluruh database bocor, data NIK dan nomor kartu tetap tidak terbaca tanpa kunci dari KMS.

**Key Management Rule #1:** Kunci enkripsi TIDAK BOLEH disimpan di database yang sama dengan data terenkripsi. Seperti menyimpan kunci di dalam brankas yang dikunci kunci itu sendiri.

---

## ⚠️ Kesalahan Umum

1. **Menggunakan ECB mode** → Pola data terlihat dalam ciphertext. JANGAN pernah gunakan ECB untuk data apapun yang memiliki pola (hampir semua data nyata).

2. **Hardcode key di source code** → `key = b"mysecretkey12345"` di Git repository → SEMUA orang yang akses repo bisa dekripsi data. Gunakan environment variable atau Key Management Service.

3. **Menggunakan IV/nonce yang sama berulang (nonce reuse)** → Dalam CBC: membocorkan pola. Dalam GCM: CATASTROPHIC — bisa mengekspos plaintext dan authentication key. SELALU generate IV/nonce baru setiap enkripsi.

4. **Menyimpan IV/nonce secara terpisah atau tidak sama sekali** → Tanpa IV, dekripsi CBC tidak mungkin. IV boleh publik — simpan bersama ciphertext (format: `iv:ciphertext`).

5. **Menggunakan DES atau MD5 untuk keamanan** → DES dipecahkan tahun 1998. MD5 bukan enkripsi (itu hash). Jangan gunakan keduanya untuk sistem baru.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan antara enkripsi simetris dan enkripsi asimetris. Mengapa enkripsi simetris (AES) digunakan untuk enkripsi data, sedangkan enkripsi asimetris (RSA) digunakan untuk pertukaran kunci?

b) Apa kelemahan mode ECB? Mengapa CBC lebih aman? Mengapa GCM lebih direkomendasikan dari CBC?

c) Sebuah developer menyimpan kunci AES di file `config.py` dan menguploadnya ke GitHub. Ancaman keamanan apa saja yang timbul?

### Soal 2 — Praktik

Buat program Python yang:
1. Minta user input teks (misalnya "Data rahasia: NIK=3273xxxxxx")
2. Generate random key AES-256 dan IV
3. Enkripsi input menggunakan AES-256-CBC
4. Tampilkan: key (hex), IV (hex), ciphertext (base64)
5. Dekripsi kembali dan verifikasi hasilnya sama dengan input

Bonus: Modifikasi program untuk menggunakan AES-256-GCM dan tambahkan associated data berisi timestamp.

---

## 📌 Ringkasan

* **Enkripsi simetris** = satu kunci untuk enkripsi dan dekripsi — cepat, cocok untuk data besar
* **DES** = deprecated (56-bit, dipecahkan 1998). **AES** = standar industri (128/256-bit, aman)
* **AES-256** = 2^256 kemungkinan kunci — secara praktis tidak bisa di-brute-force
* Mode operasi: **ECB** (JANGAN) → **CBC** (umum, butuh IV) → **GCM** (AEAD, rekomendasi modern)
* **GCM** = enkripsi + authentication tag dalam satu operasi → mendeteksi tampering
* Key management: JANGAN hardcode key. Gunakan environment variable atau KMS.
* Selalu generate **IV/nonce baru** untuk setiap enkripsi

---

*📚 Referensi: Stallings, W. (2022). Cryptography and Network Security, 8th Ed. | NIST FIPS 197 — AES Specification | Python cryptography.io | NIST SP 800-38D — GCM Mode*
