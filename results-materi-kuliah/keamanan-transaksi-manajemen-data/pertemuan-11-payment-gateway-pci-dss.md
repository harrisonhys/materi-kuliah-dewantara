# Pertemuan 11: Payment Gateway, Tokenisasi Kartu & PCI-DSS

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan alur transaksi e-commerce dari merchant ke bank melalui payment gateway
* Mendeskripsikan mekanisme tokenisasi kartu kredit (PAN → token)
* Menjelaskan 12 requirement PCI-DSS dan implikasinya bagi developer
* Membandingkan arsitektur payment gateway lokal: Midtrans, Xendit, DOKU

---

## 📖 Pengantar (Hook)

Setiap kali kamu belanja online dan memasukkan nomor kartu kredit, terjadi sesuatu yang jarang disadari:

Dalam 2-3 detik, data kartu kamu melewati setidaknya 5 sistem berbeda: browser → merchant server → payment gateway → jaringan kartu (Visa/Mastercard) → bank penerbit → balik lagi. Semua dalam kondisi terenkripsi, tokenized, dan diaudit.

Dan yang lebih menarik: merchant seperti Tokopedia **tidak pernah melihat** nomor kartu kamu yang sesungguhnya. Yang mereka terima hanyalah sebuah "token" — string acak yang tidak berguna bagi siapapun kecuali payment gateway.

Ini adalah arsitektur keamanan bertingkat yang memungkinkan 900 juta transaksi kartu terjadi setiap harinya di Indonesia — dengan tingkat fraud yang relatif rendah.

---

## 🧩 Konsep Utama

### Arsitektur Ekosistem Pembayaran

```
PEMAIN DALAM EKOSISTEM:

┌──────────┐   ┌──────────────┐   ┌──────────────────┐   ┌──────────────┐
│  Pembeli │   │   Merchant   │   │ Payment Gateway  │   │   Acquirer   │
│  (Buyer) │   │ (Tokopedia,  │   │ (Midtrans,       │   │ (Bank Merchant│
│          │   │  Shopee, dll)│   │  Xendit, DOKU)   │   │ BCA, Mandiri) │
└────┬─────┘   └──────┬───────┘   └────────┬─────────┘   └──────┬───────┘
     │                │                    │                     │
     │  1. Pilih      │                    │                     │
     │  produk+bayar  │                    │                     │
     │──────────────► │                    │                     │
     │                │  2. Kirim data     │                     │
     │                │  transaksi         │                     │
     │                │──────────────────► │                     │
     │                │                    │  3. Forward ke      │
     │                │                    │  jaringan kartu     │
     │                │                    │────────────────────►│
     │                │                    │                     │
     │                │                    │ ◄────────────────────
     │                │                    │  4. Approval/Decline│
     │                │  5. Result         │                     │
     │                │ ◄──────────────────│                     │
     │  6. Konfirmasi │                    │                     │
     │ ◄──────────────│                    │                     │

Dan juga ada:
  Jaringan Kartu (Visa/Mastercard) ← routing antara acquirer dan issuer
  Bank Penerbit (Issuer) ← bank yang menerbitkan kartu pembeli
```

### Alur Transaksi Detail

```
ALUR PEMBAYARAN KARTU KREDIT DI TOKOPEDIA (Simplified):

1. USER CHECKOUT
   User masukkan: 4111 1111 1111 1111 | 12/26 | 123 (CVV)
   
2. TOKENIZATION DI BROWSER (Midtrans.js)
   Nomor kartu TIDAK dikirim ke server Tokopedia!
   JavaScript library Midtrans:
   → Kirim data kartu LANGSUNG ke server Midtrans (via HTTPS)
   → Midtrans return: TOKEN = "tok_abc123xyz" 
   → HANYA TOKEN ini yang dikirim ke backend Tokopedia
   
3. BACKEND TOKOPEDIA → MIDTRANS API
   POST /v2/charge
   {
     "payment_type": "credit_card",
     "credit_card": {"token_id": "tok_abc123xyz"},
     "transaction_details": {"order_id": "ORD-001", "gross_amount": 500000}
   }
   
4. MIDTRANS → BANK ACQUIRER
   Midtrans mengirim data ke jaringan Visa/Mastercard
   
5. JARINGAN KARTU → BANK PENERBIT
   Bank verifikasi: limit cukup? Kartu aktif? Fraud check?
   
6. RESPONSE CHAIN (balik)
   Bank → Jaringan → Acquirer → Midtrans → Tokopedia → User
   
7. SETTLEMENT (biasanya T+1 atau T+2)
   Dana berpindah dari bank penerbit ke rekening merchant

YANG TOKOPEDIA SIMPAN: token + order info (BUKAN nomor kartu)
YANG MIDTRANS SIMPAN: mapping token → nomor kartu (harus PCI compliant)
```

