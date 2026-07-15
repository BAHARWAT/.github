# Alur Kerja & Tahapan Dekopel Proyek Sysinfo Harwat

Dokumen ini mendefinisikan fase dan tahapan pengerjaan untuk melakukan redesain database serta inisialisasi scaffolding Next.js (frontend) dan NestJS (backend). 

> [!IMPORTANT]
> **Kewajiban Peninjauan Berjenjang:**
> Setiap kali satu Fase selesai sepenuhnya, agen **WAJIB** berhenti, memberikan rincian (*breakdown*) hasil pengerjaan, dan meminta persetujuan serta tinjauan dari pengguna terlebih dahulu sebelum dapat melanjutkan ke Fase berikutnya.

---

## Fase 1: Pemindaian, Redesain, dan Normalisasi Skema Database

Tujuan: Merancang ulang skema database dari `baharwat-schema-only.sql` dengan menerapkan penyesuaian (*adjustment*) tabel, normalisasi (seperti penambahan index, *full-text index*), dan standarisasi prefix baru.

### Langkah 1.1: Pemisahan & Redesain ERD ALPALHAN (ALP) [SELESAI]
* **Target File:** [docs/database/erd-alp.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-alp.md)

### Langkah 1.2: Pemisahan & Redesain ERD SARHAN (SAR) [SELESAI]
* **Target File:** [docs/database/erd-sar.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-sar.md)

### Langkah 1.3: Pemisahan & Redesain ERD FARHAN (FAR) [SELESAI]
* **Target File:** [docs/database/erd-far.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-far.md)

### Langkah 1.4: Redesain Skema Bersama (Shared/Core) [SELESAI]
* **Target File:** [docs/database/erd-shared.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-shared.md)

### Langkah 1.5: Review & Bersihkan File Tidak Perlu [SELESAI]

---

## Fase 2: Diagram Alur Proses Bisnis (BISPRO) [SELESAI]

Tujuan: Mendokumentasikan alur logika bisnis dan transisi status workflow berdasarkan database baru hasil Fase 1.

### Langkah 2.1: Diagram BISPRO ALPALHAN [SELESAI]
* **Target File:** [docs/bispro-diagram-alp.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-alp.md)

### Langkah 2.2: Diagram BISPRO SARHAN [SELESAI]
* **Target File:** [docs/bispro-diagram-sar.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-sar.md)

### Langkah 2.3: Diagram BISPRO FARHAN [SELESAI]
* **Target File:** [docs/bispro-diagram-far.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-far.md)

### Langkah 2.4: Review & Bersihkan File Tidak Perlu [SELESAI]

---

## Fase 3: Inisialisasi Scaffolding Proyek, Migrasi Database, & Docker

Tujuan: Membuat kerangka proyek baru untuk frontend dan backend dengan struktur folder modular, menyusun skema migrasi database yang siap pakai, serta melakukan kontainerisasi (Docker).

### Langkah 3.1: Setup Proyek NestJS Backend & Docker Compose
* **Target Direktori:** [backend-app/](file:///d:/dev/sysinfo-harwat/backend-app/)
* **Aktivitas:**
  * Inisialisasi NestJS menggunakan Fastify adapter.
  * Setup ORM (TypeORM) dengan konfigurasi koneksi database.
  * Buat struktur modul: `shared`, `alp`, `sar`, `far`.
  * **[NEW]** Setup `Dockerfile` untuk backend NestJS (multi-stage build).
  * **[NEW]** Buat `docker-compose.yml` di root workspace untuk menjalankan infrastruktur database (MySQL), cache & queue (Redis), dan storage (MinIO) secara lokal.

### Langkah 3.2: Implementasi Berkas Migrasi Database (TypeORM Migrations)
* **Target Direktori:** `backend-app/src/migrations/`
* **Aktivitas:**
  * Membuat berkas migrasi TypeScript per tabel/langkah (`up()` dan `down()`) untuk seluruh tabel `shared_*`, `alp_*`, `sar_*`, dan `far_*` berdasarkan rancangan skema database hasil redesain Fase 1.
  * Memastikan migrasi berjalan sukses secara historis untuk membangun ulang skema database dari awal.

### Langkah 3.3: Setup Proyek Next.js Frontend & Docker
* **Target Direktori:** [frontend-app/](file:///d:/dev/sysinfo-harwat/frontend-app/)
* **Aktivitas:**
  * Inisialisasi Next.js menggunakan App Router, TypeScript, dan layout dasar.
  * Menerapkan struktur folder modular *Domain-Driven* yang memisahkan routing, komponen bersama, dan fitur spesifik domain (`src/app/`, `src/features/`, `src/components/`, `src/core/`).
  * **[NEW]** Setup `Dockerfile` untuk frontend Next.js (multi-stage build).

### Langkah 3.4: Final Review & Serah Terima (Fase 3 Selesai)
* **Aktivitas:**
  * Jalankan build check (`npm run build`) pada backend dan frontend.
  * Jalankan uji coba migrasi (`npm run migration:run`).
  * Uji coba menjalankan seluruh kontainer menggunakan `docker compose up -d` untuk memastikan layanan terintegrasi dengan baik.
  * Hapus seluruh file boilerplate atau file temp yang tidak diperlukan dari inisialisasi framework.
  * **STOP dan ajukan review Fase 3 ke pengguna.**
