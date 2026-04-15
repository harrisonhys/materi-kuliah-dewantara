# Pertemuan 13: Regulasi Perlindungan Data — UU PDP Indonesia & GDPR

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Menjelaskan ruang lingkup UU PDP No. 27/2022: data pribadi umum vs sensitif
* Menjelaskan hak subjek data: akses, koreksi, penghapusan, portabilitas
* Membandingkan UU PDP Indonesia vs GDPR Eropa: persamaan dan perbedaan
* Menerapkan Privacy by Design dalam pengembangan sistem
* Mengidentifikasi data pribadi sensitif dalam skenario aplikasi nyata

---

## 📖 Pengantar (Hook)

Oktober 2022: Indonesia mengesahkan UU No. 27/2022 tentang Perlindungan Data Pribadi (UU PDP). Ini adalah momen bersejarah — untuk pertama kalinya, Indonesia memiliki undang-undang komprehensif yang mengatur perlindungan data pribadi warganya.

Implikasinya bagi developer dan perusahaan teknologi sangat besar:
* Denda pelanggaran: hingga Rp 60 miliar untuk perorangan, 2% dari pendapatan tahunan global untuk badan hukum
* Sanksi pidana: 4-6 tahun penjara untuk pelanggaran tertentu
* Semua aplikasi yang mengumpulkan data warga Indonesia wajib comply

Sebagai developer, kamu tidak bisa lagi berdalih "kami hanya membuat software." Jika kamu menulis kode yang mengumpulkan data pengguna tanpa consent atau menyimpannya tidak aman, kamu ikut bertanggung jawab secara hukum.

---

## 🧩 Konsep Utama

### UU PDP No. 27/2022: Overview

```
BERLAKU: Oktober 2024 (2 tahun masa transisi dari Oktober 2022)

BERLAKU UNTUK:
  • Semua entitas yang memproses data pribadi warga Indonesia
  • Termasuk perusahaan asing yang beroperasi di Indonesia
  • Termasuk startup kecil — tidak ada pengecualian berdasarkan ukuran

DUA KATEGORI DATA PRIBADI:

1. DATA PRIBADI UMUM (Pasal 4):
   • Nama lengkap
   • Jenis kelamin
   • Kewarganegaraan
   • Agama
   • Status pernikahan
   • Data pribadi gabungan (bisa identifikasi seseorang)

2. DATA PRIBADI SENSITIF (Pasal 4 — perlindungan lebih ketat):
   • Data dan informasi kesehatan
   • Data biometrik (sidik jari, retina, wajah)
   • Data genetika
   • Kehidupan/orientasi seksual
   • Pandangan politik
   • Data keuangan pribadi
   • Data anak
   • Pesan/komunikasi pribadi
   • Data lain yang berdampak pada keamanan

PERBEDAAN PERLAKUAN:
  Data Umum: Bisa diproses dengan lawful basis
  Data Sensitif: HARUS ada consent EKSPLISIT yang spesifik
```

### Prinsip-Prinsip UU PDP

```
DELAPAN PRINSIP DASAR:

1. LIMITASI PENGUMPULAN:
   Hanya kumpulkan data yang BENAR-BENAR diperlukan
   → "Data minimization" — jangan kumpulkan kalau tidak perlu

2. TUJUAN YANG SPESIFIK:
   Data hanya boleh digunakan sesuai tujuan yang dinyatakan saat pengumpulan
   → Tidak boleh tiba-tiba dijual ke pihak ketiga tanpa consent baru

3. AKURASI DATA:
   Pengendali data wajib memastikan data akurat dan up-to-date
   → User harus bisa koreksi data yang salah

4. PEMBATASAN PENYIMPANAN:
   Data tidak boleh disimpan lebih lama dari yang diperlukan
   → Harus ada retention policy: hapus setelah N tahun/tidak aktif

5. KEAMANAN DATA:
   Wajib menerapkan langkah teknis dan organisasi yang tepat
   → Enkripsi, access control, audit log (semua yang kita pelajari!)

6. PERTANGGUNGJAWABAN:
   Pengendali data bertanggung jawab atas kepatuhan

7. KETERSEDIAAN DAN AKSESIBILITAS:
   Subjek data bisa akses datanya

8. HAK SUBJEK DATA:
   (Dijelaskan di bawah)
```

