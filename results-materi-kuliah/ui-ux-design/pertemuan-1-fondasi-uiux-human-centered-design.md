# Pertemuan 1: Fondasi UI/UX — Human-Centered Design & Double Diamond

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan perbedaan fundamental antara UI dan UX
* Mendeskripsikan 4 fase Double Diamond Design Process
* Mengidentifikasi contoh desain yang baik dan buruk di kehidupan sehari-hari
* Menjelaskan mengapa UX buruk berdampak langsung pada bisnis

---

## 📖 Pengantar (Hook)

Tahun 2013, Healthcare.gov — situs web pendaftaran asuransi kesehatan pemerintah AS — diluncurkan.

Di hari pertama, 250.000 orang mencoba mendaftar. Hanya **6 orang** yang berhasil.

Bukan karena sistemnya down. Bukan karena server overload. Tapi karena **UX yang buruk**: alur yang membingungkan, error message yang tidak membantu, dan proses yang membutuhkan 76 langkah.

Biaya perbaikan: **$500 juta**. Dampak politiknya jauh lebih besar.

Ini bukan soal estetika — ini soal bisnis, kepercayaan, dan nyawa manusia.

---

## 🧩 Konsep Utama

### UI vs UX: Perbedaan Fundamental

Ini adalah salah satu pertanyaan paling sering di interview UI/UX. Banyak orang bingung antara keduanya.

**UI (User Interface)** = Apa yang **terlihat** — tampilan, warna, tombol, ikon, tipografi. Ini adalah "kulit" dari produk digital.

**UX (User Experience)** = Apa yang **dirasakan** — seberapa mudah, menyenangkan, dan efisien pengguna dapat mencapai tujuan mereka. Ini adalah "jiwa" dari produk digital.

```
Analogi terbaik: mobil

UI = desain interior, warna cat, bentuk setir, tampilan dashboard
UX = seberapa mudah mobil dikendarai, seberapa nyaman perjalanannya,
     seberapa intuitif kontrol-kontrolnya
     
Mobil bisa tampil INDAH (UI bagus) tapi menyulitkan dikendarai (UX buruk).
Mobil bisa tampil BIASA (UI sederhana) tapi sangat nyaman dan intuitif (UX bagus).
```

| Aspek | UI | UX |
|---|---|---|
| **Fokus** | Tampilan visual | Pengalaman pengguna |
| **Pertanyaan** | "Seperti apa tampilannya?" | "Seberapa mudah digunakan?" |
| **Tool** | Figma, Photoshop, Illustrator | Figma, Miro, Maze, Google Forms |
| **Output** | Mockup, komponen, style guide | Persona, Journey Map, Prototipe, Laporan UT |
| **Melibatkan** | Visual design, branding | Research, psychology, information architecture |

**Penting:** UI dan UX tidak bisa dipisahkan. UI yang buruk merusak UX. UX yang buruk tidak bisa diselamatkan oleh UI yang cantik.

### Human-Centered Design (HCD)

Human-Centered Design adalah pendekatan desain yang menempatkan **manusia (pengguna) sebagai pusat dari setiap keputusan desain**.

Prinsip HCD menurut IDEO:
1. **Empathy** — Pahami kebutuhan, konteks, dan perasaan pengguna melalui observasi dan wawancara
2. **Define** — Definisikan masalah yang benar-benar dialami pengguna, bukan masalah yang diasumsikan
3. **Ideate** — Ciptakan banyak solusi potensial tanpa menghakimi
4. **Prototype** — Buat representasi cepat dari solusi terbaik
5. **Test** — Uji prototipe dengan pengguna nyata, belajar dari feedback

**Mengapa HCD penting?**
Karena desainer bukanlah pengguna. Kita punya "curse of knowledge" — kita tahu cara menggunakan produk kita sendiri sehingga tidak bisa melihat kebingungan pengguna. HCD memaksa kita keluar dari perspektif sendiri.

### Double Diamond Design Process

Double Diamond adalah framework desain yang dikembangkan oleh British Design Council (2005). Ia menggambarkan proses desain sebagai dua berlian — dua fase yang masing-masing dimulai dengan **divergen** (berpikir luas) lalu **konvergen** (mempersempit fokus).

