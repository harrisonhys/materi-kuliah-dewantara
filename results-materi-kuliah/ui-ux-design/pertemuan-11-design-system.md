# Pertemuan 11: Design System & UI Component Library

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Memahami Atomic Design methodology
* Membangun komponen dasar UI yang reusable di Figma
* Mendokumentasikan komponen dengan usage guidelines
* Menerapkan konsistensi desain menggunakan Design Tokens

---

## 📖 Pengantar (Hook)

Airbnb memiliki 100+ designer yang bekerja di puluhan produk berbeda — mobile iOS, Android, web, dan internal tools.

Tanpa Design System, setiap designer membuat "versi" tombol, warna, dan tipografi mereka sendiri. Hasilnya: inkonsistensi yang membingungkan pengguna dan memperlambat pengembangan.

Dengan Design System, ketika seorang designer baru bergabung, mereka bisa mulai desain produktif di hari pertama — karena semua keputusan visual dasar sudah terdokumentasi.

Design System bukan hanya tentang konsistensi — ini tentang **kecepatan dan kepercayaan**.

---

## 🧩 Konsep Utama

### Apa itu Design System?

Design System adalah kumpulan **standar**, **komponen**, dan **dokumentasi** yang memungkinkan tim desain dan development bekerja secara konsisten dan efisien.

**Komponen Design System:**
1. **Design Principles** — Nilai-nilai yang memandu keputusan desain
2. **Visual Language** — Warna, tipografi, spacing, iconography
3. **Component Library** — UI components yang siap pakai
4. **Pattern Library** — Pola solusi untuk masalah UX tertentu
5. **Documentation** — Panduan penggunaan setiap komponen

### Atomic Design Methodology

Dikembangkan oleh Brad Frost, Atomic Design menganalogikan UI dengan kimia:

```
ATOMS           MOLECULES          ORGANISMS
Elemen terkecil  Kombinasi atom     Kombinasi molekul
yang fungsional  yang bermakna      yang kompleks

[Button]        [Form Group]       [Login Form]
[Input]    →    [Label+Input] →    [Card dengan Actions]
[Icon]          [Card Minimal]     [Navigation Bar]
[Label]         

TEMPLATES         PAGES
Layout tanpa      Template + real content
konten nyata      = apa yang pengguna lihat

[Frame dengan    [Frame dengan foto,
 placeholder] →   teks, data nyata]
```

**Mengapa Atomic Design?**
* Mulai dari elemen terkecil → konsistensi terjamin dari fondasi
* Komponen bisa digabungkan infinite ways
* Perubahan di atom (misalnya warna button) otomatis naik ke atas

### Design Tokens

Design Tokens adalah **named design decisions** — variable yang menyimpan nilai-nilai desain.

```
TANPA DESIGN TOKENS:
Hardcode warna di setiap komponen:
Button → fill: #3B82F6
Link → color: #3B82F6
Icon → fill: #3B82F6
(Kalau mau ganti, harus ubah 3 tempat)

DENGAN DESIGN TOKENS:
Token: color-primary-500 = #3B82F6
Button → fill: color-primary-500
Link → color: color-primary-500
Icon → fill: color-primary-500
(Ubah nilai token → semua otomatis berubah!)

CONTOH DESIGN TOKENS:
color/brand/primary        = #3B82F6
color/semantic/success     = #10B981
color/semantic/error       = #EF4444
spacing/4                  = 4px
spacing/8                  = 8px
spacing/16                 = 16px
border-radius/small        = 4px
border-radius/medium       = 8px
border-radius/large        = 16px
font-size/body-medium      = 14px
font-weight/semibold       = 600
```

### Komponen Wajib di Design System

#### Button

```
STATE YANG PERLU ADA:
✓ Default (normal)
✓ Hover
✓ Pressed/Active
✓ Loading (spinner)
✓ Disabled
✓ Focus (untuk keyboard navigation)

VARIANT:
✓ Primary (filled)
✓ Secondary (outlined)
✓ Tertiary / Ghost (text only)
✓ Destructive (merah, untuk hapus)
✓ Icon only

SIZE:
✓ Large: 48px height
✓ Medium: 40px height
✓ Small: 32px height

DOKUMENTASI:
- Kapan pakai Primary vs Secondary vs Ghost?
- Berapa maksimal button per halaman?
- Apa yang harus ada di loading state?
```

#### Input Field

```
TYPE:
✓ Text
✓ Number
✓ Password (dengan toggle show/hide)
✓ Search (dengan ikon)
✓ Textarea (multiline)
✓ Select / Dropdown

STATE:
✓ Default
✓ Focus (aktif, border berubah warna)
✓ Filled (ada konten)
✓ Error (border merah + pesan error)
✓ Disabled
✓ Read-only

SUBCOMPONEN:
✓ Label (di atas)
✓ Placeholder (di dalam)
✓ Helper text (di bawah, untuk hint)
✓ Error message (di bawah, untuk error)
✓ Character counter (opsional)
✓ Leading icon / Trailing icon (opsional)
```

#### Card

```
JENIS CARD:
✓ Basic Card: shadow ringan, padding, radius
✓ Outlined Card: border, no shadow
✓ Elevated Card: shadow lebih tebal

SLOT (bagian yang bisa diisi konten berbeda):
✓ Header: judul, subtitle, avatar
✓ Media: gambar/video
✓ Body: teks konten
✓ Footer: action buttons, metadata

INTERACTION STATE:
✓ Default
✓ Hoverable (cursor pointer, shadow naik)
✓ Selected (untuk selectable cards)
```

