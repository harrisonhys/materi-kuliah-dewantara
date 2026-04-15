# Pertemuan 14: Perencanaan Proyek Mini

## 🎯 Learning Outcomes

Setelah belajar ini, kamu akan bisa:

- Melakukan **requirement analysis** sederhana untuk program Python yang akan dibuat
- Mendesain program menggunakan model **IPO (Input-Process-Output)** secara terstruktur
- Menulis **pseudocode** dan menggambarkan **flowchart** proyek secara jelas
- Menyusun **struktur folder** proyek Python yang rapi dan profesional
- Menulis **docstring** dan komentar yang informatif dan berguna
- Membagi tugas secara efektif dalam **tim kecil** (2–4 orang)
- Menggunakan **Git dasar** (init, add, commit, push) untuk mengelola kode bersama
- Memilih topik proyek mini yang sesuai dengan kemampuan dan waktu yang tersedia

---

## 📖 Pengantar (Hook)

Bayangkan kamu diminta bos untuk bangun rumah. Kamu punya tukang, material, dan waktu 2 minggu. Tapi kamu langsung mulai nyusun bata tanpa gambar desain dulu — hasilnya? Ruang tamu ternyata lebih kecil dari toilet, pintu kamar mandi menghadap ke arah yang salah, dan tidak ada colokan listrik di dapur.

Itulah yang sering terjadi ketika programmer langsung nulis kode tanpa perencanaan.

Di industri tech nyata, bahkan startup sekecil apapun selalu mulai dengan **requirement gathering** sebelum baris kode pertama ditulis. Di perusahaan fintech seperti GoPay, OVO, atau Jenius, setiap fitur baru melalui proses: *apa yang dibutuhkan user? → bagaimana sistemnya bekerja? → siapa yang mengerjakan apa?* — baru kemudian coding.

Pertemuan ini bukan tentang nulis kode. Ini tentang belajar **berpikir seperti software engineer** yang sesungguhnya: plan first, code second.

---

## 🧩 Konsep Utama

### 1. Requirement Analysis (Analisis Kebutuhan)

Requirement analysis adalah proses **memahami apa yang harus dilakukan program** sebelum menulis kode. Ada dua jenis:

- **Functional Requirements**: Program harus *bisa melakukan apa*?
  - Contoh: "Program harus bisa menyimpan data transaksi"
  - Contoh: "User bisa login dengan PIN 6 digit"

- **Non-Functional Requirements**: Program harus *bekerja dengan kualitas seperti apa*?
  - Contoh: "Program harus menampilkan pesan error yang jelas"
  - Contoh: "Kode harus mudah dibaca dan diberi komentar"

**Cara melakukan requirement analysis sederhana:**
1. Tanya: *Siapa yang akan menggunakan program ini?* (User persona)
2. Tanya: *Apa saja yang bisa dilakukan user di program ini?* (User stories)
3. Tanya: *Apa yang terjadi kalau ada input salah?* (Edge cases)
4. Tuliskan semua jawaban sebelum mulai coding

---

### 2. Model IPO (Input-Process-Output)

IPO adalah cara sederhana untuk memetakan alur kerja sebuah program:

| Komponen | Pertanyaan | Contoh (ATM) |
|----------|------------|--------------|
| **Input** | Data apa yang masuk ke program? | Nomor rekening, PIN, jumlah tarik |
| **Process** | Program melakukan apa dengan data itu? | Verifikasi PIN, cek saldo, kurangi saldo |
| **Output** | Apa yang dihasilkan/ditampilkan? | Konfirmasi transaksi, saldo terkini, struk |

Desain IPO membantu kamu memastikan tidak ada langkah yang terlewat sebelum mulai kode.

---

### 3. Pseudocode

Pseudocode adalah **deskripsi logika program menggunakan bahasa manusia** — bukan bahasa Python, bukan bahasa Indonesia murni, tapi campuran keduanya yang logis.

**Aturan pseudocode yang baik:**
- Gunakan kata kunci seperti `IF`, `ELSE`, `WHILE`, `FOR`, `INPUT`, `OUTPUT`
- Indentasi untuk menunjukkan blok/hirarki
- Cukup detail untuk dipahami, tidak perlu syntax Python yang exact
- Fokus pada *logika*, bukan *sintaks*

---

### 4. Flowchart

Flowchart adalah representasi **visual** dari alur program. Simbol dasar:

| Simbol | Bentuk | Digunakan untuk |
|--------|--------|-----------------|
| **Start/End** | Oval/Elips | Titik mulai dan selesai |
| **Proses** | Persegi panjang | Operasi/perhitungan |
| **Keputusan** | Belah ketupat | Kondisi IF/ELSE |
| **Input/Output** | Jajar genjang | Menerima/menampilkan data |
| **Panah** | Arrow | Alur proses |

---

### 5. Struktur Folder Proyek Python

Struktur folder yang baik membuat proyek mudah dipahami orang lain (dan dirimu sendiri 2 bulan kemudian):

```
nama-proyek/
│
├── main.py              # Entry point utama program
├── README.md            # Dokumentasi proyek
├── requirements.txt     # Library yang dibutuhkan (jika ada)
│
├── src/                 # Source code utama
│   ├── __init__.py
│   ├── utils.py         # Fungsi-fungsi helper
│   └── core.py          # Logika utama program
│
├── data/                # File data (CSV, JSON, txt)
│   └── contoh_data.txt
│
└── tests/               # File testing (opsional untuk proyek mini)
    └── test_core.py
```

Untuk proyek mini sederhana, struktur minimal yang bisa diterima:

```
nama-proyek/
├── main.py
├── README.md
└── data/
    └── (file data jika ada)
```

---

### 6. Docstring dan Komentar

**Komentar (`#`)** — penjelasan singkat untuk satu baris atau blok kode:
```python
# Validasi PIN harus 6 digit
if len(pin) != 6:
    print("PIN tidak valid")
```

**Docstring (`"""..."""`)** — dokumentasi untuk fungsi, class, atau modul:
```python
def hitung_bunga(pokok, rate, tahun):
    """
    Menghitung bunga sederhana.

    Args:
        pokok (float): Jumlah pinjaman awal
        rate (float): Suku bunga per tahun (dalam desimal, misal 0.05 untuk 5%)
        tahun (int): Lama pinjaman dalam tahun

    Returns:
        float: Total bunga yang harus dibayar
    
    Example:
        >>> hitung_bunga(1000000, 0.05, 2)
        100000.0
    """
    return pokok * rate * tahun
```

---

### 7. GitHub Dasar

Git adalah sistem **version control** — alat untuk melacak perubahan kode dan berkolaborasi dalam tim.

**Alur kerja Git dasar:**

```bash
# 1. Inisialisasi repository baru
git init

# 2. Tambahkan file ke staging area
git add nama_file.py
# atau tambah semua file sekaligus
git add .

# 3. Simpan perubahan dengan pesan deskriptif
git commit -m "Tambah fungsi login dan validasi PIN"

# 4. Kirim ke GitHub (setelah buat repo di github.com)
git remote add origin https://github.com/username/nama-repo.git
git push -u origin main
```

**Tips commit message yang baik:**
- Gunakan kata kerja present tense: "Tambah", "Perbaiki", "Hapus"
- Spesifik dan deskriptif: ❌ "update file" ✅ "Perbaiki bug validasi PIN yang salah hitung digit"
- Maksimal 72 karakter untuk baris pertama

---

### 8. Pembagian Tugas dalam Tim

Untuk proyek mini dengan tim 2–4 orang, gunakan pendekatan sederhana:

| Peran | Tanggung Jawab |
|-------|----------------|
| **Project Lead** | Koordinasi tim, pastikan semua bagian nyambung, testing final |
| **Developer 1** | Implementasi fitur utama / core logic |
| **Developer 2** | Input/Output handling, tampilan user interface (CLI) |
| **Dokumentasi** | README, komentar kode, presentasi (bisa dirangkap) |

**Tips kolaborasi tim:**
- Buat kesepakatan naming convention (nama variabel, fungsi)
- Tetapkan deadline internal 2 hari sebelum deadline asli
- Gunakan Git untuk menghindari konflik kode
- Lakukan code review sederhana: tunjukkan kode ke anggota lain sebelum merge

---

## 🧠 Ilustrasi / Analogi

### Analogi: Membangun Proyek seperti Memasak di Restoran

| Fase Proyek | Analogi Restoran |
|-------------|-----------------|
| Requirement Analysis | Menentukan menu yang akan dijual dan siapa target pelanggan |
| Desain IPO | Menulis resep: bahan (input) → cara masak (process) → hidangan (output) |
| Pseudocode | Instruksi dapur: urutan langkah memasak secara detail |
| Flowchart | Diagram alur dapur: siapa yang masak apa, kapan, dan urutannya |
| Struktur Folder | Layout dapur: kulkas, kompor, meja penyajian — semuanya punya tempat |
| Git | Buku resep tim: setiap perubahan resep dicatat, siapa yang mengubah apa |
| Pembagian Tugas | Chef, sous chef, pelayan — masing-masing peran jelas |

