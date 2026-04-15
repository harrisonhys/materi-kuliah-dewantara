# Pertemuan 9: Audit Log, Backup & Recovery Database

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Merancang sistem audit log yang efektif untuk database produksi
* Mengimplementasikan MySQL trigger untuk audit trail otomatis
* Merancang strategi backup: full, differential, dan incremental
* Mengimplementasikan encrypted backup menggunakan mysqldump + enkripsi

---

## 📖 Pengantar (Hook)

2020: Sebuah bank digital Indonesia mengalami "data anomali" — saldo beberapa nasabah berubah secara misterius. Tim forensik butuh waktu 3 minggu untuk menemukan penyebabnya — karena tidak ada audit log yang memadai.

Tanpa audit log, pertanyaan sederhana seperti:
* "Siapa yang mengubah saldo akun X pada tanggal Y?"
* "Apakah ada akses tidak wajar ke data NIK pukul 2 pagi?"
* "Record transaksi ini dihapus oleh siapa?"

...tidak bisa dijawab. Database tanpa audit log seperti ruangan tanpa CCTV — kamu tidak tahu apa yang terjadi sampai sudah terlambat.

---

## 🧩 Konsep Utama

### Audit Log: Siapa, Apa, Kapan, Dari Mana

Audit log adalah rekaman IMMUTABLE (tidak bisa diubah) dari semua aktivitas penting pada sistem database.

```
FORMAT STANDAR AUDIT LOG ENTRY:
┌────────────────────────────────────────────────────────────────┐
│  timestamp   │ 2025-01-15 14:32:05.123 UTC                     │
│  event_type  │ UPDATE                                          │
│  user        │ api_service@192.168.1.100                       │
│  table_name  │ transactions                                     │
│  record_id   │ 845623                                          │
│  old_value   │ {"status": "pending", "amount": 500000}         │
│  new_value   │ {"status": "success", "amount": 500000}         │
│  ip_address  │ 192.168.1.100                                   │
│  session_id  │ sess_abc123                                     │
│  app_context │ payment_processor_v2.handle_callback            │
└────────────────────────────────────────────────────────────────┘

PRINSIP AUDIT LOG:
✓ APPEND-ONLY: log tidak bisa diubah atau dihapus (hanya ditambah)
✓ INCLUDE OLD + NEW VALUE: tahu apa yang berubah
✓ TIMESTAMP dengan timezone: UTC untuk konsistensi global
✓ IP ADDRESS: identifikasi sumber akses
✓ CONTEXT: dari function/service apa query berasal
```

### Jenis Event yang Harus Di-Audit

```
LEVEL KRITIS (selalu log):
  • Akses ke tabel sensitif (users, transactions, kolom NIK/kartu)
  • INSERT/UPDATE/DELETE pada tabel keuangan
  • Login success dan failure
  • Perubahan privilege user
  • DDL operations (CREATE/ALTER/DROP table)

LEVEL PENTING:
  • Bulk SELECT (lebih dari 1000 records sekaligus)
  • Query dari IP/time yang tidak biasa
  • Export data ke file

LEVEL OPERASIONAL:
  • Backup start/complete
  • Database restart
  • Configuration changes
```

### Implementasi Audit dengan MySQL Triggers

