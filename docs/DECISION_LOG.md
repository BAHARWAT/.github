<!-- @format -->

# DECISION_LOG — Sysinfo Harwat

Dokumen ini mencatat keputusan teknis dan arsitektur project rewrite Sysinfo Harwat. Setiap keputusan bersifat final kecuali ada revisi eksplisit dengan tanggal dan alasan.

Snapshot: 15 Juli 2026.

---

## 1. Konteks Project

- **Jenis:** Rewrite dari existing Laravel monolith (`harwatdb`, PostgreSQL)
- **Existing system:** Monolit Laravel dengan 3 domain — Alpalhan MRO, Sarhan MRO, Farhan MRO
- **Target:** Sistem baru dengan decoupled architecture, modular per domain, full stack terpisah antara backend dan frontend
- **Platform target:** AWS (produksi), Docker + Docker Compose (local development)
- **User scope:** Skala nasional (se-Indonesia)
- **Mobile:** Tidak ada (web only untuk saat ini)
- **Integrasi eksternal:** Tidak ada (kecuali MinIO/S3 untuk file storage)
- **SLA formal:** Tidak ada

---

## 2. Arsitektur

### 2.1 Decoupled Architecture

- Backend dan frontend adalah aplikasi terpisah, dikembangkan dan di-deploy secara independen
- Backend menyediakan REST API; frontend mengonsumsi API tersebut
- Tidak ada server-side rendering dari backend
- Komunikasi: HTTP/JSON

### 2.2 Repository Organization

GitHub Organization dengan 3 repository:

| Repo       | Isi                                                |
| ---------- | -------------------------------------------------- |
| `.github`  | Profile organisasi, dokumentasi, template issue/PR |
| `backend`  | NestJS API — seluruh domain AL, SH, FH             |
| `frontend` | Web client — seluruh domain AL, SH, FH             |

### 2.3 Domain Modular

3 domain kanonis dengan naming singkatan:

| Singkatan | Nama Lengkap | Fokus                                                      |
| --------- | ------------ | ---------------------------------------------------------- |
| `AL`      | Alpalhan MRO | Aset, BOM, pemeliharaan, WO, inspeksi                      |
| `SH`      | Sarhan MRO   | Inventaris sarana-prasarana, pengajuan, Renbut             |
| `FH`      | Farhan MRO   | LAFI, Puslola, Logistics, Production Approval, CPB Digital |

Setiap domain adalah module independen di dalam satu monorepo backend/frontend. Antar domain tidak boleh ada dependency silang langsung — hanya boleh via shared module.

---

## 3. Tech Stack

### 3.1 Backend

| Layer         | Pilihan                               |
| ------------- | ------------------------------------- |
| Framework     | NestJS (Node.js + TypeScript)         |
| Database      | PostgreSQL                            |
| ORM           | Prisma                                |
| Cache / Queue | Redis (BullMQ)                        |
| File Storage  | MinIO (local dev) / AWS S3 (produksi) |
| Auth          | JWT (access token + refresh token)    |

### 3.2 Frontend

- Stack diputuskan setelah backend API contract selesai

### 3.3 Infrastruktur

| Environment       | Stack                   |
| ----------------- | ----------------------- |
| Local development | Docker + Docker Compose |
| Produksi          | AWS + Kubernetes        |

- **Local Development Environment:** Wajib menggunakan Docker + Docker Compose di root directory untuk menjalankan seluruh service secara terintegrasi (backend, database, redis, storage/MinIO).

---

## 4. Database

### 4.1 Engine

- **Existing:** MySQL 8.4.7
- **Rewrite:** PostgreSQL
- Migrasi data dari MySQL ke PostgreSQL dilakukan setelah schema baru final

### 4.2 Konvensi ID

- Seluruh tabel menggunakan **ULID** (`char(26)`) sebagai primary key
- Existing schema tidak konsisten (sebagian ULID, sebagian bigint AUTO_INCREMENT) — diperbaiki di schema baru

### 4.3 Prefix Tabel per Domain

| Domain        | Prefix tabel |
| ------------- | ------------ |
| Alpalhan      | `al_`        |
| Sarhan        | `sh_`        |
| Farhan        | `fh_`        |
| Shared / Auth | tanpa prefix |

Prefix existing (`alpalhan_new_*`, `sarhan_new_*`, `farhan_new_*`) tidak dipertahankan.

### 4.4 ERD

- ERD dibuat **per domain** (AL, SH, FH terpisah) — tidak digabung dalam satu diagram
- ERD dibuat setelah PRD backend rewrite selesai
- Urutan: `PRD backend rewrite → ERD per domain → implementasi`

---

## 5. Konvensi API

- Prefix: `/api/v1/`
- Response wrapper: `{ success, message, data }`
- Error format: `{ success: false, message, errors: array | null }`
- Tidak ada `statusCode` di dalam JSON body
- Pagination: diputuskan saat API contract per domain dibuat

---

## 6. Konvensi Kode

- Conventional Commits: `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`
- Prettier wajib sebelum setiap commit
- Tidak ada inline code comment — penjelasan di luar kode (JSDoc atau dokumentasi terpisah)
- Tidak ada `console.log` di production code
- Git commit: agent boleh commit mandiri; **push wajib konfirmasi eksplisit user**
- Branch naming: `feat/<slug>`, `fix/<slug>`, `chore/<slug>`
- Tidak ada commit langsung ke `main`

---

## 7. Urutan Kerja yang Disepakati

1. `DECISION_LOG.md` — dokumen ini
2. `GLOSSARY.md` — istilah domain bisnis per domain (AL, SH, FH)
3. `docs/database/schema-AL.md`, `schema-SH.md`, `schema-FH.md` — inventarisasi tabel existing per domain
4. PRD backend rewrite — fokus backend only (entitas, relasi, state flow, business rule, actor per domain)
5. ERD per domain — berdasarkan PRD backend
6. `AGENTS.md` + `INSTRUCTION.md`
7. Implementasi backend (NestJS)
8. Implementasi frontend (stack diputuskan setelah API contract selesai)

---

## 8. Hal yang Belum Diputuskan

| Item                                     | Status                                |
| ---------------------------------------- | ------------------------------------- |
| Frontend tech stack                      | Menunggu setelah API contract selesai |
| Strategi migrasi data MySQL → PostgreSQL | Menunggu schema baru final            |
| Multi-tenancy / RLS                      | Belum dikonfirmasi                    |
| Notifikasi realtime (WebSocket / SSE)    | Belum dikonfirmasi                    |
| Role dan permission granular per domain  | Mengikuti PRD backend rewrite         |
