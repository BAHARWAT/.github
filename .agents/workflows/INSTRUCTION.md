# Alur Kerja & Tahapan Dekopel Proyek Sysinfo Harwat

Dokumen ini mendefinisikan fase dan tahapan pengerjaan untuk melakukan redesain database serta inisialisasi scaffolding Next.js (frontend) dan NestJS (backend). 

> [!IMPORTANT]
> **Kewajiban Peninjauan Berjenjang:**
> Setiap kali satu Fase selesai sepenuhnya, agen **WAJIB** berhenti, memberikan rincian (*breakdown*) hasil pengerjaan, dan meminta persetujuan serta tinjauan dari pengguna terlebih dahulu sebelum dapat melanjutkan ke Fase berikutnya.

---

## Fase 1: Pemindaian, Redesain, dan Normalisasi Skema Database

Tujuan: Merancang ulang skema database dari `baharwat-schema-only.sql` dengan menerapkan penyesuaian (*adjustment*) tabel, normalisasi (seperti penambahan index, *full-text index*), dan standarisasi prefix baru.

### Langkah 1.1: Pemisahan & Redesain ERD ALPALHAN (ALP)
* **Target File:** [docs/database/erd-alp.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-alp.md)
* **Aktivitas:**
  * Ekstrak tabel `alpalhan_new_*` dan rancang ulang menjadi `alp_*`.
  * Lakukan optimasi indexing (indeks komposit untuk pencarian, indeks kolom asing) dan *full-text index* pada kolom teks/keterangan yang sering dicari.
  * Lakukan *adjustment* tipe data dan relasi agar struktur tabel lebih efisien dibanding skema monolit asal.
  * Dokumentasikan rancangan dalam format Mermaid ERD beserta detail kolom dan tipe data baru.

### Langkah 1.2: Pemisahan & Redesain ERD SARHAN (SAR)
* **Target File:** [docs/database/erd-sar.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-sar.md)
* **Aktivitas:**
  * Ekstrak tabel `sarhan_new_*` dan rancang ulang menjadi `sar_*`.
  * Optimasi indexing pada pencarian aset dan riwayat.
  * *Adjustment* relasi unit organisasi agar representasi pohon (*tree*) lebih efisien.
  * Dokumentasikan rancangan dalam format Mermaid ERD.

### Langkah 1.3: Pemisahan & Redesain ERD FARHAN (FAR)
* **Target File:** [docs/database/erd-far.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-far.md)
* **Aktivitas:**
  * Ekstrak tabel `farhan_new_*` dan rancang ulang menjadi `far_*` (skema ini sangat besar dengan 63 tabel, memerlukan strukturisasi modular).
  * Optimasi indeks pada tabel audit log, batch CPB, dan relasi logistik.
  * Dokumentasikan rancangan dalam format Mermaid ERD.

### Langkah 1.4: Redesain Skema Bersama (Shared/Core)
* **Target File:** [docs/database/erd-shared.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-shared.md)
* **Aktivitas:**
  * Redesain tabel core seperti `users`, `notifications`, `cache` dengan prefix `shared_*` (seperti `shared_users`, `shared_notifications`).
  * Optimasi indeks pencarian pengguna dan notifikasi.
  * Hubungkan relasi foreign key dari domain ALP, SAR, dan FAR ke tabel `shared_*`.

### Langkah 1.5: Review & Bersihkan File Tidak Perlu (Fase 1 Selesai)
* **Aktivitas:**
  * Pastikan seluruh file ERD lama/cadangan yang tidak diperlukan dihapus agar tidak membingungkan.
  * **STOP dan ajukan review Fase 1 ke pengguna.** Jangan lanjut ke Fase 2 sebelum mendapatkan persetujuan.

---

## Fase 2: Diagram Alur Proses Bisnis (BISPRO)

Tujuan: Mendokumentasikan alur logika bisnis dan transisi status workflow berdasarkan database baru hasil Fase 1.

### Langkah 2.1: Diagram BISPRO ALPALHAN
* **Target File:** [docs/bispro-diagram-alp.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-alp.md)
* **Aktivitas:** Menggambarkan sequence diagram dan flowchart transisi status MMS (SPK vs Renbut) serta siklus hidup Work Order di Bengkel.

### Langkah 2.2: Diagram BISPRO SARHAN
* **Target File:** [docs/bispro-diagram-sar.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-sar.md)
* **Aktivitas:** Menggambarkan sequence diagram alur penilaian Kotama, alur revisi pengajuan, dan rantai approval RAB.

### Langkah 2.3: Diagram BISPRO FARHAN
* **Target File:** [docs/bispro-diagram-far.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-far.md)
* **Aktivitas:** Menggambarkan alur Production Proposal & Bundling, logistik intake, serta status CPB Digital.

### Langkah 2.4: Review & Bersihkan File Tidak Perlu (Fase 2 Selesai)
* **Aktivitas:**
  * Bersihkan diagram draf atau file catatan proses bisnis yang tidak terpakai.
  * **STOP dan ajukan review Fase 2 ke pengguna.** Jangan lanjut ke Fase 3 sebelum mendapatkan persetujuan.

---

## Fase 3: Inisialisasi Scaffolding Awal Proyek & Migrasi Database

Tujuan: Membuat kerangka proyek baru untuk frontend dan backend dengan struktur folder modular, serta menyusun skema migrasi database yang siap pakai.

### Langkah 3.1: Setup Proyek NestJS Backend
* **Target Direktori:** [backend-app/](file:///d:/dev/sysinfo-harwat/.github/backend-app/)
* **Aktivitas:**
  * Inisialisasi NestJS menggunakan Fastify adapter.
  * Setup ORM (TypeORM) dengan konfigurasi koneksi database.
  * Buat struktur modul: `shared`, `alp`, `sar`, `far`.

### Langkah 3.2: Implementasi Berkas Migrasi Database (TypeORM Migrations)
* **Target Direktori:** `backend-app/src/migrations/`
* **Aktivitas:**
  * Membuat berkas migrasi TypeScript per tabel/langkah (`up()` dan `down()`) untuk seluruh tabel `shared_*`, `alp_*`, `sar_*`, dan `far_*` berdasarkan rancangan skema database hasil redesain Fase 1.
  * Memastikan migrasi berjalan sukses secara historis untuk membangun ulang skema database dari awal.

### Langkah 3.3: Setup Proyek Next.js Frontend
* **Target Direktori:** [frontend-app/](file:///d:/dev/sysinfo-harwat/.github/frontend-app/)
* **Aktivitas:**
  * Inisialisasi Next.js menggunakan App Router, TypeScript, dan layout dasar.
  * Buat struktur direktori modular berdasarkan domain bisnis (`features/alp`, `features/sar`, `features/far`).

### Langkah 3.4: Final Review & Serah Terima (Fase 3 Selesai)
* **Aktivitas:**
  * Jalankan build check (`npm run build`) pada backend dan frontend.
  * Jalankan uji coba migrasi (`npm run migration:run`).
  * Hapus seluruh file boilerplate atau file temp yang tidak diperlukan dari inisialisasi framework.
  * **STOP dan ajukan review Fase 3 ke pengguna.**
