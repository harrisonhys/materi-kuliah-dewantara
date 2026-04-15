# Pertemuan 14: Analisis Hasil UT & Iterasi Desain

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menganalisis data kualitatif dan kuantitatif dari sesi Usability Testing
* Menghitung System Usability Scale (SUS) Score
* Memprioritaskan temuan menggunakan Impact-Effort Matrix
* Melakukan iterasi desain yang terukur dengan dokumentasi before/after

---

## 📖 Pengantar (Hook)

Usability Testing tanpa analisis dan iterasi seperti pergi ke dokter, mendapat diagnosis, tapi tidak minum obat.

Banyak tim melakukan UT, mendapatkan temuan berharga, lalu... tidak melakukan apa-apa. Atau lebih buruk: mendebat temuan yang ada.

Data UT bukan pendapat — ini observasi nyata dari pengguna nyata. Dan mengabaikannya adalah keputusan bisnis yang mahal.

Pertemuan ini adalah tentang bagaimana **mengubah data menjadi desain yang lebih baik**.

---

## 🧩 Konsep Utama

### Analisis Data Kualitatif: Affinity Diagram dari UT

Data kualitatif dari UT (catatan, rekaman, quotes) perlu diorganisir sebelum bisa diinterpretasi.

```
PROSES ANALISIS KUALITATIF:

1. COLLECT
   Kumpulkan semua catatan + Rainbow Sheet dari semua sesi
   Transkrip quotes penting dari rekaman

2. EXTERNALIZE
   Tulis setiap observasi/quote di sticky note terpisah
   "P2: Mencari tombol Transfer 2 menit sebelum ketemu"
   "P1: 'Ini bedanya Kirim sama Transfer apa?'"
   "P4: Tidak membaca teks disclaimer sebelum konfirmasi"

3. CLUSTER
   Kelompokkan observasi yang berkaitan
   Kelompok: "Kebingungan Terminologi"
   Kelompok: "Kesulitan Menemukan Fitur"
   Kelompok: "Skip Langkah Konfirmasi"

4. DERIVE INSIGHT
   Dari setiap cluster → rumuskan insight
   "Pengguna tidak membedakan 'Kirim' vs 'Transfer' karena
   tidak ada penjelasan kontekstual saat pertama kali memakai"
```

### System Usability Scale (SUS)

SUS adalah kuesioner 10 pertanyaan standar industri untuk mengukur usability secara kuantitatif. Diberikan setelah sesi UT selesai.

**10 Pertanyaan SUS:**

| # | Pertanyaan | Skala |
|---|---|---|
| 1 | Saya merasa akan sering menggunakan sistem ini | 1-5 |
| 2 | Saya merasa sistem ini terlalu kompleks | 1-5 |
| 3 | Saya merasa sistem ini mudah digunakan | 1-5 |
| 4 | Saya akan membutuhkan bantuan teknisi untuk menggunakannya | 1-5 |
| 5 | Saya merasa berbagai fungsi terintegrasi dengan baik | 1-5 |
| 6 | Saya merasa terlalu banyak ketidakkonsistenan | 1-5 |
| 7 | Saya pikir pengguna lain akan belajar cepat | 1-5 |
| 8 | Sistem ini sangat tidak praktis | 1-5 |
| 9 | Saya merasa percaya diri menggunakan sistem ini | 1-5 |
| 10 | Saya butuh banyak belajar sebelum bisa menggunakan ini | 1-5 |

**Cara Hitung SUS Score:**

```
RUMUS SUS:
1. Pertanyaan ganjil (1,3,5,7,9): nilai - 1
2. Pertanyaan genap (2,4,6,8,10): 5 - nilai
3. Jumlahkan semua hasil
4. Kalikan dengan 2.5

Contoh satu partisipan:
Q1: 4  → 4-1 = 3
Q2: 2  → 5-2 = 3
Q3: 5  → 5-1 = 4
Q4: 1  → 5-1 = 4
Q5: 4  → 4-1 = 3
Q6: 2  → 5-2 = 3
Q7: 4  → 4-1 = 3
Q8: 1  → 5-1 = 4
Q9: 4  → 4-1 = 3
Q10: 1 → 5-1 = 4
Sum = 34
SUS Score = 34 × 2.5 = 85

INTERPRETASI SUS SCORE:
> 80.3 = Excellent (Grade A) ← target produk yang baik
68–80.3 = Good (Grade B)
51–68   = OK (Grade C)
< 51    = Poor — perlu perbaikan signifikan
```