---

## 🧠 Ilustrasi / Analogi

**Design System seperti buku resep masakan:**
* Resep = komponen — cara membuat satu dish
* Bahan dasar = atoms — garam, gula, tepung
* Teknik dasar = patterns — cara menggoreng, cara mengukus
* Buku resep = dokumentasi — semuanya tertulis untuk diikuti tim

**Tanpa Design System** = setiap koki masak dengan cara sendiri → masakan tidak konsisten antar cabang restoran

**Dengan Design System** = semua koki ikuti buku resep yang sama → setiap cabang rasa yang sama

---

## 💻 Praktik: Dokumentasi Komponen di Figma

### Format Dokumentasi Komponen (dalam Figma)

```
HALAMAN "COMPONENT DOCS" DI FIGMA:

SECTION: Button

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OVERVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Preview semua variant dalam satu baris]
Primary | Secondary | Ghost | Destructive

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
USAGE GUIDELINES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✓ DO: Gunakan Primary button untuk aksi utama (satu per halaman)
✓ DO: Gunakan Secondary untuk aksi alternatif
✗ DON'T: Taruh dua Primary button bersebelahan
✗ DON'T: Gunakan Destructive untuk aksi yang tidak permanen

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STATES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Default]  [Hover]  [Pressed]  [Loading]  [Disabled]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PROPS (untuk Developer)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
variant: primary | secondary | ghost | destructive
size: large | medium | small
state: default | loading | disabled
icon: optional, position: left | right
label: string (required)
```

---

## 🏢 Studi Kasus: Design System Tokopedia (Unify)

**Konteks:** Tokopedia memiliki 300+ engineer dan 30+ designer. Mereka membangun "Unify" — design system internal yang digunakan di seluruh produk.

**Komponen yang mereka standardisasi:**
* Button (15+ variant)
* Form elements (input, select, checkbox, radio, toggle)
* Navigation (top bar, bottom nav, breadcrumb)
* Feedback (toast, snackbar, alert, modal)
* Data display (card, list, table, badge)

**Cara mereka menjaga konsistensi:**
* Design tokens di Figma → sync ke code variables (JavaScript/CSS)
* Component library di React → semua engineer pakai komponen yang sama
* Dokumentasi di Storybook → developer bisa preview + test setiap komponen

**Hasilnya:**
* Waktu desain halaman baru turun 60%
* "Visual bugs" (inkonsistensi UI) turun 80%
* Onboarding engineer baru lebih cepat — bisa pakai Unify tanpa bertanya

---

## ⚠️ Kesalahan Umum

1. **Design System terlalu besar di awal** → Membangun 200 komponen sebelum produk jadi. Mulai dengan 10-15 komponen yang paling sering dipakai, tambahkan sesuai kebutuhan.

2. **Tidak ada documentation** → Library komponen tanpa panduan penggunaan = komponen tidak akan dipakai dengan benar. "Show, don't tell" saja tidak cukup.

3. **Komponen terlalu kaku** → Komponen yang tidak bisa di-customize memaksa designer membuat komponen baru. Berikan flexibility yang cukup dengan Variants.

4. **Design system tidak diperbarui** → Komponen di library sudah tidak sesuai dengan yang ada di produk. Jadwalkan "Design System Sync" reguler.

5. **Tidak ada ownership** → "Siapa yang maintain design system ini?" — tanpa ownership yang jelas, design system akan mati.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan Atomic Design dengan contoh dari aplikasi fintech. Identifikasi setidaknya 2 atom, 2 molekul, dan 1 organisme.

b) Apa perbedaan antara Design Tokens dan Color Styles di Figma? Mana yang lebih kuat untuk scaling?

### Soal 2 — Praktik (Tugas)

Bangun Design System untuk proyek desain kamu:

1. **Atoms:**
   * Button (minimal 3 variant: Primary, Secondary, Ghost) dengan minimal 4 state masing-masing
   * Input Field (minimal 4 state: Default, Focus, Error, Disabled)
   * Badge/Chip (minimal 2 warna)
   * Avatar (minimal 2 size)

2. **Molecules:**
   * Form Group (Label + Input + Helper/Error text)
   * List Item (icon + teks + aksi sekunder)
   * Transaction Card (icon + nama + nominal + status)

3. **Dokumentasi:**
   * Untuk setiap komponen: Usage Guidelines (DO dan DON'T)
   * Tampilkan semua state dalam satu frame

---

## 📌 Ringkasan

* **Design System** = standar + komponen + dokumentasi untuk konsistensi dan kecepatan
* **Atomic Design:** Atom → Molecule → Organism → Template → Page
* **Design Tokens** = named variables untuk nilai desain — single source of truth
* **Komponen wajib:** Button (semua state + variant), Input Field, Card, Navigation
* Dokumentasi adalah bagian yang sama pentingnya dengan komponen itu sendiri
* Mulai kecil (10-15 komponen) — grow sesuai kebutuhan
* Design system harus ada owner — tanpa ownership, mati dalam 6 bulan

---

*📚 Referensi: Brad Frost (2016). Atomic Design | Material Design 3 — m3.material.io | Figma Design Systems — figma.com/blog*
