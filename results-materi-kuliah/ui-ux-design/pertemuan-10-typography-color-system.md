# Pertemuan 10: Typography System & Color System

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Memilih dan menerapkan type scale yang harmonis dalam UI
* Membangun Color System dengan primary, secondary, neutral, dan semantic colors
* Menerapkan 60-30-10 rule untuk komposisi warna yang seimbang
* Memastikan contrast ratio memenuhi standar WCAG 2.1 AA

---

## 📖 Pengantar (Hook)

Dua aplikasi dengan fitur yang identik. Satu terasa profesional dan mudah dibaca. Satu terasa "ramai" dan melelahkan mata.

Perbedaannya bukan fitur — tapi **Typography** dan **Color System**.

Typografi yang baik menentukan hierarki informasi. Color system yang konsisten membangun kepercayaan dan mengurangi cognitive load. Di fintech, kepercayaan adalah segalanya — dan desain visual adalah cara pertama pengguna membangun (atau menghancurkan) kepercayaan itu.

---

## 🧩 Typography System

### Type Scale

Type Scale adalah sistem ukuran teks yang harmonis. Bukan asal pilih ukuran — ada rasio matematika di baliknya.

**Skala yang umum digunakan (Major Third — rasio 1.25):**

| Level | Nama | Ukuran (Mobile) | Gunakan untuk |
|---|---|---|---|
| Display | Display | 32-40px | Hero text, splash screen |
| H1 | Heading 1 | 24-28px | Judul halaman utama |
| H2 | Heading 2 | 20-22px | Judul section |
| H3 | Heading 3 | 16-18px | Subjudul, card title |
| Body L | Body Large | 16px | Teks konten utama |
| Body M | Body Medium | 14px | Teks sekunder, label |
| Caption | Caption | 12px | Metadata, timestamp, disclaimer |
| Overline | Overline | 11px | Label kecil, chip |

**Aturan minimum aksesibilitas:** Body text minimal 16px untuk mobile. Caption minimal 11px.

### Font Pairing

Menggunakan dua font yang harmonis — satu untuk heading, satu untuk body.

**Kombinasi yang terbukti bekerja:**

| Heading | Body | Karakter |
|---|---|---|
| Inter | Inter | Modern, clean, default banyak app |
| Plus Jakarta Sans | Inter | Lokal Indonesia, modern |
| Playfair Display | Source Sans Pro | Elegan, cocok untuk fintech premium |
| Poppins | Nunito | Friendly, cocok untuk app konsumer |

**Aturan sederhana:**
* Serif untuk judul + Sans-serif untuk body → kontras yang baik
* Atau gunakan satu font family dengan variasi weight (Regular, Medium, SemiBold, Bold)

### Line Height & Letter Spacing

```
Body text optimal:
- Line height: 1.5x ukuran font (16px font → 24px line height)
- Letter spacing: -0.5 hingga 0 untuk body (natural)
- Letter spacing: +1 hingga +2 untuk semua caps (OVERLINE)

Heading:
- Line height: 1.2x ukuran font (tight)
- Letter spacing: -1 hingga -0.5 (slightly tight untuk besar)
```

---

## 🧩 Color System

### Struktur Color System

Color System yang baik bukan hanya kumpulan warna — ini adalah sistem yang terorganisir.

**4 Kategori Warna:**

**1. Primary** — Warna brand utama, dominan, untuk aksi utama
```
Primary 50  (paling terang): #EBF5FF
Primary 100:                 #DBEAFE
Primary 200:                 #BFDBFE
Primary 300:                 #93C5FD
Primary 400:                 #60A5FA
Primary 500 (base):          #3B82F6  ← tombol primary, link
Primary 600:                 #2563EB
Primary 700:                 #1D4ED8  ← hover state
Primary 800:                 #1E40AF
Primary 900 (paling gelap):  #1E3A8A
```

**2. Neutral** — Warna abu-abu untuk background, border, teks
```
Neutral 0:   #FFFFFF  (background halaman)
Neutral 50:  #F9FAFB  (background card, input)
Neutral 100: #F3F4F6  (hover background)
Neutral 200: #E5E7EB  (border, divider)
Neutral 300: #D1D5DB  (placeholder)
Neutral 400: #9CA3AF  (disabled text)
Neutral 500: #6B7280  (secondary text)
Neutral 600: #4B5563  (body text)
Neutral 700: #374151  (heading)
Neutral 800: #1F2937  (primary text)
Neutral 900: #111827  (paling gelap)
```

