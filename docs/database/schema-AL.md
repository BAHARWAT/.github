# Schema Existing — Domain AL (Alpalhan MRO)

Source: `harwatdb` MySQL 8.4.7. Snapshot 15 Juli 2026.

Total tabel: 18

### `alpalhan_new_bom_nodes`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `fleet_id` | `char(26)` | Tidak |
| `parent_id` | `char(26)` | Ya |
| `type` | `enum('SYS','ASSY','PART')` | Tidak |
| `nomenclature` | `varchar(255)` | Tidak |
| `part_number` | `varchar(255)` | Tidak |
| `serial_number` | `varchar(255)` | Ya |
| `qty_required` | `int` | Tidak |
| `qty_fitted` | `int` | Tidak |
| `is_defect` | `tinyint(1)` | Tidak |
| `is_degraded` | `tinyint(1)` | Tidak |
| `persentase` | `tinyint` | Tidak |
| `keterangan` | `text` | Ya |
| `uraian_kerusakan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_bom_task_templates`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `task_template_id` | `char(26)` | Tidak |
| `bom_node_id` | `char(26)` | Tidak |
| `is_required` | `tinyint(1)` | Tidak |
| `sort_order` | `int` | Tidak |
| `applicability_notes` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_facilities`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `bigint` | Tidak |
| `kode` | `varchar(255)` | Ya |
| `name` | `varchar(255)` | Tidak |
| `base_id` | `varchar(255)` | Ya |
| `matra_id` | `varchar(10)` | Tidak |
| `level` | `enum('ringan','teknik','depo')` | Tidak |
| `description` | `text` | Ya |
| `latitude` | `decimal(10` | Ya |
| `longitude` | `decimal(10` | Ya |
| `status` | `enum('aktif','nonaktif')` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_fleets`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `tail_number` | `varchar(255)` | Tidak |
| `serial_number` | `varchar(255)` | Tidak |
| `model` | `varchar(255)` | Tidak |
| `matra` | `enum('AD','AL','AU')` | Tidak |
| `kategori` | `varchar(255)` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `base` | `varchar(255)` | Tidak |
| `foto_url` | `varchar(255)` | Ya |
| `status` | `enum('FMC','PMC','NMC')` | Tidak |
| `e_doc_path` | `varchar(255)` | Ya |
| `running_hours` | `decimal(10` | Tidak |
| `cycles` | `int` | Tidak |
| `next_maintenance` | `varchar(255)` | Ya |
| `remaining_hours` | `decimal(10` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `sertifikat_kelaikan` | `text` | Ya |
| `riwayat_docking` | `text` | Ya |
| `kesimpulan` | `text` | Ya |
| `kesiapan` | `text` | Ya |

### `alpalhan_new_laphar`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `fleet_id` | `char(26)` | Tidak |
| `user_id` | `bigint` | Tidak |
| `matra` | `enum('AD','AL','AU')` | Tidak |
| `tanggal` | `date` | Tidak |
| `kesiapan_persen` | `decimal(5` | Tidak |
| `catatan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_laphar_al_entries`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `laphar_id` | `char(26)` | Tidak |
| `bom_node_id` | `char(26)` | Tidak |
| `kode_sistem` | `varchar(255)` | Tidak |
| `komponen` | `varchar(255)` | Tidak |
| `kondisi` | `enum('S','TS')` | Tidak |
| `persentase` | `tinyint` | Tidak |
| `jam_putar` | `int` | Ya |
| `uraian_kerusakan` | `text` | Ya |
| `pelaksana` | `varchar(255)` | Ya |
| `keterangan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_pengajuan_harwat`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `renbut_id` | `char(26)` | Ya |
| `kode` | `varchar(255)` | Tidak |
| `fleet_id` | `char(26)` | Tidak |
| `user_id` | `bigint` | Tidak |
| `sumber_laporan` | `enum('Pilot','Mekanik','Inspeksi` | Tidak |
| `kategori` | `enum('Terjadwal','Tidak` | Tidak |
| `tingkat` | `enum('Ringan','Sedang','Berat')` | Tidak |
| `fasilitas_nama` | `varchar(255)` | Ya |
| `prioritas` | `enum('NORMAL','TINGGI','KRITIS')` | Tidak |
| `periode` | `varchar(10)` | Tidak |
| `deskripsi` | `text` | Tidak |
| `estimasi_biaya` | `bigint` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `jalur` | `varchar(255)` | Ya |
| `stock_status` | `varchar(255)` | Ya |
| `approved_mabes_by` | `bigint` | Ya |
| `approved_mabes_at` | `timestamp` | Ya |
| `approved_baharwathan_by` | `bigint` | Ya |
| `approved_baharwathan_at` | `timestamp` | Ya |
| `rejected_by` | `bigint` | Ya |
| `rejected_at` | `timestamp` | Ya |
| `catatan_reviewer` | `text` | Ya |
| `alasan_reject` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_pengajuan_parts`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `pengajuan_id` | `char(26)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `part_number` | `varchar(255)` | Tidak |
| `qty_diminta` | `int` | Tidak |
| `qty_gudang` | `int` | Tidak |
| `status_stok` | `enum('TERSEDIA','KURANG','KOSONG')` | Tidak |
| `harga_satuan` | `bigint` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_pengajuan_timeline`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `pengajuan_id` | `char(26)` | Tidak |
| `user_id` | `bigint` | Tidak |
| `level` | `tinyint` | Tidak |
| `aksi` | `varchar(255)` | Tidak |
| `catatan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_renbut`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `judul` | `varchar(255)` | Ya |
| `periode` | `varchar(255)` | Tidak |
| `matra` | `enum('AD','AL','AU')` | Tidak |
| `user_id` | `bigint` | Tidak |
| `status` | `enum('DRAFT','SUBMITTED','REVIEW','REVISION','APPROVED','REJECTED')` | Tidak |
| `total_nilai` | `bigint` | Tidak |
| `catatan_mabes` | `text` | Ya |
| `alasan_revisi` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `ttd_nama` | `varchar(255)` | Ya |
| `ttd_jabatan` | `varchar(255)` | Ya |

### `alpalhan_new_supply_items`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `part_number` | `varchar(255)` | Tidak |
| `kategori` | `enum('Komponen','Suku` | Tidak |
| `matra` | `enum('AD','AL','AU')` | Tidak |
| `lokasi_gudang` | `varchar(255)` | Tidak |
| `stok` | `int` | Tidak |
| `minimum_stok` | `int` | Tidak |
| `harga_satuan` | `bigint` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_task_template_references`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `task_template_id` | `char(26)` | Tidak |
| `reference_type` | `varchar(255)` | Tidak |
| `reference_label` | `varchar(255)` | Ya |
| `title` | `varchar(255)` | Ya |
| `description` | `text` | Ya |
| `source_document` | `varchar(255)` | Ya |
| `uri` | `varchar(255)` | Ya |
| `sort_order` | `int` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_task_template_steps`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `task_template_id` | `char(26)` | Tidak |
| `main_activity` | `varchar(255)` | Ya |
| `procedure_reference` | `varchar(255)` | Ya |
| `step_number` | `varchar(255)` | Ya |
| `stage` | `varchar(255)` | Ya |
| `action_detail` | `text` | Tidak |
| `acceptance_criteria` | `text` | Ya |
| `related_figure` | `varchar(255)` | Ya |
| `sort_order` | `int` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_task_templates`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `code` | `varchar(255)` | Ya |
| `matra` | `enum('AD','AL','AU')` | Ya |
| `platform_name` | `varchar(255)` | Ya |
| `component_name` | `varchar(255)` | Ya |
| `package` | `varchar(255)` | Ya |
| `task_name` | `varchar(255)` | Tidak |
| `trigger_type` | `enum('CALENDAR','OPERATING_HOURS','EVENT')` | Tidak |
| `interval_value` | `decimal(10` | Ya |
| `interval_unit` | `varchar(255)` | Ya |
| `interval_label` | `varchar(255)` | Tidak |
| `maintenance_level` | `varchar(255)` | Ya |
| `duration_minutes` | `int` | Ya |
| `duration_label` | `varchar(255)` | Ya |
| `personnel_requirements` | `json` | Ya |
| `tool_requirements` | `json` | Ya |
| `equipment_requirements` | `json` | Ya |
| `spare_part_requirements` | `json` | Ya |
| `main_activities` | `json` | Ya |
| `source_document` | `varchar(255)` | Ya |
| `source_reference` | `varchar(255)` | Ya |
| `is_active` | `tinyint(1)` | Tidak |
| `notes` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_work_order_requirements`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `work_order_id` | `char(26)` | Tidak |
| `work_order_task_template_id` | `char(26)` | Ya |
| `requirement_type` | `varchar(255)` | Tidak |
| `name` | `text` | Tidak |
| `status` | `enum('pending','ready','issued','not_available','returned')` | Tidak |
| `checked_by` | `bigint` | Ya |
| `checked_at` | `timestamp` | Ya |
| `notes` | `text` | Ya |
| `sort_order` | `int` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_work_order_steps`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `work_order_id` | `char(26)` | Tidak |
| `work_order_task_template_id` | `char(26)` | Ya |
| `task_template_step_id` | `char(26)` | Ya |
| `main_activity` | `varchar(255)` | Ya |
| `step_number` | `varchar(255)` | Ya |
| `stage` | `varchar(255)` | Ya |
| `action_detail` | `text` | Tidak |
| `acceptance_criteria` | `text` | Ya |
| `status` | `enum('pending','in_progress','done','skipped','na')` | Tidak |
| `checked_by` | `bigint` | Ya |
| `checked_at` | `timestamp` | Ya |
| `notes` | `text` | Ya |
| `sort_order` | `int` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_work_order_task_templates`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `work_order_id` | `char(26)` | Tidak |
| `task_template_id` | `char(26)` | Tidak |
| `component_name_snapshot` | `varchar(255)` | Ya |
| `task_name_snapshot` | `varchar(255)` | Tidak |
| `interval_label_snapshot` | `varchar(255)` | Tidak |
| `maintenance_level_snapshot` | `varchar(255)` | Ya |
| `source_document_snapshot` | `varchar(255)` | Ya |
| `source_reference_snapshot` | `varchar(255)` | Ya |
| `next_due_snapshot` | `varchar(255)` | Ya |
| `target_start_date` | `date` | Ya |
| `target_finish_date` | `date` | Ya |
| `notes` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `alpalhan_new_work_orders`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `pengajuan_id` | `char(26)` | Ya |
| `fleet_id` | `char(26)` | Tidak |
| `tingkat` | `enum('Ringan','Sedang','Berat')` | Tidak |
| `kategori` | `enum('Terjadwal','Tidak` | Tidak |
| `status` | `enum('BACKLOG','PLANNED','IN_PROGRESS','QC','DONE')` | Tidak |
| `teknisi` | `varchar(255)` | Ya |
| `pelaksana` | `enum('SATKER','L2_PEMELIHARA','L3_DEPO')` | Ya |
| `progress` | `tinyint` | Tidak |
| `estimasi_jam` | `int` | Tidak |
| `prioritas` | `enum('NORMAL','TINGGI','KRITIS')` | Tidak |
| `deskripsi` | `text` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `fasilitas_id` | `bigint` | Ya |