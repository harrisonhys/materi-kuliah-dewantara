# Pertemuan 14: Risk Assessment & Kebijakan Keamanan Data

---

## 🎯 Learning Outcomes

Setelah pertemuan ini, kamu akan bisa:

* Mengidentifikasi aset informasi dan mengklasifikasikan sensitivitasnya
* Melakukan threat modeling menggunakan STRIDE methodology
* Membuat matriks risiko (Likelihood × Impact) dan menentukan risk treatment
* Menyusun kebijakan keamanan data sederhana untuk skenario organisasi

---

## 📖 Pengantar (Hook)

"Bagaimana kita tahu apa yang perlu dilindungi? Bagaimana kita tahu serangan apa yang mungkin datang?"

Itulah pertanyaan yang dijawab oleh Risk Assessment — proses sistematis untuk mengidentifikasi, menganalisis, dan memprioritaskan risiko keamanan sebelum insiden terjadi.

Perusahaan yang tidak melakukan risk assessment yang baik biasanya baru menyadari celah keamanannya setelah diserang. Terlambat.

Perusahaan yang melakukannya dengan baik bisa tidur nyenyak — bukan karena tidak ada risiko, tapi karena risiko sudah diketahui, diprioritaskan, dan dikelola.

---

## 🧩 Konsep Utama

### Step 1: Asset Identification & Classification

Sebelum bisa melindungi sesuatu, kamu harus tahu APA yang perlu dilindungi.

```
JENIS ASET INFORMASI:

ASET DATA:
  Critical: Database transaksi, data KYC/NIK, kunci enkripsi, credentials admin
  High:     Database user, data analitik, kode sumber aplikasi
  Medium:   Log server, data konfigurasi, dokumentasi internal
  Low:      Marketing material, FAQ publik, press release

ASET SISTEM:
  Critical: Database production, payment processing server, HSM (key management)
  High:     Application servers, API gateways, authentication services
  Medium:   Monitoring systems, staging environment
  Low:      Development tools, internal wiki

ASET MANUSIA:
  Critical: Admin database, engineer keamanan
  High:     Backend developer, DevOps
  Medium:   Customer service dengan akses data
  Low:      Karyawan tanpa akses sistem produksi
```

### Step 2: Threat Modeling dengan STRIDE

STRIDE adalah framework dari Microsoft untuk mengidentifikasi ancaman secara sistematis.

```
S - SPOOFING (Pemalsuan Identitas)
    Ancaman: Penyerang berpura-pura jadi orang lain
    Contoh: Credential stuffing, phishing, SIM swap
    Kontrol: MFA, email verification, rate limiting

T - TAMPERING (Manipulasi Data)
    Ancaman: Modifikasi data tanpa otorisasi
    Contoh: Mengubah nominal transfer di request, SQL injection
    Kontrol: Digital signature, HMAC, input validation, prepared statements

R - REPUDIATION (Penyangkalan)
    Ancaman: Pengguna menyangkal aksi yang sudah dilakukan
    Contoh: "Saya tidak pernah melakukan transfer itu"
    Kontrol: Audit log, digital signature, non-repudiation

I - INFORMATION DISCLOSURE (Kebocoran Informasi)
    Ancaman: Data sensitif terekspos ke pihak tidak berwenang
    Contoh: Data breach, verbose error message, misconfigured S3 bucket
    Kontrol: Enkripsi, access control, error handling yang aman

D - DENIAL OF SERVICE (Penolakan Layanan)
    Ancaman: Sistem tidak bisa digunakan oleh pengguna sah
    Contoh: DDoS, resource exhaustion, flood attack
    Kontrol: Rate limiting, CDN, auto-scaling, WAF

E - ELEVATION OF PRIVILEGE (Eskalasi Privilege)
    Ancaman: Pengguna mendapat akses lebih dari yang seharusnya
    Contoh: SQL injection yang dapat akses root, IDOR
    Kontrol: Least privilege, role validation, authorization checks
```

**Contoh STRIDE untuk Sistem Transfer DANA:**

