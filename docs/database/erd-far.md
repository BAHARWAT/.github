# Redesain ERD & Normalisasi — Domain FARHAN (FAR)

Dokumen ini mendefinisikan hasil redesain, pemisahan, dan normalisasi skema database domain **FARHAN MRO (FAR)** dari skema monolit `harwatdb`.

---

## 1. Diagram ERD Modular (Mermaid)

Karena domain Farhan sangat besar (63 tabel), diagram ERD dibagi menjadi 3 sub-modul logis: **Logistik & Persediaan**, **Production Approval & Workbench**, dan **CPB Digital (Catatan Pengolahan Batch)**.

### Sub-Modul A: Logistik & Persediaan (Logistics)
```mermaid
erDiagram
    shared_users {
        bigint id PK
        varchar name
        varchar email
    }
    far_material {
        char id PK
        varchar kode
        varchar nama
        varchar unit
        varchar kategori
        varchar golongan
        decimal jumlah
        bigint harga_satuan
        timestamp deleted_at
    }
    far_material_batches {
        char id PK
        varchar material_id FK
        varchar batch_no
        decimal qty_tersedia
        varchar status
        varchar quality_status
    }
    far_material_qc_qa_event {
        char id PK
        char material_batch_id FK
        varchar kategori
        varchar tipe
        decimal qty_sample
        varchar hasil
        bigint dilakukan_oleh FK
    }
    far_material_unit_conversions {
        char id PK
        varchar material_id FK
        varchar from_satuan
        varchar to_satuan
        decimal multiplier
    }
    far_stok_ledger {
        char id PK
        char material_id FK
        char material_batch_id FK
        varchar unit
        varchar jenis
        decimal jumlah
        decimal stok_sesudah
    }
    far_produk {
        char id PK
        varchar kode
        varchar nama
        varchar unit
        varchar jenis
        timestamp deleted_at
    }
    far_formula {
        char id PK
        char produk_id FK
        char material_id FK
        decimal jumlah
        varchar satuan
    }
    far_obat_jadi_batch {
        char id PK
        char produksi_id FK
        char produk_id FK
        varchar kode_batch
        decimal qty_tersedia
        varchar status
    }
    far_obat_jadi_ledger {
        char id PK
        char obat_jadi_batch_id FK
        varchar unit
        varchar jenis
        decimal qty
        decimal stok_sesudah
    }

    shared_users ||--o{ far_material_qc_qa_event : "dilakukan_oleh"
    shared_users ||--o{ far_stok_ledger : "dibuat_oleh"
    shared_users ||--o{ far_obat_jadi_ledger : "dibuat_oleh"

    far_material ||--o{ far_formula : "used_in_formula"
    far_material ||--o{ far_material_batches : "has_batches"
    far_material ||--o{ far_material_unit_conversions : "has_conversions"
    far_material ||--o{ far_stok_ledger : "recorded_in_ledger"
    
    far_material_batches ||--o{ far_material_qc_qa_event : "tested_in"
    far_material_batches ||--o{ far_stok_ledger : "batch_recorded_in_ledger"

    far_produk ||--o{ far_formula : "has_ingredients"
    far_produk ||--o{ far_obat_jadi_batch : "produced_batches"
    far_obat_jadi_batch ||--o{ far_obat_jadi_ledger : "recorded_in_ledger"
```