**3. Semantic** — Warna dengan makna fungsional
```
Success:  #10B981 (hijau)   → transaksi berhasil, konfirmasi positif
Warning:  #F59E0B (kuning)  → perhatian, batas saldo hampir habis
Error:    #EF4444 (merah)   → gagal, saldo tidak cukup, error
Info:     #3B82F6 (biru)    → informasi netral
```

**4. Secondary** (opsional) — Warna aksen untuk variasi visual

### 60-30-10 Rule

Komposisi warna dalam desain UI mengikuti proporsi:

```
60% — NEUTRAL (dominan)
Latar belakang, card background, area kosong
→ Warna putih/abu-abu terang
→ Menciptakan "ruang napas" visual

30% — PRIMARY
Elemen penting: header, card terfokus, section penting
→ Warna brand utama (tapi tidak terlalu banyak)
→ Membantu hierarki visual

10% — ACCENT
CTA (call-to-action), highlight, badge, status indicator
→ Warna yang paling mencolok/kontras
→ Menarik perhatian ke aksi utama

PENERAPAN DI FINTECH APP:
60%: Background putih/abu-abu, card putih
30%: Navigation, header section
10%: Tombol "Transfer Sekarang", badge saldo, notifikasi penting
```

### WCAG 2.1 — Aksesibilitas Warna

WCAG (Web Content Accessibility Guidelines) mengatur standar kontras minimum untuk readability.

**Minimum contrast ratio:**
* **Normal text (< 18px):** 4.5:1
* **Large text (≥ 18px atau ≥ 14px bold):** 3:1
* **UI components (icon, border):** 3:1

```
CARA CEK CONTRAST RATIO:

Tools:
- Figma Plugin: "Contrast" atau "Stark"
- Website: webaim.org/resources/contrastchecker/
- Chrome DevTools → Elements → Accessibility

CONTOH:
Teks putih #FFFFFF di atas biru #3B82F6:
→ Contrast ratio: 3.04:1 → GAGAL untuk normal text!

Teks putih #FFFFFF di atas biru gelap #1D4ED8:
→ Contrast ratio: 5.74:1 → LULUS AA ✓

Teks abu #6B7280 di atas putih #FFFFFF:
→ Contrast ratio: 4.61:1 → LULUS AA ✓
```

**Mengapa aksesibilitas penting?**
* 8% laki-laki, 0.5% perempuan mengalami color blindness
* Layar di bawah sinar matahari → contrast rendah jadi tidak terbaca
* Regulasi beberapa negara mewajibkan WCAG compliance

### Dark Mode Considerations

Saat membuat dark mode, warna tidak bisa sekedar "dibalik":

```
JANGAN: Ganti semua putih jadi hitam, semua hitam jadi putih
LAKUKAN: Buat token warna terpisah untuk dark mode

Light Mode → Dark Mode:
Background: #FFFFFF      → #121212
Surface:    #F9FAFB      → #1E1E1E
On-Surface: #111827      → #E5E7EB
Primary:    #3B82F6      → #60A5FA (lebih terang di dark)
Error:      #EF4444      → #F87171 (lebih terang di dark)
```

---

## 🧠 Ilustrasi / Analogi

**Color System seperti seragam tim:**
* Warna primary = warna jersey utama
* Warna secondary = warna aksen/stripe
* Warna semantic = warna khusus kapten vs pemain biasa

**Typography Scale seperti hierarki judul berita:**
* Headline (paling besar): yang paling penting hari ini
* Subheadline: konteks tambahan
* Body: detail lengkap
* Caption: metadata (siapa, kapan)

**WCAG seperti standar keselamatan:**
* Bukan hanya untuk memenuhi regulasi — tapi agar semua orang bisa membaca
* Termasuk orang tua, pengguna di cahaya terang, dan pengguna dengan gangguan penglihatan

---

## 💻 Praktik: Bangun Color & Typography System di Figma

### Cara Buat Color Styles di Figma

```
LANGKAH:
1. Buat rectangle → beri warna
2. Klik "+" di panel Fills → "Create Style"
3. Beri nama dengan format:
   "color/primary/500"
   "color/neutral/100"
   "color/semantic/error"

4. Ulangi untuk semua warna dalam sistem

CARA PAKAI:
- Klik elemen → Fill panel → ⬥ ikon → pilih color style
- Ubah satu style → semua elemen yang pakai style itu terupdate

TIPS ORGANISASI:
- Gunakan "/" untuk grouping:
  color/brand/primary
  color/brand/secondary
  color/neutral/50 ... 900
  color/semantic/success, warning, error, info
```

