# GLOSSARY — Sysinfo Harwat

Dokumen ini mendefinisikan istilah domain bisnis yang dipakai di seluruh context documents, PRD, ERD, dan implementasi. Istilah yang sama di domain berbeda **tidak boleh diasumsikan memiliki makna yang sama**.

Snapshot: 15 Juli 2026.

---

## Istilah Lintas Domain (Shared)

| Istilah | Definisi |
|---|---|
| **Badan** | Atribut akun pengguna yang menentukan domain mana yang dapat diakses. Contoh: `Alpalhankam`, `Sarhan`, `Farhan`. |
| **Matra** | Satuan angkatan/instansi militer. Contoh: TNI AD, TNI AL, TNI AU, Kemhan, Mabes TNI. |
| **Renbut** | Rencana Kebutuhan. Dokumen konsolidasi kebutuhan per matra dan tahun anggaran. Makna dan proses materialisasinya berbeda di setiap domain. |
| **Super Admin** | Role yang melewati middleware badan dan peran tertentu. Tetap tunduk pada validasi data dan workflow. |
| **ULID** | Universally Unique Lexicographically Sortable Identifier. Format ID standar di seluruh tabel schema baru (`char(26)`). |
| **WIP** | Work In Progress. Fitur yang ada sebagian atau belum selesai di sistem existing. Tidak boleh diperlakukan sebagai perilaku yang dijanjikan. |

---

## Domain AL — Alpalhan MRO

| Istilah | Definisi |
|---|---|
| **Alpalhan** | Alat peralatan pertahanan dan keamanan. Nama domain kanonis. |
| **Alpal** | Nama pendek Alpalhan. Masih muncul di path storage dan antarmuka existing. |
| **Alpalhankam** | Nilai atribut `badan` akun untuk domain Alpalhan. Bukan domain terpisah. |
| **ACM** | Asset Configuration Management. Submodul pengelolaan konfigurasi aset (fleet dan BOM). |
| **BOM** | Bill of Materials. Hierarki komponen aset: SYS (sistem) → ASSY (rakitan) → PART (komponen). |
| **Fleet** | Entitas aset individual dalam domain Alpalhan. Mempunyai identitas, matra, unit, status kesiapan (FMC/PMC/NMC), jam, dan cycle. |
| **FMC** | Fully Mission Capable. Status aset siap penuh. |
| **PMC** | Partially Mission Capable. Status aset siap terbatas. |
| **NMC** | Not Mission Capable. Status aset tidak siap. |
| **MMS** | Maintenance Management System. Submodul pengelolaan pengajuan pemeliharaan. |
| **Harwat** | Pemeliharaan dan perawatan. |
| **Pengajuan Harwat** | Permintaan pemeliharaan yang diajukan Satker untuk fleet tertentu, berisi daftar parts dan estimasi biaya. |
| **SPK** | Surat Perintah Kerja. Dokumen yang diterbitkan saat stok tersedia untuk memulai pekerjaan bengkel. |
| **WO** | Work Order. Entitas pelaksanaan pekerjaan di bengkel, dibuat dari SPK atau preventive maintenance. |
| **Jalur SPK** | Jalur pengajuan harwat yang semua stok part-nya terpenuhi → menghasilkan SPK dan WO. |
| **Jalur Renbut** | Jalur pengajuan harwat yang ada part kosong/kurang → diteruskan ke Renbut. |
| **Renbut (AL)** | Bundel kebutuhan part per matra dan periode yang terbentuk dari pengajuan jalur Renbut yang disetujui Baharwathan. |
| **Satker** | Satuan Kerja. Aktor pembuat pengajuan harwat. Role tersimpan sebagai `satker`. |
| **Mabes UO / Strategis** | Aktor penyetuju pengajuan di level Markas Besar/Unit Organisasi. Role tersimpan sebagai `strategis`. |
| **Baharwathan** | Aktor penyetuju final jalur Renbut. Bukan role tersimpan terpisah — dipetakan dari role `inspektorat`. |
| **Pemelihara** | Aktor pelaksana pekerjaan level L2. |
| **Depo** | Aktor pelaksana pekerjaan level L3. |
| **Lapkonis** | Laporan kondisi. Format dokumen Excel yang diimpor untuk membuat/memperbarui fleet dan BOM. |
| **Laphar** | Laporan harian. Laporan kondisi aset per hari, per matra. |
| **RHI** | Reliability Health Index. Indikator kesehatan keandalan aset. |
| **MTBF** | Mean Time Between Failures. Rata-rata waktu antarkegagalan. |
| **MTTR** | Mean Time To Repair. Rata-rata waktu perbaikan. |
| **Ao** | Availability Operational. Ketersediaan operasional aset. |
| **Preventive Maintenance** | Pemeliharaan terjadwal berdasarkan template task dan interval. |
| **Task Template** | Template langkah-langkah preventive maintenance yang dikaitkan ke BOM node. |

