# Pertemuan 1: Pengantar Pemrograman — Python, Ekosistem & Setup Lingkungan

---

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

* Menjelaskan apa itu program komputer dan bagaimana cara kerjanya
* Membedakan bahasa pemrograman **interpreted** vs **compiled** dengan bahasa sederhana
* Menyebutkan kelebihan Python dan kenapa Python populer di dunia kerja
* Menginstal Python dan VS Code di komputer kamu
* Menulis dan menjalankan program Python pertama (`Hello, World!`)
* Menggunakan Python REPL untuk eksperimen cepat

---

## 📖 Pengantar (Hook)

Bayangkan kamu sedang belajar masak untuk pertama kalinya.

Kamu tidak langsung masak rendang. Kamu mulai dari: **merebus air, menggoreng telur, membuat nasi goreng sederhana.**

Belajar coding juga sama. Kamu tidak langsung bikin aplikasi fintech atau sistem AI. Kamu mulai dari:

> *"Hei komputer, cetak tulisan ini di layar!"*

Dan itulah yang akan kita lakukan hari ini — menulis perintah pertamamu ke komputer. Sederhana? Ya. Tapi ini adalah **fondasi dari segalanya**.

Di dunia kerja, hampir semua sistem digital — dari aplikasi Tokopedia, sistem transfer GoPay, hingga dashboard monitoring bank — semuanya dimulai dari kode yang ditulis programmer. Programmer adalah orang yang "berbicara" ke komputer dalam bahasa yang dimengerti mesin.

---

## 🧩 Konsep Utama

### Apa Itu Program Komputer?

Program komputer adalah **sekumpulan instruksi** yang diberikan ke komputer agar melakukan sesuatu.

Komputer sangat bodoh — dia hanya bisa melakukan apa yang kamu perintahkan, tidak lebih. Tapi dia sangat cepat dan tidak pernah lelah.

Contoh instruksi program sederhana:
1. Minta pengguna memasukkan nama
2. Simpan nama tersebut
3. Tampilkan pesan "Halo, [nama]!"

### Bahasa Pemrograman: Jembatan Manusia ↔ Mesin

Komputer hanya paham **bahasa mesin** (kode biner: 0 dan 1). Kita tidak bisa langsung nulis `01001000 01100101 01101100...` — terlalu susah!

Bahasa pemrograman adalah **jembatan** antara bahasa manusia dan bahasa mesin. Ada banyak bahasa pemrograman: Python, Java, JavaScript, C++, Go, Rust, dan masih banyak lagi.

### Interpreter vs Compiler

Ini perbedaan penting yang sering ditanyakan di wawancara kerja!

| | **Compiled** | **Interpreted** |
|---|---|---|
| **Cara kerja** | Seluruh kode diterjemahkan dulu jadi file executable, baru dijalankan | Kode diterjemahkan dan dijalankan baris per baris secara langsung |
| **Kecepatan** | Lebih cepat saat dijalankan | Sedikit lebih lambat |
| **Proses** | Compile → File → Jalankan | Langsung jalankan |
| **Contoh bahasa** | C, C++, Go, Rust | Python, JavaScript, Ruby |
| **Keunggulan** | Performa tinggi | Mudah dikembangkan & di-debug |

**Python = Interpreted language.**

Artinya: kamu tulis kode → langsung jalan. Tidak perlu proses compile dulu. Ini membuat Python sangat nyaman untuk belajar dan prototipe cepat.

### Kenapa Python?

Python dipilih sebagai bahasa di kuliah ini bukan tanpa alasan:

* **Sintaksis bersih dan mudah dibaca** — mirip bahasa Inggris sehari-hari
* **Serbaguna** — dipakai untuk web backend, data science, AI, otomasi, scripting
* **Ekosistem library yang luar biasa** — ada library untuk hampir semua kebutuhan
* **Populer di industri** — dipakai oleh Instagram, Spotify, Netflix, Bank Indonesia, Gojek
* **Komunitas besar** — mudah cari bantuan dan tutorial

---

## 🧠 Ilustrasi / Analogi

Supaya lebih mudah dipahami, bayangkan ini:

