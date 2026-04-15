# Pertemuan 12: Prototipe High-Fidelity di Figma

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Mengintegrasikan Design System ke dalam desain layar penuh
* Membuat prototipe interaktif minimal 8 layar yang terhubung
* Menerapkan micro-interactions dan animasi transisi yang tepat
* Menyiapkan prototipe untuk sesi Usability Testing

---

## 📖 Pengantar (Hook)

Pitch deck bisa meyakinkan investor tentang ide. Tapi tidak ada yang lebih meyakinkan dari **prototipe yang bisa diklik**.

"Coba sendiri" adalah argumen terkuat dalam dunia desain. Ketika calon pengguna memegang prototipe Figma kamu dan berhasil menyelesaikan task yang kamu desain — itu lebih berharga dari 100 slide presentasi.

High-fidelity prototype adalah "produk sungguhan minus kode". Dan kualitasnya sangat mempengaruhi kualitas feedback yang kamu dapatkan dari user testing.

---

## 🧩 Konsep Utama

### Apa itu High-Fidelity Prototype?

Hi-Fi prototype adalah representasi desain yang mendekati produk final:
* **Visual:** Full color, font nyata, foto/ilustrasi nyata, icon yang benar
* **Interaktif:** Bisa diklik, ada transisi, ada micro-interaction
* **Realistis:** Data yang masuk akal (nama, nominal, tanggal nyata), bukan "Lorem Ipsum"

**Kapan menggunakan Hi-Fi:**
* Presentasi ke stakeholder atau klien
* Sesi Usability Testing formal
* Pitch ke investor
* Handoff ke developer

### Integrasi Design System ke Layar

Proses membangun Hi-Fi = **menyusun ulang komponen dari Design System menjadi layar yang utuh**.

```
CHECKLIST SEBELUM MULAI:
□ Design System sudah lengkap (color, typography, components)
□ Wireframe mid-fi sudah disetujui
□ User Flow sudah jelas
□ Konten nyata sudah disiapkan (nama, angka, foto)

PROSES MEMBANGUN SATU LAYAR:
1. Buka wireframe mid-fi sebagai referensi
2. Buat Frame baru di sebelahnya
3. Ambil komponen dari panel Assets
4. Susun sesuai layout wireframe, tapi dengan visual nyata
5. Ganti placeholder dengan konten nyata
6. Tambahkan spacing yang konsisten (8px grid)
7. Cek: apakah hierarki visual jelas?
8. Cek: apakah contrast ratio terpenuhi?
9. Cek: apakah touch target minimal 44x44px?
```

### Konten yang Realistis

Satu hal yang membuat prototipe terasa "profesional" atau "amatir" adalah kualitas konten dummy.

```
❌ BURUK (tidak realistis):
Nama: User Name
Nominal: Rp 00000
Tanggal: DD/MM/YYYY
Foto: [gray box]

✅ BAGUS (realistis):
Nama: Budi Santoso
Nominal: Rp 250.000
Tanggal: 15 Jan 2025, 14:32 WIB
Foto: foto profil dari Unsplash/UI Avatars
```

### Micro-Interactions

Micro-interaction adalah animasi kecil yang memberi feedback visual atas aksi pengguna.

```
CONTOH MICRO-INTERACTIONS DI FIGMA:

1. BUTTON PRESS FEEDBACK:
   - Default state → Pressed state (gunakan Smart Animate)
   - Duration: 100-150ms
   - Easing: Ease Out
   - Visual: warna sedikit lebih gelap, scale sedikit lebih kecil (0.97)

2. FORM VALIDATION:
   - Input field: Default → Focus (border berubah warna, label naik)
   - Input field: Error (border merah, shake animation)
   - Smart Animate antara Default dan Focus variant

3. TOGGLE SWITCH:
   - Off → On: knob bergerak ke kanan
   - Warna background berubah dari abu ke hijau
   - Smart Animate antara dua variant

4. LOADING STATE:
   - Spinner yang berputar
   - Skeleton loading (placeholder abu yang shimmer)
   - Progress bar (animasi width dari 0 ke 100%)

5. BOTTOM SHEET:
   - Muncul dari bawah dengan Spring animation
   - Dismiss: drag ke bawah atau tap overlay
   - Figma: Frame transition "Move In from Bottom"
```