```sql
-- ===== SETUP SISTEM AUDIT LOG =====

USE fintech_db;

-- Tabel audit_log yang TERPISAH dari tabel utama
-- Idealnya di database berbeda atau bahkan server berbeda
CREATE TABLE audit_log (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    event_time TIMESTAMP(6) DEFAULT CURRENT_TIMESTAMP(6),
    event_type ENUM('INSERT', 'UPDATE', 'DELETE', 'SELECT') NOT NULL,
    table_name VARCHAR(100) NOT NULL,
    record_id VARCHAR(100),
    old_values JSON,
    new_values JSON,
    db_user VARCHAR(100) NOT NULL,
    client_ip VARCHAR(45),
    app_user VARCHAR(100),
    notes TEXT,
    
    INDEX idx_event_time (event_time),
    INDEX idx_table_record (table_name, record_id),
    INDEX idx_db_user (db_user)
) ENGINE=InnoDB;

-- Tabel transaksi yang akan di-audit
CREATE TABLE transactions (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id BIGINT NOT NULL,
    amount DECIMAL(15,2) NOT NULL,
    transaction_type VARCHAR(50),
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- ===== TRIGGER UNTUK INSERT =====
DELIMITER //
CREATE TRIGGER trg_transactions_after_insert
AFTER INSERT ON transactions
FOR EACH ROW
BEGIN
    INSERT INTO audit_log 
        (event_type, table_name, record_id, new_values, db_user, client_ip)
    VALUES (
        'INSERT',
        'transactions',
        CAST(NEW.id AS CHAR),
        JSON_OBJECT(
            'user_id', NEW.user_id,
            'amount', NEW.amount,
            'transaction_type', NEW.transaction_type,
            'status', NEW.status
        ),
        USER(),       -- Database user yang menjalankan query
        @client_ip    -- Application set ini sebelum query: SET @client_ip = '...'
    );
END //

-- ===== TRIGGER UNTUK UPDATE (paling penting!) =====
CREATE TRIGGER trg_transactions_after_update
AFTER UPDATE ON transactions
FOR EACH ROW
BEGIN
    -- Hanya log jika ada yang berubah
    IF OLD.status != NEW.status OR OLD.amount != NEW.amount THEN
        INSERT INTO audit_log 
            (event_type, table_name, record_id, old_values, new_values, db_user, client_ip)
        VALUES (
            'UPDATE',
            'transactions',
            CAST(NEW.id AS CHAR),
            JSON_OBJECT(
                'status', OLD.status,
                'amount', OLD.amount,
                'updated_at', OLD.updated_at
            ),
            JSON_OBJECT(
                'status', NEW.status,
                'amount', NEW.amount,
                'updated_at', NEW.updated_at
            ),
            USER(),
            @client_ip
        );
    END IF;
END //

-- ===== TRIGGER UNTUK DELETE =====
CREATE TRIGGER trg_transactions_before_delete
BEFORE DELETE ON transactions
FOR EACH ROW
BEGIN
    INSERT INTO audit_log 
        (event_type, table_name, record_id, old_values, db_user, client_ip)
    VALUES (
        'DELETE',
        'transactions',
        CAST(OLD.id AS CHAR),
        JSON_OBJECT(
            'user_id', OLD.user_id,
            'amount', OLD.amount,
            'status', OLD.status,
            'created_at', OLD.created_at
        ),
        USER(),
        @client_ip
    );
END //
DELIMITER ;

-- ===== PENGGUNAAN DARI APLIKASI =====
-- Set client_ip dan app_user sebelum query
SET @client_ip = '192.168.1.100';
SET @app_user = 'user_budi_12345';

-- Setiap INSERT/UPDATE/DELETE otomatis di-log oleh trigger
UPDATE transactions SET status = 'success' WHERE id = 1;
-- → Trigger otomatis mencatat perubahan ke audit_log
```

### Query untuk Investigasi dari Audit Log

```sql
-- ===== FORENSIK QUERIES =====

-- 1. Lihat semua perubahan pada transaksi ID tertentu
SELECT event_time, event_type, old_values, new_values, db_user, client_ip
FROM audit_log
WHERE table_name = 'transactions' AND record_id = '845623'
ORDER BY event_time;

-- 2. Siapa yang melakukan DELETE dalam 24 jam terakhir?
SELECT event_time, db_user, client_ip, record_id, old_values
FROM audit_log
WHERE event_type = 'DELETE'
AND event_time > NOW() - INTERVAL 24 HOUR
ORDER BY event_time DESC;

-- 3. Deteksi anomali: UPDATE dari IP yang tidak dikenal
SELECT DISTINCT client_ip, db_user, COUNT(*) as update_count
FROM audit_log
WHERE event_type = 'UPDATE'
AND event_time > NOW() - INTERVAL 1 HOUR
GROUP BY client_ip, db_user
HAVING update_count > 100  -- Alert jika lebih dari 100 update per jam
ORDER BY update_count DESC;

-- 4. Riwayat lengkap user tertentu
SELECT event_time, table_name, event_type, record_id
FROM audit_log
WHERE db_user LIKE '%api_service%'
ORDER BY event_time DESC
LIMIT 100;
```

---

## 🧠 Ilustrasi / Analogi

**Audit log seperti buku kas toko:**
* Setiap transaksi ditulis dengan tinta (tidak bisa dihapus)
* Kasir, waktu, jumlah, dan perubahan dicatat
* Kalau ada selisih kas: telusuri buku kas dari belakang

