# Pertemuan 8: User Flow & Wireframing Low-Fi dan Mid-Fi

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Membuat User Flow untuk skenario utama menggunakan diagram
* Membuat wireframe low-fidelity (sketsa tangan)
* Membuat wireframe mid-fidelity di Figma menggunakan placeholder
* Menggunakan metode Crazy 8s untuk ideasi cepat

---

## 📖 Pengantar (Hook)

Seorang developer menulis 3 hari kode untuk fitur checkout. Saat demo ke tim, product manager berkata: "Oh, saya kira tombol konfirmasi ada di halaman sebelumnya."

Tiga hari kode. Tiga hari yang bisa dihindari dengan 15 menit sketsa di kertas.

Wireframe bukan tentang membuat gambar yang cantik — wireframe adalah tentang **berpikir sebelum build**. Setiap hari yang dihabiskan di wireframe menyelamatkan seminggu development.

---

## 🧩 Konsep Utama

### User Flow vs Task Flow

**User Flow** = perjalanan pengguna lengkap termasuk keputusan, error state, dan cabang alternatif.

**Task Flow** = alur linear dari satu titik ke titik lain tanpa cabang.

```
TASK FLOW (sederhana, linear):
[Halaman Transfer] → [Input Nominal] → [Konfirmasi] → [Sukses]

USER FLOW (lebih lengkap, dengan cabang):

START ──────────────────────────────────────────────────── END
  │
  ▼
[Home]
  │
  ▼
[Pilih Transfer] ─── Saldo tidak cukup ──→ [Top-Up Screen]
  │                                               │
  ▼                                               │
[Input Penerima] ─── Nomor tidak valid ──→ [Error Message]
  │                                               │ (coba lagi)
  ▼                                               │
[Input Nominal] ◀────────────────────────────────┘
  │
  ▼
[Konfirmasi] ─── Kembali edit ──→ [Input Nominal]
  │
  ▼
[PIN Verification]
  │ ─── PIN salah 3x ──→ [Akun Terkunci]
  ▼
[SUCCESS ✓] ──→ [Share Receipt] atau [Home]
```

### Simbol User Flow

| Simbol | Bentuk | Makna |
|---|---|---|
| **Start/End** | Oval/Pill | Titik awal atau akhir alur |
| **Screen** | Persegi panjang | Layar/halaman |
| **Decision** | Berlian/Diamond | Pilihan (Ya/Tidak) |
| **Action** | Persegi rounded | Aksi pengguna |
| **Arrow** | Panah | Alur / navigasi |

### Fidelity Wireframe

**Low-Fidelity (Lo-Fi):**
* Sketsa tangan di kertas atau whiteboard
* Sangat kasar — kotak, garis, dan label saja
* Cepat dibuat (5-10 menit per layar)
* Mudah direvisi — tidak ada rasa "sayang" untuk berubah
* Tujuan: explore ide, alignment tim

**Mid-Fidelity (Mid-Fi):**
* Digital, biasanya di Figma menggunakan grayscale
* Lebih detail dari Lo-Fi tapi belum ada warna/foto nyata
* Placeholder untuk gambar (kotak dengan X), dummy text
* Menunjukkan layout, hierarki, dan spacing
* Tujuan: presentasi stakeholder, handoff ke dev untuk feedback

**High-Fidelity (Hi-Fi):**
* Full color, font nyata, foto nyata
* Interaktif (prototipe)
* Dibahas di Pertemuan 12

```
LO-FI (kertas)           MID-FI (Figma)
┌──────────────┐          ┌──────────────┐
│ HEADER       │          │ ═════════    │
│              │          │ ▬▬▬▬▬▬▬▬▬▬  │
│  [kotak]     │          │ ┌──────────┐ │
│   xxx xxx    │          │ │    ×     │ │
│  [kotak bsr] │          │ └──────────┘ │
│ [BTN]  [BTN] │          │ ─────────────│
└──────────────┘          │ [▬▬▬▬▬▬▬▬]  │
                          └──────────────┘
```

### Crazy 8s — Ideasi Cepat

Crazy 8s adalah teknik brainstorming visual: **8 sketsa dalam 8 menit**.

```
CARA MELAKUKAN CRAZY 8s:

1. Ambil selembar kertas A4
2. Lipat menjadi 8 bagian (3x lipatan)
3. Set timer 8 menit
4. Buat 1 sketsa unik di setiap kotak — 1 menit per sketsa
5. Jangan edit, jangan hapus — quantity over quality
6. Setelah selesai, pilih 1-2 ide terbaik dengan dot voting

ATURAN:
- Setiap sketsa harus berbeda (jangan iterasi dari yang sama)
- Boleh sketsa super kasar — stick figures OK
- Fokus pada ide, bukan artistik

MANFAAT:
- Mengalahkan "writer's block" desain
- Menghasilkan banyak opsi sebelum terpaku pada satu solusi
- Cepat mengeksplorasi pendekatan yang berbeda
```

---

## 🧠 Ilustrasi / Analogi

**Wireframe seperti denah rumah:**
* Arsitek tidak langsung bangun dinding — mereka buat blueprint dulu
* Blueprint tidak harus cantik — yang penting dimensi dan layout benar
* Setelah blueprint disetujui, baru bangun (code/high-fi design)