```
KOMPONEN: API endpoint POST /api/transfer

S (Spoofing):
  Ancaman: Hacker buat request transfer dengan berpura-pura jadi user lain
  Kontrol: JWT token validation, session binding, MFA untuk transfer besar

T (Tampering):
  Ancaman: MITM mengubah nominal transfer Rp 100.000 → Rp 1.000.000
  Kontrol: HTTPS/TLS, request signing (HMAC), idempotency key

R (Repudiation):
  Ancaman: User klaim "saya tidak transfer itu"
  Kontrol: Digital signature pada setiap instruksi, audit log lengkap

I (Information Disclosure):
  Ancaman: Error response mengekspos info rekening penerima yang tidak valid
  Kontrol: Generic error message, tidak ekspos internal info

D (Denial of Service):
  Ancaman: Bot flood /api/transfer → server down saat peak payment
  Kontrol: Rate limiting (max 10 transfer/menit), circuit breaker, queue

E (Elevation of Privilege):
  Ancaman: User biasa akses API admin untuk approve sendiri transfer-nya
  Kontrol: Role-based authorization, separate admin endpoints
```

### Step 3: Risk Matrix — Likelihood × Impact

```
RISK MATRIX 5×5:

                 IMPACT
              Insignificant  Minor  Moderate  Major  Catastrophic
              (1)           (2)    (3)       (4)    (5)
LIKELIHOOD   ┌────────────┬──────┬──────────┬──────┬────────────┐
Almost       │     5      │  10  │    15    │  20  │    25      │
Certain (5)  │  MEDIUM    │ HIGH │   HIGH   │CRIT  │    CRIT    │
             ├────────────┼──────┼──────────┼──────┼────────────┤
Likely (4)   │     4      │   8  │    12    │  16  │    20      │
             │   LOW      │ MED  │   HIGH   │ HIGH │    CRIT    │
             ├────────────┼──────┼──────────┼──────┼────────────┤
Possible (3) │     3      │   6  │     9    │  12  │    15      │
             │   LOW      │ MED  │   MEDIUM │ HIGH │    HIGH    │
             ├────────────┼──────┼──────────┼──────┼────────────┤
Unlikely (2) │     2      │   4  │     6    │   8  │    10      │
             │   LOW      │ LOW  │   MEDIUM │ MED  │    HIGH    │
             ├────────────┼──────┼──────────┼──────┼────────────┤
Rare (1)     │     1      │   2  │     3    │   4  │     5      │
             │   LOW      │ LOW  │   LOW    │ LOW  │    MEDIUM  │
             └────────────┴──────┴──────────┴──────┴────────────┘

Contoh Risk Assessment Fintech:
┌──────────────────────────────────────────────────────────────────┐
│ Risiko              │ Likelihood │ Impact │ Score │ Level       │
├──────────────────────────────────────────────────────────────────┤
│ Credential stuffing │ 5 (Almost) │ 4 (Maj)│  20   │ CRITICAL   │
│ SQL Injection       │ 3 (Possible│ 5 (Cat)│  15   │ HIGH       │
│ DDoS saat Harbolnas │ 4 (Likely) │ 4 (Maj)│  16   │ HIGH       │
│ Insider data theft  │ 2 (Unlikely│ 5 (Cat)│  10   │ HIGH       │
│ Phishing admin      │ 3 (Possible│ 4 (Maj)│  12   │ HIGH       │
│ Ransomware database │ 2 (Unlikely│ 5 (Cat)│  10   │ HIGH       │
│ Social engineering  │ 3 (Possible│ 3 (Mod)│   9   │ MEDIUM     │
│ Physical server theft│ 1 (Rare)  │ 5 (Cat)│   5   │ MEDIUM     │
└──────────────────────────────────────────────────────────────────┘
```

### Step 4: Risk Treatment Options

```
4 PILIHAN RISK TREATMENT:

1. ACCEPT (Terima)
   → Risiko rendah, cost mitigasi lebih besar dari dampak
   → Contoh: Risiko typo di dokumentasi internal
   → Action: Dokumentasikan keputusan, review berkala

2. MITIGATE (Kurangi)
   → Terapkan kontrol untuk mengurangi likelihood atau impact
   → Contoh: Credential stuffing → terapkan MFA + rate limiting
   → Cost: Implementasi kontrol keamanan

3. TRANSFER (Alihkan)
   → Pindahkan risiko ke pihak lain (asuransi, third-party)
   → Contoh: Risiko payment fraud → transfer ke payment gateway
   → Contoh: Risiko infrastruktur → cloud provider (SLA)

4. AVOID (Hindari)
   → Hentikan aktivitas yang menimbulkan risiko
   → Contoh: Tidak menyimpan data kartu kredit → hindari risiko PCI-DSS
   → Contoh: Tidak operasi di negara dengan regulasi ekstrem

RISK TREATMENT PLAN (Contoh):
┌─────────────────────────────────────────────────────────────────┐
│ Risiko: Credential Stuffing (Score: 20, CRITICAL)              │
│                                                                 │
│ Current Controls: Password + rate limiting dasar               │
│                                                                 │
│ Treatment: MITIGATE                                             │
│ New Controls:                                                   │
│   1. Wajibkan 2FA untuk semua akun (TOTP, bukan SMS)          │
│   2. Anomaly detection: alert jika >5 gagal dari IP berbeda   │
│   3. CAPTCHA untuk login setelah 3 gagal                       │
│   4. Check credential breach database (HaveIBeenPwned API)     │
│                                                                 │
│ Expected Likelihood setelah mitigasi: 5 → 2 (Unlikely)        │
│ Expected Risk Score: 20 → 8 (MEDIUM)                           │
│                                                                 │
│ Owner: Security Team                                            │
│ Deadline: Q1 2025                                               │
│ Cost: 2 developer-sprints + $200/bulan HaveIBeenPwned          │
└─────────────────────────────────────────────────────────────────┘
```