---

## Domain SH — Sarhan MRO

| Istilah | Definisi |
|---|---|
| **Sarhan** | Sarana dan prasarana. Nama domain kanonis. |
| **Aset (SH)** | Entitas sarana atau prasarana pertahanan yang diregistrasi dalam domain Sarhan. Berbeda dari Fleet di domain AL. |
| **NUP** | Nomor Urut Pendaftaran. Identitas unik aset dalam domain Sarhan. |
| **Org Unit / Satuan Kerja** | Struktur organisasi hierarkis (`parent_id`) yang menentukan cakupan data per pengguna. |
| **Kotama** | Komando Utama. Level organisasi di atas Satker/Operator. Memeriksa dan menyetujui penilaian serta pengajuan dari satuan di bawahnya. |
| **Mabes** | Markas Besar. Level di atas Kotama. Memvalidasi pengajuan setelah Kotama. |
| **Pusat** | Level tertinggi dalam domain Sarhan. Menyetujui pengajuan akhir dan mengelola Renbut serta akun pengguna. |
| **Operator (SH)** | Aktor pembuat aset dan pengajuan dalam domain Sarhan. |
| **Penilaian** | Proses Kotama menilai kondisi dan risiko aset Operator. Menghasilkan skor prioritas. |
| **Kode Kondisi** | Kode status fisik aset: LF (layak fungsi), RR (rusak ringan), RS (rusak sedang), RB (rusak berat), TED (tidak ekonomis diperbaiki). |
| **Skor Dampak** | Nilai 1–5 yang menggambarkan dampak bisnis kerusakan aset. |
| **Skor Kemungkinan** | Nilai 1–5 yang menggambarkan kemungkinan kegagalan aset, diturunkan dari kode kondisi. |
| **Risiko** | Hasil perkalian dampak × kemungkinan. |
| **P1 / P2 / P3** | Tingkat prioritas pemeliharaan: P1 (risiko ≥15), P2 (risiko ≥8), P3 (risiko lainnya). |
| **Pengajuan (SH)** | Usulan pemeliharaan aset yang diajukan Operator beserta RAB. Berbeda dari Pengajuan Harwat di domain AL. |
| **RAB** | Rencana Anggaran Biaya. Rincian volume, harga satuan, dan total biaya per item pemeliharaan. |
| **RAB Revisi** | Versi baru RAB yang dibuat saat pengajuan direvisi. Hanya satu versi aktif per pengajuan. |
| **Materialisasi** | Proses menyalin data RAB yang disetujui Pusat menjadi item Renbut. |
| **Renbut (SH)** | Dokumen konsolidasi kebutuhan pemeliharaan per matra dan tahun anggaran, terbentuk dari materialisasi pengajuan yang disetujui atau impor Excel AU. |
| **Impor Excel AU** | Jalur khusus TNI AU untuk memasukkan data Renbut langsung via file Excel. Bersifat replacement destruktif untuk AU/periode yang sama. |
| **Exercise** | Proses simulasi atau pengujian alur verifikasi berjenjang dalam domain Sarhan. |

---

## Domain FH — Farhan MRO

