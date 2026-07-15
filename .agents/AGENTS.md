# Aturan Pengembangan Proyek — Sysinfo Harwat

Dokumen ini mendefinisikan aturan spesifik proyek untuk memandu agen agar tetap selaras dengan arsitektur terdekopel, konvensi database baru, dan standar teknologi yang disepakati.

---

## 1. Arsitektur & Teknologi Stack

* **Frontend:** Next.js (TypeScript, App Router) di dalam direktori `frontend-app/`.
* **Backend:** NestJS (TypeScript, Fastify Adapter) di dalam direktori `backend-app/`.
* **Database ORM:** Menggunakan Prisma atau TypeORM untuk pengelolaan migrasi dan akses data.
* **Komunikasi:** REST API stateless dengan prefix `/api/v1/` dan otentikasi berbasis JWT.

---

## 2. Redesain & Normalisasi Database

Setiap tabel yang diambil dari `baharwat-schema-only.sql` wajib dirancang ulang dengan ketentuan berikut:

* **Konvensi Prefix Baru:**
  * ALPALHAN: `alpalhan_new_*` menjadi `alp_*`
  * SARHAN: `sarhan_new_*` menjadi `sar_*`
  * FARHAN: `farhan_new_*` menjadi `far_*`
  * Bersama/Core: Tabel bersama (seperti `users`, `notifications`, `cache`) diubah menjadi berprefix `shared_*` (contoh: `shared_users`).
* **Integritas Bisnis:** Field, tipe data, dan relasi foreign key dari skema asli wajib dipertahankan untuk menjamin fungsionalitas proses bisnis dan kode yang ada tidak rusak.
* **Normalisasi & Optimasi Indexing:**
  * Tambahkan indeks komposit (*composite index*) pada kolom yang sering digunakan bersama dalam kueri filter.
  * Tambahkan indeks pada kolom foreign key untuk mengoptimalkan performa operasi join.
  * Tambahkan *full-text index* pada kolom teks panjang (seperti catatan, uraian kerusakan, nama komponen) yang sering digunakan untuk kueri pencarian teks.

---

## 3. Disiplin Kerja Agen

* **Kepatuhan Alur Kerja:** Wajib merujuk dan mengeksekusi langkah-langkah di [.agents/workflows/INSTRUCTION.md](file:///d:/dev/sysinfo-harwat/.github/.agents/workflows/INSTRUCTION.md) secara berurutan, satu per satu. Dilarang melompat langkah tanpa persetujuan tertulis dari pengguna.
* **Verifikasi Sebelum Mengajukan:** Lakukan verifikasi sintaks diagram Mermaid dan pastikan kode dapat dikompilasi dengan sukses (`npm run build`) sebelum meminta izin commit/push.
* **Biarkan di Staging Area:** Biarkan perubahan dalam status unstaged/staged saja. Commit dan push hanya dilakukan setelah menerima instruksi tertulis eksplisit dari pengguna.
* **Dokumentasi Paralel:** Setiap kali ada perubahan skema database atau API, perbarui berkas dokumentasi terkait (seperti ERD, API Contract, dan CHANGELOG) secara paralel.