### Tokenisasi Kartu: PAN → Token

```
PAN = Primary Account Number = Nomor kartu asli (16 digit)
Token = Pengganti PAN yang aman untuk disimpan merchant

PROSES TOKENISASI:

1. ORIGINAL CARD:
   PAN:  4111 1111 1111 1111
   Expiry: 12/26
   CVV: 123
   ↓
2. PAYMENT GATEWAY (Midtrans/Xendit):
   Token: tok_lMVBmmGqwnmJRQMmF4ZQQvmfkZPHdENG
   (String acak yang tidak ada hubungan matematisnya dengan PAN)
   ↓
3. YANG MERCHANT SIMPAN:
   user_id: 12345
   saved_card_token: tok_lMVBmmGqwnmJRQMmF4ZQQvmfkZPHdENG
   card_display: **** **** **** 1111  (4 digit terakhir saja)

KEUNTUNGAN:
  • Merchant database di-hack → token tidak berguna tanpa gateway
  • Satu token bisa "expire" → re-tokenize → kartu lama tidak bisa dipakai
  • Token bisa dibatasi: hanya untuk merchant X, max Rp 100.000/transaksi
  • PCI-DSS scope merchant JAUH lebih kecil (tidak menyentuh PAN)
```

### PCI-DSS: 12 Requirement yang Wajib

PCI-DSS (Payment Card Industry Data Security Standard) adalah standar keamanan yang wajib dipatuhi oleh siapapun yang memproses, menyimpan, atau mentransmisikan data kartu.

```
12 PCI-DSS REQUIREMENTS v4.0:

BUILD AND MAINTAIN A SECURE NETWORK:
  1. Install and maintain network security controls (firewall)
  2. Apply secure configurations to all system components

PROTECT ACCOUNT DATA:
  3. Protect stored account data
     → Jangan simpan CVV/CVC sama sekali setelah otorisasi
     → Jangan simpan PIN
     → Jika simpan PAN: enkripsi atau tokenisasi
  4. Protect cardholder data with strong cryptography (TLS in transit)

MAINTAIN A VULNERABILITY MANAGEMENT PROGRAM:
  5. Protect all systems against malware (antivirus, EDR)
  6. Develop and maintain secure systems and software (SAST, DAST, patching)

IMPLEMENT STRONG ACCESS CONTROL:
  7. Restrict access to system components and cardholder data by business need
  8. Identify users and authenticate access (MFA untuk admin)
  9. Restrict physical access to cardholder data

REGULARLY MONITOR AND TEST NETWORKS:
  10. Log and monitor all access to system components and cardholder data
  11. Test security of systems and networks regularly (penetration testing)

MAINTAIN AN INFORMATION SECURITY POLICY:
  12. Support information security with organizational policies and programs

UNTUK DEVELOPER, YANG PALING RELEVAN:
  • Req 3: JANGAN simpan CVV! JANGAN simpan full PAN tanpa enkripsi
  • Req 4: HANYA gunakan TLS 1.2+ untuk transmisi data kartu
  • Req 6: Secure coding, dependency scanning, OWASP checklist
  • Req 8: MFA untuk akses ke sistem yang handle data kartu
  • Req 10: Audit log semua akses ke cardholder data
```

### Level PCI-DSS Compliance