**Trigger database seperti CCTV otomatis:**
* CCTV tidak perlu kamu nyalakan manual setiap hari
* Setiap "gerakan" (INSERT/UPDATE/DELETE) otomatis terekam
* Rekaman tersimpan di tempat berbeda dari toko (audit server terpisah)

**Backup seperti asuransi:**
* Kamu berharap tidak perlu pakai
* Tapi saat bencana datang, ada atau tidaknya asuransi sangat menentukan

---

## 💻 Contoh Teknis: Backup & Recovery

### Strategi Backup Database

```
JENIS BACKUP:

1. FULL BACKUP:
   • Snapshot seluruh database
   • Waktu: paling lama, ukuran terbesar
   • Recovery: dari full backup saja
   • Frekuensi: biasanya mingguan

2. DIFFERENTIAL BACKUP:
   • Perubahan sejak FULL BACKUP terakhir
   • Lebih cepat dari full, lebih lambat dari incremental
   • Recovery: full + differential terakhir
   • Frekuensi: biasanya harian

3. INCREMENTAL BACKUP:
   • Perubahan sejak backup TERAKHIR (full ATAU incremental)
   • Paling cepat, ukuran terkecil
   • Recovery: full + setiap incremental secara berurutan
   • Frekuensi: bisa per jam

STRATEGI UMUM FINTECH (3-2-1 Rule):
• 3 salinan data (1 produksi + 2 backup)
• 2 media berbeda (disk + cloud)
• 1 salinan offsite (AWS S3, GCS, atau datacenter lain)
```

### Script Backup Terenkripsi