### Transisi Antar Layar

| Jenis Transisi | Kapan Dipakai | Figma Setting |
|---|---|---|
| **Push Right** | Navigasi maju (next page) | Move In, Right |
| **Push Left** | Navigasi balik (back) | Move In, Left |
| **Slide Up** | Open modal/bottom sheet | Move In, Bottom |
| **Slide Down** | Close modal | Move Out, Bottom |
| **Dissolve/Fade** | Tab switch, state change | Dissolve |
| **Smart Animate** | State change dalam komponen | Smart Animate |
| **Instant** | Loading state | Instant |

### Persiapan Prototipe untuk User Testing

Prototipe untuk UT punya requirements khusus:

```
CHECKLIST PROTOTIPE UNTUK USABILITY TESTING:

STARTING POINT:
□ Tentukan flow mana yang di-test (1-3 task)
□ Set starting frame untuk setiap task

COVERAGE:
□ Semua layar dalam alur task sudah dibuat
□ Error state utama sudah ada
□ Success state jelas

FIDELITY:
□ Semua konten dummy realistis (bukan Lorem Ipsum)
□ Semua foto/gambar sudah ada (bukan placeholder)
□ Animasi tidak terlalu lambat / terlalu cepat

TEKNIS:
□ Test prototipe sendiri dari awal sampai akhir
□ Coba di device yang akan dipakai partisipan
□ Pastikan semua tap area cukup besar untuk jari
□ Backup prototipe (link Figma + exported PNG set)

KONEKSI:
□ Setiap tombol navigasi sudah terhubung
□ Overlay (modal) bisa di-dismiss
□ Back button bekerja dengan benar
```

---

## 🧠 Ilustrasi / Analogi

**Hi-Fi Prototype seperti film pendek:**
* Lo-Fi = storyboard/sketsa cerita
* Mid-Fi = animatik (sketsa bergerak)
* Hi-Fi = short film yang sudah diedit dan bisa ditonton

**Micro-interaction seperti bahasa tubuh:**
* Bicara tanpa ekspresi dan gerakan → terasa kaku
* Anggukan saat setuju, senyum saat menyambut → natural
* UI tanpa micro-interaction → kaku
* UI dengan micro-interaction yang tepat → natural dan responsif

---

## 💻 Panduan: Membuat Animasi yang Baik di Figma

### Smart Animate Tips

```
AGAR SMART ANIMATE BEKERJA OPTIMAL:

1. NAMA LAYER HARUS SAMA persis antara dua frame
   Frame 1: Layer "Button/Primary"
   Frame 2: Layer "Button/Primary" ← nama sama = smooth animation

2. GUNAKAN VARIANTS untuk state yang berubah
   - Button Default ← bukan copy-paste, tapi variant yang sama
   - Button Pressed

3. EASING RECOMMENDATIONS:
   - Tap/Press: Ease Out (0.3s) — cepat, responsif
   - Modal open: Spring 200 stiffness — natural, elastic
   - Page transition: Ease In Out (0.35s) — smooth
   - Fade: Linear (0.2s) — konsisten

4. DURATION:
   - Micro-interaction: 100-200ms (terlalu cepat = tidak terasa)
   - Page transition: 250-350ms (terlalu lambat = terasa berat)
   - Modal/overlay: 300-400ms (perlu sedikit lebih drama)
```

### Perbedaan Prototipe yang Baik vs Buruk

```
❌ PROTOTIPE YANG BURUK:
- Hanya satu alur "happy path" (tidak ada error state)
- Tombol tidak memberikan feedback saat diklik
- Transisi instant (no animation) atau terlalu lambat
- Konten dummy tidak realistis (Lorem Ipsum, 00000)
- Tidak bisa di-dismiss dari modal/overlay
- Berbeda ukuran antara frame-frame (inkonsisten)

✅ PROTOTIPE YANG BAIK:
- Ada happy path + minimal satu error state
- Semua tombol utama memberikan feedback visual
- Transisi natural dan konsisten
- Konten nyata dan relevan
- Semua overlay bisa di-dismiss
- Ukuran frame konsisten di semua halaman
```

