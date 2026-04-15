# Pertemuan 9: Figma Intermediate — Components, Auto Layout & Prototyping

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Membuat dan menggunakan Figma Components dengan Variants
* Menerapkan Auto Layout untuk responsive dan flexible design
* Membuat koneksi prototipe interaktif (links, overlays, transitions)
* Menggunakan Figma Dev Mode untuk handoff ke developer

---

## 📖 Pengantar (Hook)

Bayangkan kamu desain 20 halaman aplikasi dan semua tombol primary berwarna biru. Lalu klien minta warnanya diganti ke hijau.

Tanpa Figma Components: ubah 20 tombol satu per satu. 45 menit kerja.

Dengan Figma Components: ubah 1 komponen master → semua 20 tombol berubah otomatis. 30 detik kerja.

Components bukan hanya tentang efisiensi — ini tentang **konsistensi** yang tidak mungkin dijaga secara manual di desain yang kompleks.

---

## 🧩 Konsep Utama

### Figma Components

Component adalah elemen desain yang bisa **digunakan ulang** di seluruh file. Perubahan di component master otomatis teraplikasi ke semua instance.

**Terminologi:**
* **Main Component** (⬥) — "Master" component. Perubahan di sini menyebar ke semua instance
* **Instance** — Salinan yang bisa dipakai di mana saja. Perubahan lokal tidak mempengaruhi main component

```
CARA BUAT COMPONENT:
1. Desain elemen (misalnya tombol)
2. Klik kanan → Create Component
3. Ikon ⬥ muncul di layer

CARA PAKAI COMPONENT:
Option 1: Copy-paste instance dari panel Assets
Option 2: Ctrl/Cmd+Shift+E → cari di Assets panel
Option 3: Drag dari Assets panel ke canvas

OVERRIDE INSTANCE:
- Ubah teks: double-click teks di instance
- Ubah warna teks: tetap bisa di-override
- Ubah warna background: bisa override dengan caution
- Swap icon: perlu Variants (lihat di bawah)
```

### Variants

Variants adalah kumpulan "versi berbeda" dari satu component, diorganisir dalam satu Component Set.

```
BUTTON COMPONENT SET:
┌────────────────────────────────────────┐
│ Button                                 │
│                                        │
│ Property: State   Property: Size       │
│ ○ Default         ○ Large              │
│ ○ Hover           ○ Medium             │
│ ○ Pressed         ○ Small              │
│ ○ Disabled                             │
│                                        │
│ Variant combinations:                  │
│ Default/Large   Hover/Large   ...      │
│ Default/Medium  Hover/Medium  ...      │
│ Default/Small   Hover/Small   ...      │
└────────────────────────────────────────┘

CARA BUAT VARIANTS:
1. Buat main component pertama (Default/Large)
2. Duplicate → ubah tampilan → rename sesuai variant
3. Select semua → klik "Combine as Variants" di Properties panel
4. Tambahkan Properties di component panel kanan

MANFAAT:
- Satu tempat untuk manage semua state button
- Switch variant langsung di Properties panel saat desain
- Prototipe interaktif: hover/click bisa trigger variant swap
```

### Auto Layout

Auto Layout membuat frame yang **otomatis menyesuaikan ukuran** berdasarkan konten di dalamnya.

```
TANPA AUTO LAYOUT:
- Tambah teks panjang → teks overflow, harus resize manual
- Tambah item baru ke list → harus geser semua item di bawah manual
- Susah bikin spacing konsisten

DENGAN AUTO LAYOUT:
- Tambah teks panjang → frame otomatis melebar
- Tambah item ke list → item di bawah otomatis turun
- Spacing konsisten otomatis

CARA PAKAI AUTO LAYOUT:
1. Select frame atau beberapa layer
2. Shift+A (atau klik ikon ⊕ di Properties panel)
3. Pilih: Horizontal, Vertical, atau Wrap
4. Set: Padding, Gap (spacing antar item), Alignment

CONTOH PENGGUNAAN:
- Chip/Tag: teks berubah → chip otomatis resize
- Card: konten bertambah → card otomatis tinggi
- Navigation bar: tambah/hapus tab → otomatis redistribute
- List item: nama panjang → item otomatis wrap
```