| Istilah | Definisi |
|---|---|
| **Farhan** | Nama domain kanonis untuk area produksi farmasi/obat. |
| **LAFI** | Lembaga Farmasi. Unit pelaksana produksi obat. |
| **Kalafi** | Kepala LAFI. Aktor yang membuat usulan dari instruksi dan mengoreksi item revisi milik unitnya. |
| **Operator (FH)** | Aktor yang ditugaskan menjalankan batch produksi dan mengisi tahap CPB. |
| **Puslola** | Pusat Logistik dan Perbekalan. Unit koordinasi lintas LAFI. Memonitor, memeriksa usulan, mengelola bundel dan revisi, mencatat keputusan. |
| **Kabadan** | Kepala Badan. Pembuat keputusan final atas bundel produksi. Keputusan dilakukan **di luar sistem** (off-system). |
| **BBO** | Bahan Baku Obat. Material yang diterima, diproses QC/QA, dan dialokasikan untuk produksi. |
| **Quality State BBO** | Status mutu BBO: `diterima` → `qc_proses` → `qa_proses` → `released`. Alternatif: `ditolak`, `perlu_retest`, `expired`. |
| **Instruksi** | Dokumen yang dikirim Puslola ke LAFI sebagai perintah untuk membuat usulan produksi. |
| **Usulan** | Rencana produksi yang dibuat Kalafi dari instruksi, berisi produk, operator, dan rencana bahan. Satu instruksi hanya boleh punya satu usulan. |
| **Koreksi Biasa** | Permintaan perbaikan usulan dari Puslola ke Kalafi (jalur normal). |
| **Koreksi Internal** | Permintaan perbaikan internal di dalam Puslola sebelum usulan dibundel. |
| **Bundel** | Kumpulan usulan yang dikunci, diberi versi, snapshot, hash, dan PDF oleh Puslola untuk diajukan ke Kabadan. |
| **Planning Version** | Nomor versi bundel. Dimulai dari 1, dinaikkan setiap koreksi/revisi. |
| **Snapshot** | Salinan konten bundel yang dibekukan pada saat bundel dibuat/direvisi. Immutable. |
| **Revisi Bundel** | Putaran perbaikan bundel setelah keputusan `perlu_revisi` dari Kabadan. Setiap item revisi punya ownership (Puslola atau Kalafi). |
| **Direct Approve** | Jalur persetujuan bundel langsung di dalam sistem oleh authorized manager dengan PDF evidence. |
| **Off-system Approve** | Jalur persetujuan bundel melalui pencatatan event keputusan Kabadan di luar sistem, diikuti finalisasi surat. |
| **Surat Final** | Dokumen PDF resmi yang diunggah setelah keputusan off-system Kabadan. Wajib memenuhi validasi metadata, MIME, SHA-256, dan isi. |
| **Alokasi** | Penambahan stok agregat BBO saat keputusan/finalisasi bundel disetujui. |
| **Konsumsi** | Pengurangan stok BBO saat batch produksi dimulai. |
| **Execution** | Entitas pelaksanaan batch produksi. Status: `belum_mulai` → `aktif` → `selesai`. |
| **CPB Digital** | Catatan Pengolahan Batch digital. Dokumen produksi per batch yang mencatat seluruh tahap produksi, QC, QA, rekonsiliasi, dan tanda tangan. |
| **Production Stage** | Tahap produksi dalam CPB yang diisi operator. Dapat di-reopen sebelum finalisasi. |
| **QC Step** | Langkah pemeriksaan mutu dalam CPB. |
| **QA Release** | Persetujuan Quality Assurance yang menandai obat jadi siap dilepas ke persediaan. |
| **Obat Jadi** | Produk akhir hasil produksi yang dilepaskan ke persediaan setelah QA Release dan CPB finalized. |
| **Stok Ledger** | Catatan pergerakan stok BBO (masuk/keluar) per entri dengan identitas unik. |
| **Obat Jadi Ledger** | Catatan pergerakan obat jadi (masuk/keluar) setelah production release. |
| **Renbut (FH)** | Rencana Kebutuhan dalam konteks Farhan. Dikelola Puslola, terpisah dari Renbut AL dan SH. |
| **CPB Bootstrap** | Proses pembuatan CPB baru dari templat aktif saat bundel disetujui (direct approve atau finalisasi surat). |
| **Finalisasi** | Aksi terminal pada CPB yang mengunci seluruh catatan produksi. Tidak dapat dibatalkan atau dibuka kembali. |