```
MERCHANT LEVEL berdasarkan volume transaksi:
┌─────────┬──────────────────────┬────────────────────────────────┐
│ Level   │ Volume (per tahun)   │ Requirement                    │
├─────────┼──────────────────────┼────────────────────────────────┤
│ Level 1 │ > 6 juta transaksi   │ Annual on-site audit oleh QSA  │
│         │                      │ Quarterly network scan (ASV)   │
├─────────┼──────────────────────┼────────────────────────────────┤
│ Level 2 │ 1-6 juta transaksi   │ Annual SAQ + quarterly scan    │
├─────────┼──────────────────────┼────────────────────────────────┤
│ Level 3 │ 20rb-1jt transaksi   │ Annual SAQ + quarterly scan    │
├─────────┼──────────────────────┼────────────────────────────────┤
│ Level 4 │ < 20.000 transaksi   │ Annual SAQ                     │
└─────────┴──────────────────────┴────────────────────────────────┘

SAQ = Self-Assessment Questionnaire (checklist kepatuhan)
QSA = Qualified Security Assessor (auditor tersertifikasi)
ASV = Approved Scanning Vendor (penyedia vulnerability scan)

CARA PALING MUDAH UNTUK STARTUP:
  Gunakan payment gateway (Midtrans, Xendit) yang sudah PCI Level 1
  → Merchant tidak perlu menyentuh data kartu sama sekali
  → PCI-DSS scope merchant = SAQ A (paling minimal, hanya 12 pertanyaan)
```

### Perbandingan Payment Gateway Lokal

```
┌────────────────────────────────────────────────────────────────────┐
│ Feature           │ Midtrans    │ Xendit      │ DOKU              │
├────────────────────────────────────────────────────────────────────┤
│ Didirikan         │ 2012 (GoTo) │ 2015        │ 2007              │
│ PCI-DSS Level     │ Level 1     │ Level 1     │ Level 1           │
│ Metode Pembayaran │ 50+         │ 30+         │ 20+               │
│ API Style         │ RESTful     │ RESTful     │ RESTful           │
│ Virtual Account   │ ✓ (7 bank)  │ ✓ (10 bank) │ ✓                 │
│ QRIS              │ ✓           │ ✓           │ ✓                 │
│ Kartu Kredit      │ ✓           │ ✓           │ ✓                 │
│ Paylater          │ ✓ (GoPayLater)│ ✓ (Akulaku)│ ✗               │
│ Dokumentasi       │ Sangat Baik │ Baik        │ Baik              │
│ Sandbox           │ ✓           │ ✓           │ ✓                 │
└────────────────────────────────────────────────────────────────────┘

Kapan memilih mana:
  Startup baru → Midtrans atau Xendit (dokumentasi terbaik, onboarding cepat)
  Enterprise dengan volume tinggi → negosiasi langsung, seringkali multi-gateway
  Khusus kartu kredit → Midtrans (integrasi Gopay paling mulus)
```

---

## 🧠 Ilustrasi / Analogi

**Payment Gateway seperti petugas tol elektronik:**
* Kamu tidak perlu keluarkan kartu di setiap gerbang
* Kartu sudah "terdaftar" di sistem, setiap lewat otomatis terpotong
* Merchant (gerbang tol) tidak simpan nomor kartumu — hanya "ID transaksi"

**Tokenisasi seperti coat check di restoran:**
* Kamu titipkan jaket (kartu asli) ke penjaga (payment gateway)
* Kamu dapat nomor tiket (token)
* Waitress (merchant) hanya pegang nomor tiket — tidak tahu isi jaket kamu
* Saat pulang, tunjukkan tiket → dapat jaket kembali

**PCI-DSS seperti lisensi medis:**
* Semua dokter wajib ikut standar yang sama (terlepas ukuran praktik)
* Audit reguler memastikan standar terpenuhi
* Melanggar standar = kehilangan lisensi (merchant: kehilangan akses kartu)

---

## 💻 Contoh Teknis

### Integrasi Midtrans API (Python)

