# PRD — Sysinfo Harwat

Dokumen ini mendefinisikan kebutuhan produk sistem Sysinfo Harwat dalam arsitektur decoupled (backend API + frontend web). Mencakup tiga domain: **AL** (Alpalhan MRO), **SH** (Sarhan MRO), dan **FH** (Farhan MRO).

Snapshot: 15 Juli 2026.

---

## 1. Gambaran Sistem

Sysinfo Harwat adalah sistem manajemen pemeliharaan, inventaris, dan produksi yang melayani kebutuhan lintas instansi militer. Satu pintu autentikasi melayani ketiga domain; akses per domain dibatasi oleh atribut `badan`, `role`, `matra`, dan `unit organisasi` masing-masing pengguna.

### Arsitektur

- **Backend:** REST API, prefix `/api/v1/`
- **Frontend:** Web client, stack-agnostic, mengonsumsi REST API
- **Auth:** JWT (access token + refresh token)
- **File Storage:** Private bucket (MinIO/S3), akses via signed URL

### Prinsip Umum

- Tidak ada data demo, fallback statis, atau nilai acak di production path
- Setiap status workflow dienforced di backend — bukan konvensi UI semata
- Role dan akses diperiksa per endpoint, bukan hanya di level route group
- File tidak disimpan sebagai binary di database — hanya path/URL

---

## 2. Aktor & Akses Global

| Aktor | Badan | Akses |
|---|---|---|
| Super Admin | Semua | Melewati middleware badan dan role tertentu; tetap tunduk validasi data dan workflow |
| Pengguna AL | `Alpalhankam` | Akses domain AL sesuai role |
| Pengguna SH | `Sarhan` | Akses domain SH sesuai role |
| Pengguna FH | `Farhan` | Akses domain FH sesuai role |

---

## 3. Domain AL — Alpalhan MRO

### 3.1 Aktor

| Role | Nilai tersimpan | Tanggung jawab |
|---|---|---|
| Satker | `satker` | Membuat dan submit pengajuan harwat; input laporan harian |
| Strategis / Mabes UO | `strategis` | Menyetujui atau menolak pengajuan sematra |
| Baharwathan | `inspektorat` | Pengesahan final jalur Renbut |
| Pemelihara | `pemelihara` | Pelaksana WO level L2 |
| Depo | `depo` | Pelaksana WO level L3 |
| Super Admin | — | Bypass helper role |

### 3.2 Fitur per Submodul

#### ACM — Asset Configuration Management

| Fitur | Backend | Frontend |
|---|---|---|
| Daftar fleet dengan filter matra/unit | `GET /al/fleets` | Tabel dengan filter |
| Detail fleet + BOM hierarki SYS→ASSY→PART | `GET /al/fleets/:id/bom` | Tree view BOM |
| Buat fleet manual | `POST /al/fleets` | Form |
| Impor fleet + BOM dari Excel Lapkonis | `POST /al/fleets/import` | Upload Excel |
| Upload dokumen / foto fleet | `POST /al/fleets/:id/documents` | Upload file |

#### Preventive Maintenance

| Fitur | Backend | Frontend |
|---|---|---|
| Daftar task template aktif per fleet | `GET /al/task-templates` | Daftar dengan filter |
| Proyeksi due date dari template + BOM | `GET /al/fleets/:id/preventive-projections` | Tabel proyeksi (label REAL saja, tanpa SIM) |
| Buat WO preventive | `POST /al/work-orders/preventive` | Form pilih task + fasilitas |

#### MMS — Pengajuan Harwat

| Fitur | Backend | Frontend |
|---|---|---|
| Buat pengajuan (draft / langsung submit) | `POST /al/pengajuan-harwat` | Form wizard |
| Submit draft | `POST /al/pengajuan-harwat/:id/submit` | Tombol submit |
| Daftar pengajuan (filter status/matra) | `GET /al/pengajuan-harwat` | Tabel |
| Detail pengajuan + parts + timeline | `GET /al/pengajuan-harwat/:id` | Detail view |
| Approve jalur SPK (Mabes UO) | `POST /al/pengajuan-harwat/:id/approve-spk` | Tombol aksi |
| Approve jalur Renbut (Mabes UO) | `POST /al/pengajuan-harwat/:id/approve-renbut` | Tombol aksi |
| Approve final Renbut (Baharwathan) | `POST /al/pengajuan-harwat/:id/approve-baharwathan` | Tombol aksi |
| Reject pengajuan | `POST /al/pengajuan-harwat/:id/reject` | Form alasan |
| Riwayat pengajuan | `GET /al/pengajuan-harwat/:id/timeline` | Timeline |

