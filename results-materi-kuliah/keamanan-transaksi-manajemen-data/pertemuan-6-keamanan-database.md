# Pertemuan 6: Keamanan Database — Hak Akses, Enkripsi Kolom & Row-Level Security

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Membuat user MySQL dengan privilege terbatas menggunakan prinsip least privilege
* Mengimplementasikan Role-Based Access Control (RBAC) di database
* Mengenkripsi kolom sensitif (NIK, nomor kartu) menggunakan AES_ENCRYPT di MySQL
* Menerapkan Row-Level Security untuk multi-tenant database

---

## 📖 Pengantar (Hook)

Bayangkan kamu punya gudang berisi data 50 juta nasabah. Kamu memiliki 50 karyawan: 5 admin database, 20 tim customer service, 25 developer backend.

Apakah semua 50 karyawan butuh kunci ke seluruh gudang? Tentu tidak.
* Developer hanya perlu akses ke tabel yang relevan dengan fitur yang mereka kerjakan
* Customer service hanya perlu baca data nasabah, bukan ubah atau hapus
* Admin butuh akses penuh, tapi tetap harus di-audit

Itulah prinsip **Least Privilege** dalam keamanan database: setiap pihak hanya mendapat akses minimum yang dibutuhkan untuk tugasnya. Tidak lebih, tidak kurang.

---

## 🧩 Konsep Utama

### Prinsip Least Privilege

```
PRINSIP LEAST PRIVILEGE:
Setiap user, service, atau aplikasi hanya boleh memiliki
permission MINIMUM yang dibutuhkan untuk menjalankan tugasnya.

CONTOH SKENARIO SISTEM RUMAH SAKIT:
┌─────────────────────────────────────────────────────────────┐
│ Role         │ Tabel yang Boleh Diakses │ Operasi           │
├─────────────────────────────────────────────────────────────┤
│ Admin DB     │ Semua tabel              │ SELECT,INSERT,    │
│              │                          │ UPDATE,DELETE,DDL │
├─────────────────────────────────────────────────────────────┤
│ Dokter       │ patients, diagnoses,     │ SELECT, INSERT,   │
│              │ prescriptions            │ UPDATE            │
├─────────────────────────────────────────────────────────────┤
│ Resepsionis  │ appointments, patients   │ SELECT, INSERT,   │
│              │ (hanya kolom nama & kontak)│ UPDATE (terbatas)│
├─────────────────────────────────────────────────────────────┤
│ Bagian Tagihan│ billing, patients       │ SELECT, INSERT    │
│              │ (hanya info pembayaran)  │                   │
├─────────────────────────────────────────────────────────────┤
│ App Backend  │ Tabel sesuai fungsinya   │ SELECT, INSERT,   │
│ (API service)│                          │ UPDATE (no DDL)   │
└─────────────────────────────────────────────────────────────┘

ATURAN KRITIS:
✗ JANGAN gunakan root/admin untuk koneksi aplikasi
✗ JANGAN gunakan satu user DB untuk seluruh aplikasi
✓ Setiap service/role punya user DB terpisah dengan privilege minimal
```

### User & Privilege Management di MySQL