### Prototyping di Figma

Prototype menghubungkan frame-frame menjadi simulasi alur interaktif yang bisa ditest.

```
CARA BUAT PROTOTIPE DASAR:
1. Klik tab "Prototype" di panel kanan
2. Hover ke frame/komponen → klik titik connector (+)
3. Drag ke frame tujuan
4. Set trigger: On Click / On Hover / After Delay / etc.
5. Set action: Navigate To / Open Overlay / Scroll To / etc.
6. Set animation: Instant / Dissolve / Smart Animate / etc.

JENIS INTERAKSI:
- Navigate To: pindah ke screen lain
- Open Overlay: tampilkan modal/bottom sheet di atas screen saat ini
- Scroll To: scroll ke bagian tertentu di screen
- Back: kembali ke screen sebelumnya

SMART ANIMATE:
- Saat dua frame punya layer dengan nama SAMA, Figma
  otomatis animasikan transisi antara state
- Contoh: button dengan dua variant (default/pressed) →
  Smart Animate akan animasikan perubahan warnanya
```

### Dev Mode

Dev Mode memudahkan developer mengakses specs dari desain Figma.

```
CARA AKSES DEV MODE:
1. Buka file Figma
2. Klik tombol "<>" di kanan atas → Toggle Dev Mode
3. Klik elemen apapun → lihat specs di panel kanan

INFORMASI YANG TERSEDIA DI DEV MODE:
- Ukuran & posisi (px)
- Warna (Hex, RGB, HSL)
- Typography (font, size, weight, line height)
- Spacing & padding
- Corner radius, border
- CSS / Swift / Android XML code snippet (auto-generated!)
- Export settings untuk assets

BEST PRACTICES UNTUK HANDOFF:
- Beri nama layer yang deskriptif (bukan "Rectangle 23")
- Gunakan Design Tokens / styles untuk warna & typography
- Tambahkan notes di komponen untuk behavior yang tidak visual
```

---

## 🧠 Ilustrasi / Analogi

**Components seperti template surat resmi:**
* Template = main component: header perusahaan, format standar
* Setiap surat = instance: konten berbeda, format sama
* Ganti logo di template → semua surat otomatis berubah

**Auto Layout seperti wadah karet:**
* Tanpa Auto Layout = wadah kaku ukuran tetap (konten overflow atau ada celah)
* Dengan Auto Layout = wadah karet yang stretch sesuai isi

**Smart Animate seperti "morphing" animasi:**
* Layer dengan nama sama di dua frame = Figma tahu cara animasikannya
* Seperti instruktur animasi yang tahu "titik A" dan "titik B" dan menggambar pergerakannya

---

## 💻 Praktik: Bangun Component Library Dasar

```
LANGKAH-LANGKAH:

STEP 1 — Foundation
1. Buat frame "Colors" → tambahkan semua warna sebagai rectangle + nama
2. Buat frame "Typography" → tampilkan semua text style

STEP 2 — Atoms (komponen paling kecil)
3. Icon Set:
   - Buat frame 24x24px
   - Gunakan vector shapes atau import dari icon library
   - Jadikan component

4. Button Component:
   - Buat 4 variant: Primary/Default, Primary/Hover, 
     Secondary/Default, Secondary/Hover
   - Gunakan Auto Layout untuk padding konsisten
   - Add variant property: State + Type

5. Input Field:
   - 4 state: Default, Focus, Error, Disabled
   - Auto Layout untuk padding
   - Slot untuk icon (optional)

STEP 3 — Molecules
6. Form Row:
   - Label + Input Field
   - Auto Layout: vertical, gap 4px

7. Card:
   - Image placeholder + Title + Subtitle + CTA
   - Auto Layout: padding 16px, gap 12px

STEP 4 — Organism
8. Navigation Bar:
   - 4-5 Nav Item components (icon + label)
   - Auto Layout: horizontal, space-between
   - Active/Inactive variant per item
```