---

## 💻 Template Struktur Proyek & Pseudocode

> Di pertemuan ini, tidak ada "contoh kode Python" biasa. Sebagai gantinya, kamu akan melihat **template pseudocode** dan **struktur proyek** yang bisa langsung digunakan.

### Template Requirement Analysis

```
NAMA PROYEK: [Nama Proyek Kamu]
ANGGOTA TIM: [Nama 1], [Nama 2], ...
TANGGAL: [Tanggal]

=== DESKRIPSI PROGRAM ===
[1-2 kalimat tentang apa yang dilakukan program ini]

=== USER PERSONA ===
Siapa yang akan menggunakan program ini?
- [Deskripsi user]

=== FUNCTIONAL REQUIREMENTS ===
Program HARUS bisa:
1. [Fitur 1]
2. [Fitur 2]
3. [Fitur 3]

Program TIDAK PERLU bisa:
1. [Batasan 1]

=== DESAIN IPO ===
INPUT:
- [Data/input apa yang diterima program?]

PROCESS:
- [Apa yang program lakukan dengan input tersebut?]

OUTPUT:
- [Apa yang ditampilkan/disimpan?]

=== EDGE CASES ===
Apa yang terjadi jika:
- User memasukkan input kosong? → [Solusi]
- User memasukkan tipe data salah? → [Solusi]
- [Kasus khusus lain]? → [Solusi]
```

---

### Template Pseudocode

```
PROGRAM: [Nama Program]

MULAI

  TAMPILKAN menu utama
  INPUT pilihan_user

  SELAGI pilihan_user != "keluar":
    
    JIKA pilihan_user == "1":
      PANGGIL fungsi_fitur_1()
    
    JIKA TIDAK pilihan_user == "2":
      PANGGIL fungsi_fitur_2()
    
    JIKA TIDAK:
      TAMPILKAN "Pilihan tidak valid"
    
    TAMPILKAN menu utama lagi
    INPUT pilihan_user baru

  TAMPILKAN "Terima kasih, program selesai"

SELESAI

---

FUNGSI fungsi_fitur_1():
  INPUT data_yang_dibutuhkan
  VALIDASI data_yang_dibutuhkan
  
  JIKA data valid:
    PROSES data
    TAMPILKAN hasil
  JIKA TIDAK:
    TAMPILKAN pesan error
    KEMBALI ke menu
```

---

## 🏦 Studi Kasus Nyata (Fintech / Backend)

### Proyek: Simulasi ATM Sederhana — Dari Nol Sampai Siap Coding

**Konteks:** Tim kecil (3 orang mahasiswa) diminta membuat program simulasi ATM sederhana menggunakan Python CLI dalam waktu 1 minggu. Program ini harus mencerminkan cara kerja nyata ATM, meskipun dalam skala kecil.

---

#### Tahap 1: Requirement Analysis

```
NAMA PROYEK: Simulasi ATM Sederhana
ANGGOTA TIM: Budi (Lead), Sari (Dev), Anton (Docs)
TANGGAL: 15 April 2026

=== DESKRIPSI PROGRAM ===
Program simulasi ATM berbasis teks (CLI) yang memungkinkan user
melakukan operasi perbankan dasar: cek saldo, tarik tunai, dan
transfer antar rekening.

=== USER PERSONA ===
Nasabah bank yang ingin melakukan transaksi mandiri tanpa teller.

=== FUNCTIONAL REQUIREMENTS ===
Program HARUS bisa:
1. Menerima login dengan nomor rekening dan PIN (4 digit)
2. Menampilkan saldo rekening
3. Menarik tunai (dengan validasi saldo mencukupi)
4. Transfer ke rekening lain (dengan validasi rekening tujuan)
5. Menampilkan riwayat 5 transaksi terakhir
6. Logout dengan aman

Program TIDAK PERLU bisa:
1. Koneksi ke database nyata (gunakan dictionary Python)
2. Menangani multiple user login bersamaan
3. Enkripsi PIN (bukan scope proyek ini)

=== DESAIN IPO ===
INPUT:
- Nomor rekening (string, 10 digit)
- PIN (string, 4 digit)
- Pilihan menu (integer, 1-5)
- Jumlah uang untuk tarik/transfer (float)
- Nomor rekening tujuan transfer (string)

PROCESS:
- Verifikasi nomor rekening dan PIN
- Cek saldo mencukupi sebelum transaksi
- Update saldo rekening sumber dan tujuan
- Catat transaksi ke riwayat

OUTPUT:
- Pesan selamat datang + nama nasabah
- Informasi saldo terkini
- Konfirmasi transaksi berhasil/gagal
- Struk transaksi (tampil di layar)
- Riwayat transaksi

=== EDGE CASES ===
- PIN salah → tampilkan "PIN salah", maksimal 3 kali percobaan
- Saldo tidak cukup → tampilkan "Saldo tidak mencukupi"
- Rekening tujuan tidak ada → tampilkan "Rekening tidak ditemukan"
- Input jumlah negatif → tampilkan "Jumlah tidak valid"
- Input bukan angka → tangani dengan exception handling
```