```python
"""
Script backup MySQL yang terenkripsi menggunakan AES-256
"""
import subprocess
import os
from datetime import datetime
from cryptography.hazmat.primitives.ciphers.aead import AESGCM
import logging

logger = logging.getLogger(__name__)

class DatabaseBackupManager:
    """
    Manajemen backup database dengan enkripsi.
    Backup disimpan sebagai file terenkripsi — tidak bisa dibaca tanpa key.
    """
    
    def __init__(self, db_config: dict, backup_dir: str, encryption_key: bytes):
        self.db_config = db_config
        self.backup_dir = backup_dir
        self.aesgcm = AESGCM(encryption_key)
        os.makedirs(backup_dir, exist_ok=True)
    
    def create_full_backup(self, database: str) -> str:
        """
        Buat full backup menggunakan mysqldump, lalu enkripsi.
        Return: path file backup terenkripsi.
        """
        timestamp = datetime.utcnow().strftime('%Y%m%d_%H%M%S')
        dump_file = f"/tmp/backup_{database}_{timestamp}.sql"
        encrypted_file = f"{self.backup_dir}/backup_{database}_{timestamp}.enc"
        
        logger.info(f"Starting full backup of {database}...")
        
        try:
            # Step 1: Dump database ke file SQL
            mysqldump_cmd = [
                'mysqldump',
                f"--user={self.db_config['user']}",
                f"--password={self.db_config['password']}",
                f"--host={self.db_config['host']}",
                '--single-transaction',    # Konsisten tanpa lock tabel
                '--routines',              # Include stored procedures
                '--triggers',             # Include triggers
                '--events',               # Include events
                '--hex-blob',             # Handle binary data
                database
            ]
            
            with open(dump_file, 'w') as f:
                result = subprocess.run(
                    mysqldump_cmd,
                    stdout=f,
                    stderr=subprocess.PIPE,
                    timeout=3600  # Max 1 jam
                )
            
            if result.returncode != 0:
                raise RuntimeError(f"mysqldump failed: {result.stderr.decode()}")
            
            dump_size = os.path.getsize(dump_file)
            logger.info(f"Dump size: {dump_size / 1024 / 1024:.1f} MB")
            
            # Step 2: Enkripsi file dump
            self._encrypt_file(dump_file, encrypted_file)
            
            encrypted_size = os.path.getsize(encrypted_file)
            logger.info(f"Backup complete: {encrypted_file} ({encrypted_size / 1024 / 1024:.1f} MB)")
            
            # Step 3: Audit log backup
            logger.info(f"AUDIT: BACKUP|{database}|{timestamp}|{dump_size}|SUCCESS")
            
            return encrypted_file
        
        finally:
            # Hapus file dump plaintext setelah dienkripsi
            if os.path.exists(dump_file):
                os.remove(dump_file)
                logger.info("Temporary plaintext dump file removed")
    
    def _encrypt_file(self, input_file: str, output_file: str):
        """Enkripsi file menggunakan AES-256-GCM."""
        nonce = os.urandom(12)
        
        with open(input_file, 'rb') as f:
            plaintext = f.read()
        
        ciphertext = self.aesgcm.encrypt(nonce, plaintext, None)
        
        with open(output_file, 'wb') as f:
            f.write(nonce)       # 12 bytes nonce di awal
            f.write(ciphertext)  # Ciphertext + 16 bytes auth tag
    
    def decrypt_backup(self, encrypted_file: str, output_file: str):
        """Dekripsi file backup untuk recovery."""
        with open(encrypted_file, 'rb') as f:
            nonce = f.read(12)
            ciphertext = f.read()
        
        try:
            plaintext = self.aesgcm.decrypt(nonce, ciphertext, None)
            with open(output_file, 'wb') as f:
                f.write(plaintext)
            logger.info(f"Backup decrypted: {output_file}")
        except Exception as e:
            raise ValueError(f"Backup integrity check failed: {e}")
    
    def restore_from_backup(self, backup_file: str, database: str):
        """
        Restore database dari backup terenkripsi.
        HATI-HATI: Ini akan MENIMPA database yang ada!
        """
        restore_sql = f"/tmp/restore_{database}.sql"
        
        logger.warning(f"RESTORE OPERATION: {database} from {backup_file}")
        logger.warning("This will OVERWRITE the existing database!")
        
        try:
            # Dekripsi backup
            self.decrypt_backup(backup_file, restore_sql)
            
            # Restore menggunakan mysql client
            mysql_cmd = [
                'mysql',
                f"--user={self.db_config['user']}",
                f"--password={self.db_config['password']}",
                f"--host={self.db_config['host']}",
                database
            ]
            
            with open(restore_sql, 'r') as f:
                result = subprocess.run(
                    mysql_cmd,
                    stdin=f,
                    stderr=subprocess.PIPE,
                    timeout=7200
                )
            
            if result.returncode != 0:
                raise RuntimeError(f"Restore failed: {result.stderr.decode()}")
            
            logger.info(f"AUDIT: RESTORE|{database}|{backup_file}|SUCCESS")
        
        finally:
            if os.path.exists(restore_sql):
                os.remove(restore_sql)

    def verify_backup_integrity(self, backup_file: str) -> bool:
        """
        Verifikasi integritas backup tanpa melakukan restore penuh.
        """
        try:
            verify_output = f"/tmp/verify_{os.path.basename(backup_file)}.sql"
            self.decrypt_backup(backup_file, verify_output)
            
            # Check apakah file SQL valid
            with open(verify_output, 'r') as f:
                first_line = f.readline()
                has_dump_marker = '-- MySQL dump' in first_line
            
            os.remove(verify_output)
            return has_dump_marker
        
        except Exception as e:
            logger.error(f"Backup verification failed: {e}")
            return False
```

### RTO & RPO Planning

```
RTO (Recovery Time Objective): Berapa lama maksimal sistem boleh down?
RPO (Recovery Point Objective): Berapa lama data yang boleh hilang?

Contoh untuk fintech:
┌─────────────────────────────────────────────────────────┐
│ Tier     │ Contoh            │ RTO      │ RPO           │
├─────────────────────────────────────────────────────────┤
│ Kritis   │ Payment processing│ < 1 jam  │ 0 (no loss)   │
│ Tinggi   │ User authentication│ < 4 jam  │ < 1 jam       │
│ Sedang   │ Analytics dashboard│ < 24 jam │ < 24 jam      │
│ Rendah   │ Audit reports     │ < 1 minggu│ < 24 jam      │
└─────────────────────────────────────────────────────────┘

Backup Schedule berdasarkan RTO/RPO:
  RPO 0 → Real-time replication (MySQL binlog replication)
  RPO 1 jam → Incremental backup per jam + monitoring
  RPO 24 jam → Full daily backup + differential harian
```

---

## 🏢 Studi Kasus: Incident Response Database di DANA

**Konteks:** Suatu hari, tim DANA mendapatkan alert: "Unusual bulk DELETE detected on transactions table (5,000 records in 30 seconds)"

**Investigasi menggunakan audit log:**