### Incident Response Plan

```
INCIDENT RESPONSE LIFECYCLE (NIST Framework):

1. PREPARATION:
   • Siapkan runbook untuk setiap jenis insiden
   • Tentukan siapa yang on-call dan contact tree
   • Setup monitoring dan alerting
   • Regular drill/simulasi

2. DETECTION & ANALYSIS:
   • Sistem monitoring alert: anomalous queries, failed logins
   • Triage: severity level (P1 critical, P2 high, P3 medium, P4 low)
   • Identifikasi scope: berapa user terdampak?

3. CONTAINMENT:
   • Isolasi sistem yang terkompromi
   • Block IP/user yang mencurigakan
   • Preserve evidence (jangan langsung hapus log!)
   • Short-term vs Long-term containment

4. ERADICATION:
   • Temukan dan hapus root cause
   • Patch vulnerability
   • Rotate credentials yang terkompromi

5. RECOVERY:
   • Restore dari backup yang bersih
   • Monitor untuk memastikan tidak ada persistence
   • Gradual restore ke operasi normal

6. POST-INCIDENT REVIEW:
   • Timeline lengkap kejadian
   • Root cause analysis
   • Lessons learned
   • Update runbook dan controls

TEMPLATE INCIDENT SEVERITY:
┌────────────────────────────────────────────────────────────────┐
│ P1 (CRITICAL): Response dalam 15 menit                         │
│   • Data breach > 1000 user                                     │
│   • Production system down total                               │
│   • Active exploitation sedang terjadi                         │
│   Notifikasi: Seluruh tim engineering + C-level + Legal        │
│                                                                │
│ P2 (HIGH): Response dalam 2 jam                                │
│   • Data breach < 1000 user                                    │
│   • Service degradation signifikan                             │
│   • Vulnerability kritis ditemukan                             │
│                                                                │
│ P3 (MEDIUM): Response dalam 24 jam                             │
│   • Attempted attack yang berhasil diblok                      │
│   • Vulnerability medium ditemukan                             │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Ilustrasi / Analogi

**Risk Matrix seperti triage UGD rumah sakit:**
* Pasien datang dengan berbagai kondisi: darurat, perlu perhatian, ringan
* Tidak semua bisa ditangani sekaligus → prioritaskan berdasarkan urgency × severity
* Critical → tangani sekarang. Low → tunggu giliran.

**STRIDE seperti daftar pertanyaan security reviewer:**
* Sebelum approve setiap fitur: "Apakah mungkin di-spoof? Di-tamper? Disangkal?"
* Seperti checklist penerbangan sebelum take-off — tidak boleh skip

**Risk Treatment seperti asuransi mobil:**
* Accept: tidak diasuransikan (akibat tanggung sendiri)
* Mitigate: pasang alarm + kunci stir (kurangi kemungkinan kemalingan)
* Transfer: beli asuransi komprehensif (kalau kena, ditanggung asuransi)
* Avoid: tidak beli mobil (hilangkan risiko kemalingan mobil)

---

## 💻 Contoh Teknis

### Template Risk Register (Python)

```python
"""
Risk Register sederhana untuk dokumentasi dan tracking risiko keamanan
"""
from dataclasses import dataclass
from enum import Enum
from datetime import date
from typing import List, Optional

class RiskLevel(Enum):
    LOW = "LOW"
    MEDIUM = "MEDIUM"
    HIGH = "HIGH"
    CRITICAL = "CRITICAL"

class TreatmentType(Enum):
    ACCEPT = "ACCEPT"
    MITIGATE = "MITIGATE"
    TRANSFER = "TRANSFER"
    AVOID = "AVOID"

