# Pertemuan 5: Scope Statement WBS dan Work Package

> **RPS:** CPMK-3, Sub-CPMK-5 • **Durasi:** 150 menit • **Bobot:** 6%

## 1. 🎯 Learning Outcomes

Kamu mampu mendefinisikan product/project scope, menulis acceptance dan exclusion, membangun deliverable-oriented WBS mengikuti 100 percent rule, mendefinisikan work package, dan menjaga traceability.

## 2. 📖 Pengantar

“Buat sistem e-commerce” bukan scope yang dapat dikelola. Scope perlu dipecah menjadi deliverable yang dapat diestimasi, ditugaskan, diuji, dan diterima.

## 3. 🧩 Konsep Utama

Scope Statement memuat deskripsi produk/proyek, deliverables, acceptance criteria, exclusions, assumptions, dan constraints. WBS adalah dekomposisi hierarkis total scope proyek—bukan sekadar daftar aktivitas atau struktur organisasi.

```text
1.0 E-commerce Release
├─1.1 Discovery & Design
├─1.2 Customer Experience
│ ├─1.2.1 Catalog
│ └─1.2.2 Checkout
├─1.3 Backend & Integration
├─1.4 Migration
└─1.5 Deployment & Training
```

Work package adalah level yang cukup kecil untuk estimasi, assignment, tracking, dan control. WBS Dictionary menjelaskan owner, boundaries, acceptance, dependency, dan estimate basis.

**100 percent rule:** WBS mencakup seluruh scope yang disetujui, tanpa pekerjaan di luar scope dan tanpa duplikasi.

## 4. 🧠 Analogi

WBS seperti memecah bangunan menjadi fondasi, struktur, listrik, dan ruangan—berdasarkan hasil yang harus ada, bukan daftar gerakan tukang.

## 5. 💻 Template Work Package

```yaml
id: 1.2.2
name: Checkout
deliverable: checkout tervalidasi dan terintegrasi payment sandbox
acceptance: success/failure/idempotency scenarios lulus
owner: checkout team
dependencies: product catalog, payment API
estimate_basis: analogous + expert judgment
```

## 6. 🏦 Studi Kasus Industri

Tim fintech lupa memasukkan compliance review dan operational training ke WBS. Software selesai tetapi belum bisa dirilis. **Solusi:** WBS mencakup product dan project work, termasuk security, data migration, UAT, audit, deployment, training, dan transition.

## 7. 📊 Aktivitas 150 Menit

20 menit scope; 25 menit WBS; 20 menit work package; 60 menit workshop; 15 menit peer 100%-rule check; 10 menit revisi.

## 8. ⚠️ Kesalahan Umum

- WBS berupa timeline.
- Dekomposisi terlalu besar/kecil.
- Pekerjaan manajemen/test/training hilang.
- Deliverable tidak memiliki acceptance.
- Cabang overlap dan menghitung ganda.
- Scope creep dimasukkan diam-diam.

## 9. 🧪 Latihan dan Penilaian

Buat Scope Statement, WBS minimal 3 level, dan WBS Dictionary untuk 8 work package. Rubrik: scope 25%, dekomposisi 30%, acceptance 20%, traceability 15%, visual/konsistensi 10%.

## 10. 📌 Ringkasan

- Scope menjelaskan apa yang termasuk/tidak.
- WBS berorientasi deliverable.
- Work package mendukung estimasi/control.
- Acceptance membuat hasil dapat diverifikasi.
- Semua pekerjaan penting harus tercakup.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Scope baseline | Scope statement, WBS, dan dictionary yang disetujui |
| WBS | Dekomposisi hierarkis total scope |
| Work package | Unit WBS yang dapat dikelola |
| 100 percent rule | Seluruh scope tercakup tanpa duplikasi |

## 12. Referensi

- PMI, **PMBOK Guide**, <https://www.pmi.org/standards/pmbok>
- PMI Standards, <https://www.pmi.org/standards>

