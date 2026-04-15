# Pertemuan 8: SQL Injection Prevention & Secure Coding

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Memahami dan mendemonstrasikan berbagai jenis SQL Injection
* Mengidentifikasi kode yang rentan SQL Injection
* Menerapkan Prepared Statements dan Parameterized Queries sebagai solusi utama
* Menggunakan input validation dan error handling yang aman

---

## 📖 Pengantar (Hook)

2008: Hacker menyerang sistem retailer Heartland Payment Systems menggunakan SQL Injection. Hasilnya: 130 juta data kartu kredit dicuri. Kerugian: $140 juta.

2012: Hacker Suraksasena berhasil masuk ke sistem pemerintah Indonesia menggunakan SQL Injection sederhana — teknik yang sudah diketahui sejak 1998.

SQL Injection tetap menjadi ancaman #1 (atau #3 di OWASP 2021) meskipun solusinya sudah sangat jelas. Mengapa? Karena developer masih menulis kode seperti ini:

```python
query = "SELECT * FROM users WHERE username='" + username + "'"
```

Satu baris ini bisa menghancurkan seluruh database.

---

## 🧩 Konsep Utama

### Cara Kerja SQL Injection

SQL Injection terjadi ketika **input user dimasukkan langsung ke SQL query** tanpa sanitasi, sehingga attacker bisa menyisipkan kode SQL berbahaya.

```
QUERY NORMAL:
query = "SELECT * FROM users WHERE username='" + username + "' AND password='" + password + "'"

INPUT NORMAL:
  username = "budi"
  password = "rahasia123"
  
  Hasil: SELECT * FROM users WHERE username='budi' AND password='rahasia123'
  → Login normal ✓

INPUT JAHAT (SQL Injection):
  username = "admin'--"
  password = "apapun"
  
  Hasil: SELECT * FROM users WHERE username='admin'--' AND password='apapun'
  → '--' adalah komentar SQL!
  → Query efektif: SELECT * FROM users WHERE username='admin'
  → Kondisi password DISKIP! Login sebagai admin tanpa password! ✗
```

### Jenis-Jenis SQL Injection

#### 1. Classic (In-Band) SQL Injection

```
UNION-BASED: Ekstrak data dari tabel lain

Input: ' UNION SELECT username, password, 3, 4 FROM admin_users --

Query:
  SELECT id, name, email, phone FROM customers WHERE id='1'
  UNION SELECT username, password, 3, 4 FROM admin_users --'

→ Hasil: Data admin_users ditampilkan bersama data customers!
→ Attacker mendapatkan username dan password admin
```

#### 2. Blind SQL Injection

```
Tidak ada output langsung, tapi attacker bisa inferensikan jawaban dari perilaku aplikasi.

BOOLEAN-BASED:
  Input TRUE:  id=1 AND 1=1 --  → halaman normal tampil
  Input FALSE: id=1 AND 1=2 --  → halaman error/kosong

  Inferensi: "apakah karakter pertama database name = 'p'?"
  id=1 AND SUBSTRING(database(),1,1)='p' --
  → Jika halaman normal: ya, nama database dimulai dengan 'p'

TIME-BASED:
  Input: id=1; IF(1=1, SLEEP(5), 0) --
  → Jika respons lambat 5 detik: injeksi berhasil!
  
  Attacker bisa "encode" data dengan delay:
  id=1; IF(SUBSTRING(password,1,1)='a', SLEEP(2), 0) --
  → Jika delay 2 detik: karakter pertama password = 'a'
```

### Prepared Statements: Solusi Utama

Prepared Statements memisahkan **kode SQL** dari **data user**, sehingga data tidak bisa diinterpretasi sebagai kode SQL.

```python
# ❌ RENTAN SQL Injection
def login_vulnerable(username, password, conn):
    query = f"SELECT * FROM users WHERE username='{username}' AND password='{password}'"
    cursor = conn.cursor()
    cursor.execute(query)  # Data tercampur dengan kode SQL!
    return cursor.fetchone()

# ✅ AMAN dengan Prepared Statements
def login_secure(username, password, conn):
    # Placeholder %s — user input TIDAK masuk ke string SQL
    query = "SELECT * FROM users WHERE username=%s AND password=%s"
    cursor = conn.cursor()
    cursor.execute(query, (username, password))  # Parameter terpisah!
    return cursor.fetchone()

# Mengapa ini aman?
# Dengan Prepared Statement:
# 1. Database COMPILE query dulu: "SELECT * FROM users WHERE username=? AND password=?"
# 2. Lalu user input di-bind sebagai DATA LITERAL, bukan kode
# 3. Input "admin'--" akan dianggap sebagai STRING "admin'--", bukan kode SQL
```