```
DISCOVER           DEFINE             DEVELOP            DELIVER
(Diverge)        (Converge)          (Diverge)          (Converge)
    ◇─────────────◇                      ◇─────────────◇
   /               \                    /               \
  /   Research &    \                  /   Ideation &    \
 /    Exploration    \                /   Prototyping    \
◇                    ◇──────────────◇                    ◇
                   Problem           
                 Statement           
                                        
FASE 1: DISCOVER          FASE 2: DEFINE
Apa yang terjadi?         Masalah apa yang perlu dipecahkan?
- User interviews         - Affinity diagrams
- Surveys                 - User Personas
- Observations            - User Journey Maps
- Desk research           - HMW statements

FASE 3: DEVELOP           FASE 4: DELIVER
Bagaimana cara memecahkannya?  Solusi mana yang berhasil?
- Crazy 8s ideation       - Usability Testing
- Wireframes              - Iteration
- Prototypes              - Final Design
```

**Kesalahan umum:** Langsung melompat ke fase DEVELOP (desain di Figma) tanpa melewati DISCOVER dan DEFINE. Hasilnya: solusi yang bagus secara visual tapi tidak menyelesaikan masalah yang tepat.

---

## 🧠 Ilustrasi / Analogi

**Analogi Restoran:**

| Komponen Restoran | UI/UX Equivalent |
|---|---|
| Interior, dekorasi, piring cantik | UI — tampilan visual |
| Kemudahan menemukan meja, kejelasan menu, kecepatan pelayanan | UX — pengalaman keseluruhan |
| Survei kepuasan pelanggan | User Research |
| "Bagaimana cara meningkatkan pengalaman makan?" | Problem Statement |
| Mencoba berbagai layout meja | Prototyping |
| Memperhatikan pelanggan saat makan | Usability Observation |

**UX yang buruk dalam kehidupan sehari-hari:**
* Pintu yang tidak jelas mana sisi push dan pull → "Norman Door"
* Remote TV dengan 50 tombol yang jarang digunakan
* Formulir pemerintah yang membutuhkan informasi yang sama di 5 halaman berbeda
* Tombol "Unsubscribe" yang sengaja dibuat kecil dan tersembunyi

---

## 💻 Praktik: Analisis UI/UX Aplikasi Lokal

Tidak ada kode di sesi ini — praktiknya adalah **analisis kritis**.

### Framework Analisis Cepat (5-Why UX)

Ketika melihat desain yang buruk, tanyakan:
1. **Apa** yang sulit digunakan? (Identifikasi masalah spesifik)
2. **Mengapa** ini terjadi? (Root cause)
3. **Siapa** yang terdampak? (Segmen pengguna)
4. **Seberapa parah** dampaknya? (Severity: Fatal → Moderate → Minor)
5. **Apa** solusi yang mungkin?

### Contoh Analisis: Aplikasi Transfer Bank

**Masalah yang diamati:** Pengguna sering salah memasukkan nominal transfer
**Mengapa:** Field input tidak menampilkan format ribuan otomatis
**Siapa terdampak:** Semua pengguna saat transfer
**Severity:** Fatal (salah nominal = kerugian finansial)
**Solusi:** Auto-format nominal (Rp 1.000.000), konfirmasi sebelum submit, batas maksimal visible

---

## 🏢 Studi Kasus Nyata (Fintech / Digital)

#### GoPay: Evolusi UX dari GoJek ke Super App

**2015 — UX Awal:** Tombol pembayaran tersembunyi di dalam fitur order GoJek. Pengguna harus mencari cara top-up. Proses pembayaran membutuhkan banyak langkah.

**2018 — Pemisahan dan Simplifikasi:** GoPay menjadi entitas terpisah dengan tab dedicated. Top-up bisa dari mana saja. Angka saldo selalu visible di home screen.

**2021-sekarang — Super App UX:** Satu layar home dengan hierarki visual yang jelas. Fitur terpopuler (Transfer, Bayar, Tagihan) di posisi paling mudah dijangkau jempol. Onboarding yang dipersonalisasi berdasarkan perilaku pengguna.