---

## 🏢 Studi Kasus: Figma Component Library Jenius App

**Konteks:** Jenius (Bank BTPN) membangun Design System di Figma untuk konsistensi antara 15+ designer yang bekerja di tim berbeda.

**Struktur Component Library:**
* **Level 1 — Design Tokens:** Warna (primary, neutral, semantic), Typography, Spacing
* **Level 2 — Atoms:** Icon, Button, Input, Badge, Tag, Avatar
* **Level 3 — Molecules:** Form Group, List Item, Card, Alert, Modal
* **Level 4 — Organisms:** Navigation Bar, App Bar, Tab Bar, Transaction Card

**Hasilnya:**
* Waktu desain layar baru turun dari 3 jam ke 45 menit
* Konsistensi visual naik — tidak ada lagi "button yang berbeda-beda"
* Developer handoff lebih cepat — semua spec ada di Figma Dev Mode

**Lessons Learned:**
* Component harus punya dokumentasi — siapa yang boleh pakai kapan?
* Terlalu banyak variant bikin bingung — mulai simple, tambah kemudian
* Naming convention krusial: `Button/Primary/Default` lebih jelas dari `Button1`

---

## ⚠️ Kesalahan Umum

1. **Override yang tidak konsisten** → Mengganti warna/font di setiap instance alih-alih update main component. Akibat: inconsistency yang susah ditrack.

2. **Nested components terlalu dalam** → Component di dalam component di dalam component → sulit di-maintain. Batasi maksimal 3 level nesting.

3. **Auto Layout tanpa constraints** → Saat resize frame, konten bisa bergeser tidak sesuai. Set constraints (Fix / Hug / Fill) untuk setiap elemen.

4. **Prototipe terlalu banyak interaksi** → Prototipe untuk user testing tidak harus 100% functional. Fokus pada alur yang ingin di-test saja.

5. **Layer tidak dinamai** → "Frame 1", "Rectangle 23" → developer tidak bisa tahu ini komponen apa. Naming convention yang konsisten sangat penting untuk handoff.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan antara Main Component dan Instance di Figma. Apa yang terjadi jika kamu mengubah warna teks di Main Component?

b) Kapan kamu menggunakan Auto Layout dibanding frame biasa? Berikan 3 contoh use case yang tepat untuk Auto Layout.

### Soal 2 — Praktik (Tugas)

1. Bangun **Figma Component Library** untuk proyek kamu dengan minimal:
   * Color Styles (min. 6 warna)
   * Text Styles (min. 4 level)
   * 2 komponen atom: Button (min. 2 state) + Input Field (min. 3 state)
   * 1 komponen molecule: Card
   * Semua komponen menggunakan Auto Layout dan Variants

2. Buat **prototipe interaktif** dari wireframe mid-fi pertemuan lalu dengan minimal:
   * 8 layar terhubung dengan navigasi yang benar
   * 1 overlay (modal/bottom sheet)
   * Smart Animate pada minimal 1 transisi

---

## 📌 Ringkasan

* **Components** = elemen reusable; ubah master → semua instance terupdate
* **Variants** = kumpulan state/versi dari satu component dalam satu Component Set
* **Auto Layout** = frame yang otomatis resize sesuai konten; gunakan untuk semua elemen yang kontennya bisa berubah
* **Prototyping** = hubungkan frame dengan trigger + action + animation
* **Smart Animate** = animasikan transisi otomatis antara layer dengan nama yang sama
* **Dev Mode** = akses specs, warna, typography, dan code snippets untuk developer handoff
* Naming convention yang konsisten = kunci maintainability component library

---

*📚 Referensi: Figma Learn — help.figma.com | Material Design 3 — m3.material.io | Apple Human Interface Guidelines*
