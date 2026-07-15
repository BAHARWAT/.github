# BAHARWAT — Badan Pemeliharaan dan Perawatan

BAHARWAT (Baharwathan) adalah komando inspektorat dan otoritas logistik pertahanan yang bertugas mengoordinasikan pemeliharaan, perawatan, perbaikan, dan kelaikan alat peralatan pertahanan dan keamanan serta sarana-prasarana militer lintas matra.

Organisasi ini mengelola ekosistem **Sysinfo Harwat**, sebuah platform manajemen MRO (*Maintenance, Repair, and Overhaul*) terintegrasi yang terbagi dalam arsitektur terdekopel (decoupled).

---

## Portofolio Repositori

Ekosistem Sysinfo Harwat dibagi menjadi tiga repositori utama untuk mendukung skalabilitas, kemudahan pemeliharaan, dan performa tinggi skala nasional:

1. **[.github](https://github.com/BAHARWAT/.github)** (Repositori Ini)
   * Berisi dokumentasi global, PRD (*Product Requirement Document*), standarisasi API, dokumen arsitektur database, dan panduan proses bisnis lintas domain.
2. **[backend-app](https://github.com/BAHARWAT/backend-app)**
   * REST API Backend stateless yang dibangun menggunakan **NestJS** dengan **Fastify adapter** untuk konkurensi tinggi, optimasi performa, serta manajemen data modular.
3. **[frontend-app](https://github.com/BAHARWAT/frontend-app)**
   * Web client interaktif yang dibangun menggunakan **Next.js** (App Router & TypeScript) untuk menyajikan dashboard pemantauan, form wizard, dan analitik MRO.

---

## Domain Bisnis MRO

Sysinfo Harwat melayani tiga domain bisnis militer dengan alur kerja, otorisasi, dan regulasi spesifik:

### 1. ALPALHAN MRO (ALP)
Mengelola konfigurasi teknis aset militer (*fleet*), penelusuran hierarki komponen (*Bill of Materials*), pemeliharaan preventif terjadwal, dan manajemen bengkel kerja/WO (*Work Order*).
* **Dokumentasi Utama:** [docs/database/erd-alp.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-alp.md) & [docs/bispro-diagram-alp.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-alp.md)

### 2. SARHAN MRO (SAR)
Mengelola registrasi sarana-prasarana pertahanan, penilaian kondisi fisik, analisis risiko Kotama (prioritas P1/P2/P3), verifikasi RAB berjenjang, dan konsolidasi Rencana Kebutuhan (Renbut).
* **Dokumentasi Utama:** [docs/database/erd-sar.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-sar.md) & [docs/bispro-diagram-sar.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-sar.md)

### 3. FARHAN MRO (FAR)
Mengelola rantai produksi farmasi militer (LAFI & Puslola), persetujuan rencana produksi, logistik bahan baku obat (BBO), pengawasan mutu (QA/QC), dan Catatan Pengolahan Batch (CPB) Digital.
* **Dokumentasi Utama:** [docs/database/erd-far.md](file:///d:/dev/sysinfo-harwat/.github/docs/database/erd-far.md) & [docs/bispro-diagram-far.md](file:///d:/dev/sysinfo-harwat/.github/docs/bispro-diagram-far.md)

---

## Struktur Dokumentasi Repositori

Dokumentasi repositori disusun secara terstruktur di bawah direktori `docs/`:

```
.github/
├── .agents/
│   ├── AGENTS.md                  # Aturan pengembangan proyek
│   └── workflows/
│       └── INSTRUCTION.md         # Langkah-langkah alur kerja agen
├── docs/
│   ├── api/
│   │   ├── STANDARDIZATION.md     # Standarisasi response dan error API
│   │   └── contracts/             # Kontrak API per fitur (Auth, Invitations, dll)
│   ├── database/
│   │   ├── ERD.md                 # Referensi database utama
│   │   ├── schema-shared.md       # Skema tabel core/shared
│   │   ├── schema-AL.md           # Skema tabel ALPALHAN monolit
│   │   ├── schema-SH.md           # Skema tabel SARHAN monolit
│   │   └── schema-FH.md           # Skema tabel FARHAN monolit
│   ├── BUSINESS-PROCESS.md        # Ringkasan proses bisnis terpadu
│   ├── Glossary.md                # Kamus istilah bisnis lintas domain
│   ├── KNOWN_ISSUES.md            # Catatan issue dan solusi teknis
│   ├── MODULE_BOUNDARY_MAP.md     # Pemetaan batas modul backend/frontend
│   └── STATE_MANAGEMENT_MAP.md    # Strategi manajemen state frontend
├── PRD.md                         # Product Requirement Document global
├── CHANGELOG.md                   # Catatan perubahan rilis
└── README.md                      # Profil organisasi BAHARWAT (File Ini)
```