### Impact-Effort Matrix

Setelah semua temuan terkumpul, prioritaskan menggunakan matriks 2x2:

```
                HIGH IMPACT
                     │
  ┌──────────────────┼──────────────────┐
  │                  │                  │
  │  QUICK WINS  ①   │  BIG BETS    ②  │
  │  Impact tinggi   │  Impact tinggi   │
  │  Effort rendah   │  Effort tinggi   │
  │  → Kerjakan dulu!│  → Plan carefully│
  │                  │                  │
LOW────────────────────────────────────HIGH
EFFORT             │                   EFFORT
  │                  │                  │
  │  DON'T BOTHER ④  │  FILL-INS    ③  │
  │  Impact rendah   │  Impact rendah   │
  │  Effort rendah   │  Effort tinggi   │
  │  → Mungkin skip  │  → Avoid         │
  │                  │                  │
  └──────────────────┼──────────────────┘
                     │
                LOW IMPACT

PETAKAN SETIAP TEMUAN KE MATRIKS:
① Quick Win: Ganti label "Transfer" → "Kirim Uang ke Bank" (teks saja)
② Big Bet: Redesign full onboarding flow untuk pengguna baru
③ Fill-in: Tambahkan animasi di halaman loading
④ Don't Bother: Ubah warna ikon yang sudah 3 dari 5 pengguna suka
```

### Dokumentasi Before/After

Setiap perubahan desain harus terdokumentasi dengan jelas:

```
FORMAT DOKUMENTASI ITERASI:

TEMUAN:
"3 dari 5 partisipan tidak menemukan tombol 'Topup' — 
mereka mencari di menu lain selama >30 detik"

ROOT CAUSE:
Tombol "Top Up" tersembunyi di bawah scroll, tidak visible 
tanpa scroll di mobile. Pengguna mengira halaman sudah habis.

SEBELUM (screenshot before):
[gambar wireframe/hi-fi sebelum perubahan]
Topup button ada di bawah fold, tidak ada visual indicator
bahwa ada konten di bawah

PERUBAHAN YANG DILAKUKAN:
1. Pindahkan tombol Top Up ke area above the fold
2. Tambahkan Quick Action row di bawah saldo dengan 3 aksi utama

SETELAH (screenshot after):
[gambar desain setelah perubahan]
Quick actions (Transfer, Top Up, Bayar) visible langsung

EXPECTED IMPACT:
Task completion rate untuk Top Up naik dari 60% ke 90%+
```

---

## 🧠 Ilustrasi / Analogi

**Analisis UT seperti diagnosa dokter setelah hasil lab:**
* Hasil lab = data UT (kualitatif + SUS score)
* Diagnosa = insight dari analisis
* Resep = perubahan desain (iterasi)
* Kontrol = UT ulang setelah iterasi

**Impact-Effort Matrix seperti antrian triage di UGD:**
* Quick Win = kondisi darurat yang mudah ditangani → masuk dulu
* Big Bet = operasi besar yang perlu direncanakan matang
* Fill-in = kondisi ringan tapi perlu waktu lama → tunggu
* Don't Bother = tidak gawat dan tidak worth effort

---

## 💻 Praktik: Template Laporan UT

```
LAPORAN USABILITY TESTING
[Nama Proyek] | [Tanggal]

1. RINGKASAN EKSEKUTIF
   SUS Score Rata-rata: [X]/100 — [Grade]
   Task Completion Rate: Task 1: X%, Task 2: X%, Task 3: X%
   Jumlah Temuan Kritis: X
   Jumlah Temuan Mayor: X
   Jumlah Temuan Minor: X

2. METODOLOGI
   Tipe Testing: Moderated, 1-on-1
   Jumlah Partisipan: 5
   Durasi per Sesi: 45-60 menit
   Tool: Figma Prototype + Zoom/Offline

3. PROFIL PARTISIPAN
   [Tabel: Inisial, Usia, Pekerjaan, Tech-savviness]

4. TEMUAN DETAIL (per task)
   Task 1: [nama task]
   - Completion rate: X/5
   - Waktu rata-rata: X menit
   - Temuan utama: [deskripsi + frekuensi]
   
5. IMPACT-EFFORT MATRIX
   [Diagram 2x2 dengan semua temuan terpetakan]

6. REKOMENDASI ITERASI
   [Prioritas 1-3: Quick Wins]
   [Prioritas 4-6: Big Bets untuk sprint berikutnya]

7. SEBELUM vs SESUDAH
   [Screenshot per temuan yang sudah diiterasi]

8. NEXT STEPS
   - UT ulang untuk validasi iterasi
   - Metric yang akan dipantau post-launch
```