```sql
-- ===== SETUP: Sistem Manajemen Pasien Rumah Sakit =====

-- Buat database
CREATE DATABASE hospital_db;
USE hospital_db;

-- Buat tabel
CREATE TABLE patients (
    id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    nik VARCHAR(255),           -- Akan dienkripsi
    phone VARCHAR(20),
    email VARCHAR(100),
    blood_type CHAR(3),
    medical_notes TEXT,         -- Catatan medis sensitif
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE diagnoses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    doctor_id INT,
    diagnosis TEXT,
    diagnosis_date DATE,
    FOREIGN KEY (patient_id) REFERENCES patients(id)
);

CREATE TABLE appointments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    appointment_date DATETIME,
    status ENUM('scheduled', 'completed', 'cancelled'),
    FOREIGN KEY (patient_id) REFERENCES patients(id)
);

-- ===== USER MANAGEMENT =====

-- 1. User untuk aplikasi backend (paling terbatas)
CREATE USER 'app_backend'@'localhost' IDENTIFIED BY 'StrongP@ss2025!';
-- Hanya operasi DML yang diperlukan, tidak bisa DROP/ALTER
GRANT SELECT, INSERT, UPDATE ON hospital_db.patients TO 'app_backend'@'localhost';
GRANT SELECT, INSERT ON hospital_db.diagnoses TO 'app_backend'@'localhost';
GRANT SELECT, INSERT, UPDATE ON hospital_db.appointments TO 'app_backend'@'localhost';
-- TIDAK ada DELETE, TIDAK ada DROP, TIDAK ada DDL

-- 2. User untuk dokter (read + insert diagnosa)
CREATE USER 'dr_user'@'localhost' IDENTIFIED BY 'DoctorP@ss!';
GRANT SELECT ON hospital_db.patients TO 'dr_user'@'localhost';
GRANT SELECT, INSERT, UPDATE ON hospital_db.diagnoses TO 'dr_user'@'localhost';
GRANT SELECT ON hospital_db.appointments TO 'dr_user'@'localhost';

-- 3. User untuk resepsionis (akses terbatas — hanya kolom non-sensitif)
CREATE USER 'receptionist'@'localhost' IDENTIFIED BY 'Recept!onist2025';
-- Buat VIEW yang menyembunyikan kolom sensitif
CREATE VIEW patient_basic_info AS
    SELECT id, full_name, phone, email FROM patients;
GRANT SELECT ON hospital_db.patient_basic_info TO 'receptionist'@'localhost';
GRANT SELECT, INSERT, UPDATE ON hospital_db.appointments TO 'receptionist'@'localhost';

-- 4. Cek privilege yang sudah diberikan
SHOW GRANTS FOR 'app_backend'@'localhost';
SHOW GRANTS FOR 'receptionist'@'localhost';

FLUSH PRIVILEGES;
```

### Enkripsi Kolom Sensitif dengan AES_ENCRYPT

```sql
-- ===== ENKRIPSI KOLOM DI MYSQL =====
-- MySQL memiliki built-in AES_ENCRYPT dan AES_DECRYPT

-- Setup: Simpan kunci enkripsi di MySQL variable (dalam produksi: gunakan KMS)
-- JANGAN hardcode kunci di aplikasi atau SQL file yang dicommit ke Git!
SET @encryption_key = 'your-256-bit-key-here-32bytes!!!';

-- INSERT dengan enkripsi NIK
INSERT INTO patients (full_name, nik, phone, email, blood_type)
VALUES (
    'Budi Santoso',
    AES_ENCRYPT('3273012501900001', @encryption_key),  -- NIK dienkripsi
    '081234567890',
    'budi@email.com',
    'A'
);

-- SELECT dengan dekripsi (hanya user yang tahu kunci bisa lihat NIK)
SELECT 
    id,
    full_name,
    CAST(AES_DECRYPT(nik, @encryption_key) AS CHAR) AS nik_decrypted,
    phone,
    email
FROM patients;

-- Pencarian berdasarkan NIK terenkripsi
SELECT * FROM patients
WHERE nik = AES_ENCRYPT('3273012501900001', @encryption_key);

-- Catatan: AES_ENCRYPT di MySQL menggunakan AES-128-ECB secara default
-- Untuk keamanan lebih baik, gunakan mode CBC di application layer (Python)
-- dan simpan IV + ciphertext di kolom database
```

### Enkripsi Lebih Aman: Application-Level Encryption (Rekomendasi)

