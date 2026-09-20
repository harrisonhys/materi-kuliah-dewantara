# Pertemuan 2: Crawling Indexing dan Ranking

> **RPS:** CPMK-1, Sub-CPMK-2 • **Durasi:** 150 menit • **Bobot:** 3%

## 1. 🎯 Learning Outcomes

Kamu mampu menjelaskan discovery, crawling, rendering, indexing, canonicalization, dan serving; membedakan crawlability dari indexability; mendiagnosis halaman tidak terindeks; serta memakai robots, sitemap, dan URL Inspection secara tepat.

## 2. 📖 Pengantar

Halaman bagus tidak berguna jika mesin tidak menemukannya atau tidak boleh mengindeksnya. Sebaliknya, halaman yang terindeks belum tentu muncul untuk query tertentu. Diagnosis harus mengikuti tahapan, bukan menebak “algoritma tidak suka”.

## 3. 🧩 Konsep Utama

```text
Discovery → Crawl → Render → Index/Canonicalize → Serve/Rank
```

- **Discovery:** URL ditemukan melalui link, sitemap, atau sumber lain.
- **Crawling:** crawler mengambil resource jika akses memungkinkan.
- **Rendering:** JavaScript dapat dijalankan untuk melihat konten.
- **Indexing:** isi dianalisis dan kandidat canonical dipilih.
- **Serving:** hasil relevan ditampilkan untuk query/konteks.

Google tidak menjamin semua halaman akan di-crawl, diindeks, atau ditampilkan.

### Kontrol yang Sering Tertukar

| Mekanisme | Fungsi | Bukan untuk |
|---|---|---|
| `robots.txt` | Mengatur akses crawler | Menjamin URL hilang dari indeks |
| `noindex` | Meminta halaman tidak diindeks | Menghemat crawl jika akses diblokir |
| Sitemap | Memberi hint URL canonical penting | Menjamin indexing/ranking |
| Canonical | Menyatakan versi pilihan duplikat | Redirect pengguna |
| Redirect 301/308 | Memindahkan pengguna dan sinyal | Menyimpan dua URL aktif identik |

Ranking mempertimbangkan relevansi dan kualitas dengan banyak sinyal; jangan membuat daftar “200 faktor” seolah semuanya diketahui dan berbobot tetap.

## 4. 🧠 Analogi

Crawling seperti petugas mengambil buku; indexing seperti katalog; canonicalization memilih edisi representatif; ranking seperti pustakawan memilih buku paling membantu untuk pertanyaan tertentu.

## 5. 💻 Contoh Teknis

`robots.txt`:

```text
User-agent: *
Disallow: /admin/
Sitemap: https://example.com/sitemap.xml
```

Meta robots:

```html
<meta name="robots" content="noindex,follow">
```

Checklist diagnosis:

```text
1. URL mengembalikan 200?
2. Dapat diakses tanpa login?
3. Tidak diblokir robots?
4. Tidak noindex?
5. Canonical menunjuk ke mana?
6. Konten utama ada saat render?
7. Ada internal link/sitemap?
8. Apa hasil URL Inspection?
```

## 6. 🏦 Studi Kasus Industri

Marketplace meluncurkan kategori promo, tetapi template tanpa sengaja membawa `noindex` dari staging. **Dampak:** halaman tidak masuk indeks saat kampanye berjalan. **Solusi:** pre-release SEO checklist, automated check status/meta/canonical, sitemap hanya URL 200 canonical, dan URL Inspection setelah rilis.

## 7. 📊 Aktivitas 150 Menit

25 menit alur search; 25 menit kontrol teknis; 20 menit demo header/source; 45 menit diagnosis kasus; 20 menit diagram kelompok; 15 menit kuis.

## 8. ⚠️ Kesalahan Umum

- Memblokir di robots lalu berharap `noindex` terbaca.
- Memasukkan URL redirect/error ke sitemap.
- Canonical menunjuk ke URL tidak relevan.
- Menyamakan indexed dengan ranking tinggi.
- Meminta indexing berulang tanpa memperbaiki masalah.

## 9. 🧪 Latihan dan Penilaian

Diagnosis lima URL contoh: tentukan tahap gagal, evidence, severity, dan fix. Gambar alur crawler dari homepage ke halaman produk. Rubrik: ketepatan tahap 30%, evidence 30%, rekomendasi 25%, diagram 15%.

## 10. 📌 Ringkasan

- Crawlability, indexability, dan ranking berbeda.
- Diagnosis mengikuti pipeline.
- Sitemap adalah hint, bukan jaminan.
- Robots bukan alat canonicalization.
- Evidence teknis lebih kuat daripada asumsi.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Canonical | URL representatif dari kumpulan duplikat |
| Crawling | Pengambilan halaman/resource oleh crawler |
| Indexing | Analisis dan penyimpanan informasi halaman |
| Rendering | Pemrosesan halaman termasuk JavaScript |
| Sitemap | Daftar URL yang ingin diinformasikan ke mesin |

## 12. Referensi

- Google, **How Search Works**, <https://developers.google.com/search/docs/fundamentals/how-search-works>
- Google, **Crawling and indexing**, <https://developers.google.com/search/docs/crawling-indexing>