**Drop dari existing:** `MmsSpkController` (halaman statis/demo), route `UpdatePengajuanHarwatRequest` (scaffold tidak aktif).

#### Renbut AL

| Fitur | Backend | Frontend |
|---|---|---|
| Daftar Renbut per matra/periode | `GET /al/renbut` | Tabel dengan filter |
| Detail Renbut + item pengajuan | `GET /al/renbut/:id` | Detail view |

**Drop dari existing:** dead route `POST /renbut/:id/status` (method tidak ada).

#### Bengkel — WO & Checklist

| Fitur | Backend | Frontend |
|---|---|---|
| Daftar fasilitas aktif | `GET /al/facilities` | Daftar |
| Board WO per fasilitas | `GET /al/work-orders` | Kanban / tabel |
| Detail WO + requirement + step | `GET /al/work-orders/:id` | Detail |
| Update status WO (dengan validasi transisi) | `POST /al/work-orders/:id/status` | Dropdown status |
| Update requirement | `POST /al/work-orders/:id/requirements/:req/status` | Checklist |
| Update step | `POST /al/work-orders/:id/steps/:step/status` | Checklist |

**Perubahan dari existing:** transisi status WO dienforced — tidak boleh lompat bebas antar status. `DONE` adalah terminal.

#### Stok / Gudang

| Fitur | Backend | Frontend |
|---|---|---|
| Daftar supply items per matra/gudang | `GET /al/supply-items` | Tabel |
| Receipt stok | `POST /al/supply-items/:id/receipt` | Form qty |
| Issue stok langsung | `POST /al/supply-items/:id/issue` | Form qty |
| Ledger pergerakan stok | `GET /al/supply-items/:id/ledger` | Tabel histori |

**Perubahan dari existing:** ledger wajib tersimpan per transaksi (tidak lagi hard-coded).

#### Inspeksi & Laporan Harian

| Fitur | Backend | Frontend |
|---|---|---|
| Input laporan harian AL | `POST /al/laporan-harian` | Form entry BOM |
| Daftar laporan harian per fleet | `GET /al/laporan-harian` | Tabel |
| Detail laporan | `GET /al/laporan-harian/:id` | Detail |
| Kepatuhan inspeksi per fleet | `GET /al/inspeksi/kepatuhan` | Tabel status |

**Drop dari existing:** `storeAu()` dan `storeAd()` (mock redirect) — **belum masuk scope**, dikonfirmasi terpisah.

#### RHI — Reliability Health Index

| Fitur | Backend | Frontend |
|---|---|---|
| Health index per fleet | `GET /al/rhi/health` | Dashboard widget |
| MTBF, MTTR, Ao per fleet | `GET /al/rhi/metrics` | Grafik |
| Failure & degradation log | `GET /al/rhi/failures` | Tabel |
| Gap & alert | `GET /al/rhi/alerts` | Widget alert |

**Perubahan dari existing:** tidak ada data CONTROL atau enriched — semua dari DB transaksi.

#### Report

| Fitur | Backend | Frontend |
|---|---|---|
| Ringkasan WO per periode | `GET /al/reports/wo-summary` | Tabel/grafik |
| Laporan kesiapan fleet | `GET /al/reports/readiness` | Tabel/grafik |
| Laporan Renbut | `GET /al/reports/renbut` | Tabel |

**Perubahan dari existing:** tidak ada MTBF/MTTR acak atau anggaran asumsi.

**Drop total dari existing:** Chatbot (dependensi eksternal, bukan core), Technical Library (array statis).

### 3.3 Status Workflow AL

#### Pengajuan Harwat