### Cara Buat Text Styles

```
LANGKAH:
1. Buat text box → atur font, size, weight, line height
2. Di panel Typography → klik "+" → "Create Style"
3. Beri nama:
   "text/display"
   "text/heading/h1"
   "text/heading/h2"
   "text/body/large"
   "text/body/medium"
   "text/caption"

VARIASI DALAM SATU LEVEL:
"text/body/medium/regular"
"text/body/medium/semibold"
```

---

## 🏢 Studi Kasus: Color System Jenius (BTPN)

**Brand Identity:** Jenius memposisikan diri sebagai "bank digital yang smart & friendly" untuk anak muda urban.

**Primary Color:** Deep Blue #1A73E8 — kepercayaan + teknologi
**Secondary Color:** Teal #00BFA5 — fresh + modern
**Neutral:** Scale dari #FFFFFF ke #212121

**Keputusan desain yang menarik:**
* Menggunakan soft gradients (bukan flat color) untuk kartu tabungan — memberikan kesan premium
* Semantic colors sangat eksplisit: saldo berkurang = merah, bertambah = hijau (tidak ada ambiguitas)
* Dark mode menggunakan #121B2E (dark navy) bukan #000000 — terasa lebih premium dan familiar

**Hasil:** NPS (Net Promoter Score) Jenius naik signifikan setelah redesign visual — pengguna melaporkan aplikasi terasa "lebih premium dan lebih dipercaya."

---

## ⚠️ Kesalahan Umum

1. **Terlalu banyak warna** → Jika desain kamu punya 15+ warna berbeda, itu bukan sistem — itu kekacauan. Disiplin dengan sistem yang sudah dibuat.

2. **Mengabaikan contrast ratio** → "Tapi kelihatan bagus di layar saya" — layar developer biasanya dikalibrasi bagus. Di layar pengguna di bawah sinar matahari, bisa tidak terbaca.

3. **Typography monoton** → Semua teks ukuran sama → tidak ada hierarki → pengguna tidak tahu apa yang paling penting.

4. **60-30-10 terbalik** → Menggunakan warna aksen (10%) sebagai warna dominan → terlalu "ramai" dan melelahkan mata.

5. **Tidak konsisten menggunakan semantic colors** → Error kadang merah, kadang oranye, kadang teks biasa. Pengguna tidak tahu mana yang kritis.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan mengapa contrast ratio WCAG 4.5:1 penting, bukan hanya dari perspektif regulasi tapi dari perspektif pengalaman pengguna nyata.

b) Apa perbedaan antara semantic color dan primary color? Berikan contoh penggunaan yang tepat untuk masing-masing.

### Soal 2 — Praktik (Tugas)

Untuk proyek desain kamu, bangun Typography & Color System di Figma:

1. **Typography System:**
   * Pilih 1-2 font family
   * Definisikan minimal 6 level text (Display, H1, H2, Body L, Body M, Caption)
   * Buat sebagai Figma Text Styles

2. **Color System:**
   * Primary palette (minimal 5 shade)
   * Neutral palette (minimal 7 shade)
   * 4 semantic colors (success, warning, error, info)
   * Semua dibuat sebagai Figma Color Styles

3. **Validasi:**
   * Cek semua kombinasi teks di atas background menggunakan contrast checker
   * Dokumentasikan hasil cek (PASS/FAIL AA)

---

## 📌 Ringkasan

* **Typography System** = type scale yang harmonis untuk konsistensi hierarki visual
* Minimal 5 level: Display → H1/H2 → Body → Caption
* Line height body: 1.5x; Heading: 1.2x
* **Color System** = Primary + Neutral + Semantic (+ Secondary opsional)
* **60-30-10 Rule:** 60% neutral, 30% primary, 10% accent
* **WCAG 2.1 AA:** normal text minimal 4.5:1 contrast ratio
* Semantic colors harus konsisten: error selalu merah, success selalu hijau
* Dark mode = buat token terpisah, jangan sekedar balik warna
* Tools kontras: Figma Stark plugin, webaim.org, Chrome DevTools

---

*📚 Referensi: Material Design 3 — m3.material.io | Apple HIG — developer.apple.com/design | WCAG 2.1 — w3.org/WAI/WCAG21 | Refactoring UI — Adam Wathan*
