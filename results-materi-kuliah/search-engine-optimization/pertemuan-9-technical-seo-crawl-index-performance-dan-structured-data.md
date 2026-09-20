# Pertemuan 9: Technical SEO Crawl Index Performance dan Structured Data

> **RPS:** CPMK-4, Sub-CPMK-8 • **Durasi:** 150 menit • **Bobot:** 6%

## 1. 🎯 Learning Outcomes

Kamu mampu menganalisis crawlability/indexability, sitemap, robots, canonical, redirect, architecture, mobile, HTTPS, Core Web Vitals, dan structured data; menghasilkan temuan berbukti serta prioritas perbaikan.

## 2. 📖 Pengantar

Technical SEO memastikan konten yang tepat dapat ditemukan, dipahami, dipilih sebagai canonical, dan digunakan dengan baik. Ia bukan membuat mesin “tertipu”, melainkan mengurangi hambatan pengguna dan crawler.

## 3. 🧩 Konsep Utama

### Audit Berlapis

```text
Status/akses → robots/noindex → canonical/redirect → internal links/sitemap
→ rendering/mobile → performance → structured data → validation
```

### Canonical dan Redirect

Redirect adalah sinyal kuat ketika URL dipindahkan; `rel=canonical` adalah sinyal kuat untuk versi representatif; sitemap adalah sinyal lebih lemah. Sinyal harus konsisten.

### Core Web Vitals

| Metrik | Mengukur | Target “good” |
|---|---|---:|
| LCP | Kecepatan konten utama | ≤2,5 detik |
| INP | Respons interaksi | ≤200 ms |
| CLS | Stabilitas visual | ≤0,1 |

Nilai dievaluasi pada persentil ke-75; bedakan field data pengguna nyata dari lab data diagnosis.

### Structured Data

Markup harus mewakili konten yang terlihat. JSON-LD direkomendasikan Google. Markup valid membuat halaman **eligible**, bukan menjamin rich result atau ranking. Pilih tipe yang didukung dan relevan.

## 4. 🧠 Analogi

Technical SEO seperti infrastruktur mal: jalan masuk, direktori, eskalator, kecepatan layanan, dan label toko. Produk bagus sulit ditemukan jika jalurnya rusak.

## 5. 💻 Contoh Teknis

```html
<link rel="canonical" href="https://example.com/products/kasir-pro">
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Kasir Pro",
  "description": "Aplikasi kasir untuk UMKM"
}
</script>
```

Jangan menambahkan rating palsu atau properti yang tidak tampak pada halaman. Validasi syntax dengan Rich Results Test, lalu periksa guideline kualitas.

## 6. 🏦 Studi Kasus Industri

Marketplace memiliki filter warna/ukuran yang menciptakan jutaan URL crawlable. **Dampak:** crawl boros, duplicate content, laporan berisik. **Solusi:** tentukan faceted URL bernilai pencarian, konsolidasikan sinyal canonical/internal link/sitemap, kontrol crawl secara hati-hati, dan pastikan kategori penting tetap dapat diakses.

## 7. 📊 Aktivitas 150 Menit

25 menit crawl/index; 20 menit canonical/redirect; 25 menit CWV; 20 menit structured data; 45 menit audit; 15 menit prioritas.

## 8. ⚠️ Kesalahan Umum

- Robots digunakan untuk canonicalization.
- Redirect chain/loop.
- Semua filter dimasukkan sitemap.
- Mengejar skor Lighthouse 100 tanpa dampak pengguna.
- Structured data tidak sesuai konten.
- Menganggap valid markup menjamin rich result.

## 9. 🧪 Latihan dan Penilaian

Audit 10 URL lintas template dengan source/header, PSI/Lighthouse, sitemap/robots, canonical, mobile, dan schema. Hasilkan 8 temuan evidence-based. Rubrik: cakupan 25%, evidence 30%, diagnosis 25%, prioritas 20%.

## 10. 📌 Ringkasan

- Technical SEO menghilangkan hambatan akses dan pemahaman.
- Sinyal canonical harus konsisten.
- CWV mengukur loading, respons, dan stabilitas.
- Field dan lab data berbeda fungsi.
- Schema harus jujur dan relevan.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| CLS | Pergeseran layout visual |
| INP | Responsivitas interaksi |
| LCP | Waktu tampil konten utama |
| JSON-LD | Format structured data berbasis JSON |
| Rich result | Tampilan hasil yang diperkaya |

## 12. Referensi

- Google, **Canonical URLs**, <https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls>
- Google, **Structured data guidelines**, <https://developers.google.com/search/docs/appearance/structured-data/sd-policies>
- web.dev, **Web Vitals**, <https://web.dev/articles/vitals>