---

#### Tahap 2: Desain IPO (Diagram)

```
┌─────────────────────────────────────────────────────┐
│                    SIMULASI ATM                      │
├──────────────┬──────────────────┬───────────────────┤
│    INPUT     │    PROCESS       │     OUTPUT        │
├──────────────┼──────────────────┼───────────────────┤
│ No. Rekening │ Validasi login   │ Sambutan user     │
│ PIN          │ Cek PIN (maks 3x)│ Menu utama        │
│ Pilihan menu │                  │                   │
├──────────────┼──────────────────┼───────────────────┤
│ (Cek Saldo)  │ Ambil data saldo │ Tampilkan saldo   │
│ -            │ dari "database"  │ + timestamp       │
├──────────────┼──────────────────┼───────────────────┤
│ Jumlah tarik │ Cek saldo cukup  │ Konfirmasi/error  │
│              │ Kurangi saldo    │ Saldo baru        │
├──────────────┼──────────────────┼───────────────────┤
│ No. rek tuj. │ Validasi rek tuj │ Konfirmasi        │
│ Jumlah trans.│ Update 2 saldo   │ transfer          │
│              │ Catat riwayat    │ Struk transaksi   │
└──────────────┴──────────────────┴───────────────────┘
```

---

#### Tahap 3: Pseudocode Lengkap

```
PROGRAM: Simulasi ATM Sederhana

=== DATA ===
DATABASE_NASABAH = {
  "1234567890": {nama: "Budi", pin: "1234", saldo: 5000000, riwayat: []},
  "0987654321": {nama: "Sari", pin: "5678", saldo: 3000000, riwayat: []}
}

=== MAIN PROGRAM ===
MULAI
  TAMPILKAN "Selamat datang di ATM Simulator"
  
  nasabah = PANGGIL proses_login()
  
  JIKA nasabah berhasil login:
    PANGGIL menu_utama(nasabah)
  JIKA TIDAK:
    TAMPILKAN "Kartu diblokir. Hubungi bank."
  
  TAMPILKAN "Terima kasih. Sampai jumpa!"
SELESAI

=== FUNGSI proses_login() ===
  percobaan = 0
  
  INPUT no_rekening
  CARI no_rekening di DATABASE_NASABAH
  
  JIKA tidak ditemukan:
    TAMPILKAN "Rekening tidak ditemukan"
    RETURN None
  
  SELAGI percobaan < 3:
    INPUT pin
    JIKA pin == pin_di_database:
      RETURN data_nasabah
    JIKA TIDAK:
      percobaan += 1
      TAMPILKAN "PIN salah. Sisa percobaan: {3 - percobaan}"
  
  RETURN None  # Semua percobaan habis

=== FUNGSI menu_utama(nasabah) ===
  berjalan = True
  
  SELAGI berjalan:
    TAMPILKAN menu:
      1. Cek Saldo
      2. Tarik Tunai
      3. Transfer
      4. Riwayat Transaksi
      5. Keluar
    
    INPUT pilihan
    
    JIKA pilihan == 1: PANGGIL cek_saldo(nasabah)
    JIKA pilihan == 2: PANGGIL tarik_tunai(nasabah)
    JIKA pilihan == 3: PANGGIL transfer(nasabah)
    JIKA pilihan == 4: PANGGIL lihat_riwayat(nasabah)
    JIKA pilihan == 5: berjalan = False
    JIKA TIDAK: TAMPILKAN "Pilihan tidak valid"

=== FUNGSI tarik_tunai(nasabah) ===
  INPUT jumlah
  VALIDASI jumlah > 0 dan merupakan angka
  
  JIKA nasabah.saldo >= jumlah:
    nasabah.saldo -= jumlah
    CATAT transaksi ke nasabah.riwayat
    TAMPILKAN struk penarikan
  JIKA TIDAK:
    TAMPILKAN "Saldo tidak mencukupi"
```

