# Pertemuan 11: Implementasi SEO dan Before After Evidence

> **RPS:** CPMK-3/4/6, Sub-CPMK-10 • **Durasi:** 150 menit • **Bobot:** 6%

## 1. 🎯 Learning Outcomes

Kamu mampu menerjemahkan rekomendasi menjadi change plan, mengimplementasikan perubahan terkontrol, mendokumentasikan before-after, memverifikasi deployment, mengelola risiko/rollback, dan membedakan output langsung dari outcome SEO yang tertunda.

## 2. 📖 Pengantar

Strategi terbaik tidak memberi hasil jika implementasi salah. Mengubah title, template canonical, atau redirect tanpa kontrol dapat memperbaiki satu halaman dan merusak ribuan lainnya.

## 3. 🧩 Konsep Utama

### Change Lifecycle

```text
Finding → ticket → acceptance criteria → backup/staging → implement
→ QA → deploy → verify → request recrawl bila perlu → monitor
```

### Evidence

Catat URL/template, tanggal, screenshot/source/header, nilai lama/baru, alasan, owner, commit/ticket, test, dan status. Untuk perubahan massal, simpan sample representatif dan hitungan cakupan.

### Output vs Outcome

- **Output:** title unik, redirect aktif, LCP lab membaik.
- **Leading indicator:** crawl/indexing/impressions berubah.
- **Outcome:** clicks, qualified organic traffic, leads/revenue.

Outcome dapat dipengaruhi seasonality, kompetitor, algoritma, dan perubahan lain. Hindari klaim kausal tanpa desain pengukuran yang layak.

### Rollback

Tetapkan trigger rollback: error rate, halaman kritis hilang, redirect loop, conversion turun tajam, atau rendering rusak. Prioritaskan keamanan operasional.

## 4. 🧠 Analogi

Implementasi SEO seperti renovasi gedung aktif. Blueprint perlu, tetapi area kerja, pemeriksaan, dokumentasi, dan jalan kembali sama pentingnya.

## 5. 💻 Template Change Log

```csv
date,url_or_template,issue,old_value,new_value,evidence,owner,status,validation,rollback
2026-09-20,product,duplicate title,default,dynamic product title,ticket-21,web,live,recrawl sample,revert commit
```

Verifikasi redirect:

```bash
curl -I https://example.com/old-url
```

Pastikan target relevan, status tepat, tidak berantai, dan final URL 200.

## 6. 🏦 Studi Kasus Industri

Tim mengganti seluruh URL artikel tanpa mapping karena “lebih SEO-friendly”. **Dampak:** backlink menuju 404 dan organic traffic jatuh. **Solusi:** inventory old-new, redirect langsung, perbarui internal link/canonical/sitemap, test sample dan pola, monitor Search Console/log, serta simpan rollback.

## 7. 📊 Aktivitas 150 Menit

20 menit change plan; 20 menit evidence; 20 menit QA/rollback; 65 menit implementation studio; 15 menit peer QA; 10 menit sign-off.

## 8. ⚠️ Kesalahan Umum

- Mengubah production tanpa baseline.
- Before-after memakai kondisi/tool berbeda.
- Deploy massal tanpa sample test.
- Menghapus URL tanpa redirect relevan.
- Mengklaim ranking naik akibat satu perubahan.
- Tidak mencatat owner dan rollback.

## 9. 🧪 Latihan dan Penilaian

Implementasikan minimal tiga perubahan prioritas pada aset berizin/staging. Serahkan change log, evidence, QA, dan monitoring plan. Rubrik: kesesuaian prioritas 25%, implementasi 30%, evidence 20%, QA/rollback 15%, dokumentasi 10%.

## 10. 📌 Ringkasan

- Rekomendasi perlu acceptance criteria.
- Implementasi harus terkontrol dan dapat dibalik.
- Before-after membutuhkan baseline konsisten.
- Output tidak sama dengan outcome.
- Monitoring menutup siklus perubahan.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Acceptance criteria | Syarat agar perubahan dianggap benar |
| Change log | Catatan perubahan dan bukti |
| Deployment | Penerapan perubahan ke lingkungan target |
| Rollback | Mengembalikan perubahan |

## 12. Referensi

- Google, **Site moves with URL changes**, <https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes>
- Google, **Ask Google to recrawl URLs**, <https://developers.google.com/search/docs/crawling-indexing/ask-google-to-recrawl>