**Pelajaran HCD:** Setiap perubahan didahului riset pengguna. Tim UX GoPay melakukan ratusan sesi user interview per tahun untuk memahami bagaimana pengguna Indonesia sebenarnya menggunakan uang digital — termasuk di warung, di pasar tradisional, dan saat sinyal lemah.

**Dampak bisnis:** Monthly Active Users GoPay tumbuh dari ~20 juta (2018) ke 100+ juta (2023). Sebagian besar pertumbuhan dikaitkan dengan improvement UX yang mengurangi friction.

---

## 📊 Visualisasi: Impact of Poor UX

```
BIAYA UX BURUK:

Fase Desain                    Biaya Perbaikan
──────────────────────────────────────────────
Research & Design              1x  (baseline)
                                    ↑
Development                    10x (harus recode)
                                    ↑
Testing                        100x (bug fixing + redesign)
                                    ↑
Post-Launch                    1000x (user support + reputasi + churn)

Rule of thumb: setiap $1 yang diinvestasikan di UX research
menghemat $10-$100 di biaya perbaikan post-launch.
```

---

## ⚠️ Kesalahan Umum

1. **"Aku adalah pengguna, jadi aku tahu apa yang pengguna mau"** → Tidak ada desainer yang merepresentasikan semua pengguna. Riset pengguna adalah satu-satunya cara tahu.

2. **Mendesain untuk diri sendiri, bukan untuk pengguna** → Disebut "self-referential design". Hasilnya: produk yang hanya dapat digunakan oleh desainer dan engineers.

3. **Melewati fase Discover dan Define** → "Kita sudah tahu masalahnya, langsung desain saja." Biasanya menghasilkan solusi yang tepat untuk masalah yang salah.

4. **Mengira UX = UI** → Meng-hire UI designer dan berharap UX-nya otomatis bagus. UX mencakup seluruh pengalaman, termasuk customer service, packaging, dan komunikasi marketing.

---

## 🧪 Latihan

### Soal 1 — Konsep

a) Pilih satu aplikasi yang kamu gunakan sehari-hari. Identifikasi satu masalah UX yang kamu rasakan. Jelaskan mengapa itu adalah masalah UX, bukan hanya masalah UI.

b) Jelaskan perbedaan antara fase Discover dan Define dalam Double Diamond. Mengapa keduanya penting dilakukan secara berurutan?

c) Bayangkan kamu diminta mendesain aplikasi transfer uang untuk lansia berusia 65+. Bagaimana pendekatan HCD akan mengubah keputusan desain kamu dibanding mendesain untuk target umur 20-30?

### Soal 2 — Praktik

Lakukan **Analisis Cepat** pada salah satu aplikasi berikut (pilih satu):
* Mobile banking BCA, Mandiri, atau BRI
* Tokopedia atau Shopee
* Gojek atau Grab

Untuk aplikasi yang dipilih, identifikasi:
1. Satu contoh **UI yang baik** (tampilan yang membantu pengguna)
2. Satu contoh **UX yang baik** (alur atau interaksi yang mudah)
3. Satu contoh **UX yang buruk** (sesuatu yang membingungkan atau menyulitkan)
4. Usulan perbaikan untuk poin 3

---

## 📌 Ringkasan

* **UI** = tampilan visual (apa yang terlihat)
* **UX** = pengalaman pengguna (apa yang dirasakan)
* **HCD** = desain berpusat pada manusia — empati, riset, iterasi
* **Double Diamond:** Discover → Define → Develop → Deliver
* UX buruk bukan sekadar masalah estetika — ini masalah bisnis dengan biaya nyata
* Jangan skip fase Discover dan Define — solusi terbaik dimulai dari memahami masalah yang tepat
* Desainer bukan pengguna — riset pengguna adalah satu-satunya cara tahu kebenaran

---

*📚 Referensi: Norman, D.A. (2013). The Design of Everyday Things | Krug, S. (2014). Don't Make Me Think | Garrett, J.J. (2010). Elements of User Experience*
