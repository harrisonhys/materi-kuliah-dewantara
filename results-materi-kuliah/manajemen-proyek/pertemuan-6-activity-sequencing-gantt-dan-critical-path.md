# Pertemuan 6: Activity Sequencing Gantt dan Critical Path

> **RPS:** CPMK-3, Sub-CPMK-6 • **Durasi:** 150 menit • **Bobot:** 7%

## 1. 🎯 Learning Outcomes

Kamu mampu menurunkan activity dari work package, menentukan dependency, mengestimasi durasi, membuat network/Gantt, menetapkan milestone, menghitung critical path/float sederhana, dan menyusun schedule baseline.

## 2. 📖 Pengantar

Daftar tugas tidak menjawab urutan dan dampak keterlambatan. Jika API harus siap sebelum integrasi, keterlambatan API dapat menggeser UAT dan peluncuran.

## 3. 🧩 Konsep Utama

Urutan perencanaan:

```text
WBS → activities → dependencies → resources/calendars
→ duration → network/critical path → milestones → baseline
```

Dependency: finish-to-start, start-to-start, finish-to-finish, dan start-to-finish (jarang). Bedakan mandatory, discretionary, external, dan internal.

Durasi ≠ effort. Dua orang × lima hari adalah effort 10 person-days tetapi durasi bisa lima hari—jika pekerjaan benar-benar paralel.

Critical path adalah jalur berdurasi terpanjang yang menentukan tanggal selesai saat ini. Float menunjukkan kelonggaran; critical path dapat berubah.

## 4. 🧠 Analogi

Memasak jamuan: nasi dan sup dapat paralel, tetapi plating menunggu keduanya. Menambah koki tidak selalu mempercepat tahap yang tidak dapat dibagi.

## 5. 💻 Contoh

| ID | Activity | Durasi | Predecessor |
|---|---|---:|---|
| A | Finalize requirement | 3d | - |
| B | API design | 4d | A |
| C | UI design | 4d | A |
| D | Integration | 5d | B,C |
| E | UAT | 3d | D |

Jalur A-B-D-E dan A-C-D-E sama-sama 15 hari pada contoh ini.

## 6. 🏦 Studi Kasus Industri

Vendor payment certification dianggap aktivitas satu hari, padahal slot review eksternal dua minggu. **Dampak:** go-live terlambat. **Solusi:** catat external dependency, lead time, owner, early booking, contingency, dan milestone approval.

## 7. 📊 Aktivitas 150 Menit

20 menit activity/dependency; 25 menit estimation; 25 menit network/critical path; 60 menit Gantt workshop; 15 menit schedule challenge; 5 menit baseline check.

## 8. ⚠️ Kesalahan Umum

- Semua task dibuat finish-to-start.
- Effort disamakan durasi.
- Resource calendar diabaikan.
- Milestone diberi durasi.
- Gantt tidak traceable ke WBS.
- Tanggal dipaksakan sebelum dependency dianalisis.

## 9. 🧪 Latihan dan Penilaian

Buat activity list, dependency network, estimate basis, critical path, 5 milestone, dan Gantt. Rubrik: traceability 20%, dependency 25%, estimate 20%, schedule/CP 25%, dokumentasi 10%.

## 10. 📌 Ringkasan

- Schedule berasal dari scope/WBS.
- Dependency membentuk network.
- Durasi dipengaruhi resource/calendar.
- Critical path menentukan tanggal selesai saat ini.
- Baseline dipakai untuk control, bukan dibuah diam-diam.

## 11. Glosarium

| Istilah | Penjelasan |
|---|---|
| Critical path | Jalur yang menentukan durasi proyek |
| Dependency | Hubungan urutan aktivitas |
| Float | Kelonggaran jadwal |
| Milestone | Titik penting berdurasi nol |
| Schedule baseline | Jadwal disetujui untuk pembanding |

## 12. Referensi

- PMI, **Project Management Basics**, <https://www.pmi.org/certifications/~/link.aspx?_id=FF99743C71A271367017995CAEBDD95A&_z=z>
- PMI, **PMBOK Guide**, <https://www.pmi.org/standards/pmbok>