---

## 🧠 Ilustrasi / Analogi

**SQL Injection seperti pemalsuan cek:**
* Teller bank yang naif: "Bayar ke: [nama penerima]" → attacker tulis "Bayar ke: Siapapun ATAU 1=1"
* Teller bank yang aman: form terpisah untuk "nama penerima" — input di-validate sebelum diproses

**Prepared Statement seperti formulir resmi:**
* Formulir punya kotak-kotak terpisah: [NAMA], [ALAMAT], [JUMLAH]
* Pengguna mengisi kotak — tidak bisa keluar dari kotak untuk tambahkan instruksi baru
* Meskipun pengguna menulis SQL di kotak [NAMA], itu tetap dianggap sebagai teks nama biasa

---

## 💻 Contoh Teknis

### Demonstrasi Lengkap: Rentan vs Aman

```python
"""
Perbandingan kode rentan vs aman untuk berbagai operasi database
"""
import mysql.connector
import sqlite3  # Untuk demo lokal tanpa MySQL

# Setup SQLite untuk demo
def setup_demo_db():
    conn = sqlite3.connect(':memory:')
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE users (
        id INTEGER PRIMARY KEY,
        username TEXT NOT NULL,
        password_hash TEXT NOT NULL,
        email TEXT,
        role TEXT DEFAULT 'user'
    )''')
    cursor.execute('''CREATE TABLE transactions (
        id INTEGER PRIMARY KEY,
        user_id INTEGER,
        amount REAL,
        status TEXT
    )''')
    # Insert dummy data
    cursor.execute("INSERT INTO users VALUES (1, 'admin', 'hash_admin', 'admin@co.id', 'admin')")
    cursor.execute("INSERT INTO users VALUES (2, 'budi', 'hash_budi', 'budi@mail.com', 'user')")
    cursor.execute("INSERT INTO transactions VALUES (1, 2, 500000, 'success')")
    conn.commit()
    return conn

# =====================================================
# RENTAN: String concatenation
# =====================================================

def get_user_vulnerable(conn, username: str):
    """❌ RENTAN — jangan pernah gunakan ini!"""
    query = f"SELECT * FROM users WHERE username = '{username}'"
    print(f"  [RENTAN] Query: {query}")
    try:
        cursor = conn.cursor()
        cursor.execute(query)
        return cursor.fetchall()
    except Exception as e:
        return f"Error: {e}"

# =====================================================
# AMAN: Prepared Statements
# =====================================================

def get_user_secure(conn, username: str):
    """✅ AMAN — parameterized query"""
    query = "SELECT * FROM users WHERE username = ?"
    cursor = conn.cursor()
    cursor.execute(query, (username,))  # Input di-bind sebagai data
    return cursor.fetchall()


# =====================================================
# INPUT VALIDATION: Defense in Depth
# =====================================================

import re

class UserRepository:
    """
    Repository dengan multiple layer of defense:
    1. Input validation (format check)
    2. Prepared statements (SQL injection prevention)
    3. Error handling yang aman (tidak expose internal info)
    """
    
    def __init__(self, conn):
        self.conn = conn
    
    def _validate_username(self, username: str) -> bool:
        """Validasi format username: hanya alphanumeric dan underscore, 3-50 chars"""
        if not isinstance(username, str):
            return False
        return bool(re.match(r'^[a-zA-Z0-9_]{3,50}$', username))
    
    def _validate_amount(self, amount) -> bool:
        """Validasi amount adalah angka positif"""
        try:
            float_amount = float(amount)
            return 0 < float_amount <= 100_000_000  # Max 100 juta
        except (TypeError, ValueError):
            return False
    
    def get_user(self, username: str):
        """Cari user dengan validasi + prepared statement."""
        # Layer 1: Input validation
        if not self._validate_username(username):
            raise ValueError("Format username tidak valid")
        
        # Layer 2: Prepared statement
        cursor = self.conn.cursor()
        cursor.execute(
            "SELECT id, username, email, role FROM users WHERE username = ?",
            (username,)
        )
        user = cursor.fetchone()
        
        # Layer 3: Return minimal info (tidak expose password hash)
        return user
    
    def get_user_transactions(self, user_id: int, requesting_user_id: int):
        """
        Ambil transaksi user — HARUS verifikasi kepemilikan!
        Mencegah IDOR (Insecure Direct Object Reference)
        """
        # Verifikasi requesting user hanya bisa akses datanya sendiri
        if user_id != requesting_user_id:
            raise PermissionError("Access denied")
        
        cursor = self.conn.cursor()
        cursor.execute(
            "SELECT id, amount, status FROM transactions WHERE user_id = ?",
            (user_id,)
        )
        return cursor.fetchall()
    
    def search_transactions(self, user_id: int, status: str = None, 
                             min_amount: float = None):
        """
        Dynamic query yang tetap aman dengan prepared statements.
        """
        # Whitelist untuk kolom/nilai yang bisa difilter
        VALID_STATUSES = {'success', 'pending', 'failed'}
        
        if status and status not in VALID_STATUSES:
            raise ValueError(f"Status tidak valid. Pilihan: {VALID_STATUSES}")
        
        # Build query dengan params, BUKAN string concat
        conditions = ["user_id = ?"]
        params = [user_id]
        
        if status:
            conditions.append("status = ?")
            params.append(status)
        
        if min_amount is not None:
            if not self._validate_amount(min_amount):
                raise ValueError("Amount tidak valid")
            conditions.append("amount >= ?")
            params.append(min_amount)
        
        query = f"SELECT * FROM transactions WHERE {' AND '.join(conditions)}"
        cursor = self.conn.cursor()
        cursor.execute(query, params)
        return cursor.fetchall()


# =====================================================
# ERROR HANDLING YANG AMAN
# =====================================================

import logging

logger = logging.getLogger(__name__)

def safe_db_operation(conn, user_input: str):
    """
    Error handling yang aman — tidak mengekspos info internal ke user.
    """
    try:
        repo = UserRepository(conn)
        return repo.get_user(user_input)
    
    except ValueError as e:
        # Input validation error — aman ditampilkan ke user
        return {"error": str(e)}, 400
    
    except PermissionError as e:
        # Authorization error — aman ditampilkan ke user
        return {"error": "Access denied"}, 403
    
    except Exception as e:
        # Internal error — LOG tapi JANGAN tampilkan ke user!
        logger.error(f"Database error for input '{user_input}': {e}", exc_info=True)
        # User mendapat pesan generic — tidak ada info tentang database internal
        return {"error": "Terjadi kesalahan. Silakan coba lagi."}, 500


# =====================================================
# DEMO
# =====================================================

conn = setup_demo_db()

print("=== SQL INJECTION DEMO ===\n")

# Serangan: bypass login
malicious_input = "admin'--"
print(f"[ATTACK] Input: {malicious_input}")
result_vuln = get_user_vulnerable(conn, malicious_input)
print(f"  Vulnerable result: {result_vuln}")
result_safe = get_user_secure(conn, malicious_input)
print(f"  Secure result: {result_safe} (kosong karena tidak ada user 'admin'--')\n")

# Serangan: UNION extraction
malicious_union = "' UNION SELECT id, username, password_hash, email, role FROM users--"
print(f"[ATTACK] UNION injection: {malicious_union[:50]}...")
result_union = get_user_vulnerable(conn, malicious_union)
print(f"  Vulnerable result: {result_union}")
result_union_safe = get_user_secure(conn, malicious_union)
print(f"  Secure result: {result_union_safe}\n")

# Input validation
print("=== INPUT VALIDATION ===")
repo = UserRepository(conn)
try:
    repo.get_user("admin'; DROP TABLE users;--")
except ValueError as e:
    print(f"  Input rejected: {e}")

print(f"  Valid input result: {repo.get_user('budi')}")
```