| Konsep | Analogi Dunia Nyata |
|---|---|
| **Komputer** | Koki yang sangat patuh — hanya masak sesuai resep, tidak pakai inisiatif |
| **Program** | Resep masakan — langkah-langkah yang harus diikuti koki |
| **Bahasa pemrograman** | Bahasa resep (ada yang pakai bahasa Inggris/Indonesia — komputer punya bahasanya sendiri) |
| **Interpreter** | Penerjemah yang menerjemahkan resep kamu ke bahasa koki, kalimat per kalimat, sambil koki memasak |
| **Compiler** | Penerjemah yang menerjemahkan seluruh buku resep dulu jadi buku baru dalam bahasa koki, baru koki mulai masak |
| **Python** | Resep dengan bahasa paling sederhana dan mudah dipahami |

---

## 💻 Contoh Teknis

### Setup Python + VS Code

**Langkah 1: Install Python**
1. Buka https://python.org/downloads
2. Download versi terbaru (Python 3.12+)
3. Saat instalasi di Windows: **centang "Add Python to PATH"** ← ini penting!
4. Verifikasi: buka terminal, ketik `python --version`

**Langkah 2: Install VS Code**
1. Buka https://code.visualstudio.com
2. Install VS Code
3. Buka VS Code → Install ekstensi **"Python"** (by Microsoft)

**Langkah 3: Hello World!**

Buat file baru bernama `hello.py`, tulis kode ini:

```python
# Program pertama saya!
print("Hello, World!")
print("Nama saya: [nama kamu]")
print("Saya siap belajar Python!")
```

Jalankan dengan: klik tombol ▶ di VS Code, atau ketik di terminal:
```bash
python hello.py
```

Output yang muncul:
```
Hello, World!
Nama saya: [nama kamu]
Saya siap belajar Python!
```

**Selamat! Kamu sudah jadi programmer! 🎉**

### Berkenalan dengan REPL

REPL = **Read, Evaluate, Print, Loop** — mode interaktif Python.

Buka terminal, ketik `python` (atau `python3`), lalu coba:

```python
>>> 2 + 2
4
>>> print("Halo!")
Halo!
>>> nama = "Budi"
>>> print("Selamat datang,", nama)
Selamat datang, Budi
>>> 10 * 5
50
```

REPL sangat berguna untuk eksperimen cepat tanpa harus bikin file baru.

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

#### Kenapa Backend Bank Pakai Python?

Bayangkan kamu kerja sebagai backend engineer di sebuah startup fintech. Tugas kamu hari pertama:

> *"Buat script untuk memproses laporan transaksi harian dari 50.000 nasabah."*

File CSV berisi 50.000 baris data transaksi. Script Python kamu harus:
1. Membaca file CSV
2. Menghitung total transaksi per nasabah
3. Menandai transaksi yang mencurigakan (> Rp 50 juta dalam sehari)
4. Menyimpan hasil ke file laporan baru

**Masalah jika tidak ada koding:**
- Tim harus buka Excel manual → 50.000 baris → 8 jam kerja, risiko human error
- Tidak bisa diulang setiap hari secara otomatis

**Solusi dengan Python:**
```python
# Pseudocode sederhana
baca_file("transaksi_hari_ini.csv")
untuk_setiap_nasabah:
    hitung_total_transaksi()
    jika total > 50_000_000:
        tandai_mencurigakan()
simpan_laporan("laporan_output.csv")
```

**Hasil:** proses 50.000 baris selesai dalam **3 detik**. Script ini bisa dijalankan otomatis setiap tengah malam.

**Dampak bisnis:** Efisiensi operasional meningkat, risiko fraud terdeteksi lebih cepat, tim analis fokus pada analisis bukan data entry.

→ Ini baru **script sederhana** — tapi dampaknya nyata di dunia kerja. Inilah mengapa belajar programming itu penting.

---

## 📊 Visualisasi

### Cara Kerja Program Python (Flow)

```
Kamu nulis kode .py
        ↓
Python Interpreter membaca baris per baris
        ↓
Interpreter menerjemahkan ke instruksi mesin
        ↓
CPU menjalankan instruksi
        ↓
Output muncul di layar
```

### Perbandingan Ekosistem Bahasa Pemrograman