---

#### Tahap 4: Pembagian Tugas Tim

| Tugas | Penanggung Jawab | Deadline | Status |
|-------|-----------------|----------|--------|
| Requirement Analysis & Desain IPO | Semua (diskusi) | Hari 1 | ✅ |
| Setup struktur folder & Git repo | Budi | Hari 1 | ✅ |
| Fungsi `proses_login()` | Budi | Hari 2 | 🔄 |
| Fungsi `cek_saldo()` dan `lihat_riwayat()` | Sari | Hari 2 | 🔄 |
| Fungsi `tarik_tunai()` dan `transfer()` | Sari | Hari 3 | ⬜ |
| Exception handling & validasi input | Budi | Hari 4 | ⬜ |
| README.md dan docstring | Anton | Hari 4-5 | ⬜ |
| Testing & bug fix | Semua | Hari 6 | ⬜ |
| Final review & presentasi | Semua | Hari 7 | ⬜ |

**Catatan bisnis:** Di fintech nyata, bahkan untuk fitur sekecil "ganti PIN", proses planning seperti ini dilakukan secara lebih formal dengan dokumen Product Requirements Document (PRD). Kenapa? Karena bug di sistem keuangan bisa berarti kerugian finansial nyata — misalnya bug di fitur transfer yang membuat saldo tidak berkurang tapi uang sudah terkirim (ini pernah terjadi di beberapa platform e-wallet kecil!).

---

## 📊 Visualisasi

### Flowchart Alur Login ATM

```
        [MULAI]
           |
           v
    [Input No. Rekening]
           |
           v
    <Rekening Ada?> --TIDAK--> [Tampilkan "Rekening tidak ditemukan"] --> [SELESAI]
           |
          YA
           |
           v
    [Input PIN] <-----------+
           |                |
           v                |
    <PIN Benar?> --TIDAK--> [percobaan++]
           |                |
          YA          <percobaan < 3?> --TIDAK--> [Blokir] --> [SELESAI]
           |                |
           v               YA
    [Masuk Menu]            |
           |                +---- (kembali ke Input PIN)
           v
    [Tampilkan Menu]
           |
    [Input Pilihan] --> ... (lanjut ke menu masing-masing)
```

### Checklist Sebelum Mulai Coding

```
[ ] Requirement analysis sudah selesai dan disetujui semua anggota
[ ] Desain IPO sudah dibuat untuk setiap fitur utama
[ ] Pseudocode sudah ditulis untuk fungsi-fungsi utama
[ ] Struktur folder sudah dibuat
[ ] Git repo sudah di-init dan dihubungkan ke GitHub
[ ] Pembagian tugas sudah jelas dan deadline sudah disepakati
[ ] Naming convention sudah disepakati (snake_case untuk Python)
[ ] Bahasa komentar/docstring sudah disepakati (Indonesia atau Inggris)
```

---

## ⚠️ Kesalahan Umum

### 1. Langsung coding tanpa planning
**Masalah:** Kode jadi tidak terstruktur, banyak fungsi yang overlap, sulit dibagi ke anggota tim.
**Solusi:** Luangkan minimal 30 menit untuk planning sebelum buka code editor.

### 2. Pseudocode terlalu detail (jadi kode Python)
**Masalah:** "Pseudocode" berisi syntax Python yang exact — ini bukan pseudocode, ini coding setengah jadi.
**Solusi:** Pseudocode harus bisa dibaca orang yang tidak tahu Python. Kalau ada tanda titik dua `:` dan indentasi Python, terlalu detail.

### 3. Struktur folder terlalu rumit untuk proyek mini
**Masalah:** Bikin folder `src/`, `tests/`, `docs/`, `migrations/` untuk program 100 baris — overkill.
**Solusi:** Mulai dari struktur minimal: `main.py` + `README.md`. Tambah folder lain kalau sudah benar-benar dibutuhkan.

### 4. Commit message tidak informatif
**Masalah:** Semua commit bertuliskan "update" atau "fix" — tidak ada informasi sama sekali.
**Solusi:** Tulis apa yang kamu ubah dan kenapa: `"Tambah validasi PIN maksimal 3 percobaan"`.

