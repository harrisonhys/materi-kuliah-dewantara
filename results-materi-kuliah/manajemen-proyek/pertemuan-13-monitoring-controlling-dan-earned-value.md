# Pertemuan 13: Monitoring Controlling dan Earned Value

> **RPS:** CPMK-5, Sub-CPMK-12 • **Durasi:** 150 menit • **Bobot:** 7%

## 1. 🎯 Learning Outcomes

Kamu mampu mengumpulkan planned vs actual, menilai milestone/progress/quality/risk/issue, membedakan monitoring dan control, menghitung variance serta EVM dasar, memforecast, dan merekomendasikan corrective action.

## 2. 📖 Pengantar

“Proyek 80% selesai” tidak berguna tanpa definisi. Apakah 80% effort, scope diterima, biaya, atau pendapat tim? Monitoring membutuhkan baseline dan aturan pengukuran.

## 3. 🧩 Konsep Utama

Monitoring mengumpulkan/memahami kondisi; controlling memutuskan respons untuk menjaga tujuan/value.

### Earned Value Dasar

- **PV:** budgeted value pekerjaan yang direncanakan.
- **EV:** budgeted value pekerjaan yang benar-benar selesai.
- **AC:** biaya aktual.

```text
SV = EV - PV       SPI = EV / PV
CV = EV - AC       CPI = EV / AC
```

Nilai negatif SV/CV menunjukkan kondisi buruk pada konteks dasar; SPI/CPI <1 menunjukkan inefisiensi. EVM perlu scope/budget/progress measurement konsisten.

### Forecast

Contoh sederhana bila cost performance berlanjut:

```text
EAC = BAC / CPI
ETC = EAC - AC
VAC = BAC - EAC
```

Pilih formula forecast sesuai asumsi; dokumentasikan.

### Dashboard

Milestone, scope acceptance, schedule/cost variance, quality, top risk/issue, change, dependency, decisions, benefits leading indicators.

## 4. 🧠 Analogi

Monitoring seperti membaca dashboard mobil; controlling adalah memperlambat, mengisi bahan bakar, atau mengganti rute berdasarkan informasi itu.

## 5. 💻 Contoh

Jika PV=100, EV=80, AC=90 juta:

```text
SV=-20; SPI=0.80 (terlambat)
CV=-10; CPI=0.89 (over budget per value)
```

Jangan menyembunyikan kualitas buruk dengan mengklaim task “selesai”. Gunakan acceptance/Definition of Done.

## 6. 🏦 Studi Kasus Industri

Tim mobile melaporkan 90% complete, tetapi security test, store approval, training, dan migration rehearsal belum ada. **Solusi:** ukur progress berbasis deliverable accepted, tetapkan weight transparan, tampilkan confidence dan remaining risk.

## 7. 📊 Aktivitas 150 Menit

20 menit baseline/progress; 30 menit EVM; 20 menit forecast; 20 menit dashboard; 45 menit simulation; 15 menit corrective review.

## 8. ⚠️ Kesalahan Umum

- Persentase selesai subjektif.
- Baseline diubah agar terlihat hijau.
- EVM tanpa acceptance.
- Satu RAG menutupi detail.
- Forecast formula tanpa asumsi.
- Corrective action tanpa owner/due.

## 9. 🧪 Latihan dan Penilaian

Hitung PV/EV/AC, variance/index/forecast, analisis trend, update issue/risk log, dan tulis status serta corrective action. Rubrik: perhitungan 30%, interpretasi 30%, tindakan 25%, komunikasi 15%.

## 10. 📌 Ringkasan

- Monitoring memakai baseline dan actual.
- Control menghasilkan keputusan/tindakan.
- EV mengukur nilai pekerjaan selesai.
- Forecast selalu memiliki asumsi.
- Progress harus terkait acceptance/quality.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| AC | Actual Cost |
| EV | Earned Value |
| EAC | Estimate at Completion |
| PV | Planned Value |
| Variance | Selisih actual/earned terhadap baseline |

## 12. Referensi

- PMI, **PMBOK Guide**, <https://www.pmi.org/standards/pmbok>
- PMI, **Standards**, <https://www.pmi.org/standards>

