# Pertemuan 12: SEO Analytics Search Console dan Google Analytics

> **RPS:** CPMK-5, Sub-CPMK-11 • **Durasi:** 150 menit • **Bobot:** 6%

## 1. 🎯 Learning Outcomes

Kamu mampu membaca clicks, impressions, CTR, average position, organic sessions, engagement, dan conversion; memilih dimension/filter; menghitung KPI; memahami perbedaan Search Console dan Analytics; serta membuat dashboard yang tidak menyesatkan.

## 2. 📖 Pengantar

Search Console mengatakan 1.000 klik, Analytics menunjukkan 850 sesi. Keduanya tidak harus sama: definisi, sumber data, atribusi, consent, canonical, dan pemrosesan berbeda. Analis mencari pola, bukan memaksa angka identik.

## 3. 🧩 Konsep Utama

### Sumber Kebenaran

- **Search Console:** performa pada Google Search—query, page, country, device, appearance.
- **Google Analytics:** perilaku setelah masuk situs—session, engagement, event, conversion.

Google menyebut Search Console sebagai sumber kebenaran search performance dan Analytics untuk perilaku di situs.

### Metrik

```text
CTR = clicks / impressions × 100%
Conversion rate = conversions / relevant sessions × 100%
```

Average position adalah rata-rata posisi teratas dan dipengaruhi variasi query/konteks. Fokuskan tren impressions/clicks dan segmentasi, bukan satu angka ranking.

### Dimension dan Segmentasi

Analisis query × page × device × country × date × search appearance. Selalu bandingkan periode setara dan pertimbangkan seasonality.

### Dashboard

Dashboard harus menjawab pertanyaan: apa berubah, di mana, mengapa mungkin terjadi, dampak bisnis, dan tindakan berikutnya. Hindari vanity dashboard penuh angka tanpa keputusan.

## 4. 🧠 Analogi

Search Console seperti laporan orang yang melihat dan masuk dari papan petunjuk kota. Analytics seperti CCTV dan kasir di dalam toko. Jumlahnya berbeda karena mengukur titik perjalanan berbeda.

## 5. 💻 Contoh Analisis

| Query/Page | Impr. | Click | CTR | Pos. | Organic sessions | Conv. |
|---|---:|---:|---:|---:|---:|---:|
| biaya qris | 10.000 | 300 | 3% | 5,2 | 270 | 12 |

Pertanyaan: CTR rendah karena snippet, intent mismatch, atau fitur SERP? Mengapa 300 klik menjadi 270 sesi? Apakah 12 conversion bernilai?

## 6. 🏦 Studi Kasus Industri

Fintech merayakan kenaikan 80% traffic artikel glossary, tetapi leads tidak berubah. **Diagnosis:** query sangat top-funnel dan CTA tidak relevan. **Solusi:** pertahankan konten berguna, tambahkan jalur edukasi kontekstual, pisahkan KPI awareness dari conversion, dan jangan memaksa CTA pinjaman pada pembaca yang hanya mencari definisi.

## 7. 📊 Aktivitas 150 Menit

25 menit metrik; 25 menit tool differences; 25 menit filter/segment; 50 menit dashboard; 15 menit insight writing; 10 menit peer review.

## 8. ⚠️ Kesalahan Umum

- Menyamakan clicks dan sessions.
- Menjumlah average position.
- Membandingkan periode berbeda panjang/season.
- Melihat aggregate tanpa segmentasi.
- Menganggap correlation sebagai causation.
- Menampilkan data query sensitif tanpa kontrol.

## 9. 🧪 Latihan dan Penilaian

Buat dashboard dari data anonim: minimal 4 KPI, 4 dimensions, perbandingan periode, 5 insight, 5 tindakan. Jelaskan discrepancy GSC/GA. Rubrik: akurasi 30%, segmentasi 20%, insight 25%, bisnis 15%, visual/etika 10%.

## 10. 📌 Ringkasan

- GSC dan GA mengukur tahap berbeda.
- CTR adalah clicks/impressions.
- Segmentasi mengungkap pola tersembunyi.
- Trend lebih berguna daripada snapshot.
- Dashboard harus mendorong keputusan.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Average position | Rata-rata posisi teratas pada impression |
| Click | Interaksi menuju situs dari hasil Search |
| Dimension | Cara mengelompokkan data |
| Impression | Kemunculan link/konten sesuai definisi report |
| Organic session | Kunjungan yang diatribusi ke organic search |

## 12. Referensi

- Search Console, **Performance report**, <https://support.google.com/webmasters/answer/7576553>
- Google, **Search Console and Analytics data**, <https://developers.google.com/search/docs/monitor-debug/google-analytics-search-console>