| Dari | Aksi | Aktor | Ke |
|---|---|---|---|
| — | Buat draft | Satker | `DRAFT` |
| — | Submit langsung | Satker | `SUBMITTED` |
| `DRAFT` | Submit | Satker (pemilik) | `SUBMITTED` |
| `SUBMITTED` | Approve jalur SPK | Strategis sematra / Super Admin | `SPK_CREATED` |
| `SUBMITTED` | Approve jalur Renbut | Strategis sematra / Super Admin | `SUBMITTED_BAHARWATHAN` |
| `SUBMITTED` | Reject | Strategis sematra / Super Admin | `REJECTED` |
| `SUBMITTED_BAHARWATHAN` | Approve | Inspektorat | `RENBUT_FINAL` |
| `SUBMITTED_BAHARWATHAN` | Reject | Inspektorat | `REJECTED` |

`REJECTED`, `SPK_CREATED`, `RENBUT_FINAL` adalah status terminal. Tidak ada revisi atau resubmit.

#### Work Order

| Dari | Ke | Syarat |
|---|---|---|
| — | `PLANNED` | Create dari pengajuan atau preventive |
| `PLANNED` | `IN_PROGRESS` | Progress > 0 |
| `IN_PROGRESS` | `QC` | Semua step selesai atau progress = 100 |
| `QC` | `DONE` | Aksi eksplisit |

Tidak ada lompatan status. `DONE` terminal.

#### Requirement WO

`pending` → `ready` → `issued` → `returned` / `not_available`

#### Step WO

`pending` → `in_progress` → `done` / `skipped` / `na`

---

## 4. Domain SH — Sarhan MRO

### 4.1 Aktor

| Role | Tanggung jawab |
|---|---|
| Operator | Registrasi aset, pengajuan, RAB, lampiran, revisi |
| Kotama | Penilaian aset, review pengajuan tahap Kotama |
| Mabes | Validasi pengajuan tahap Mabes, Renbut matra |
| Pusat | Persetujuan akhir, impor Excel AU, Renbut semua matra, administrasi akun |
| Super Admin | Bypass middleware badan/role |

### 4.2 Fitur per Submodul

#### Inventaris Aset

| Fitur | Backend | Frontend |
|---|---|---|
| Daftar aset dengan filter matra/unit/kategori/kondisi | `GET /sh/aset` | Tabel dengan filter |
| Detail aset + galeri + dokumen | `GET /sh/aset/:id` | Detail view |
| Buat aset | `POST /sh/aset` | Form |
| Edit aset | `PUT /sh/aset/:id` | Form |
| Hapus aset | `DELETE /sh/aset/:id` | Konfirmasi (dengan guard owner + status check) |
| Upload galeri | `POST /sh/aset/:id/gallery` | Upload file |

**Perubahan dari existing:** delete aset wajib ada ownership check dan status check (tidak boleh hapus jika ada Renbut terkait).

#### Penilaian Aset (Kotama)

| Fitur | Backend | Frontend |
|---|---|---|
| Antrean penilaian (scope unit Kotama + descendants) | `GET /sh/penilaian` | Tabel antrean |
| Submit penilaian (setuju / revisi / tolak) | `POST /sh/penilaian/:id/assess` | Form skor 1–5 |

#### Pengajuan & RAB Berjenjang

| Fitur | Backend | Frontend |
|---|---|---|
| Buat pengajuan + RAB v1 | `POST /sh/pengajuan` | Form wizard |
| Daftar pengajuan (scope per role) | `GET /sh/pengajuan` | Tabel |
| Detail pengajuan + RAB + lampiran + histori | `GET /sh/pengajuan/:id` | Detail view |
| Edit RAB (buat versi baru) | `POST /sh/pengajuan/:id/rab` | Form RAB |
| Upload lampiran | `POST /sh/pengajuan/:id/lampiran` | Upload file |
| Approve Kotama | `POST /sh/pengajuan/:id/approve-kotama` | Tombol aksi |
| Revisi ke Operator (Kotama/Mabes) | `POST /sh/pengajuan/:id/revisi` | Form alasan |
| Tolak (Kotama/Mabes/Pusat) | `POST /sh/pengajuan/:id/tolak` | Form alasan |
| Submit ulang setelah revisi (Operator) | `POST /sh/pengajuan/:id/resubmit` | Tombol |
| Validasi Mabes | `POST /sh/pengajuan/:id/validasi` | Tombol aksi |
| Approve Pusat (+ materialisasi Renbut) | `POST /sh/pengajuan/:id/approve-pusat` | Tombol aksi |