### Hak Subjek Data (Pasal 5-16 UU PDP)

```
DELAPAN HAK SUBJEK DATA:

1. HAK MENDAPATKAN INFORMASI
   → Pengguna berhak tahu: apa yang dikumpulkan, mengapa, siapa yang terima
   → Implementasi: Privacy Policy yang jelas dan mudah dipahami

2. HAK MENGAKSES DATA PRIBADI
   → "Tunjukkan semua data yang kamu simpan tentang saya"
   → Implementasi: endpoint /api/user/my-data yang return semua data

3. HAK MEMPERBAIKI/MEMPERBARUI
   → "Data nama saya salah, tolong perbaiki"
   → Implementasi: form edit profil, request perbaikan data

4. HAK MENGHAPUS (RIGHT TO BE FORGOTTEN)
   → "Hapus semua data saya"
   → Implementasi: fitur "Hapus Akun" yang benar-benar hapus data
   → TANTANGAN: data yang tersebar di multiple systems, backup

5. HAK MENARIK KEMBALI CONSENT
   → "Saya tarik izin yang sudah saya berikan"
   → Implementasi: unsubscribe, opt-out dari data processing

6. HAK MENDAPATKAN DATA DALAM FORMAT PORTABLE
   → "Berikan data saya dalam format JSON/CSV untuk dipindahkan ke layanan lain"
   → Implementasi: fitur export data

7. HAK KEBERATAN ATAS PEMROSESAN
   → User bisa keberatan jika data diproses untuk marketing, profiling, dll
   → Implementasi: opt-out dari targeted advertising

8. HAK TIDAK MENJADI SUBJEK AUTOMATED DECISION
   → Keputusan yang berdampak signifikan tidak boleh fully automated tanpa review manusia
   → Contoh: penolakan kredit tidak boleh hanya berdasarkan algoritma tanpa review
```

### Kewajiban Pengendali dan Prosesor Data

```
PENGENDALI DATA (Controller):
  Menentukan tujuan dan cara pemrosesan data
  → Platform fintech, e-commerce, aplikasi

PROSESOR DATA (Processor):
  Memproses data atas nama pengendali
  → Cloud provider (AWS, GCP), payment gateway, analytics tools

KEWAJIBAN UTAMA:
  ① Dasar Hukum (Lawful Basis):
    Sebelum memproses, harus ada salah satu:
    • Persetujuan (consent) yang jelas dan spesifik
    • Pelaksanaan kontrak
    • Kewajiban hukum
    • Kepentingan vital
    • Tugas publik
    • Kepentingan sah (legitimate interest)

  ② Data Breach Notification:
    Harus lapor ke BSSN/KOMINFO dalam 14 hari kerja jika ada breach
    Harus notify subjek data yang terdampak

  ③ Data Protection Impact Assessment (DPIA):
    Wajib untuk pemrosesan berisiko tinggi
    (data sensitif dalam jumlah besar, profiling, dll)

  ④ Data Protection Officer (DPO):
    Wajib menunjuk DPO untuk pengendali yang:
    • Memproses data sensitif dalam skala besar
    • Melakukan monitoring sistematis
```

### Perbandingan UU PDP vs GDPR