@dataclass
class Risk:
    id: str
    title: str
    description: str
    threat_category: str  # STRIDE category
    affected_asset: str
    likelihood: int       # 1-5
    impact: int           # 1-5
    current_controls: List[str]
    treatment: TreatmentType
    planned_controls: List[str]
    residual_likelihood: int  # Setelah kontrol baru
    residual_impact: int
    owner: str
    target_date: date
    status: str           # "OPEN", "IN_PROGRESS", "CLOSED"
    notes: Optional[str] = None
    
    @property
    def risk_score(self) -> int:
        return self.likelihood * self.impact
    
    @property
    def risk_level(self) -> RiskLevel:
        score = self.risk_score
        if score >= 15: return RiskLevel.CRITICAL
        elif score >= 10: return RiskLevel.HIGH
        elif score >= 5: return RiskLevel.MEDIUM
        else: return RiskLevel.LOW
    
    @property
    def residual_score(self) -> int:
        return self.residual_likelihood * self.residual_impact
    
    @property
    def residual_level(self) -> RiskLevel:
        score = self.residual_score
        if score >= 15: return RiskLevel.CRITICAL
        elif score >= 10: return RiskLevel.HIGH
        elif score >= 5: return RiskLevel.MEDIUM
        else: return RiskLevel.LOW


# Contoh risk register untuk sistem fintech
risk_register = [
    Risk(
        id="RISK-001",
        title="Credential Stuffing Attack",
        description="Bot menggunakan credential dari breach lain untuk login ke akun pengguna",
        threat_category="Spoofing",
        affected_asset="User Accounts, Saldo Pengguna",
        likelihood=5,
        impact=4,
        current_controls=["Rate limiting dasar (10 req/menit)"],
        treatment=TreatmentType.MITIGATE,
        planned_controls=[
            "Mandatory TOTP 2FA",
            "Anomaly detection untuk login patterns",
            "HaveIBeenPwned integration untuk check exposed passwords",
            "CAPTCHA setelah 3 gagal"
        ],
        residual_likelihood=2,
        residual_impact=4,
        owner="Security Team",
        target_date=date(2025, 3, 31),
        status="IN_PROGRESS"
    ),
    Risk(
        id="RISK-002",
        title="SQL Injection pada API Transfer",
        description="Input tidak tersanitasi di endpoint transfer memungkinkan injection",
        threat_category="Tampering",
        affected_asset="Database Transaksi",
        likelihood=3,
        impact=5,
        current_controls=["Input length validation"],
        treatment=TreatmentType.MITIGATE,
        planned_controls=[
            "Code review semua SQL queries",
            "Implementasi prepared statements 100%",
            "WAF rule untuk SQL injection patterns",
            "SAST scanning di CI/CD pipeline"
        ],
        residual_likelihood=1,
        residual_impact=5,
        owner="Backend Team",
        target_date=date(2025, 1, 31),
        status="OPEN"
    )
]

# Print risk register report
def print_risk_report(risks: List[Risk]):
    print(f"\n{'='*70}")
    print(f"RISK REGISTER — {date.today()}")
    print(f"{'='*70}")
    
    for risk in sorted(risks, key=lambda r: r.risk_score, reverse=True):
        print(f"\n[{risk.id}] {risk.title}")
        print(f"  Category: {risk.threat_category} | Asset: {risk.affected_asset}")
        print(f"  Current Risk: {risk.likelihood}×{risk.impact}={risk.risk_score} [{risk.risk_level.value}]")
        print(f"  Residual Risk: {risk.residual_likelihood}×{risk.residual_impact}={risk.residual_score} [{risk.residual_level.value}]")
        print(f"  Treatment: {risk.treatment.value} | Status: {risk.status}")
        print(f"  Owner: {risk.owner} | Target: {risk.target_date}")
        print(f"  Planned Controls:")
        for ctrl in risk.planned_controls:
            print(f"    • {ctrl}")

print_risk_report(risk_register)
```

---

## 🏢 Studi Kasus: Risk Assessment Xendit Sebelum Ekspansi ke Filipina

**Konteks:** Xendit ingin ekspansi ke Filipina. Sebelum launch, mereka perlu risk assessment baru karena:
* Regulasi berbeda (Bangko Sentral ng Pilipinas, bukan OJK)
* Threat landscape berbeda (lebih banyak fraud mobile)
* Data sovereignty: data warga Filipina harus di infrastruktur Filipina

**Risk Assessment highlights:**

```
TOP 5 RISKS BARU:

1. Regulasi compliance (CRITICAL):
   BSP (Bangko Sentral) membutuhkan data residency di Filipina
   → Treatment: Buat region AWS ap-southeast-1 (Singapore) tidak cukup
   → Action: Setup dedicated infrastruktur di Filipina

2. Cross-border fraud patterns (HIGH):
   Pola fraud berbeda: lebih banyak GCash account takeover
   → Treatment: Tuning fraud detection model untuk Filipina
   → Action: Partner dengan fraud intelligence provider lokal

3. New payment rails (HIGH):
   InstaPay (Filipina) berbeda dari RTGS/BI-FAST (Indonesia)
   → Treatment: Security review integrasi API baru
   → Action: Penetration testing sebelum go-live

4. Staff background check (MEDIUM):
   Karyawan lokal Filipina belum ter-screen sama standar Indonesia
   → Treatment: Background check sesuai standar BSP
   → Action: Update HR policy, training keamanan

5. Natural disaster BCP (MEDIUM):
   Filipina lebih rawan bencana alam (typhoon, earthquake)
   → Treatment: Enhanced BCP dengan RTO < 4 jam
   → Action: Multi-region setup + regular drill
```

---

## ⚠️ Kesalahan Umum

1. **Risk assessment satu kali, tidak di-update** → Threat landscape berubah setiap bulan. Risk register harus di-review minimal per kuartal. Risiko baru (zero-day, regulasi baru) harus ditambahkan.

2. **Risk treatment tanpa deadline dan owner** → "Kita akan mitigasi risiko SQL injection" tanpa owner dan deadline = tidak akan pernah selesai. Setiap risk treatment harus punya PIC dan tanggal target.

3. **Tidak mempertimbangkan human factor** → Risk assessment yang fokus hanya pada teknis dan mengabaikan risiko manusia (phishing karyawan, insider threat) tidak lengkap.

4. **Mengabaikan third-party risk** → Vendor, cloud provider, payment processor — semua memiliki risiko sendiri. Jika AWS down, sistem kamu down. Masukkan vendor ke risk register.

5. **Tidak ada incident response drill** → "Kita sudah punya incident response plan" tapi tidak pernah ditest = plan tidak berguna saat krisis nyata. Lakukan tabletop exercise dan drill minimal per tahun.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan STRIDE methodology dan berikan contoh untuk setiap kategori dari konteks aplikasi mobile banking.

b) Apa perbedaan antara "Residual Risk" dan "Inherent Risk"? Mengapa residual risk yang relevan untuk keputusan bisnis?

c) Sebuah startup memutuskan untuk "Accept" risiko tidak memiliki 2FA karena "user experience akan terganggu". Evaluasi keputusan ini.

### Soal 2 — Praktik (Workshop)

Lakukan threat modeling dan risk assessment untuk sistem e-commerce sederhana:

Sistem: Platform jual-beli dengan fitur: register/login, upload produk, checkout dengan kartu kredit (via Midtrans), chat antar user

1. Identifikasi 5 aset informasi paling kritis dan klasifikasikan sensitivitasnya
2. Untuk setiap aset, identifikasi ancaman menggunakan STRIDE
3. Buat risk matrix dengan minimal 8 risiko berbeda
4. Tentukan risk treatment untuk 3 risiko tertinggi dengan detail (kontrol apa, siapa owner, deadline)
5. Buat incident response plan untuk skenario "data breach pada tabel users"

---

## 📌 Ringkasan

* **Risk Assessment:** Identifikasi aset → Threat modeling → Hitung risiko → Tentukan treatment
* **Asset Classification:** Critical, High, Medium, Low — setiap level butuh proteksi berbeda
* **STRIDE:** Spoofing, Tampering, Repudiation, Information Disclosure, DoS, Elevation of Privilege
* **Risk Score = Likelihood × Impact** (1-5 masing-masing) → 5×5 = 25 max
* **Risk Treatment:** Accept / Mitigate / Transfer / Avoid — pilih berdasarkan cost-benefit
* Residual risk = risk setelah kontrol baru diterapkan — ini yang jadi target
* **Incident Response:** Preparation → Detection → Containment → Eradication → Recovery → Review
* Risk assessment harus di-update berkala, punya owner, dan di-test

---

*📚 Referensi: Anderson, R. (2020). Security Engineering, 3rd Ed. | NIST Cybersecurity Framework | Microsoft STRIDE | NIST SP 800-30 — Risk Assessment | ISO 27001 — Information Security Management*