### Stored Procedures sebagai Extra Layer

```sql
-- Stored procedure untuk login (safer than direct query)
DELIMITER //
CREATE PROCEDURE sp_login(
    IN p_username VARCHAR(50),
    IN p_password_hash VARCHAR(255)
)
BEGIN
    SELECT id, username, email, role
    FROM users
    WHERE username = p_username
    AND password_hash = p_password_hash
    AND is_active = 1
    LIMIT 1;  -- Hanya ambil satu record
END //
DELIMITER ;

-- Panggil dari aplikasi:
-- cursor.callproc('sp_login', (username, password_hash))
```

---

## 🏢 Studi Kasus: SQLMAP Testing pada Xendit (Ethical Context)

**Konteks:** Sebelum launch, tim security Xendit melakukan penetration testing menggunakan SQLMAP untuk menemukan kerentanan.

**Proses ethical hacking (dalam lab/staging environment):**

```bash
# HANYA DI LINGKUNGAN LAB YANG DIIZINKAN!
# JANGAN pernah test sistem tanpa izin tertulis!

# 1. Scan basic untuk deteksi SQL injection
sqlmap -u "http://staging.app/api/user?id=1" \
       --cookie="session=abc123" \
       --level=3 --risk=2

# 2. Jika ditemukan injection point, ekstrak database names
sqlmap -u "http://staging.app/api/user?id=1" \
       --dbs

# 3. Ekstrak tabel dalam database
sqlmap -u "http://staging.app/api/user?id=1" \
       -D staging_db --tables

# Output yang diharapkan setelah fix:
# [INFO] the back-end DBMS is MySQL
# [WARNING] heuristic (basic) test shows parameter 'id' might not be injectable
# [ERROR] all tested parameters do not appear to be injectable
```