```
┌──────────────────────────────────────────────────────────────────┐
│ Aspek              │ UU PDP Indonesia        │ GDPR (EU/EEA)     │
├──────────────────────────────────────────────────────────────────┤
│ Berlaku sejak      │ Oktober 2024            │ Mei 2018          │
│ Wilayah            │ Indonesia               │ EU + EEA          │
│ Ekstra-territorial │ Ya (data WNI di luar RI)│ Ya (data warga EU)│
├──────────────────────────────────────────────────────────────────┤
│ Hak subjek data    │ 8 hak (mirip GDPR)      │ 8 hak             │
│ Consent requirement│ Eksplisit untuk sensitif│ Eksplisit untuk all│
│ Lawful basis       │ 6 basis                 │ 6 basis           │
├──────────────────────────────────────────────────────────────────┤
│ Denda maksimum     │ 2% pendapatan tahunan   │ 4% pendapatan     │
│                    │ + Rp 60M perorangan     │ global / €20M     │
│ Sanksi pidana      │ Ya (4-6 tahun)          │ Tidak ada         │
├──────────────────────────────────────────────────────────────────┤
│ Breach notification│ 14 hari kerja           │ 72 jam            │
│ DPO requirement    │ Kasus tertentu          │ Kasus tertentu    │
├──────────────────────────────────────────────────────────────────┤
│ Penegakan          │ Belum teruji (baru)     │ Tegas (Meta: €1.2B│
│                    │                         │ fine 2023)        │
└──────────────────────────────────────────────────────────────────┘

PERBEDAAN KUNCI:
1. GDPR: breach notification 72 jam → UU PDP: 14 hari kerja (lebih longgar)
2. GDPR: tidak ada sanksi pidana → UU PDP: ada pidana penjara
3. GDPR: penegakan lebih matang → UU PDP: masih dalam fase awal
4. GDPR: consent default ke "tidak" → UU PDP: mirip tapi implementasi berbeda
```

---

## 🧠 Ilustrasi / Analogi

**Data pribadi seperti surat-surat pribadi di laci meja:**
* Orang lain tidak boleh membuka laci tanpa izin (consent)
* Kalau sudah diizinkan lihat surat A, bukan berarti boleh lihat semua surat (limitation)
* Saat keperluan selesai, surat harus dikembalikan (retention policy)
* Kalau laci dibobol orang, kamu harus lapor ke polisi (breach notification)

**Privacy by Design seperti membangun rumah dengan privasi dari awal:**
* Bukan menambahkan tirai SETELAH rumah dibangun dengan dinding kaca
* Privasi dirancang sejak blueprint, bukan afterthought
* Kamar tidur tidak punya jendela menghadap jalan — built-in privacy

**Right to be Forgotten seperti permintaan hapus catatan:**
* User berhak minta semua "catatan" tentang dirinya dihapus
* Seperti menghapus rekam medis dari rumah sakit yang sudah tidak dikunjungi
* Tantangan: rekam medis bisa ada di multiple lokasi (backup, cabang, dll)

---

## 💻 Contoh Teknis

### Implementasi Hak Subjek Data di Python/Flask