### Sub-Modul B: Production Approval & Workbench
```mermaid
erDiagram
    shared_users {
        bigint id PK
        varchar name
        varchar email
    }
    far_produksi_instruction {
        char id PK
        varchar nomor
        varchar unit
        varchar nama_obat_arahan
        decimal jumlah_arahan
        varchar status
    }
    far_produksi {
        char id PK
        varchar kode_batch
        char produk_id FK
        varchar unit
        int jumlah_batch
        varchar tahap
        varchar approval_status
        char current_bundle_id FK
    }
    far_produksi_material {
        char id PK
        char produksi_id FK
        char material_id FK
        decimal jumlah_plan
        decimal jumlah_realisasi
    }
    far_produksi_tahap {
        char id PK
        char produksi_id FK
        varchar kategori
        varchar tahap
        bigint dilakukan_oleh FK
    }
    far_produksi_bundle {
        char id PK
        varchar kode
        varchar status
        int current_version_number
        bigint dibatalkan_oleh FK
        bigint dibuat_oleh FK
    }
    far_produksi_bundle_member {
        char id PK
        char bundle_id FK
        char produksi_id FK
        varchar unit
        int position
    }
    far_produksi_bundle_version {
        char id PK
        char bundle_id FK
        int version_number
        varchar document_number
        json snapshot_payload
    }
    far_produksi_bundle_decision {
        char id PK
        char bundle_id FK
        char reviewed_version_id FK
        varchar type
        bigint decided_by FK
    }
    far_produksi_bundle_decision_allocation {
        char id PK
        char decision_id FK
        varchar unit
        char material_id FK
        decimal approved_additional_allocation
    }
    far_produksi_bundle_revision_round {
        char id PK
        char bundle_id FK
        int round_number
        varchar status
        bigint opened_by FK
    }
    far_produksi_bundle_revision_item {
        char id PK
        char revision_round_id FK
        char produksi_id FK
        text required_note
        varchar state
        bigint assigned_by FK
    }

    shared_users ||--o{ far_produksi : "submitted_by"
    shared_users ||--o{ far_produksi : "approved_by"
    shared_users ||--o{ far_produksi_bundle : "created_by"
    shared_users ||--o{ far_produksi_bundle : "finalized_by"
    shared_users ||--o{ far_produksi_bundle_decision : "decided_by"
    shared_users ||--o{ far_produksi_bundle_revision_item : "assigned_to"

    far_produksi_instruction ||--o{ far_produksi : "spawns_proposal"
    far_produksi ||--o{ far_produksi_material : "requires_materials"
    far_produksi ||--o{ far_produksi_tahap : "requires_steps"
    
    far_produksi_bundle ||--o{ far_produksi_bundle_version : "has_versions"
    far_produksi_bundle ||--o{ far_produksi_bundle_member : "contains_proposals"
    far_produksi ||--o{ far_produksi_bundle_member : "bundled_in"
    
    far_produksi_bundle ||--o{ far_produksi_bundle_decision : "has_decisions"
    far_produksi_bundle_decision ||--o{ far_produksi_bundle_decision_allocation : "allocates_extra_materials"
    
    far_produksi_bundle ||--o{ far_produksi_bundle_revision_round : "has_revision_rounds"
    far_produksi_bundle_revision_round ||--o{ far_produksi_bundle_revision_item : "contains_revision_items"
    far_produksi ||--o{ far_produksi_bundle_revision_item : "revised_in"
```

