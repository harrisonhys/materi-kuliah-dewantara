# Pertemuan 7: Cost Estimation dan Budget Baseline

> **RPS:** CPMK-3, Sub-CPMK-7 • **Durasi:** 150 menit • **Bobot:** 7%

## 1. 🎯 Learning Outcomes

Kamu mampu mengidentifikasi kategori biaya, memakai analogous/parametric/bottom-up/three-point estimate, mendokumentasikan basis asumsi, mengagregasi cost ke budget, dan membedakan contingency serta management reserve.

## 2. 📖 Pengantar

Biaya proyek TI bukan hanya gaji developer. Cloud, license, security test, migrasi, vendor, training, support transisi, pajak, dan risiko dapat menjadi komponen besar.

## 3. 🧩 Konsep Utama

### Metode

- **Analogous:** proyek serupa; cepat, kasar.
- **Parametric:** unit × rate.
- **Bottom-up:** estimate work package; rinci, mahal.
- **Three-point:** optimistic, most likely, pessimistic.

```text
Expected (PERT) = (O + 4M + P) / 6
```

Cost estimate + contingency reserve untuk known-unknowns membentuk cost baseline. Management reserve menangani unknown-unknowns dan biasanya di luar baseline namun bagian total funding.

### Total Cost of Ownership

Pertimbangkan build, operate, maintain, decommission. Proyek cloud murah di awal dapat mahal secara berulang.

## 4. 🧠 Analogi

Budget seperti rencana perjalanan: tiket bukan seluruh biaya; ada hotel, transportasi, asuransi, perubahan harga, dan dana darurat.

## 5. 💻 Template Budget

| WBS | Item | Qty×Rate | Estimate | Basis | Reserve |
|---|---|---:|---:|---|---:|
| 1.3 | Dev backend | 40d×1.5jt | 60jt | bottom-up | 10% |
| 1.4 | Pentest | fixed | 35jt | quote vendor | 15% |

Tambahkan currency, tax, inflation/rate assumption, accrual timing, dan approval threshold.

## 6. 🏦 Studi Kasus Industri

Startup hanya menghitung biaya pembangunan fraud engine, tidak menghitung data provider dan monitoring tahunan. **Dampak:** TCO melebihi manfaat. **Solusi:** bandingkan build/buy, recurring cost, volume-based pricing, exit cost, reserve, dan benefit assumption.

## 7. 📊 Aktivitas 150 Menit

20 menit kategori; 25 menit metode; 20 menit reserve; 20 menit TCO; 50 menit workshop budget; 15 menit challenge review.

## 8. ⚠️ Kesalahan Umum

- Angka tunggal tanpa basis.
- Cost tidak traceable ke WBS.
- Recurring/hidden cost hilang.
- Contingency dipakai menutup scope tambahan.
- Double counting reserve.
- Optimism bias tidak dibahas.

## 9. 🧪 Latihan dan Penilaian

Buat estimate per work package, basis, range, contingency, cash-flow ringkas, dan total funding. Rubrik: cakupan 25%, metode 25%, traceability 20%, reserve/TCO 20%, dokumentasi 10%.

## 10. 📌 Ringkasan

- Cost estimate perlu basis/asumsi.
- Metode dipilih sesuai detail/data.
- Budget terhubung ke WBS/schedule.
- Reserve memiliki tujuan dan governance.
- TCO melihat biaya setelah delivery.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Cost baseline | Budget time-phased yang disetujui |
| Contingency reserve | Cadangan known-unknowns |
| Management reserve | Cadangan unknown-unknowns |
| TCO | Total Cost of Ownership |

## 12. Referensi

- PMI, **Standards**, <https://www.pmi.org/standards>
- PMI, **PMBOK Guide**, <https://www.pmi.org/standards/pmbok>