### 5. Tidak mendokumentasikan edge cases
**Masalah:** Program crash atau berperilaku aneh ketika user input sesuatu yang tidak diharapkan.
**Solusi:** Selalu tanya "apa yang terjadi kalau...?" saat requirement analysis.

### 6. Git hanya dipakai oleh satu anggota
**Masalah:** Git repo ada tapi hanya satu orang yang push — tidak ada manfaat kolaborasinya.
**Solusi:** Pastikan semua anggota punya akses ke repo dan melakukan commit dari komputer masing-masing.

---

## 🧪 Latihan / Studi Kasus

### Soal 1: Konsep (Requirement Analysis)

Kamu diminta membuat program "Kuis Interaktif" berbasis CLI. Program ini harus bisa:
- Menampilkan soal pilihan ganda (5 soal)
- Menerima jawaban user
- Menampilkan skor akhir

**Tugas:**
a) Buat tabel Desain IPO untuk program ini (tentukan semua Input, Process, dan Output-nya)
b) Identifikasi minimal 3 edge case yang harus ditangani program
c) Tulis pseudocode untuk fungsi utama `tampilkan_soal(soal, pilihan)`

---

### Soal 2: Studi Kasus (Perencanaan Proyek)

**Skenario:** Tim kamu terdiri dari 3 orang dan mendapat tugas membuat "Aplikasi To-do List CLI" yang bisa:
- Menambah task baru
- Menandai task sebagai selesai
- Menghapus task
- Menampilkan semua task (termasuk yang sudah selesai)
- Menyimpan data task ke file `.txt` agar tidak hilang saat program ditutup

**Tugas:**
a) Buat Requirement Analysis lengkap menggunakan template yang sudah diberikan
b) Rancang struktur folder proyek yang sesuai
c) Buat tabel pembagian tugas untuk tim 3 orang dengan timeline 5 hari
d) Tulis pseudocode untuk minimal 2 fungsi utama (misalnya: `tambah_task()` dan `simpan_ke_file()`)
e) Tuliskan 3 pertanyaan yang harus dijawab tim sebelum mulai coding (edge cases atau keputusan desain)

---

### Ide Proyek Mini yang Bisa Kamu Pilih

| No. | Nama Proyek | Tingkat Kesulitan | Konsep yang Tercakup |
|-----|-------------|-------------------|---------------------|
| 1 | **To-do List CLI** | ⭐⭐ Mudah | List, file I/O, loop, kondisi |
| 2 | **Kuis Interaktif** | ⭐⭐ Mudah | Dictionary, random, scoring |
| 3 | **Simulasi ATM** | ⭐⭐⭐ Menengah | Dictionary, fungsi, exception, file I/O |
| 4 | **Konversi Unit** | ⭐⭐ Mudah | Fungsi, input/output, dictionary |
| 5 | **Mini Chatbot** | ⭐⭐⭐ Menengah | String matching, dictionary, loop, kondisi |

**Tips memilih proyek:**
- Pilih yang paling kamu pahami alurnya — jangan pilih yang kelihatan keren tapi bingung cara kerjanya
- Pastikan semua anggota tim setuju dengan topik sebelum mulai
- Proyek sederhana yang dikerjakan dengan rapi lebih baik dari proyek ambisius yang setengah jadi

---

## 📌 Ringkasan

- **Requirement Analysis** = pahami dulu *apa* yang harus dibuat, bukan langsung *bagaimana* membuatnya
- **Model IPO** = peta alur program: Input → Process → Output; buat untuk setiap fitur
- **Pseudocode** = logika program dalam bahasa manusia, bukan syntax Python
- **Flowchart** = visualisasi alur dengan oval (start/end), kotak (proses), berlian (keputusan)
- **Struktur folder** = minimal `main.py` + `README.md`; tambah folder lain sesuai kebutuhan
- **Docstring** = dokumentasi fungsi menggunakan `"""..."""` berisi: deskripsi, args, returns
- **Git dasar** = `git init` → `git add .` → `git commit -m "pesan"` → `git push`
- **Pembagian tugas** = tetapkan peran jelas (Lead, Dev, Docs) + deadline internal 2 hari lebih awal
- **Pilih proyek** yang dipahami logikanya, bukan yang paling kelihatan canggih
- **Planning ≠ buang waktu** — planning yang baik justru menghemat waktu coding dan debugging