**Crazy 8s seperti warm-up atlet:**
* Pelari tidak langsung sprint jarak jauh — mereka warm up dulu
* Crazy 8s "memanaskan" pikiran kreatif sebelum desain serius
* Mengeksplor banyak arah membuka perspektif yang tidak terpikir sebelumnya

---

## 💻 Praktik: Wireframing di Figma

### Setup Figma untuk Mid-Fi Wireframe

```
FRAME SETUP:
- Mobile: 375 x 812px (iPhone 14)
- Tablet: 768 x 1024px (iPad)
- Desktop: 1440 x 900px

KOMPONEN DASAR YANG PERLU ADA:
1. Image Placeholder
   - Rectangle 100%
   - Fill: #E0E0E0 (abu-abu muda)
   - Dengan garis diagonal (Line tool, 45°)
   - Atau teks "[Image]" di tengah

2. Text Placeholder
   - Rectangle dengan ukuran teks
   - Fill: #BDBDBD
   - Cornerradius: 4px

3. Button
   - Rectangle 160x44px
   - Primary: Fill #212121 (hitam), teks putih
   - Secondary: Stroke #212121, fill transparan

4. Input Field
   - Rectangle full-width, height 48px
   - Fill: #F5F5F5
   - Stroke: #E0E0E0
   - Corner radius: 8px

5. Navigation Bar
   - Frame 375x83px di bawah
   - 4-5 ikon tab dengan label

TIPS:
- Gunakan HANYA warna grayscale (#000000 sampai #FFFFFF)
- Tidak ada foto nyata — semua placeholder
- Font: satu font, variasi size saja (untuk hierarchy)
- Gunakan Figma Components untuk elemen yang berulang
```

### Anotasi Wireframe

Wireframe yang baik harus disertai anotasi — penjelasan perilaku interaktif.

```
CONTOH ANOTASI:
① Tap area ini → navigasi ke halaman Detail Transaksi
② Swipe kiri → opsi Hapus
③ Pull to refresh → reload data transaksi
④ Disable state: tombol "Kirim" disabled selama nominal = 0
⑤ Error state: border field berubah merah, pesan error muncul di bawah
```

---

## 🏢 Studi Kasus: Wireframe Flow Transfer GoPay

**Alur yang di-wireframe:**
1. Home → tab Transfer
2. Input penerima (dari kontak atau nomor manual)
3. Input nominal + catatan
4. Halaman konfirmasi (review semua detail)
5. PIN/biometrik verification
6. Success screen dengan opsi share

**Keputusan desain dari wireframe:**
* Di halaman konfirmasi, tampilkan semua detail dengan bahasa yang jelas — nama penerima, nominal, biaya (jika ada), total yang terdebit
* Success screen: tampilkan ringkasan + dua CTA (Kirim Lagi, Kembali ke Home)
* Error flow: jika saldo kurang → tampilkan saldo saat ini + tombol Top Up langsung

---

## ⚠️ Kesalahan Umum

1. **Langsung ke Hi-Fi tanpa Lo-Fi** → Melewatkan eksplorasi ide dini. Hi-Fi membutuhkan waktu lebih banyak dan lebih "sayang" untuk diubah.

2. **Wireframe terlalu detail** → Jika wireframe sudah punya warna dan foto nyata, itu bukan wireframe — itu mockup. Pertahankan grayscale untuk mid-fi.

3. **User Flow tidak memasukkan error state** → Pengguna nyata sering membuat kesalahan. Alur error dan recovery harus direncanakan dari awal.

4. **Lupa anotasi** → Wireframe diam tidak bisa menjelaskan interaksi. Tambahkan anotasi untuk behavior yang tidak terlihat dari gambar.

5. **Crazy 8s tidak cukup cepat** → Jika sketsa terlalu perfeksionis, atur timer lebih ketat. Tujuannya eksplorasi, bukan karya seni.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Jelaskan perbedaan User Flow dan Task Flow. Kapan kamu menggunakan masing-masing?

b) Mengapa wireframe Lo-Fi sering lebih berharga dari Hi-Fi di awal proses desain?

### Soal 2 — Praktik (Tugas)

Untuk proyek desain kamu:
1. Buat **User Flow** untuk 2-3 skenario utama (gunakan simbol yang benar, termasuk decision point dan error state)
2. Lakukan **Crazy 8s** untuk halaman yang paling kompleks — 8 sketsa, 8 menit
3. Buat **sketsa Lo-Fi** minimal 6 layar utama (foto sketsa, upload)
4. Buat **wireframe Mid-Fi** di Figma minimal 8 layar terhubung dengan user flow yang benar
5. Tambahkan anotasi pada minimal 3 interaksi non-obvious

---

## 📌 Ringkasan

* **User Flow** = alur pengguna lengkap termasuk cabang, keputusan, dan error state
* **Task Flow** = alur linear tanpa cabang — untuk dokumentasi sederhana
* **Lo-Fi** = sketsa kasar, cepat, untuk eksplorasi — jangan takut ubah
* **Mid-Fi** = digital grayscale di Figma, lebih detail, untuk presentasi dan feedback
* **Crazy 8s** = 8 sketsa berbeda dalam 8 menit — untuk mengalahkan design block
* Wireframe selalu disertai **anotasi** untuk menjelaskan interaksi
* Prinsip: semakin awal kamu menemukan masalah (di wireframe), semakin murah biaya perbaikannya

---

*📚 Referensi: Krug, S. (2014). Don't Make Me Think | Figma Learn — help.figma.com | NNGroup — Wireframing*