```python
"""
Contoh integrasi Midtrans Payment Gateway
Install: pip install midtransclient
"""
import midtransclient
import os
import uuid
import hmac
import hashlib

# Konfigurasi (dari environment variable, JANGAN hardcode!)
MIDTRANS_SERVER_KEY = os.environ.get('MIDTRANS_SERVER_KEY', 'SB-Mid-server-...')
MIDTRANS_CLIENT_KEY = os.environ.get('MIDTRANS_CLIENT_KEY', 'SB-Mid-client-...')
IS_PRODUCTION = os.environ.get('MIDTRANS_PRODUCTION', 'false').lower() == 'true'

# Init Snap API (untuk redirect/popup checkout)
snap = midtransclient.Snap(
    is_production=IS_PRODUCTION,
    server_key=MIDTRANS_SERVER_KEY,
    client_key=MIDTRANS_CLIENT_KEY
)

def create_payment_transaction(order: dict, customer: dict) -> dict:
    """
    Buat transaksi baru dan dapatkan snap_token untuk frontend.
    snap_token digunakan browser untuk menampilkan popup pembayaran.
    Nomor kartu TIDAK pernah melewati server kita!
    """
    order_id = f"ORDER-{uuid.uuid4().hex[:8].upper()}"
    
    param = {
        "transaction_details": {
            "order_id": order_id,
            "gross_amount": order['total_amount']
        },
        "item_details": [
            {
                "id": item['product_id'],
                "price": item['price'],
                "quantity": item['quantity'],
                "name": item['name']
            }
            for item in order['items']
        ],
        "customer_details": {
            "first_name": customer['first_name'],
            "last_name": customer['last_name'],
            "email": customer['email'],
            "phone": customer['phone']
        },
        "callbacks": {
            "finish": f"https://yourapp.com/payment/finish/{order_id}"
        }
    }
    
    transaction = snap.create_transaction(param)
    
    return {
        "order_id": order_id,
        "snap_token": transaction['token'],  # Digunakan oleh Midtrans.js di frontend
        "redirect_url": transaction['redirect_url']
    }


def verify_payment_notification(notification_data: dict, order_from_db: dict) -> bool:
    """
    Verifikasi notifikasi payment dari Midtrans.
    KRITIS: Selalu verifikasi signature untuk mencegah pemalsuan notifikasi!
    """
    order_id = notification_data.get('order_id')
    status_code = notification_data.get('status_code')
    gross_amount = notification_data.get('gross_amount')
    received_signature = notification_data.get('signature_key', '')
    
    # LANGKAH 1: Verifikasi signature
    # Format: SHA512(order_id + status_code + gross_amount + server_key)
    signature_string = f"{order_id}{status_code}{gross_amount}{MIDTRANS_SERVER_KEY}"
    expected_signature = hashlib.sha512(signature_string.encode()).hexdigest()
    
    if not hmac.compare_digest(received_signature, expected_signature):
        raise SecurityError("ALERT: Invalid notification signature - possible fraud!")
    
    # LANGKAH 2: Verifikasi amount
    received_amount = float(gross_amount)
    expected_amount = order_from_db['total_amount']
    
    if abs(received_amount - expected_amount) > 0.01:  # Tolerance untuk floating point
        raise SecurityError(f"Amount mismatch: expected {expected_amount}, got {received_amount}")
    
    # LANGKAH 3: Cek transaction status
    transaction_status = notification_data.get('transaction_status')
    fraud_status = notification_data.get('fraud_status')
    
    if transaction_status == 'capture' and fraud_status == 'accept':
        return True  # Payment berhasil, proses order
    elif transaction_status == 'settlement':
        return True  # VA payment berhasil
    else:
        return False  # Pending atau gagal


class SecurityError(Exception):
    pass
```

---

## 🏢 Studi Kasus: Mengapa Tokopedia Tidak Menyimpan Nomor Kartu

**Situasi:** Tokopedia mendapat bintang 5 dari pengguna karena bisa bayar dengan kartu kredit yang "tersimpan" — tanpa perlu input ulang setiap checkout.

**Yang sebenarnya terjadi:**