```python
"""
Lebih aman: enkripsi di application layer menggunakan AES-256-GCM
sebelum menyimpan ke MySQL
"""
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
import os
import mysql.connector

class SecurePatientRepository:
    """Repository dengan enkripsi application-level untuk kolom sensitif."""
    
    def __init__(self, db_config: dict, encryption_key: bytes):
        self.db = mysql.connector.connect(**db_config)
        # Key dari environment variable atau KMS — TIDAK dari database!
        self.aesgcm = AESGCM(encryption_key)
    
    def _encrypt_field(self, value: str) -> str:
        """Enkripsi field sensitif, simpan sebagai hex string."""
        nonce = os.urandom(12)
        ciphertext = self.aesgcm.encrypt(nonce, value.encode(), None)
        return (nonce + ciphertext).hex()  # Format: nonce + ciphertext
    
    def _decrypt_field(self, encrypted_hex: str) -> str:
        """Dekripsi field dari hex string."""
        data = bytes.fromhex(encrypted_hex)
        nonce = data[:12]
        ciphertext = data[12:]
        return self.aesgcm.decrypt(nonce, ciphertext, None).decode()
    
    def insert_patient(self, patient: dict) -> int:
        """Insert patient dengan NIK terenkripsi."""
        cursor = self.db.cursor()
        encrypted_nik = self._encrypt_field(patient['nik'])
        
        sql = """INSERT INTO patients (full_name, nik, phone, email, blood_type)
                 VALUES (%s, %s, %s, %s, %s)"""
        cursor.execute(sql, (
            patient['full_name'],
            encrypted_nik,  # NIK sudah dienkripsi
            patient['phone'],
            patient['email'],
            patient.get('blood_type', '-')
        ))
        self.db.commit()
        return cursor.lastrowid
    
    def get_patient(self, patient_id: int) -> dict:
        """Ambil patient dengan NIK terdekripsi."""
        cursor = self.db.cursor(dictionary=True)
        cursor.execute("SELECT * FROM patients WHERE id = %s", (patient_id,))
        row = cursor.fetchone()
        
        if row:
            row['nik'] = self._decrypt_field(row['nik'])  # Dekripsi NIK
        return row
```

### Row-Level Security: Multi-Tenant Database

```sql
-- ===== ROW-LEVEL SECURITY untuk Multi-Tenant =====
-- Contoh: Sistem pembayaran yang melayani banyak merchant

CREATE TABLE transactions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    merchant_id INT NOT NULL,
    amount DECIMAL(15,2),
    status VARCHAR(20),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- TANPA RLS: Merchant A bisa akses data Merchant B (BUG!)
SELECT * FROM transactions WHERE merchant_id = 999;  -- Merchant lain!

-- ===== SOLUSI 1: View per merchant =====
-- Buat view yang filter berdasarkan merchant yang login
DELIMITER //
CREATE PROCEDURE create_merchant_view(IN p_merchant_id INT)
BEGIN
    SET @sql = CONCAT(
        'CREATE OR REPLACE VIEW merchant_transactions AS ',
        'SELECT * FROM transactions WHERE merchant_id = ', p_merchant_id
    );
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;
END //
DELIMITER ;

-- ===== SOLUSI 2: Application-Level RLS (lebih umum) =====
-- Di Python Flask/Django, SELALU tambahkan filter merchant_id dari session
```

```python
from flask import session

def get_merchant_transactions(status_filter=None):
    """
    SELALU filter berdasarkan merchant_id dari session.
    JANGAN izinkan user input merchant_id secara langsung!
    """
    merchant_id = session.get('merchant_id')
    if not merchant_id:
        raise PermissionError("Unauthorized")
    
    query = "SELECT * FROM transactions WHERE merchant_id = %s"
    params = [merchant_id]
    
    if status_filter:
        query += " AND status = %s"
        params.append(status_filter)
    
    # merchant_id dari session (server-side), TIDAK dari user input
    return db.execute(query, params)
```

---

## 🧠 Ilustrasi / Analogi

**Database user seperti kunci kantor:**
* Cleaning service punya kunci untuk masuk tapi tidak bisa buka brankas
* Accounting punya akses ruang keuangan tapi tidak bisa masuk server room
* IT admin punya akses semua tapi setiap akses tercatat

**Enkripsi kolom seperti amplop dalam lemari:**
* Dokumen sensitif (NIK, nomor kartu) dimasukkan dalam amplop tertutup
* Bahkan orang yang punya akses ke lemari tidak bisa baca tanpa membuka amplop
* Kunci amplop terpisah dari kunci lemari

**Row-Level Security seperti ruang pelanggan di bank:**
* Nasabah A hanya bisa lihat rekeningnya sendiri, bukan rekening B
* Meskipun keduanya ada di "tabel yang sama" (sistem bank)

---

## 🏢 Studi Kasus: Konfigurasi Database Security GoPay

**Konteks:** GoPay menyimpan data jutaan transaksi. Berbagai tim (fraud, customer service, backend, data analyst) perlu akses.

**Strategi akses berlapis:**