```python
"""
Implementasi endpoint untuk hak-hak subjek data (UU PDP / GDPR compliance)
"""
from flask import Flask, request, jsonify, session
from datetime import datetime
import json
import os

app = Flask(__name__)

class UserDataController:
    """
    Controller untuk mengelola hak-hak subjek data.
    Semua operasi harus di-audit dan memerlukan autentikasi.
    """
    
    def __init__(self, db, audit_logger):
        self.db = db
        self.audit = audit_logger
    
    def export_user_data(self, user_id: int) -> dict:
        """
        HAK PORTABILITAS: Ekspor semua data pengguna dalam format terstruktur.
        Return JSON yang bisa digunakan untuk pindah ke layanan lain.
        """
        user = self.db.get_user(user_id)
        transactions = self.db.get_user_transactions(user_id)
        preferences = self.db.get_user_preferences(user_id)
        
        # Format machine-readable
        export = {
            'export_date': datetime.utcnow().isoformat(),
            'data_controller': 'PT MyFintech Indonesia',
            'subject': {
                'user_id': user_id,
                'personal_data': {
                    'name': user.get('full_name'),
                    'email': user.get('email'),
                    'phone': user.get('phone'),
                    'date_of_birth': user.get('dob'),
                    'address': user.get('address')
                },
                'account_data': {
                    'created_at': user.get('created_at'),
                    'last_login': user.get('last_login'),
                    'kyc_status': user.get('kyc_status')
                },
                'transaction_history': [
                    {
                        'id': tx['id'],
                        'date': tx['created_at'],
                        'type': tx['type'],
                        'amount': tx['amount'],
                        'status': tx['status']
                    }
                    for tx in transactions
                ],
                'preferences': preferences
            }
        }
        
        self.audit.log(
            user_id=user_id,
            action='DATA_EXPORT',
            details='User requested full data export'
        )
        
        return export
    
    def delete_user_data(self, user_id: int, reason: str = None) -> dict:
        """
        HAK PENGHAPUSAN (Right to be Forgotten).
        CATATAN: Beberapa data mungkin tidak bisa dihapus (kewajiban regulasi).
        """
        # Data yang BISA dihapus
        deletable = {
            'profile_data': True,
            'preferences': True,
            'device_tokens': True,
            'marketing_preferences': True,
            'behavioral_data': True
        }
        
        # Data yang TIDAK BISA dihapus (kewajiban hukum)
        non_deletable = {
            'transaction_records': 'Wajib disimpan 10 tahun (OJK regulation)',
            'kyc_documents': 'Wajib disimpan 5 tahun (PPATK/AML regulation)',
            'audit_logs': 'Wajib disimpan untuk investigasi hukum'
        }
        
        # Anonymize (bukan delete untuk data yang tidak bisa dihapus)
        self.db.anonymize_user_personal_data(user_id)
        # Ganti: nama → "Deleted User", email → "deleted_XXXXX@deleted.invalid"
        # NIK → null, alamat → null, dll
        
        # Hapus data yang bisa dihapus
        self.db.delete_user_preferences(user_id)
        self.db.delete_device_tokens(user_id)
        self.db.revoke_all_consents(user_id)
        
        self.audit.log(
            user_id=user_id,
            action='DATA_DELETION_REQUEST',
            details=f'Reason: {reason or "User request"}'
        )
        
        return {
            'status': 'processed',
            'deleted': list(deletable.keys()),
            'retained': non_deletable,
            'reason_for_retention': 'Kewajiban regulasi perbankan (OJK/PPATK)'
        }
    
    def update_consent(self, user_id: int, consent_items: dict) -> dict:
        """
        Simpan/update consent pengguna secara granular.
        Setiap consent harus spesifik (bukan blanket consent).
        """
        valid_consent_types = {
            'marketing_email',
            'marketing_sms',
            'data_analytics',
            'third_party_sharing',
            'location_tracking',
            'behavioral_profiling'
        }
        
        # Validasi
        for key in consent_items.keys():
            if key not in valid_consent_types:
                raise ValueError(f"Unknown consent type: {key}")
        
        # Simpan dengan timestamp (untuk audit trail consent)
        for consent_type, is_granted in consent_items.items():
            self.db.upsert_consent(
                user_id=user_id,
                consent_type=consent_type,
                is_granted=is_granted,
                timestamp=datetime.utcnow(),
                ip_address=request.remote_addr,
                user_agent=request.user_agent.string
            )
        
        self.audit.log(
            user_id=user_id,
            action='CONSENT_UPDATE',
            details=json.dumps(consent_items)
        )
        
        return {'status': 'updated', 'consents': consent_items}
    
    def get_privacy_info(self, user_id: int) -> dict:
        """
        HAK MENDAPATKAN INFORMASI: Transparansi tentang apa yang disimpan.
        """
        return {
            'data_collected': [
                'Nama lengkap, email, nomor telepon',
                'Riwayat transaksi',
                'Data KYC (KTP, selfie)',
                'Data perangkat (device ID, IP address)',
                'Preferensi dan pengaturan'
            ],
            'purposes': [
                'Verifikasi identitas (KYC)',
                'Pemrosesan transaksi',
                'Pencegahan penipuan',
                'Komunikasi layanan'
            ],
            'third_party_sharing': [
                'OJK (kewajiban regulasi)',
                'PPATK (pelaporan AML)',
                'Payment processor (Midtrans/Xendit) — terbatas pada data transaksi'
            ],
            'retention_period': {
                'transaction_data': '10 tahun (OJK requirement)',
                'kyc_documents': '5 tahun setelah close account',
                'log_data': '2 tahun',
                'marketing_data': 'Sampai consent ditarik'
            },
            'data_protection_officer': 'dpo@myfintech.id',
            'complaint_channel': 'privasi@myfintech.id atau Kominfo'
        }
```