**Perubahan dari existing:** scope Kotama untuk daftar pengajuan dienforced di query — hanya pengajuan dari unit Kotama dan descendants.

#### Renbut SH

| Fitur | Backend | Frontend |
|---|---|---|
| Daftar Renbut per matra/periode | `GET /sh/renbut` | Tabel |
| Detail Renbut + item | `GET /sh/renbut/:id` | Detail view |
| Preview impor Excel AU | `POST /sh/renbut/import/preview` | Tabel preview |
| Konfirmasi impor Excel AU | `POST /sh/renbut/import/confirm` | Tombol konfirmasi + warning destruktif |
| Ekspor PDF | `GET /sh/renbut/:id/export/pdf` | Download |
| Ekspor Word | `GET /sh/renbut/:id/export/word` | Download |
| Ekspor Excel | `GET /sh/renbut/:id/export/excel` | Download |

#### Administrasi Pengguna SH

| Fitur | Backend | Frontend |
|---|---|---|
| Daftar akun Sarhan | `GET /sh/users` | Tabel |
| Buat akun | `POST /sh/users` | Form |
| Edit akun | `PUT /sh/users/:id` | Form |
| Hapus akun | `DELETE /sh/users/:id` | Konfirmasi |

**Drop dari existing:** Dashboard analytics WIP (angka tetap/acak), master data array statis.

### 4.3 Status Workflow SH

#### Penilaian Aset

| Dari | Aksi | Ke |
|---|---|---|
| — | Registrasi aset | `menunggu_penilaian_kotama` |
| `menunggu_penilaian_kotama` | Setuju Kotama | `disetujui_kotama` |
| `menunggu_penilaian_kotama` | Revisi Kotama | `revisi_satker` |
| `revisi_satker` | Update aset Operator | `menunggu_penilaian_kotama` |
| `menunggu_penilaian_kotama` | Tolak Kotama | `ditolak_kotama` |

#### Pengajuan

| Dari | Aksi | Aktor | Ke |
|---|---|---|---|
| — | Submit | Operator | `masuk` |
| `masuk` | Approve | Kotama | `diverifikasi_kotama` |
| `masuk` | Revisi | Kotama | `revisi` |
| `revisi` | Submit ulang | Operator (pemilik) | `masuk` |
| `diverifikasi_kotama` | Validasi | Mabes | `divalidasi` |
| `diverifikasi_kotama` | Revisi | Mabes | `revisi` |
| `divalidasi` | Approve | Pusat | `disetujui` |
| `divalidasi` | Tolak | Pusat | `ditolak` |
| Tahap terkait | Tolak | Reviewer | `ditolak` |

`disetujui` dan `ditolak` adalah status terminal.

---

## 5. Domain FH — Farhan MRO

### 5.1 Aktor

| Role | Area | Tanggung jawab |
|---|---|---|
| Kalafi | LAFI, Production Approval | Membuat usulan dari instruksi, koreksi item revisi milik unit |
| Operator | LAFI, CPB | Menjalankan batch dan mengisi tahap produksi |
| Petugas Logistik | Logistics | Penerimaan BBO, receipt/issue stok |
| QC/QA | Logistics, CPB | Pemeriksaan mutu BBO dan langkah QC/QA CPB |
| Puslola | Puslola, Production Approval | Monitor, review usulan, kelola bundel, catat keputusan |
| Super Admin | Semua | Bypass role tertentu |

### 5.2 Fitur per Submodul

#### LAFI

| Fitur | Backend | Frontend |
|---|---|---|
| Inventaris alat LAFI | `GET /fh/lafi/inventaris` | Tabel |
| CRUD inventaris | `POST/PUT/DELETE /fh/lafi/inventaris` | Form |
| Persetujuan kalibrasi/perbaikan inventaris | `POST /fh/lafi/inventaris/:id/approve` | Tombol |
| Daftar personel | `GET /fh/lafi/personel` | Tabel |
| Daftar produk & formula | `GET /fh/lafi/produk` | Tabel |
| Perizinan (NIE, CPOB, PKS) | `GET/POST /fh/lafi/perizinan` | Form + daftar |
| Renbut LAFI | `GET/POST /fh/lafi/renbut` | Form + daftar |
| Kegiatan produksi (timeline) | `GET /fh/lafi/produksi` | Timeline |
| QC/QA produksi | `GET/POST /fh/lafi/qc` | Form + status |
| Laporan LAFI | `GET /fh/lafi/laporan` | Tabel/unduh |

