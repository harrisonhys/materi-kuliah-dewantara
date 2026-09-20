# Pertemuan 9: Risk Management dan Risk Register

> **RPS:** CPMK-4, Sub-CPMK-8 • **Durasi:** 150 menit • **Bobot:** 5%

## 1. 🎯 Learning Outcomes

Kamu mampu membedakan risk dan issue, mengidentifikasi cause–event–impact, menilai probability/impact, menentukan exposure/priority, memilih response untuk threat/opportunity, menetapkan owner, dan memonitor residual/secondary risk.

## 2. 📖 Pengantar

“Vendor terlambat” bukan risk statement lengkap. Apa penyebabnya, event apa yang mungkin terjadi, dan apa dampaknya? Bahasa jelas menghasilkan respons yang dapat dijalankan.

## 3. 🧩 Konsep Utama

```text
Because of <cause>, <uncertain event> may occur,
leading to <impact on objectives>.
```

Risk belum terjadi; issue sudah terjadi. Threat response: avoid, mitigate, transfer, accept, escalate. Opportunity: exploit, enhance, share, accept, escalate.

```text
Exposure sederhana = Probability × Impact
```

Skor membantu prioritas, tetapi tidak menggantikan judgment, proximity, urgency, detectability, dan interconnected risk.

Risk Register: ID, category, statement, trigger, probability, impacts, score, response, action, owner, due, residual/secondary risk, status.

## 4. 🧠 Analogi

Risk management seperti membawa payung karena melihat peluang hujan; issue management seperti menangani kebocoran setelah hujan masuk.

## 5. 💻 Contoh

| Risk | P | I | Response | Owner | Trigger |
|---|---:|---:|---|---|---|
| API vendor berubah menjelang UAT | 3 | 5 | mitigate: contract test+version pin | Tech lead | deprecation notice |

Contingency plan dijalankan jika trigger terjadi; fallback/reserve perlu governance.

## 6. 🏦 Studi Kasus Industri

Fintech mengintegrasikan penyedia e-KYC tunggal. **Risk:** outage/regulatory change. **Response:** SLA, monitoring, retry/circuit breaker, manual fallback, alternative provider feasibility, incident communication, dan risk owner. Risiko privasi tetap tidak boleh “diterima” tanpa otoritas tepat.

## 7. 📊 Aktivitas 150 Menit

20 menit konsep; 25 menit identification; 25 menit assessment; 25 menit responses; 40 menit simulation; 15 menit review.

## 8. ⚠️ Kesalahan Umum

- Risk ditulis sebagai kategori umum.
- Semua risk diberi score tinggi.
- Owner adalah “team”.
- Response tanpa action/due date.
- Issue tetap disimpan sebagai risk.
- Opportunity diabaikan.

## 9. 🧪 Latihan dan Penilaian

Buat register minimal 15 risk, matrix, top-5 responses, trigger, owner, residual risk, dan simulasi satu issue. Rubrik: statement 25%, assessment 20%, response 30%, ownership 15%, monitoring 10%.

## 10. 📌 Ringkasan

- Risk adalah ketidakpastian terhadap tujuan.
- Statement menghubungkan cause-event-impact.
- Prioritas memerlukan data dan judgment.
- Response harus punya owner/action.
- Risk register hidup sepanjang proyek.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Issue | Masalah yang sudah terjadi |
| Residual risk | Risiko tersisa setelah response |
| Risk appetite | Tingkat risiko yang bersedia diambil |
| Trigger | Tanda response/contingency dijalankan |

## 12. Referensi

- PMI, **PMBOK Guide**, <https://www.pmi.org/standards/pmbok>
- PMI Standards, <https://www.pmi.org/standards>