---

## 🏢 Studi Kasus: Prototipe untuk Pitch OVO Pay Later

**Konteks:** Tim produk OVO membuat Hi-Fi prototype untuk presentasi ke C-suite sebelum fitur Pay Later dikembangkan.

**Scope prototipe:**
* 12 layar: dari onboarding Pay Later hingga transaksi pertama + confirmation
* 3 flow: apply for Pay Later, bayar dengan Pay Later, lihat tagihan
* Error states: limit tidak cukup, verifikasi KTP gagal

**Keputusan teknis:**
* Smart Animate untuk semua transisi antar halaman — terasa seperti app nyata
* Spring animation untuk bottom sheet — natural feel
* Skeleton loading state sebelum data muncul — realistis
* Real dummy data: nama "Andika Pratama", KTP foto, nominal nyata

**Hasil presentasi:**
* Stakeholder langsung paham UX flow tanpa penjelasan panjang
* Feedback spesifik: "Step 4 terlalu banyak informasi" → langsung diubah dalam 30 menit
* Persetujuan development keluar 3 minggu lebih cepat dari biasanya

---

## ⚠️ Kesalahan Umum

1. **Terlalu banyak animasi** → Setiap tap punya animasi panjang → prototipe terasa lambat dan mengganggu. Micro-interaction untuk feedback, bukan untuk show-off.

2. **Tidak test sendiri sebelum UT** → "Tombol ini tidak terhubung." Temukan ini saat UT adalah momen paling memalukan. Test menyeluruh sebelum sesi.

3. **Frame ukuran tidak konsisten** → Satu layar 375x812, layar lain 375x800. Transisi akan terasa "lompat".

4. **Forgot edge cases** → User klik di luar area yang disambungkan → prototipe "stuck". Pastikan semua area besar (termasuk background) sudah terhubung atau ada fallback.

5. **Konten terlalu panjang** → Teks yang overflow container → terasa tidak profesional. Cek ulang semua konten dengan teks yang mungkin panjang (nama, nomor rekening).

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Mengapa konten yang realistis penting dalam Hi-Fi prototype untuk user testing? Apa yang bisa terjadi jika terlalu banyak placeholder Lorem Ipsum?

b) Jelaskan kapan menggunakan Dissolve vs Smart Animate vs Push transition.

### Soal 2 — Praktik (Tugas)

Buat prototipe Hi-Fi untuk proyek desain kamu:

1. **Layar:** Minimal 8 layar yang membentuk minimal 2 complete user flow
2. **Components:** Gunakan Design System yang sudah dibangun di pertemuan 10-11
3. **Konten:** Semua konten harus realistis — tidak boleh Lorem Ipsum
4. **Animasi:** Minimal:
   * 3 Smart Animate transitions
   * 1 overlay (modal atau bottom sheet) dengan dismiss
   * 1 micro-interaction pada komponen interaktif
5. **Testing Ready:** Test mandiri dari awal sampai akhir setiap flow

---

## 📌 Ringkasan

* **Hi-Fi Prototype** = visual nyata + interaktif, mendekati produk final
* Integrasikan Design System ke layar: ambil komponen, susun sesuai layout, isi konten nyata
* **Konten realistis** = perbedaan antara prototipe yang terasa profesional dan amatir
* **Micro-interactions** = bahasa tubuh UI — button feedback, form validation, loading state
* **Smart Animate** = animasikan layer dengan nama sama antara dua frame
* Transisi: Push untuk navigasi, Dissolve untuk tab switch, Spring untuk modal
* Sebelum Usability Testing: test sendiri dari awal → cover error state → pastikan semua tap area terhubung

---

*📚 Referensi: Figma Learn — help.figma.com | NNGroup — Prototyping | Material Design Motion — m3.material.io/styles/motion*