#### Puslola

| Fitur | Backend | Frontend |
|---|---|---|
| Dashboard monitoring produksi per LAFI | `GET /fh/puslola/dashboard` | Widget + tabel |
| Monitoring mutu dan distribusi | `GET /fh/puslola/monitoring` | Tabel |
| Distribusi obat jadi | `GET/POST /fh/puslola/distribusi` | Form + daftar |
| Stok opname | `POST /fh/puslola/stok-opname` | Form |
| Renbut Puslola | `GET/POST /fh/puslola/renbut` | Form + daftar |
| Kerja sama & perizinan Puslola | `GET/POST /fh/puslola/kerjasama` | Form + daftar |
| Pengawasan mutu Puslola | `GET /fh/puslola/mutu` | Tabel |

#### Logistics

| Fitur | Backend | Frontend |
|---|---|---|
| Penerimaan BBO per batch | `POST /fh/logistics/bbo/receipt` | Form |
| Daftar BBO + quality state | `GET /fh/logistics/bbo` | Tabel |
| Update quality state BBO | `POST /fh/logistics/bbo/:id/quality` | Form transisi status |
| Alokasi stok agregat | `POST /fh/logistics/alokasi` | — (dipicu oleh Production Approval) |
| Konsumsi stok saat batch mulai | `POST /fh/logistics/konsumsi` | — (dipicu oleh CPB) |
| Ledger stok | `GET /fh/logistics/ledger` | Tabel |
| Pelepasan obat jadi | `GET /fh/logistics/obat-jadi` | Tabel |
| Ledger obat jadi | `GET /fh/logistics/obat-jadi/ledger` | Tabel |

#### Production Approval

| Fitur | Backend | Frontend |
|---|---|---|
| Buat & kirim instruksi (Puslola) | `POST /fh/pa/instruksi` | Form |
| Batalkan instruksi | `POST /fh/pa/instruksi/:id/batal` | Tombol |
| Buat usulan dari instruksi (Kalafi) | `POST /fh/pa/usulan` | Form |
| Koreksi usulan ke Kalafi | `POST /fh/pa/usulan/:id/koreksi` | Form catatan |
| Bundel usulan terpilih (Puslola) | `POST /fh/pa/bundel` | Form pilih usulan |
| Detail bundel + versi + snapshot | `GET /fh/pa/bundel/:id` | Detail view |
| Direct approve bundel (authorized manager) | `POST /fh/pa/bundel/:id/approve` | Form + upload PDF evidence |
| Catat event Kabadan off-system | `POST /fh/pa/bundel/:id/record-kabadan` | Form metadata |
| Upload & finalisasi surat final | `POST /fh/pa/bundel/:id/finalisasi-surat` | Upload PDF |
| Revisi bundel | `POST /fh/pa/bundel/:id/revisi` | Form item revisi |
| Tolak bundel | `POST /fh/pa/bundel/:id/tolak` | Form alasan |
| Audit log bundel | `GET /fh/pa/bundel/:id/audit` | Tabel |
| Notifikasi setelah commit | Otomatis via DB after-commit | Notifikasi in-app |

#### CPB Digital

| Fitur | Backend | Frontend |
|---|---|---|
| Detail CPB batch (di-bootstrap otomatis) | `GET /fh/cpb/:id` | Detail view |
| Update production stage | `POST /fh/cpb/:id/stages/:stage` | Form |
| Reopen production stage | `POST /fh/cpb/:id/stages/:stage/reopen` | Tombol |
| Update QC step | `POST /fh/cpb/:id/qc/:step` | Form |
| Reopen QC step | `POST /fh/cpb/:id/qc/:step/reopen` | Tombol |
| Input sampel IPC | `POST /fh/cpb/:id/ipc-samples` | Form |
| Rekonsiliasi pengemasan | `POST /fh/cpb/:id/rekonsiliasi` | Form |
| Upload tanda tangan | `POST /fh/cpb/:id/signatures` | Upload file |
| QA Release | `POST /fh/cpb/:id/qa-release` | Tombol |
| Finalisasi CPB (terminal) | `POST /fh/cpb/:id/finalize` | Tombol + konfirmasi |
| Download CPB (docx) | `GET /fh/cpb/:id/download` | Download |
| Audit log CPB | `GET /fh/cpb/:id/audit` | Tabel |