---

## 🏢 Studi Kasus: Iterasi OVO Berdasarkan UT

**Context:** Setelah UT fitur "OVO PayLater", tim mendapatkan SUS score rata-rata 58 (Grade C).

**Top 3 Temuan dari Impact-Effort Matrix:**

**#1 — Quick Win (Impact tinggi, Effort rendah):**
* Temuan: 4/5 pengguna tidak tahu berapa sisa limit
* Iterasi: Tambahkan widget "Sisa Limit: Rp X" di halaman PayLater
* Effort: ~2 jam desain, ~1 hari development

**#2 — Quick Win:**
* Temuan: 3/5 pengguna tidak sadar tombol konfirmasi ada di bawah (perlu scroll)
* Iterasi: Sticky CTA button yang selalu visible di bawah layar
* Effort: ~1 jam desain, ~4 jam development

**#3 — Big Bet (Impact tinggi, Effort tinggi):**
* Temuan: 5/5 pengguna tidak paham mekanisme cicilan dan tanggal tagih
* Iterasi: Redesign full onboarding PayLater dengan step-by-step explanation dan simulator cicilan
* Effort: ~2 minggu desain + development

**Hasil setelah iterasi Quick Wins:**
* SUS Score naik dari 58 ke 74 (Grade B)
* Task completion rate naik dari 55% ke 83%

---

## ⚠️ Kesalahan Umum

1. **Bereaksi terhadap setiap temuan tanpa prioritisasi** → Tidak semua temuan perlu diiterasi. Impact-Effort Matrix membantu fokus.

2. **Iterasi tanpa dokumentasi before/after** → Tidak bisa membuktikan improvement. Selalu screenshot sebelum dan sesudah.

3. **Tidak validasi iterasi dengan UT lagi** → Iterasi bisa menciptakan masalah baru. UT ulang (bisa lebih singkat) untuk validasi.

4. **Mendebat temuan UT** → "Tapi pengguna itu tidak representatif!" → 4 dari 5 pengguna mengalami masalah yang sama adalah data valid, bukan anekdot.

5. **Laporan UT terlalu panjang** → Stakeholder tidak baca laporan 50 halaman. Executive summary 1 halaman + detail temuan > laporan lengkap yang diabaikan.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Bagaimana kamu menghitung SUS Score? Jika rata-rata SUS score dari 5 partisipan adalah 64, apa artinya dan apa yang harus dilakukan?

b) Jelaskan perbedaan antara temuan di kuadran "Quick Win" dan "Big Bet" dalam Impact-Effort Matrix. Berikan masing-masing satu contoh dari konteks aplikasi fintech.

### Soal 2 — Praktik (Tugas)

Berdasarkan sesi UT yang dilakukan di pertemuan 13:

1. **SUS Score:**
   * Minta setiap partisipan mengisi kuesioner SUS setelah sesi
   * Hitung SUS Score individu dan rata-rata
   * Interpretasikan hasilnya

2. **Analisis:**
   * Pindahkan semua temuan dari Rainbow Sheet ke Affinity Diagram
   * Kelompokkan temuan ke dalam tema
   * Petakan ke Impact-Effort Matrix

3. **Iterasi:**
   * Pilih minimal 3 temuan prioritas untuk diiterasi
   * Lakukan iterasi di Figma
   * Dokumentasikan before/after untuk setiap iterasi

4. **Laporan:**
   * Buat laporan UT ringkas (maksimal 3 halaman) dengan semua elemen di atas

---

## 📌 Ringkasan

* **Data kualitatif** dari UT → proses melalui Affinity Diagram → cluster → insight
* **SUS Score** = kuesioner 10 pertanyaan, skor 0-100. Target: >80 (Grade A)
* **Impact-Effort Matrix** = alat prioritisasi: Quick Wins dulu, Big Bets direncanakan matang
* Setiap iterasi wajib disertai **dokumentasi before/after + root cause + expected impact**
* Laporan UT: ringkas untuk stakeholder, detail untuk tim desain
* **Validasi iterasi** dengan UT ulang — jangan asumsi iterasi sudah benar

---

*📚 Referensi: Krug, S. (2014). Don't Make Me Think | Nielsen Norman Group — SUS Scoring | UX Collective — Impact Effort Matrix*