### Privacy by Design: Prinsip Implementasi

```python
# ===== PRIVACY BY DESIGN dalam Kode =====

# 1. DATA MINIMIZATION
# ❌ BURUK: Kumpulkan semua data karena "mungkin berguna"
class BadUserProfile:
    def __init__(self):
        self.fields = ['name', 'email', 'phone', 'dob', 'address',
                       'income', 'job', 'education', 'hobbies', 
                       'social_media', 'location_history', ...]  # Over-collection!

# ✅ BAIK: Kumpulkan hanya yang benar-benar diperlukan
class GoodUserProfile:
    REQUIRED_FIELDS = ['name', 'email', 'phone']  # Untuk akun
    KYC_FIELDS = ['nik', 'selfie']  # Hanya untuk verifikasi identitas
    OPTIONAL_FIELDS = ['dob']  # Dengan consent untuk fitur tertentu

# 2. PSEUDONYMIZATION
def anonymize_for_analytics(user_data: dict) -> dict:
    """
    Ubah data personal menjadi pseudonymous untuk analytics.
    Analyst dapat insight tanpa melihat data pribadi sesungguhnya.
    """
    import hashlib
    return {
        'user_hash': hashlib.sha256(str(user_data['user_id']).encode()).hexdigest()[:16],
        'age_bucket': f"{(user_data['age'] // 10) * 10}-{(user_data['age'] // 10) * 10 + 9}",
        'city': user_data.get('city', 'unknown'),
        'transaction_count': user_data['transaction_count'],
        # Tidak ada: nama, email, NIK, nomor rekening
    }

# 3. PURPOSE LIMITATION
class DataAccess:
    """
    Pastikan data hanya digunakan sesuai purpose yang dikonsent.
    """
    @staticmethod
    def can_use_for_marketing(user_id: int, db) -> bool:
        consent = db.get_consent(user_id, 'marketing_email')
        return consent is not None and consent.is_granted

    @staticmethod
    def can_share_with_third_party(user_id: int, db) -> bool:
        consent = db.get_consent(user_id, 'third_party_sharing')
        return consent is not None and consent.is_granted

# 4. RETENTION POLICY
class DataRetentionJob:
    """
    Cron job untuk menghapus data yang sudah melewati retention period.
    """
    RETENTION_POLICIES = {
        'inactive_user_behavioral_data': 180,  # 180 hari
        'marketing_preferences': 365,           # 1 tahun sejak opt-out
        'device_tokens': 90,                    # 90 hari sejak inaktif
        'session_logs': 30,                     # 30 hari
    }
    
    def run_cleanup(self, db):
        for data_type, retention_days in self.RETENTION_POLICIES.items():
            deleted_count = db.delete_expired_data(data_type, retention_days)
            print(f"Deleted {deleted_count} expired {data_type} records")
```

---

## 🏢 Studi Kasus: Kepatuhan UU PDP di Fintech Indonesia

**Kasus: Analisis kepatuhan aplikasi DANA terhadap UU PDP**

**Yang DANA lakukan dengan baik:**
```
✓ Privacy Policy tersedia di app (bahasa Indonesia + Inggris)
✓ Explicit consent untuk pengumpulan data sensitif (KYC)
✓ Fitur "Hapus Akun" tersedia di settings
✓ Export data tersedia (melalui customer service)
✓ Contact DPO ada di privacy policy
✓ Data breach notification procedure sudah ada (wajib saat masih GDPR consideration)
```