**Apa yang ditemukan dan diperbaiki:**
1. Endpoint `/api/report?date=2025-01-01` rentan time-based blind injection
2. Fix: Gunakan prepared statement, validasi format tanggal (`YYYY-MM-DD`)
3. Hasil setelah fix: SQLMAP tidak menemukan injection point

**Pelajaran:** Lakukan security testing **sebelum production**, bukan setelah breach.

---

## ⚠️ Kesalahan Umum

1. **String concatenation untuk query** → `"WHERE id=" + user_id` — ini adalah root cause SQL injection. SELALU gunakan prepared statements atau parameterized queries.

2. **Sanitasi manual yang tidak lengkap** → Mencoba escape karakter sendiri (`replace("'", "''")`) → Mudah di-bypass. Library prepared statements melakukan ini dengan benar.

3. **Error message yang verbose** → `"MySQL Error: Table 'users' not found in database 'prod_db'"` → Memberikan informasi berharga ke attacker. Gunakan pesan generic untuk user, log detail hanya di server.

4. **Whitelist vs Blacklist** → Mencoba blacklist karakter berbahaya (`"'", ";`, `"--"`) → Tidak akan pernah lengkap. Gunakan whitelist: validasi bahwa input sesuai format yang diharapkan (regex).

5. **Mengabaikan ORDER BY dan tabel/kolom dinamis** → `ORDER BY {user_input}` tidak bisa menggunakan prepared statement → Gunakan whitelist: `if col not in ['id', 'name', 'date']: raise ValueError`

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan antara Classic SQL Injection dan Blind SQL Injection. Berikan contoh masing-masing.

b) Mengapa escaping karakter (`replace("'", "''")`) bukan solusi yang tepat untuk mencegah SQL Injection? Apa yang lebih baik?

c) Seorang developer mengatakan: "Saya sudah menggunakan firewall WAF, jadi tidak perlu khawatir SQL Injection di kode." Evaluasi pernyataan ini.

### Soal 2 — Praktik

Diberikan kode berikut yang rentan SQL Injection:

```python
def search_products(conn, category, min_price, max_price, sort_by):
    query = f"""
        SELECT * FROM products 
        WHERE category = '{category}'
        AND price BETWEEN {min_price} AND {max_price}
        ORDER BY {sort_by}
    """
    return conn.execute(query).fetchall()
```

a) Identifikasi semua titik rentan SQL Injection dalam kode ini
b) Tulis versi yang aman menggunakan prepared statements dan input validation
c) Demonstrasikan bahwa SQL Injection tidak lagi bisa dilakukan pada versi aman

---

## 📌 Ringkasan

* **SQL Injection:** Input user dieksekusi sebagai kode SQL → akses tidak sah ke database
* **Jenis:** Classic (output langsung), Union-based (gabung tabel), Blind Boolean, Blind Time-based
* **Root cause:** Mencampur kode SQL dengan data user dalam satu string
* **Solusi utama:** **Prepared Statements / Parameterized Queries** — data tidak pernah masuk ke string SQL
* **Defense in Depth:**
  1. Input validation (whitelist format yang valid)
  2. Prepared statements (solusi utama)
  3. Stored procedures (layer tambahan)
  4. Error handling aman (jangan expose internal info)
  5. Least privilege DB user (batas kerusakan jika exploit berhasil)
* SQL Injection adalah OWASP A03 — masih #1 penyebab breach database di dunia

---

*📚 Referensi: OWASP SQL Injection Prevention Cheat Sheet | Anderson, R. (2020). Security Engineering | MySQL Security Docs | SQLMAP Documentation — sqlmap.org*
