# Pertemuan 6: SEO Audit dan Prioritas Impact Effort

> **RPS:** CPMK-3/4, Sub-CPMK-6 • **Durasi:** 150 menit • **Bobot:** 7%

## 1. 🎯 Learning Outcomes

Kamu mampu menjalankan audit berbasis sample dan tool, memisahkan observation/impact/recommendation, mengelompokkan on-page/technical/content/off-page issue, memberi severity, serta memprioritaskan perbaikan berdasarkan impact, effort, confidence, dan dependency.

## 2. 📖 Pengantar

Crawler dapat menghasilkan ribuan warning. Audit bukan mengekspor tool lalu menyebut semuanya masalah. Auditor harus membuktikan dampak, menemukan pola/template penyebab, dan memberi tindakan yang realistis.

## 3. 🧩 Konsep Utama

### Alur Audit

```text
Scope → Baseline → Crawl/sample → Validate → Diagnose root cause
→ Prioritize → Recommend → Assign owner → Verify
```

Format temuan:

```text
[HIGH] Halaman kategori memiliki canonical ke homepage
Evidence: 42 URL kategori; sample A/B; HTML source
Impact: URL penting berisiko tidak dipilih sebagai canonical
Root cause: template category
Fix: self-referencing canonical pada URL 200 indexable
Owner: engineering
Validation: recrawl + URL Inspection
```

### Severity vs Priority

Severity menggambarkan dampak masalah; priority juga memasukkan effort, confidence, dependency, dan tujuan bisnis. Gunakan quick wins secara hati-hati—jangan menunda blocker hanya karena sulit.

### Sampling

Kelompokkan template: homepage, category, product, article, pagination, filter. Uji sample per template dan verifikasi temuan tool secara manual.

## 4. 🧠 Analogi

Audit SEO seperti pemeriksaan kesehatan: hasil alat adalah indikator, bukan diagnosis final. Dokter menghubungkan gejala, konteks, dan risiko sebelum memberi terapi.

## 5. 💻 Contoh Matriks

| Issue | Evidence | Impact | Effort | Confidence | Priority |
|---|---|---:|---:|---:|---|
| noindex kategori | source + inspection | 5 | 2 | 5 | P0 |
| meta description duplikat | crawl | 2 | 2 | 4 | P2 |
| LCP hero besar | PSI + waterfall | 4 | 3 | 4 | P1 |

Simpan konfigurasi crawl, waktu, scope, user-agent, dan batasan akses agar audit dapat direproduksi.

## 6. 🏦 Studi Kasus Industri

E-commerce menerima laporan “10.000 title duplikat”. Ternyata 9.500 berasal dari faceted URL yang seharusnya tidak menjadi landing organik. **Solusi:** perbaiki desain crawl/index faceted navigation terlebih dahulu, lalu optimalkan title pada URL canonical bernilai. Mengedit 10.000 title bukan root-cause fix.

## 7. 📊 Aktivitas 150 Menit

20 menit scope; 25 menit crawl/sample; 25 menit validasi; 25 menit format findings; 40 menit audit proyek; 15 menit triage.

## 8. ⚠️ Kesalahan Umum

- Menyalin semua warning tool.
- Tidak menyimpan evidence URL.
- Severity tinggi tanpa dampak bisnis.
- Rekomendasi generik tanpa owner/validation.
- Mengubah production tanpa backup/approval.
- Tidak menyatakan keterbatasan data.

## 9. 🧪 Latihan dan Penilaian

Buat audit minimal 15 temuan tervalidasi, kelompokkan, prioritaskan, dan pilih 5 rekomendasi utama. Rubrik: evidence 30%, diagnosis 25%, prioritas 20%, actionability 15%, dokumentasi 10%.

## 10. 📌 Ringkasan

- Audit adalah diagnosis, bukan export tool.
- Validasi manual dan root cause wajib.
- Severity berbeda dari priority.
- Temuan harus punya evidence, owner, fix, dan validation.
- Scope dan limitation harus eksplisit.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Evidence | Bukti yang mendukung temuan |
| Impact-effort | Kerangka prioritas manfaat dan biaya |
| Root cause | Penyebab mendasar pola masalah |
| Severity | Tingkat dampak masalah |

## 12. Referensi

- Google, **Search Essentials**, <https://developers.google.com/search/docs/essentials>
- Google, **Search Console URL Inspection**, <https://support.google.com/webmasters/answer/9012289>