**Area yang perlu diperhatikan (umum di industri):**
```
⚠️ Privacy Policy terlalu panjang dan sulit dipahami user biasa
   → Solusi: Layered privacy notice (ringkasan + detail)

⚠️ Consent terlalu blanket ("dengan mendaftar, kamu setuju semua penggunaan data")
   → Solusi: Granular consent per purpose (marketing, analytics, dll terpisah)

⚠️ Retention policy tidak dijelaskan dengan jelas ke user
   → Solusi: Tambahkan bagian "Berapa lama kami simpan datamu?"

⚠️ Proses penghapusan akun membutuhkan CS support, belum self-service
   → Solusi: Automated deletion flow di app
```

---

## ⚠️ Kesalahan Umum

1. **Privacy Policy = copy-paste template generik** → Banyak startup copy template dari internet tanpa menyesuaikan dengan data yang sebenarnya dikumpulkan. UU PDP mensyaratkan privacy notice yang akurat dan spesifik.

2. **Consent sebagai satu checkbox untuk semua** → "Dengan mendaftar, kamu menyetujui semua pemrosesan data." Ini bukan consent yang valid menurut UU PDP dan GDPR. Consent harus spesifik per tujuan.

3. **"Hapus Akun" tidak benar-benar menghapus data** → User klik hapus akun → akun "dinonaktifkan" tapi semua data masih ada. Right to erasure berarti data benar-benar dihapus atau dianonimkan (kecuali yang wajib disimpan).

4. **Tidak tahu data apa yang dikumpulkan** → Tim engineering dan legal tidak sync. Engineering kumpulkan data untuk "analytics" tapi tidak ada dalam privacy notice. Ini violasi UU PDP.

5. **Tidak ada prosedur breach notification** → Saat breach terjadi, tim tidak tahu harus lapor ke siapa, kapan, dan format apa. Siapkan incident response plan termasuk breach notification procedure SEBELUM breach terjadi.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Sebutkan perbedaan antara "data pribadi umum" dan "data pribadi sensitif" menurut UU PDP. Berikan 3 contoh masing-masing dari konteks aplikasi fintech.

b) Apa yang dimaksud dengan "Right to be Forgotten"? Apakah perusahaan harus menghapus semua data jika user meminta? Jelaskan kondisi pengecualiannya.

c) Sebuah startup fintech ingin menggunakan data transaksi pengguna untuk melatih model machine learning untuk rekomendasi produk. Langkah apa yang harus dilakukan agar comply dengan UU PDP?

### Soal 2 — Analisis Regulasi

Pilih satu aplikasi digital Indonesia (fintech/e-commerce/kesehatan). Lakukan analisis kepatuhan UU PDP:

1. Identifikasi jenis data pribadi yang dikumpulkan (umum vs sensitif)
2. Evaluasi apakah privacy policy memenuhi syarat transparansi UU PDP (tersedia, bahasa Indonesia, spesifik)
3. Cek apakah ada fitur untuk hak subjek data (akses, koreksi, hapus, ekspor)
4. Identifikasi 3 potensi pelanggaran
5. Berikan rekomendasi perbaikan

---

## 📌 Ringkasan

* **UU PDP No. 27/2022:** berlaku Oktober 2024, wajib untuk semua entitas yang proses data warga Indonesia
* **Data pribadi umum:** nama, email, dll. **Data sensitif:** NIK, biometrik, finansial, kesehatan — perlu explicit consent
* **8 hak subjek data:** akses, koreksi, hapus (right to be forgotten), portabilitas, tarik consent, keberatan
* **Lawful basis:** harus ada sebelum memproses data (consent, kontrak, kewajiban hukum, dll)
* **Breach notification:** 14 hari kerja ke BSSN + notify subjek yang terdampak
* **UU PDP vs GDPR:** mirip dalam prinsip, berbeda di sanksi (UU PDP: pidana penjara, GDPR tidak)
* **Privacy by Design:** privasi diintegrasikan sejak desain sistem, bukan afterthought
* Praktis: data minimization, pseudonymization, granular consent, retention policy, DPO

---

*📚 Referensi: UU No. 27/2022 tentang Perlindungan Data Pribadi | GDPR Regulation (EU) 2016/679 | Kominfo — Panduan UU PDP | IAPP — Privacy by Design | Schneier, B. (2015). Secrets and Lies*
