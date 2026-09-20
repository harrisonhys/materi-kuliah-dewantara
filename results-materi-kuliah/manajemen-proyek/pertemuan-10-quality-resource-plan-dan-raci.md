# Pertemuan 10: Quality Resource Plan dan RACI

> **RPS:** CPMK-4, Sub-CPMK-9 • **Durasi:** 150 menit • **Bobot:** 5%

## 1. 🎯 Learning Outcomes

Kamu mampu mendefinisikan quality criteria/metric, membedakan QA dan QC, menentukan acceptance/Definition of Done, merencanakan human/nonhuman resource, membangun RACI, dan menangani capacity/skill gap.

## 2. 📖 Pengantar

“Sistem berkualitas” tidak dapat diuji. Kualitas harus diterjemahkan menjadi kriteria seperti availability, response time, defect severity, accessibility, security, data accuracy, dan acceptance.

## 3. 🧩 Konsep Utama

- **Quality planning:** standar, metric, acceptance, process.
- **QA:** memastikan proses mampu menghasilkan kualitas.
- **QC:** memeriksa deliverable/result.
- **Cost of quality:** prevention, appraisal, failure internal/external.

Resource plan mencakup role, competency, quantity, availability, calendar, acquisition, onboarding, development, release, equipment/tool.

RACI:

- **R:** melakukan pekerjaan;
- **A:** satu pihak accountable pada hasil;
- **C:** memberi input dua arah;
- **I:** menerima informasi.

RACI bukan jadwal dan tidak menggantikan job description atau governance detail.

## 4. 🧠 Analogi

Resep adalah quality plan, pelatihan koki adalah QA, mencicipi makanan adalah QC, dan RACI memastikan siapa memasak, menyetujui, dimintai saran, serta diberi tahu.

## 5. 💻 Contoh Quality Metric

| Deliverable | Metric/acceptance | Method | Owner |
|---|---|---|---|
| Payment API | p95 <500ms; error <0.5%; security tests pass | load/security test | QA lead |
| Training | ≥85% user lulus scenario | observation/test | Change lead |

## 6. 🏦 Studi Kasus Industri

Tim mengukur kualitas hanya dari jumlah bug. Sistem transaksi lolos functional test tetapi audit log tidak lengkap. **Solusi:** quality plan mencakup integrity, security, auditability, recovery, performance, accessibility, dan operational readiness; acceptance owner jelas.

## 7. 📊 Aktivitas 150 Menit

25 menit quality; 20 menit QA/QC; 25 menit resource/capacity; 20 menit RACI; 45 menit workshop; 15 menit review.

## 8. ⚠️ Kesalahan Umum

- Metric tidak punya threshold.
- QA disamakan testing.
- Banyak A pada satu activity.
- Semua orang dibuat C/I.
- Capacity 100% diasumsikan tersedia.
- Skill/training/transition diabaikan.

## 9. 🧪 Latihan dan Penilaian

Buat Quality Plan untuk 5 deliverable, resource histogram sederhana, skill gap action, dan RACI minimal 12 aktivitas. Rubrik: criteria 30%, QA/QC 20%, resource realism 25%, RACI 15%, konsistensi 10%.

## 10. 📌 Ringkasan

- Quality harus terukur dan disepakati.
- QA memperbaiki proses; QC memeriksa hasil.
- Resource lebih dari jumlah orang.
- RACI memperjelas tanggung jawab.
- Acceptance terkait stakeholder dan value.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Acceptance criteria | Kondisi deliverable diterima |
| QA | Assurance terhadap proses kualitas |
| QC | Control/pemeriksaan hasil |
| RACI | Responsible, Accountable, Consulted, Informed |

## 12. Referensi

- PMI, **Project Management Basics**, <https://www.pmi.org/certifications/~/link.aspx?_id=FF99743C71A271367017995CAEBDD95A&_z=z>
- PMI, **PMBOK Guide**, <https://www.pmi.org/standards/pmbok>

