# Pertemuan 11: Local Storage SQLite dan Strategi Caching

> **Acuan RPS:** CPMK-3, Sub-CPMK-6 • **Durasi:** 150 menit • **Artefak:** data lokal yang dapat dibaca kembali

---

## 1. 🎯 Learning Outcomes

Kamu mampu memilih jenis penyimpanan lokal, merancang tabel SQLite sederhana, menjalankan CRUD dengan aman, memahami migration, membedakan cache dan source of truth, serta merancang pengalaman offline tanpa menyesatkan pengguna.

---

## 2. 📖 Pengantar

Pengguna mengharapkan draft tetap ada setelah aplikasi ditutup dan daftar terakhir masih dapat dibaca saat sinyal hilang. Namun menyimpan semuanya tanpa strategi menimbulkan data basi, konflik, dan risiko kebocoran.

---

## 3. 🧩 Konsep Utama

### 3.1 Memilih Penyimpanan

| Kebutuhan | Pilihan umum |
|---|---|
| Preferensi sederhana | key-value storage |
| Data terstruktur/query | SQLite |
| Token/credential tertentu | secure storage platform |
| File besar | file system dengan metadata |
| Data resmi lintas perangkat | backend/cloud |

Jangan menyimpan password mentah. Penyimpanan lokal dapat diekstrak pada perangkat yang disusupi; minimalkan data sensitif.

### 3.2 SQLite

SQLite adalah database relasional embedded. Data disimpan dalam tabel dan diakses melalui SQL. Gunakan parameter binding, bukan menggabungkan input ke string SQL.

```sql
CREATE TABLE drafts (
  id TEXT PRIMARY KEY,
  title TEXT NOT NULL,
  content TEXT NOT NULL,
  updated_at INTEGER NOT NULL,
  sync_status TEXT NOT NULL
);
```

### 3.3 CRUD dan Transaction

- Create: membuat draft.
- Read: mengambil draft.
- Update: memperbarui isi/status.
- Delete: menghapus.
- Transaction: memastikan beberapa operasi berhasil atau gagal sebagai satu unit.

### 3.4 Migration

Ketika schema berubah, naikkan versi dan migrasikan data. Menghapus database pengguna hanya karena menambah kolom adalah praktik buruk.

### 3.5 Cache Strategy

```text
cache-first: tampilkan lokal → refresh network → perbarui
network-first: coba network → fallback cache
offline-first: tulis lokal → antrikan sinkronisasi
```

Tampilkan indikator last updated dan status sinkronisasi. Cache bukan otomatis sumber kebenaran.

---

## 4. 🧠 Analogi

Cache seperti fotokopi daftar harga. Berguna ketika kantor pusat tidak dapat dihubungi, tetapi tanggal salinan harus jelas. SQLite seperti lemari arsip lokal yang dapat dicari berdasarkan indeks.

---

## 5. 💻 Contoh Teknis

```bash
flutter pub add sqflite path
```

```dart
final db = await openDatabase(
  join(await getDatabasesPath(), 'app.db'),
  version: 1,
  onCreate: (db, version) async {
    await db.execute('''
      CREATE TABLE drafts(
        id TEXT PRIMARY KEY,
        title TEXT NOT NULL,
        updated_at INTEGER NOT NULL
      )
    ''');
  },
);

await db.insert(
  'drafts',
  {'id': 'd-1', 'title': 'Reservasi', 'updated_at': DateTime.now().millisecondsSinceEpoch},
  conflictAlgorithm: ConflictAlgorithm.replace,
);

final rows = await db.query('drafts', orderBy: 'updated_at DESC');
```

Bungkus detail database dalam local data source/repository. UI tidak seharusnya menulis SQL.

---

## 6. 🏦 Studi Kasus Fintech

### Saldo Cache Tampil Tanpa Peringatan

**Masalah:** aplikasi offline menampilkan saldo kemarin seolah-olah nilai terkini.

**Dampak:** keputusan pengguna salah dan kepercayaan rusak.

**Solusi:** label “terakhir diperbarui”, status offline, blokir tindakan yang memerlukan verifikasi mutakhir, refresh saat koneksi kembali, dan jadikan backend sumber kebenaran. Cache saldo berbeda dari ledger transaksi resmi.

---

## 7. 📊 Aktivitas 150 Menit

20 menit pilihan storage, 25 menit schema/CRUD, 20 menit transaction/migration, 20 menit caching, 50 menit praktikum, 15 menit uji restart/offline.

---

## 8. ⚠️ Kesalahan Umum

- Menyimpan secret di penyimpanan biasa.
- UI mengakses database langsung.
- SQL dibentuk dari input pengguna.
- Tidak merencanakan migration.
- Cache tanpa timestamp.
- Konflik sinkronisasi diabaikan.
- Database connection/resource tidak dikelola.

---

## 9. 🧪 Latihan dan Penilaian

Simpan satu entitas proyek; lakukan create, read, update, delete; buktikan data tetap ada setelah restart; tampilkan last updated dan sync status. Rancang migration penambahan satu kolom.

Rubrik: schema/CRUD 30%, persistensi 20%, arsitektur 15%, cache/offline state 20%, keamanan/migration 15%.

---

## 10. 📌 Ringkasan

- Pilih storage sesuai bentuk dan sensitivitas data.
- SQLite cocok untuk data lokal terstruktur.
- Transaction menjaga konsistensi multi-operasi.
- Schema perlu versioning dan migration.
- Cache harus menjelaskan usia dan status sinkronisasi.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Cache | Salinan data untuk akses cepat/offline |
| CRUD | Create, Read, Update, Delete |
| Migration | Perubahan schema antarversi |
| SQLite | Database relasional embedded |
| Transaction | Sekelompok operasi atomik |

## 12. Referensi

- Flutter, **Persist data with SQLite**, <https://docs.flutter.dev/cookbook/persistence/sqlite>
- SQLite, **Transactions**, <https://www.sqlite.org/lang_transaction.html>