```
GoPay Database Security Model:

┌─────────────────────────────────────────────────────────┐
│                    PRODUCTION DB                        │
│  (MySQL dengan enkripsi at rest + TLS in transit)       │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  service_gopay_api (user DB untuk API backend)          │
│  → SELECT, INSERT, UPDATE pada: transactions, wallets   │
│  → TIDAK ADA DELETE, DDL, SUPER privilege               │
│                                                         │
│  service_fraud_engine (user untuk fraud detection)      │
│  → SELECT ONLY pada: transactions, users, risk_scores   │
│  → Akses dari IP tertentu saja (IDENTIFIED BY + host)   │
│                                                         │
│  analyst_readonly (user untuk data team)                │
│  → SELECT ONLY pada READ REPLICA (bukan production)     │
│  → Akses ke data warehouse, bukan production langsung   │
│                                                         │
│  backup_agent (user untuk backup)                       │
│  → SELECT, LOCK TABLES, SHOW DATABASES                  │
│  → Hanya dari server backup (IP whitelist)              │
│                                                         │
│  Kolom sensitif (nomor kartu, NIK) → AES-256-GCM        │
│  Key management → AWS KMS (bukan di database)           │
│                                                         │
└─────────────────────────────────────────────────────────┘

Row-Level Security:
→ Setiap query dari API wajib include user_id dari JWT token
→ Middleware verifikasi bahwa user hanya akses data miliknya
→ Tidak ada endpoint yang return data semua user sekaligus
```

---

## ⚠️ Kesalahan Umum

1. **Koneksi database menggunakan user root** → User root punya akses penuh ke semua database. Jika koneksi ini bocor (SQL injection), attacker punya kontrol penuh. SELALU gunakan user terbatas.

2. **Satu user database untuk semua service** → Microservice A dan B pakai user DB yang sama → jika A dicompromised, B juga ikut. Setiap service harus punya user DB sendiri.

3. **Enkripsi seluruh tabel alih-alih kolom sensitif** → Full-disk encryption tidak melindungi dari SQL injection (attacker akses via aplikasi, bukan disk). Enkripsi kolom sensitif secara selektif.

4. **Menyimpan kunci enkripsi di database yang sama** → Seperti menyimpan kunci brankas di dalam brankas. Gunakan variabel environment atau KMS yang terpisah.

5. **Tidak ada audit log untuk akses data sensitif** → "Siapa yang mengakses data NIK si A?" → tanpa audit log, tidak bisa dijawab. Setup MySQL general log atau audit plugin untuk operasi pada tabel sensitif.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan prinsip Least Privilege. Mengapa aplikasi backend tidak boleh menggunakan user database dengan privilege root?

b) Apa perbedaan antara enkripsi kolom di database layer (AES_ENCRYPT MySQL) vs application layer? Mana yang lebih direkomendasikan dan mengapa?

c) Jelaskan apa itu Row-Level Security dan berikan contoh skenario di mana ini sangat penting (selain contoh di atas).

### Soal 2 — Praktik (Lab)

Setup database MySQL dengan skenario sistem rumah sakit:
1. Buat database `hospital_db` dengan 3 tabel: `patients`, `diagnoses`, `appointments`
2. Buat 3 user: `app_admin`, `dr_viewer`, `receptionist` dengan privilege berbeda
3. Enkripsi kolom NIK menggunakan AES_ENCRYPT (atau application-level encryption)
4. Buat VIEW `patient_basic_info` untuk resepsionis yang menyembunyikan NIK
5. Test masing-masing user: verifikasi bahwa akses sesuai privilege yang diberikan

---

## 📌 Ringkasan

* **Least Privilege:** Setiap user/service hanya dapat privilege minimum yang diperlukan
* Jangan gunakan `root` untuk koneksi aplikasi — buat user terpisah per service/role
* **RBAC di MySQL:** `GRANT` privilege spesifik per user, per tabel, per operasi
* **Enkripsi kolom:** Lindungi data sensitif (NIK, nomor kartu) meskipun database di-dump
* Application-level encryption (AES-256-GCM) lebih direkomendasikan dari database-level (AES_ENCRYPT ECB)
* **Row-Level Security:** Filter data berdasarkan identity pengguna — satu user hanya lihat datanya sendiri
* Selalu audit akses ke data sensitif — tanpa log, forensik setelah breach tidak mungkin

---

*📚 Referensi: MySQL Security Documentation — dev.mysql.com | Anderson, R. (2020). Security Engineering, 3rd Ed. | OWASP Database Security Cheat Sheet | CIS MySQL Benchmark*
