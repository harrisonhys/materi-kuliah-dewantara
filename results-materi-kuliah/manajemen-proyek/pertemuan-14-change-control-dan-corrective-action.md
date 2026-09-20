# Pertemuan 14: Change Control dan Corrective Action

> **RPS:** CPMK-5, Sub-CPMK-13 • **Durasi:** 150 menit • **Bobot:** 8%

## 1. 🎯 Learning Outcomes

Kamu mampu membedakan issue/corrective/preventive/change, menyusun Change Request, menganalisis dampak terintegrasi, memfasilitasi approval, memperbarui baseline/log, dan mengomunikasikan keputusan.

## 2. 📖 Pengantar

Perubahan bukan musuh; perubahan tak terkendali adalah masalah. Permintaan “tambahkan login biometrik, kecil kok” dapat memengaruhi security, device support, test, schedule, cost, dan compliance.

## 3. 🧩 Konsep Utama

```text
Request → log/screen → clarify → impact analysis
→ options/recommendation → approve/reject/defer
→ update plans/baselines → implement → verify/close
```

Change Request: requester, reason/value, description, urgency, affected requirement/deliverable, options, impact scope/schedule/cost/quality/resource/risk/benefit, recommendation, approver, decision.

### Tindakan

- **Corrective:** mengembalikan performa ke rencana.
- **Preventive:** mengurangi peluang deviasi masa depan.
- **Defect repair:** memperbaiki deliverable tidak sesuai.

Change Control Board/authority disesuaikan threshold; perubahan kecil tidak perlu birokrasi sama dengan perubahan strategis.

Dalam adaptive delivery, backlog reprioritization menangani banyak perubahan scope, tetapi Product Goal, Sprint Goal, Definition of Done, budget/governance, dan risiko tetap perlu kontrol.

## 4. 🧠 Analogi

Change control seperti perubahan rute penerbangan: masukan boleh datang, tetapi pilot/otoritas menilai cuaca, bahan bakar, waktu, dan keselamatan sebelum memutuskan.

## 5. 💻 Contoh Impact Summary

| Dimensi | Dampak biometrik |
|---|---|
| Scope | device enrollment, fallback, settings |
| Schedule | +2 sprint + security review |
| Cost | device lab/vendor SDK |
| Quality | accessibility, recovery |
| Risk | account takeover, lockout |
| Benefit | faster login, possible adoption |

## 6. 🏦 Studi Kasus Industri

Regulator mewajibkan verifikasi tambahan sebelum go-live. Ini bukan “scope creep” biasa. **Solusi:** emergency analysis, legal/compliance authority, options (delay/phased launch), rebaseline terkontrol, evidence keputusan, dan komunikasi stakeholder.

## 7. 📊 Aktivitas 150 Menit

20 menit taxonomy; 25 menit workflow; 25 menit impact; 20 menit governance; 45 menit simulation CCB; 15 menit update artifacts.

## 8. ⚠️ Kesalahan Umum

- Perubahan dilakukan lewat chat tanpa log.
- Hanya menghitung coding effort.
- Baseline diubah sebelum approval.
- Semua request disebut scope creep.
- Reject tanpa opsi/trade-off.
- Keputusan tidak dikomunikasikan.

## 9. 🧪 Latihan dan Penilaian

Proses tiga request berbeda, buat impact analysis/options/decision, update baseline dan logs untuk yang disetujui. Rubrik: completeness 25%, integrated impact 30%, recommendation 20%, governance 15%, update 10%.

## 10. 📌 Ringkasan

- Perubahan harus terlihat dan dinilai.
- Impact selalu lintas domain.
- Authority/threshold perlu jelas.
- Approval diikuti update artifact.
- Adaptive bukan berarti tanpa control.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| CCB | Change Control Board |
| Change request | Usulan perubahan terdokumentasi |
| Corrective action | Tindakan memperbaiki deviasi |
| Rebaseline | Menetapkan baseline baru setelah approval |

## 12. Referensi

- PMI, **PMBOK Guide**, <https://www.pmi.org/standards/pmbok>
- Scrum Guide 2020, <https://scrumguides.org/download.html>