### 5.3 Status Workflow FH

#### Quality State BBO

`diterima` → `qc_proses` → `qa_proses` → `released`
Alternatif: `ditolak`, `perlu_retest`, `expired`

#### Instruksi

`dikirim` → `ditindaklanjuti` / `dibatalkan`

#### Approval Usulan

`menunggu_pemeriksaan` → `koreksi_kalafi` / `koreksi_internal` / `siap_dibundel` → `masuk_bundel` → `revisi_bundel_*` / `disetujui` / `ditolak` / `dibatalkan`

#### Bundel

| Dari | Aksi | Ke |
|---|---|---|
| — | Publish v1 | `menunggu_keputusan` |
| `menunggu_keputusan` | Revisi | `perlu_revisi` |
| `perlu_revisi` | Semua item selesai | `menunggu_keputusan` (versi baru) |
| `menunggu_keputusan` | Direct approve + PDF | `disetujui` |
| `menunggu_keputusan` | Catat event Kabadan | `menunggu_surat_final` |
| `menunggu_surat_final` | Finalisasi surat | `final` |
| `menunggu_keputusan` | Tolak | `ditolak` |
| Non-terminal | Batal | `dibatalkan` |

`disetujui`, `final`, `ditolak`, `dibatalkan` adalah status terminal.

#### Pelaksanaan (Execution)

`belum_mulai` → `aktif` → `selesai`

#### CPB Digital

`draft` → `aktif` → `completed` → `finalized` (terminal)
Reopen production stage mengembalikan ke `aktif`.

---

## 6. Batas Tanggung Jawab Backend vs Frontend

| Concern | Backend | Frontend |
|---|---|---|
| Validasi business rule | Ya — di Service layer | Tidak — hanya validasi UX (format, required field) |
| Enforcement transisi status | Ya — ditolak jika transisi tidak valid | Tidak — hanya menyembunyikan tombol yang tidak relevan |
| Kalkulasi skor/risiko/prioritas | Ya | Tidak |
| Signed URL untuk file | Ya — generate dan kembalikan ke frontend | Tidak — hanya render URL yang diterima |
| Scope data per role/matra/unit | Ya — di query level | Tidak — tidak ada filter sisi klien untuk data sensitif |
| Pagination | Ya — cursor atau offset | Mengikuti response backend |
| Notifikasi in-app | Backend simpan; frontend polling atau subscribe | Render notifikasi dari response API |
| Audit log | Ya — backend catat semua mutasi | Hanya render |
| Demo/fallback data | Tidak ada | Tidak ada |

---

## 7. Fitur yang Di-drop dari Existing

| Fitur | Domain | Alasan |
|---|---|---|
| Chatbot (proxy eksternal) | AL | Dependensi eksternal non-core; bukan transaksi DB |
| Technical Library (array statis) | AL | Tidak ada DB backing; bukan fitur transaksional |
| `storeAu()` / `storeAd()` laporan harian | AL | Mock redirect; belum diimplementasi |
| `MmsSpkController` (halaman statis) | AL | Data demo; bukan WO hasil transaksi |
| Dead route `POST /renbut/:id/status` | AL | Method tidak ada |
| Dashboard analytics (angka tetap/acak) | SH | Tidak DB-backed |
| Master data array statis | SH | Tidak DB-backed |
| Data demo/fallback pada board Bengkel | AL | Diganti data nyata atau kosong |
| Data CONTROL/enriched pada RHI | AL | Diganti data DB murni |
| MTBF/MTTR acak pada report | AL | Diganti kalkulasi dari data transaksi atau kosong |

---

## 8. Non-Cakupan

- Proses pengadaan atau pembayaran setelah Renbut terbentuk
- Eksekusi fisik pemeliharaan di luar sistem
- Integrasi sistem eksternal selain storage
- Deployment dan infrastruktur CI/CD