| Kegunaan | Bahasa Populer |
|---|---|
| Web Backend | Python, Java, Go, Node.js |
| Web Frontend | JavaScript, TypeScript |
| Mobile | Swift (iOS), Kotlin (Android) |
| Data Science / AI | Python, R |
| Sistem / Embedded | C, C++, Rust |
| Scripting & Otomasi | Python, Bash |

**Python ada di mana-mana** — itulah kenapa dia jadi pilihan pertama untuk belajar.

---

## ⚠️ Kesalahan Umum

1. **Lupa centang "Add Python to PATH" saat instalasi di Windows**
   → Akibat: perintah `python` tidak dikenali di terminal
   → Solusi: uninstall dan install ulang Python, centang opsi tersebut

2. **Bingung antara `python` vs `python3`**
   → Di beberapa sistem (terutama Mac/Linux), `python` = Python 2 (lama), gunakan `python3`
   → Di kuliah ini, kita pakai Python 3.x

3. **Lupa simpan file sebelum menjalankan**
   → VS Code menampilkan titik (●) di tab jika ada perubahan yang belum disimpan
   → Shortcut: `Ctrl+S` (Windows) / `Cmd+S` (Mac)

4. **Miskonsepsi: "Python lambat, jadi tidak dipakai di industri"**
   → Salah! Instagram, Pinterest, Dropbox backend-nya pakai Python
   → Untuk task yang butuh performa ekstrem, Python bisa "memanggil" kode C/C++ via library
   → Python unggul di produktivitas developer — kode lebih cepat ditulis

5. **Takut salah**
   → Di REPL, salah ketik tidak merusak apapun. Komputer tidak marah.
   → Error message adalah teman — dia memberitahu kamu apa yang salah

---

## 🧪 Latihan / Studi Kasus

### Soal 1 — Konsep

Jawab dengan bahasa kamu sendiri:

a) Apa perbedaan utama antara bahasa pemrograman **compiled** dan **interpreted**? Berikan 1 contoh bahasa untuk masing-masing!

b) Sebutkan 3 alasan kenapa Python cocok digunakan sebagai bahasa pertama untuk belajar pemrograman!

c) Apa yang dimaksud dengan REPL? Kapan kamu akan menggunakannya?

---

### Soal 2 — Studi Kasus

Kamu bekerja sebagai junior developer di sebuah toko online. Setiap hari, toko ini menerima rata-rata 500 pesanan. Saat ini, admin mencatat semua pesanan secara manual di spreadsheet.

**Pertanyaan:**
1. Apa masalah yang muncul jika sistem ini terus dipertahankan seiring pertumbuhan bisnis?
2. Bagaimana program komputer bisa membantu menyelesaikan masalah ini?
3. Fitur apa saja yang menurut kamu dibutuhkan dari program tersebut? (tuliskan dalam bentuk poin-poin sederhana, bukan kode)

---

### Soal 3 — Praktik (Wajib Dicoba!)

1. Install Python dan VS Code di komputermu (jika belum ada)
2. Buat file `perkenalan.py` yang menampilkan:
   ```
   Nama    : [nama kamu]
   Prodi   : Sistem Teknologi dan Informasi
   Semester: 2
   Hobi    : [hobi kamu]
   Motto   : [motto atau kalimat favorit kamu]
   ```
3. Screenshot outputnya dan simpan untuk dikumpulkan!

---

## 📌 Ringkasan

* **Program komputer** = kumpulan instruksi yang diberikan ke komputer
* **Bahasa pemrograman** = jembatan antara manusia dan mesin
* **Compiled** = terjemahkan semua dulu → jalankan (lebih cepat runtime, contoh: C, Go)
* **Interpreted** = terjemahkan dan jalankan baris per baris (lebih fleksibel, contoh: Python)
* **Python** = interpreted, sintaksis bersih, serbaguna, populer di industri
* **Setup:** Python 3.x + VS Code + ekstensi Python
* **Hello World** = program pertama yang membuktikan lingkungan kamu berjalan dengan benar
* **REPL** = mode interaktif Python untuk eksperimen cepat (`python` di terminal)
* Di dunia kerja, bahkan script sederhana bisa menghemat jam kerja dan mengurangi human error

---

*📚 Referensi: Matthes, E. (2023). Python Crash Course, 3rd Ed. | python.org*