```sql
-- Langkah 1: Identifikasi scope
SELECT COUNT(*) as deleted_count, MIN(event_time) as start_time, MAX(event_time) as end_time
FROM audit_log
WHERE event_type = 'DELETE' 
AND table_name = 'transactions'
AND event_time BETWEEN '2025-01-15 02:00:00' AND '2025-01-15 02:05:00';
-- Hasil: 5,234 records dihapus antara 02:03 dan 02:04

-- Langkah 2: Identifikasi actor
SELECT DISTINCT db_user, client_ip, app_user
FROM audit_log
WHERE event_type = 'DELETE' AND event_time BETWEEN '...' AND '...';
-- Hasil: db_user='api_batch'@10.0.1.50, client_ip='10.0.1.50'

-- Langkah 3: Rekonstruksi data yang terhapus dari audit_log
SELECT old_values FROM audit_log
WHERE event_type = 'DELETE' AND table_name = 'transactions'
AND event_time BETWEEN '2025-01-15 02:03:00' AND '2025-01-15 02:04:00';
-- old_values berisi semua data yang terhapus — bisa di-restore!
```

**Temuan:** Script batch yang salah terkonfigurasi — bukannya archive ke cold storage, malah menghapus dari production.

**Recovery:**
1. Ambil backup incremental 2 jam sebelumnya
2. Replay dari audit_log untuk delta 2 jam terakhir
3. Total downtime: 45 menit

**Tanpa audit log:** Recovery tidak mungkin — data hilang permanen.

---

## ⚠️ Kesalahan Umum

1. **Audit log di tabel yang sama** → Jika production DB dicompromised atau terhapus, audit log juga hilang. Simpan audit log di database/server yang terpisah.

2. **Tidak test restore secara berkala** → "Backup ada tapi tidak pernah dicoba restore" — saat disaster, backup ternyata corrupt atau prosedurnya salah. Lakukan disaster recovery drill minimal per kuartal.

3. **Backup tidak dienkripsi** → Backup berisi salinan seluruh database. Jika file backup bocor (upload ke tempat salah), semua data terekspos. SELALU enkripsi backup.

4. **Hanya full backup, tidak ada incremental** → Backup full mingguan dengan RPO 1 minggu berarti bisa kehilangan data 7 hari. Kombinasikan dengan incremental/differential.

5. **Tidak monitor ukuran audit log** → Audit log bisa tumbuh sangat cepat (terutama di tabel transaksi). Setup archiving otomatis: log > 90 hari → pindah ke cold storage.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Apa perbedaan antara Full, Differential, dan Incremental backup? Jelaskan proses recovery untuk masing-masing skenario.

b) Mengapa audit log harus disimpan di server atau database yang berbeda dari database yang di-audit?

c) Apa itu RTO dan RPO? Jika sebuah payment gateway memiliki RTO 2 jam dan RPO 0 (zero data loss), strategi backup dan replication apa yang diperlukan?

### Soal 2 — Praktik (Lab)

Buat sistem audit log untuk database MySQL:
1. Buat tabel `users` dan `transactions` di database `fintech_db`
2. Buat tabel `audit_log` yang terpisah
3. Implementasikan trigger AFTER INSERT, AFTER UPDATE, BEFORE DELETE untuk tabel `transactions`
4. Lakukan beberapa operasi INSERT/UPDATE/DELETE
5. Query audit log untuk: melihat perubahan record tertentu, menemukan semua DELETE dalam 1 jam terakhir
6. Bonus: Buat script Python untuk backup terenkripsi menggunakan mysqldump + AES-256

---

## 📌 Ringkasan

* **Audit log** = rekaman immutable: siapa, apa, kapan, dari mana, sebelum → sesudah
* **MySQL Triggers** = implementasi audit log otomatis tanpa mengubah kode aplikasi
* Format entry: timestamp, event_type, table, record_id, old_values, new_values, user, IP
* **Backup strategy:** Full (mingguan) + Differential (harian) + Incremental (per jam)
* **3-2-1 Rule:** 3 salinan, 2 media berbeda, 1 offsite
* **RTO/RPO** menentukan strategi: RPO 0 = real-time replication, RPO 24h = daily backup cukup
* **SELALU enkripsi backup** — file backup = salinan seluruh database
* **SELALU test restore** — backup yang belum pernah di-restore mungkin tidak bisa di-restore

---

*📚 Referensi: MySQL Security Documentation | Anderson, R. (2020). Security Engineering | NIST SP 800-34 — Contingency Planning | OWASP Logging Cheat Sheet*