### Sub-Modul C: CPB Digital (Catatan Pengolahan Batch)
```mermaid
erDiagram
    shared_users {
        bigint id PK
        varchar name
        varchar email
    }
    far_produk {
        char id PK
        varchar kode
        varchar nama
        timestamp deleted_at
    }
    far_cpb_template {
        char id PK
        char produk_id FK
        varchar unit
        varchar version
        tinyint is_active
        timestamp deleted_at
    }
    far_cpb_template_tahap {
        char id PK
        char template_id FK
        int urutan
        varchar kode
        varchar nama
        enum pemilik_role
    }
    far_cpb_template_qc_step {
        char id PK
        char template_id FK
        int urutan
        varchar kode
        varchar nama
        enum pemilik_role
    }
    far_produksi {
        char id PK
        varchar kode_batch
        char produk_id FK
        varchar unit
    }
    far_cpb_batch {
        char id PK
        char produksi_id FK
        char template_id FK
        varchar status
        bigint finalized_by FK
    }
    far_cpb_batch_tahap {
        char id PK
        char cpb_batch_id FK
        char template_tahap_id FK
        int urutan
        varchar kode
        varchar status
        bigint operator_id FK
    }
    far_cpb_batch_qc_step {
        char id PK
        char cpb_batch_id FK
        char template_qc_step_id FK
        int urutan
        varchar kode
        varchar status
        bigint operator_id FK
    }
    far_cpb_batch_ipc_sample {
        char id PK
        char cpb_batch_qc_step_id FK
        int urutan
        varchar label
        varchar hasil_ms_tms
        bigint diambil_oleh FK
    }
    far_cpb_batch_kemas_reconciliation {
        char id PK
        char cpb_batch_id FK
        varchar nama_kemas
        decimal target
        decimal dipakai
        decimal rusak
    }
    far_cpb_batch_signature {
        char id PK
        char cpb_batch_id FK
        varchar scan_path
        bigint uploaded_by FK
    }
    far_cpb_batch_audit_log {
        char id PK
        char cpb_batch_id FK
        varchar action
        bigint user_id FK
    }

    shared_users ||--o{ far_cpb_batch : "finalized_by"
    shared_users ||--o{ far_cpb_batch_tahap : "completed_by"
    shared_users ||--o{ far_cpb_batch_qc_step : "completed_by"
    shared_users ||--o{ far_cpb_batch_signature : "uploaded_by"

    far_produk ||--o{ far_cpb_template : "blueprint_for"
    far_cpb_template ||--o{ far_cpb_template_tahap : "has_tahap_templates"
    far_cpb_template ||--o{ far_cpb_template_qc_step : "has_qc_templates"

    far_produksi ||--o{ far_cpb_batch : "executes_production"
    far_cpb_template ||--o{ far_cpb_batch : "bootstraps"
    
    far_cpb_batch ||--o{ far_cpb_batch_tahap : "has_stages"
    far_cpb_batch ||--o{ far_cpb_batch_qc_step : "has_qc_steps"
    far_cpb_batch_qc_step ||--o{ far_cpb_batch_ipc_sample : "takes_samples"
    
    far_cpb_batch ||--o{ far_cpb_batch_kemas_reconciliation : "reconciles_packaging"
    far_cpb_batch ||--o{ far_cpb_batch_signature : "signed_by"
    far_cpb_batch ||--o{ far_cpb_batch_audit_log : "tracked_in"
```

---

## 2. Penyesuaian Skema & Normalisasi Kolom (far_*)

Untuk meningkatkan efisiensi dan keamanan data, penyesuaian (*adjustments*) berikut diterapkan pada berkas `baharwat-schema-only.sql`:

1. **Perbaikan Tipe Data Desimal (Truncation Fix):**
   * Semua kolom volume, kuantitas bahan, dan multiplier konversi yang terpotong tipe desimalnya (`decimal(15` atau `decimal(18`) dikembalikan ke presisi yang benar:
     * `multiplier` dan `conversion_multiplier` -> `decimal(18, 6)`
     * `bobot_per_satuan` & `jumlah` (formula) -> `decimal(15, 4)`
     * `volume`, `jumlah` (stok/mutasi) -> `decimal(15, 2)`
     * `latitude`, `longitude` -> `decimal(10, 7)`
2. **Indeks Komposit & FK:**
   * `far_cpb_batch_audit_log` -> `INDEX (cpb_batch_id, created_at)`
   * `far_cpb_batch_tahap` -> `INDEX (cpb_batch_id, urutan)`
   * `far_cpb_batch_qc_step` -> `INDEX (cpb_batch_id, urutan)`
   * `far_stok_ledger` -> `INDEX (material_id, material_batch_id, unit)` (Mempercepat perhitungan live saldo stok).
   * Indeks foreign key dipasang pada seluruh relasi relasional utama.
3. **Full-Text Indexing (Pencarian Cepat):**
   * `far_material` -> `FULLTEXT INDEX (kode, nama)`
   * `far_produk` -> `FULLTEXT INDEX (kode, nama)`
   * `far_produksi_instruction` -> `FULLTEXT INDEX (nomor, nama_obat_arahan)`
   * `far_puslola_renbut_items` -> `FULLTEXT INDEX (uraian)`
4. **Relasi Foreign Key Bersama (Shared):**
   * Semua referensi pengguna seperti `operator_id`, `approved_oleh`, `dibuat_oleh`, `decided_by`, `assigned_by`, dll. diarahkan secara tegas merujuk ke `shared_users.id`.
5. **Dukungan Soft Delete (deleted_at):**
   * Kolom `deleted_at` `timestamp` NULL ditambahkan pada tabel-tabel master data penting berikut guna menjaga integritas data historis:
     * `far_material`
     * `far_produk`
     * `far_cpb_template`
     * `far_suppliers`
     * `far_personel`
