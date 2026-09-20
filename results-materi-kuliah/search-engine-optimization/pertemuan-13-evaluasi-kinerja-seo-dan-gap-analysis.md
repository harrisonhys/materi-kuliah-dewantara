# Pertemuan 13: Evaluasi Kinerja SEO dan Gap Analysis

> **RPS:** CPMK-5, Sub-CPMK-11 • **Durasi:** 150 menit • **Bobot:** 5%

## 1. 🎯 Learning Outcomes

Kamu mampu membandingkan baseline dan hasil, mendiagnosis perubahan berdasarkan segmentasi, membedakan pola site-wide/template/page, mempertimbangkan seasonality dan faktor eksternal, serta menyusun hipotesis yang dapat diuji.

## 2. 📖 Pengantar

Traffic turun 30% tidak otomatis berarti penalti. Bisa terjadi perubahan demand, tracking, migrasi URL, kompetitor, SERP, indexation, seasonality, atau kualitas. Evaluasi yang baik mengurangi spekulasi dengan urutan diagnosis.

## 3. 🧩 Konsep Utama

### Kerangka Evaluasi

```text
Validasi data → tentukan waktu/pola → segmentasi → hubungkan perubahan
→ buat hipotesis → cari evidence → prioritaskan eksperimen/perbaikan
```

### Jenis Pola

- site-wide atau kelompok page;
- clicks vs impressions vs CTR vs position;
- branded vs non-branded;
- device/country/search appearance;
- query demand vs page performance;
- sudden vs gradual;
- seasonality vs abnormal.

### Before–After yang Adil

Bandingkan periode setara, beri anotasi deploy/campaign/update, periksa tracking, dan gunakan kelompok kontrol bila memungkinkan. Jangan memilih tanggal yang hanya mendukung cerita.

### Hipotesis

```text
Observasi: impressions kategori turun setelah 12 September.
Hipotesis: canonical template berubah ke homepage.
Evidence yang dicari: source, URL Inspection, affected templates.
Action: fix template; validate sample; monitor indexing/impressions.
```

## 4. 🧠 Analogi

Gap analysis seperti investigasi listrik padam: cek alat ukur, cakupan rumah/kompleks, waktu kejadian, perubahan terakhir, lalu uji penyebab—bukan langsung mengganti semua kabel.

## 5. 💻 Contoh Decision Table

| Pola | Dugaan awal | Pemeriksaan |
|---|---|---|
| Impressions turun, position stabil | demand/coverage | Trends, indexing, segment |
| Impressions stabil, CTR turun | snippet/SERP/intent | query-page, SERP compare |
| Position dan clicks turun template tertentu | quality/technical/competition | releases, crawl, content |
| GA turun, GSC stabil | analytics/site issue | tags, consent, landing UX |

## 6. 🏦 Studi Kasus Industri

Organic traffic layanan pajak turun setelah musim pelaporan. Tim ingin menulis 100 artikel baru. **Diagnosis:** demand musiman; landing inti tetap stabil. **Solusi:** bandingkan year-over-year, gunakan Trends, rencanakan kalender musim berikutnya, dan fokus pada gap evergreen yang benar-benar bernilai.

## 7. 📊 Aktivitas 150 Menit

20 menit framework; 25 menit data validation; 25 menit segmentation; 50 menit case diagnosis; 20 menit hypothesis review; 10 menit action plan.

## 8. ⚠️ Kesalahan Umum

- Menyebut semua penurunan sebagai penalti.
- Membandingkan minggu libur dan minggu normal.
- Mengabaikan tracking change.
- Menarik kesimpulan dari data agregat.
- Memilih metrik setelah melihat hasil.
- Membuat rekomendasi tanpa hipotesis.

## 9. 🧪 Latihan dan Penilaian

Analisis dataset before-after: validasi, segmentasikan, tulis minimal tiga hipotesis, evidence pendukung/penolak, dan tindakan. Rubrik: validasi 20%, segmentasi 25%, logika 30%, actionability 15%, komunikasi 10%.

## 10. 📌 Ringkasan

- Validasi data sebelum diagnosis.
- Segmentasi menentukan lokasi masalah.
- Seasonality dan tracking adalah confounder penting.
- Hipotesis harus dapat diuji.
- Insight berakhir pada tindakan dan monitoring.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Baseline | Kondisi pembanding awal |
| Confounder | Faktor lain yang memengaruhi hasil |
| Gap analysis | Analisis selisih kondisi aktual dan target |
| Hypothesis | Penjelasan sementara yang dapat diuji |
| Seasonality | Pola perubahan karena musim/waktu |

## 12. Referensi

- Google, **Debug traffic drops**, <https://developers.google.com/search/docs/monitor-debug/debugging-search-traffic-drops>
- Search Console, **Performance common tasks**, <https://support.google.com/webmasters/answer/17010961>