```
YANG USER PIKIR: "Tokopedia simpan nomor kartuku"

YANG SEBENARNYA:
┌─────────────────────────────────────────────────────────┐
│ Database Tokopedia (user_saved_cards):                  │
│   user_id: 12345                                        │
│   gateway: "midtrans"                                   │
│   token: "tok_abc123xyz"     ← Ini yang disimpan        │
│   card_display: "**** 1111"  ← 4 digit terakhir saja    │
│   card_type: "VISA"                                     │
│                                                         │
│ YANG TIDAK ADA:                                         │
│   full_card_number: ❌ TIDAK DISIMPAN                   │
│   cvv: ❌ TIDAK DISIMPAN (PCI-DSS melarang keras)       │
│   expiry: ❌ TIDAK DISIMPAN DI TOKOPEDIA                │
└─────────────────────────────────────────────────────────┘

FLOW "BAYAR DENGAN KARTU TERSIMPAN":
  1. User klik "Bayar dengan **** 1111"
  2. Tokopedia kirim ke Midtrans: {token: "tok_abc123xyz", amount: 500000}
  3. Midtrans resolve token → nomor kartu asli (hanya Midtrans yang tahu)
  4. Midtrans proses ke bank
  5. Result: approved/declined

DAMPAK KEAMANAN:
  Jika database Tokopedia di-hack → hacker dapat token, bukan nomor kartu
  Token hanya bisa digunakan melalui Midtrans API dengan server key
  → Tidak ada nilai untuk hacker di dark web
```

---

## ⚠️ Kesalahan Umum

1. **Menyimpan CVV/CVC di database** → MELANGGAR PCI-DSS requirement 3. CVV tidak boleh disimpan sama sekali setelah otorisasi. Bahkan Midtrans pun tidak boleh menyimpannya.

2. **Meneruskan data kartu melalui server merchant** → Jika data kartu melewati backend kamu, kamu masuk ke PCI-DSS scope penuh. Gunakan tokenization library (Midtrans.js, Xendit.js) yang mengirim data LANGSUNG ke gateway dari browser.

3. **Tidak verifikasi signature notifikasi** → Hacker bisa kirim notifikasi palsu "payment success" ke webhook kamu. SELALU verifikasi HMAC signature setiap notifikasi.

4. **Menggunakan credentials payment gateway yang sama untuk semua environment** → Produksi dan sandbox harus pakai credentials berbeda. Jika sandbox key bocor, tidak ada dampak ke produksi.

5. **Tidak ada idempotency handling untuk webhook** → Midtrans bisa kirim notifikasi yang sama lebih dari sekali. Tanpa idempotency check, order bisa diproses dua kali (ship barang dua kali, kredit user dua kali).

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan mengapa merchant tidak perlu menyimpan nomor kartu pengguna untuk fitur "kartu tersimpan". Apa yang sebenarnya disimpan?

b) Sebutkan 5 PCI-DSS requirement yang paling relevan bagi developer backend. Untuk masing-masing, berikan contoh implementasi konkret.

c) Apa perbedaan antara tokenisasi (payment gateway) dan enkripsi? Mana yang lebih cocok untuk "kartu tersimpan"?

### Soal 2 — Analisis Arsitektur

Sebuah startup fintech ingin membangun marketplace dengan fitur pembayaran kartu kredit. Mereka mempertimbangkan dua arsitektur:

**Arsitektur A:** Frontend kirim nomor kartu ke backend → backend simpan kartu di database terenkripsi → backend kirim ke payment processor saat checkout

**Arsitektur B:** Frontend gunakan Midtrans.js untuk tokenisasi langsung → frontend kirim token ke backend → backend gunakan token untuk charge

a) Identifikasi semua masalah keamanan dan compliance di Arsitektur A
b) Jelaskan mengapa Arsitektur B lebih baik
c) Apa PCI-DSS scope level untuk masing-masing arsitektur?

---

## 📌 Ringkasan

* **Payment Gateway** = perantara aman antara merchant dan jaringan kartu
* Data kartu melewati: browser → gateway → acquirer → network → issuer → balik
* **Tokenisasi:** PAN → token (string acak yang tidak berguna tanpa gateway)
* Merchant tidak perlu dan tidak boleh menyimpan nomor kartu atau CVV
* **PCI-DSS:** 12 requirement wajib untuk semua yang memproses data kartu
* Req 3: Jangan simpan CVV. Req 4: TLS 1.2+ untuk transmisi. Req 10: Audit log
* Menggunakan payment gateway = scope PCI-DSS minimal (SAQ A)
* SELALU verifikasi HMAC signature pada webhook notifikasi pembayaran

---

*📚 Referensi: PCI Security Standards Council — pcisecuritystandards.org | Midtrans API Documentation | Xendit API Documentation | Anderson, R. (2020). Security Engineering | Bank Indonesia Regulasi Sistem Pembayaran*
