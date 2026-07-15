# Schema Existing — Domain FH (Farhan MRO)

Source: `harwatdb` MySQL 8.4.7. Snapshot 15 Juli 2026.

Total tabel: 63

### `farhan_new_cpb_batch`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `produksi_id` | `char(26)` | Tidak |
| `template_id` | `char(26)` | Tidak |
| `template_version_snapshot` | `varchar(255)` | Tidak |
| `header_meta_snapshot` | `json` | Ya |
| `bulan_kadaluarsa` | `varchar(255)` | Ya |
| `status` | `varchar(255)` | Tidak |
| `docx_generated_at` | `timestamp` | Ya |
| `docx_path` | `varchar(255)` | Ya |
| `finalized_at` | `timestamp` | Ya |
| `finalized_by` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_cpb_batch_audit_log`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `cpb_batch_id` | `char(26)` | Tidak |
| `cpb_batch_tahap_id` | `char(26)` | Ya |
| `cpb_batch_qc_step_id` | `char(26)` | Ya |
| `action` | `varchar(255)` | Tidak |
| `field_key` | `varchar(255)` | Ya |
| `old_value` | `json` | Ya |
| `new_value` | `json` | Ya |
| `user_id` | `bigint` | Ya |
| `ip_address` | `varchar(255)` | Ya |
| `user_agent` | `varchar(255)` | Ya |
| `created_at` | `timestamp` | Tidak |

### `farhan_new_cpb_batch_ipc_sample`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `cpb_batch_qc_step_id` | `char(26)` | Tidak |
| `urutan` | `int` | Tidak |
| `label` | `varchar(255)` | Ya |
| `data` | `json` | Ya |
| `hasil_ms_tms` | `varchar(255)` | Ya |
| `diambil_pada` | `datetime` | Ya |
| `diambil_oleh` | `bigint` | Ya |
| `catatan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_cpb_batch_kemas_reconciliation`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `cpb_batch_id` | `char(26)` | Tidak |
| `nama_kemas` | `varchar(255)` | Tidak |
| `satuan` | `varchar(255)` | Ya |
| `target` | `decimal(15` | Tidak |
| `dipakai` | `decimal(15` | Ya |
| `rusak` | `decimal(15` | Ya |
| `kelebihan` | `decimal(15` | Ya |
| `selisih_persen` | `decimal(8` | Ya |
| `status` | `varchar(255)` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_cpb_batch_qc_step`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `cpb_batch_id` | `char(26)` | Tidak |
| `template_qc_step_id` | `char(26)` | Tidak |
| `urutan` | `int` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `data` | `json` | Ya |
| `operator_id` | `bigint` | Ya |
| `catatan` | `text` | Ya |
| `completed_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_cpb_batch_signature`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `cpb_batch_id` | `char(26)` | Tidak |
| `scan_path` | `varchar(255)` | Tidak |
| `file_size` | `bigint` | Tidak |
| `mime_type` | `varchar(255)` | Ya |
| `uploaded_at` | `timestamp` | Tidak |
| `uploaded_by` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_cpb_batch_tahap`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `cpb_batch_id` | `char(26)` | Tidak |
| `template_tahap_id` | `char(26)` | Tidak |
| `urutan` | `int` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `data` | `json` | Ya |
| `operator_id` | `bigint` | Ya |
| `serah_dari` | `bigint` | Ya |
| `serah_ke` | `bigint` | Ya |
| `serah_tanggal` | `date` | Ya |
| `catatan_deviasi` | `text` | Ya |
| `completed_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_cpb_template`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `produk_id` | `char(26)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `version` | `varchar(255)` | Tidak |
| `is_active` | `tinyint(1)` | Tidak |
| `nie` | `varchar(255)` | Ya |
| `bobot_per_satuan` | `decimal(15` | Ya |
| `satuan_dasar` | `varchar(255)` | Ya |
| `kemasan` | `varchar(255)` | Ya |
| `pelarut` | `varchar(255)` | Ya |
| `docx_template_path` | `varchar(255)` | Ya |
| `header_meta` | `json` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_cpb_template_qc_step`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `template_id` | `char(26)` | Tidak |
| `urutan` | `int` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `deskripsi` | `text` | Ya |
| `related_tahap_kode` | `varchar(255)` | Ya |
| `schema_fields` | `json` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `pemilik_role` | `enum('operator','qc','qa')` | Tidak |

### `farhan_new_cpb_template_tahap`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `template_id` | `char(26)` | Tidak |
| `urutan` | `int` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `deskripsi` | `text` | Ya |
| `schema_fields` | `json` | Tidak |
| `catatan_protap` | `json` | Ya |
| `pemilik_role` | `enum('operator','qc','qa')` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_disposable`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `operator_id` | `bigint` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `tanggal` | `date` | Tidak |
| `tujuan` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `surat_qa` | `varchar(255)` | Ya |
| `surat_qc` | `varchar(255)` | Ya |
| `approved_oleh` | `bigint` | Ya |
| `approved_at` | `timestamp` | Ya |
| `alasan_tolak` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_disposable_item`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `disposable_id` | `char(26)` | Tidak |
| `material_id` | `char(26)` | Tidak |
| `jumlah` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `alasan` | `varchar(255)` | Tidak |

### `farhan_new_formula`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `produk_id` | `char(26)` | Tidak |
| `material_id` | `char(26)` | Tidak |
| `jumlah` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `keterangan` | `varchar(255)` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_inventaris_alat`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `merek_tipe` | `varchar(255)` | Ya |
| `tanggal_perolehan` | `date` | Ya |
| `keterangan` | `text` | Ya |
| `source_file` | `varchar(255)` | Ya |
| `source_sheet` | `varchar(255)` | Ya |
| `source_row` | `int` | Ya |
| `imported_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `sumber_file` | `varchar(255)` | Ya |
| `sheet_name` | `varchar(255)` | Ya |
| `baris_sumber` | `int` | Ya |
| `kode_akun` | `varchar(255)` | Ya |
| `kategori_akun` | `varchar(255)` | Ya |
| `kategori_excel` | `varchar(255)` | Ya |
| `jenis_excel` | `varchar(255)` | Ya |
| `nomor_barang` | `varchar(255)` | Ya |
| `nup` | `varchar(255)` | Ya |
| `nama_barang` | `varchar(255)` | Ya |
| `satuan` | `varchar(255)` | Ya |
| `kuantitas_awal` | `int` | Tidak |
| `nilai_awal` | `bigint` | Tidak |
| `mutasi_tambah_qty` | `int` | Tidak |
| `mutasi_tambah_nilai` | `bigint` | Tidak |
| `mutasi_kurang_qty` | `int` | Tidak |
| `mutasi_kurang_nilai` | `bigint` | Tidak |
| `kuantitas` | `int` | Tidak |
| `nilai_total` | `bigint` | Tidak |
| `kondisi` | `varchar(255)` | Ya |
| `lokasi_barang` | `varchar(255)` | Ya |
| `jadwal_kalibrasi` | `date` | Ya |
| `jadwal_kualifikasi` | `date` | Ya |

### `farhan_new_inventaris_assets`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `source_file` | `varchar(255)` | Ya |
| `source_sheet` | `varchar(255)` | Ya |
| `source_row` | `int` | Ya |
| `imported_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `asset_type` | `varchar(255)` | Ya |
| `asset_id` | `char(26)` | Ya |
| `nama_display` | `varchar(255)` | Ya |
| `nomor_display` | `varchar(255)` | Ya |
| `lokasi_display` | `varchar(255)` | Ya |

### `farhan_new_inventaris_event`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `inventaris_id` | `char(26)` | Ya |
| `inventaris_asset_id` | `char(26)` | Ya |
| `jenis` | `varchar(255)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `judul` | `varchar(255)` | Tidak |
| `tahap` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `prioritas` | `varchar(255)` | Ya |
| `metadata` | `json` | Tidak |
| `tanggal_rencana` | `date` | Tidak |
| `target_selesai` | `date` | Tidak |
| `tanggal_selesai` | `date` | Ya |
| `pic` | `varchar(255)` | Ya |
| `vendor` | `varchar(255)` | Ya |
| `estimasi_biaya` | `bigint` | Ya |
| `realisasi_biaya` | `bigint` | Ya |
| `catatan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_inventaris_event_approvals`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `event_id` | `char(26)` | Tidak |
| `role` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `catatan` | `text` | Ya |
| `reviewed_by` | `bigint` | Ya |
| `reviewed_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_inventaris_event_histories`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `event_id` | `char(26)` | Tidak |
| `action` | `varchar(255)` | Tidak |
| `role` | `varchar(255)` | Ya |
| `status_from` | `varchar(255)` | Ya |
| `status_to` | `varchar(255)` | Ya |
| `field_key` | `varchar(255)` | Ya |
| `old_value` | `json` | Ya |
| `new_value` | `json` | Ya |
| `catatan` | `text` | Ya |
| `actor_id` | `bigint` | Ya |
| `metadata` | `json` | Ya |
| `created_at` | `timestamp` | Ya |

### `farhan_new_inventaris_event_tahap`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `event_id` | `char(26)` | Tidak |
| `tahap` | `varchar(255)` | Tidak |
| `catatan` | `text` | Ya |
| `tanggal_mulai` | `timestamp` | Ya |
| `tanggal_selesai` | `timestamp` | Ya |
| `dokumen_path` | `varchar(255)` | Ya |
| `hasil_evaluasi` | `text` | Ya |
| `parameter_hasil` | `json` | Ya |
| `nomor_sertifikat` | `varchar(255)` | Ya |
| `sertifikat_path` | `varchar(255)` | Ya |
| `dilakukan_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |

### `farhan_new_inventaris_gedung`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `nama_gedung` | `varchar(255)` | Tidak |
| `kode_gedung` | `varchar(255)` | Ya |
| `nomor_inventaris` | `varchar(255)` | Ya |
| `kategori` | `varchar(255)` | Ya |
| `lokasi` | `varchar(255)` | Ya |
| `luas` | `decimal(15` | Ya |
| `tahun_bangun` | `int` | Ya |
| `kondisi` | `varchar(255)` | Ya |
| `nilai_perolehan` | `bigint` | Ya |
| `keterangan` | `text` | Ya |
| `source_file` | `varchar(255)` | Ya |
| `source_sheet` | `varchar(255)` | Ya |
| `source_row` | `int` | Ya |
| `imported_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `sumber_file` | `varchar(255)` | Ya |
| `sheet_name` | `varchar(255)` | Ya |
| `baris_sumber` | `int` | Ya |
| `alamat` | `text` | Ya |
| `tahun_perolehan` | `int` | Ya |
| `njop` | `bigint` | Ya |
| `nup` | `varchar(255)` | Ya |
| `panjang` | `decimal(15` | Ya |
| `lebar` | `decimal(15` | Ya |
| `nilai_saat_ini` | `bigint` | Ya |
| `batas_ambang_pakai_tahun` | `int` | Ya |

### `farhan_new_kalibrasi_details`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `event_id` | `char(26)` | Tidak |
| `jenis_kalibrasi` | `varchar(255)` | Ya |
| `parameter_kalibrasi` | `json` | Ya |
| `rentang_ukur` | `varchar(255)` | Ya |
| `toleransi` | `varchar(255)` | Ya |
| `hasil_evaluasi` | `varchar(255)` | Ya |
| `parameter_hasil` | `json` | Ya |
| `sertifikat_path` | `varchar(255)` | Ya |
| `kondisi_akhir` | `varchar(255)` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_lembaga_kesehatan`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `kategori` | `varchar(255)` | Tidak |
| `matra` | `varchar(255)` | Tidak |
| `tingkat` | `varchar(255)` | Ya |
| `subtipe` | `varchar(255)` | Ya |
| `kotama_wilayah` | `varchar(255)` | Ya |
| `induk_komando` | `varchar(255)` | Ya |
| `nama` | `varchar(255)` | Tidak |
| `alamat` | `text` | Ya |
| `provinsi` | `varchar(255)` | Ya |
| `wilayah` | `varchar(255)` | Ya |
| `latitude` | `decimal(10` | Ya |
| `longitude` | `decimal(10` | Ya |
| `kontak_komandan` | `varchar(255)` | Ya |
| `kontak_telepon` | `varchar(255)` | Ya |
| `kontak_email` | `varchar(255)` | Ya |
| `jumlah_tempat_tidur` | `smallint` | Ya |
| `status_akreditasi` | `varchar(255)` | Ya |
| `kapasitas_kuota_persen` | `tinyint` | Ya |
| `status` | `varchar(255)` | Tidak |
| `meta` | `json` | Ya |
| `dibuat_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_litbang`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `judul` | `varchar(255)` | Tidak |
| `deskripsi` | `text` | Ya |
| `operator_id` | `bigint` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `tahap` | `varchar(255)` | Tidak |
| `tanggal_mulai` | `date` | Ya |
| `tanggal_selesai` | `date` | Ya |
| `status` | `varchar(255)` | Tidak |
| `approved_oleh` | `bigint` | Ya |
| `approved_at` | `timestamp` | Ya |
| `alasan_tolak` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_litbang_material`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `litbang_id` | `char(26)` | Tidak |
| `material_id` | `char(26)` | Tidak |
| `jumlah` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `keterangan` | `varchar(255)` | Ya |

### `farhan_new_material`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `kategori` | `varchar(255)` | Tidak |
| `category_taxonomy_id` | `char(26)` | Ya |
| `jenis_taxonomy_id` | `char(26)` | Ya |
| `golongan` | `varchar(255)` | Ya |
| `bahan_aktif` | `varchar(255)` | Ya |
| `satuan` | `varchar(255)` | Tidak |
| `jenis` | `varchar(255)` | Ya |
| `jumlah` | `decimal(15` | Tidak |
| `harga_satuan` | `bigint` | Tidak |
| `nilai_barang` | `bigint` | Ya |
| `expired_date` | `date` | Ya |
| `keterangan` | `text` | Ya |
| `mutasi` | `varchar(255)` | Ya |
| `satuan_excel_original` | `varchar(255)` | Ya |
| `ukuran` | `varchar(255)` | Ya |
| `perolehan_pabrik` | `varchar(255)` | Ya |
| `source_file` | `varchar(255)` | Ya |
| `source_sheet` | `varchar(255)` | Ya |
| `source_row` | `int` | Ya |
| `imported_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_material_batches`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `material_id` | `varchar(255)` | Tidak |
| `supplier_id` | `varchar(255)` | Ya |
| `manufacturer` | `varchar(255)` | Ya |
| `batch_no` | `varchar(255)` | Tidak |
| `expired_date` | `date` | Ya |
| `qty_awal` | `decimal(15` | Tidak |
| `qty_tersedia` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `harga_satuan` | `bigint` | Tidak |
| `received_at` | `date` | Ya |
| `status` | `varchar(255)` | Tidak |
| `quality_status` | `varchar(255)` | Tidak |
| `released_at` | `date` | Ya |
| `next_retest_due` | `date` | Ya |
| `qc_passed_at` | `date` | Ya |
| `qa_passed_at` | `date` | Ya |
| `last_retested_at` | `date` | Ya |
| `merged_at` | `datetime` | Ya |
| `last_event_id` | `char(26)` | Ya |
| `alasan_tolak` | `text` | Ya |
| `current_workflow_type` | `varchar(30)` | Ya |
| `source_file` | `varchar(255)` | Ya |
| `source_sheet` | `varchar(255)` | Ya |
| `source_row` | `int` | Ya |
| `imported_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_material_qc_qa_event`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `material_batch_id` | `char(26)` | Tidak |
| `produksi_id` | `char(26)` | Ya |
| `kategori` | `varchar(255)` | Tidak |
| `tipe` | `varchar(255)` | Tidak |
| `qty_sample` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `tahap` | `varchar(255)` | Ya |
| `hasil` | `varchar(255)` | Ya |
| `dokumen_path` | `varchar(255)` | Ya |
| `catatan` | `text` | Ya |
| `dilakukan_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Tidak |

### `farhan_new_material_taxonomies`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `parent_id` | `char(26)` | Ya |
| `nama` | `varchar(255)` | Tidak |
| `slug` | `varchar(255)` | Tidak |
| `sort_order` | `int` | Tidak |
| `is_active` | `tinyint(1)` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_material_unit_conversions`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `material_id` | `varchar(255)` | Tidak |
| `from_satuan` | `varchar(255)` | Tidak |
| `to_satuan` | `varchar(255)` | Tidak |
| `multiplier` | `decimal(18` | Tidak |
| `catatan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_obat_jadi_batch`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `produksi_id` | `char(26)` | Tidak |
| `produk_id` | `char(26)` | Tidak |
| `unit_asal` | `varchar(255)` | Tidak |
| `kode_batch` | `varchar(255)` | Tidak |
| `tanggal_release` | `date` | Tidak |
| `expired_date` | `date` | Ya |
| `qty_awal` | `decimal(15` | Tidak |
| `qty_tersedia` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `qty_target` | `decimal(15` | Ya |
| `qty_real` | `decimal(15` | Ya |
| `qty_gap` | `decimal(15` | Ya |
| `status` | `varchar(255)` | Tidak |
| `source_type` | `varchar(255)` | Tidak |
| `supplier_id` | `varchar(255)` | Ya |
| `manufacturer` | `varchar(255)` | Ya |
| `source_file` | `varchar(255)` | Ya |
| `source_sheet` | `varchar(255)` | Ya |
| `source_row` | `int` | Ya |
| `imported_at` | `timestamp` | Ya |
| `released_oleh` | `bigint` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_obat_jadi_ledger`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `obat_jadi_batch_id` | `char(26)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `jenis` | `varchar(255)` | Tidak |
| `sumber` | `varchar(255)` | Tidak |
| `referensi_id` | `varchar(255)` | Ya |
| `qty` | `decimal(15` | Tidak |
| `stok_sebelum` | `decimal(15` | Tidak |
| `stok_sesudah` | `decimal(15` | Tidak |
| `keterangan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Tidak |
| `created_at` | `timestamp` | Tidak |

### `farhan_new_penerimaan_puslola`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `pengiriman_id` | `char(26)` | Tidak |
| `nomor_penerimaan` | `varchar(255)` | Tidak |
| `tanggal_terima` | `date` | Tidak |
| `status_verifikasi` | `varchar(255)` | Tidak |
| `dokumen_penerimaan` | `varchar(255)` | Tidak |
| `catatan` | `text` | Ya |
| `diterima_oleh` | `bigint` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_penerimaan_puslola_item`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `penerimaan_id` | `char(26)` | Tidak |
| `pengiriman_item_id` | `char(26)` | Tidak |
| `qty_dikirim` | `decimal(15` | Tidak |
| `qty_diterima` | `decimal(15` | Tidak |
| `kondisi` | `varchar(255)` | Tidak |
| `catatan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_pengiriman_puslola`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `nomor_pengiriman` | `varchar(255)` | Tidak |
| `unit_asal` | `varchar(255)` | Tidak |
| `tujuan_unit` | `varchar(255)` | Tidak |
| `tanggal_kirim` | `date` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `catatan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_pengiriman_puslola_item`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `pengiriman_id` | `char(26)` | Tidak |
| `obat_jadi_batch_id` | `char(26)` | Tidak |
| `produk_id` | `char(26)` | Tidak |
| `produk_nama` | `varchar(255)` | Tidak |
| `kode_batch` | `varchar(255)` | Tidak |
| `expired_date` | `date` | Ya |
| `qty_kirim` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_perbaikan_details`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `event_id` | `char(26)` | Tidak |
| `source_event_id` | `char(26)` | Ya |
| `jenis_perbaikan` | `varchar(255)` | Ya |
| `tingkat_kerusakan` | `varchar(255)` | Ya |
| `catatan_kerusakan` | `text` | Ya |
| `parameter_kalibrasi` | `json` | Ya |
| `rentang_ukur` | `varchar(255)` | Ya |
| `toleransi` | `varchar(255)` | Ya |
| `kondisi_akhir` | `varchar(255)` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_personel`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `pangkat` | `varchar(100)` | Ya |
| `nip` | `varchar(50)` | Ya |
| `nrp` | `varchar(50)` | Ya |
| `korps` | `varchar(100)` | Ya |
| `kategori_personel` | `varchar(100)` | Ya |
| `status_kepegawaian` | `varchar(50)` | Tidak |
| `kualifikasi` | `varchar(255)` | Tidak |
| `jenjang_pendidikan` | `varchar(50)` | Tidak |
| `jabatan` | `varchar(255)` | Ya |
| `spesialisasi` | `varchar(100)` | Ya |
| `posisi` | `varchar(50)` | Ya |
| `unit_kerja` | `varchar(255)` | Ya |
| `departemen` | `varchar(255)` | Ya |
| `status` | `varchar(50)` | Tidak |
| `sub_status` | `varchar(50)` | Tidak |
| `source_file` | `varchar(255)` | Ya |
| `source_sheet` | `varchar(255)` | Ya |
| `source_row` | `int` | Ya |
| `imported_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_personel_lisensi`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `personel_id` | `char(26)` | Tidak |
| `jenis` | `varchar(255)` | Tidak |
| `nomor` | `varchar(255)` | Ya |
| `penerbit` | `varchar(255)` | Ya |
| `tanggal_terbit` | `date` | Ya |
| `berlaku_sampai` | `date` | Ya |
| `status` | `varchar(50)` | Tidak |
| `file_path` | `varchar(255)` | Ya |
| `catatan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_produk`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `jenis` | `varchar(255)` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_produksi`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode_batch` | `varchar(255)` | Tidak |
| `produk_id` | `char(26)` | Tidak |
| `operator_id` | `bigint` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `jumlah_batch` | `int` | Tidak |
| `jumlah_target` | `int` | Ya |
| `jumlah_real` | `int` | Ya |
| `satuan_real` | `varchar(255)` | Ya |
| `satuan_target` | `varchar(255)` | Ya |
| `bulan_kadaluarsa` | `varchar(7)` | Ya |
| `target_selesai` | `date` | Tidak |
| `tanggal_mulai` | `date` | Ya |
| `tanggal_selesai` | `date` | Ya |
| `tahap` | `varchar(255)` | Tidak |
| `tahap_produksi` | `varchar(255)` | Ya |
| `tahap_qc` | `varchar(255)` | Ya |
| `tahap_qa` | `varchar(255)` | Ya |
| `catatan` | `text` | Ya |
| `surat_qa` | `varchar(255)` | Ya |
| `surat_qc` | `varchar(255)` | Ya |
| `surat_kalafi` | `varchar(255)` | Ya |
| `dokumen_persetujuan` | `json` | Ya |
| `peralatan` | `json` | Ya |
| `spesifikasi` | `text` | Ya |
| `submitted_oleh` | `bigint` | Ya |
| `submitted_at` | `timestamp` | Ya |
| `kalafi_approved_oleh` | `bigint` | Ya |
| `kalafi_approved_at` | `timestamp` | Ya |
| `pusat_approved_oleh` | `bigint` | Ya |
| `pusat_approved_at` | `timestamp` | Ya |
| `approved_oleh` | `bigint` | Ya |
| `approved_at` | `timestamp` | Ya |
| `alasan_tolak` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `workflow_managed` | `tinyint(1)` | Tidak |
| `instruction_id` | `char(26)` | Ya |
| `approval_status` | `varchar(255)` | Ya |
| `current_bundle_id` | `char(26)` | Ya |
| `submitted_by` | `bigint` | Ya |
| `approval_locked_at` | `timestamp` | Ya |
| `planning_version` | `int` | Tidak |

### `farhan_new_produksi_bundle`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `current_version_number` | `int` | Tidak |
| `final_version_number` | `int` | Ya |
| `final_letter_disk` | `varchar(255)` | Ya |
| `final_letter_path` | `varchar(255)` | Ya |
| `final_letter_original_name` | `varchar(255)` | Ya |
| `final_letter_mime` | `varchar(100)` | Ya |
| `final_letter_size` | `bigint` | Ya |
| `final_letter_sha256` | `char(64)` | Ya |
| `final_letter_number` | `varchar(255)` | Ya |
| `final_letter_date` | `date` | Ya |
| `final_letter_signatory` | `varchar(255)` | Ya |
| `final_letter_note` | `text` | Ya |
| `final_letter_uploaded_by` | `bigint` | Ya |
| `final_letter_uploaded_at` | `timestamp` | Ya |
| `finalized_by` | `bigint` | Ya |
| `finalized_at` | `timestamp` | Ya |
| `dibatalkan_oleh` | `bigint` | Ya |
| `dibatalkan_at` | `timestamp` | Ya |
| `alasan_pembatalan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_produksi_bundle_decision`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `bundle_id` | `char(26)` | Tidak |
| `reviewed_version_id` | `char(26)` | Tidak |
| `type` | `varchar(20)` | Tidak |
| `reason` | `text` | Ya |
| `decided_by` | `bigint` | Tidak |
| `decided_at` | `timestamp` | Tidak |
| `idempotency_key` | `varchar(100)` | Tidak |
| `approval_evidence_disk` | `varchar(255)` | Ya |
| `approval_evidence_path` | `varchar(255)` | Ya |
| `approval_evidence_original_name` | `varchar(255)` | Ya |
| `approval_evidence_mime` | `varchar(100)` | Ya |
| `approval_evidence_size` | `bigint` | Ya |
| `approval_evidence_sha256` | `char(64)` | Ya |
| `approval_evidence_uploaded_by` | `bigint` | Ya |
| `approval_evidence_uploaded_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_produksi_bundle_decision_allocation`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `decision_id` | `char(26)` | Tidak |
| `unit` | `varchar(30)` | Tidak |
| `material_id` | `char(26)` | Tidak |
| `live_requirement` | `decimal(18` | Tidak |
| `live_stock` | `decimal(18` | Tidak |
| `live_shortage` | `decimal(18` | Tidak |
| `approved_additional_allocation` | `decimal(18` | Tidak |
| `base_unit` | `varchar(50)` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_produksi_bundle_member`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `bundle_id` | `char(26)` | Tidak |
| `produksi_id` | `char(26)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `position` | `int` | Tidak |
| `added_by` | `bigint` | Tidak |
| `created_at` | `timestamp` | Tidak |

### `farhan_new_produksi_bundle_revision_item`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `revision_round_id` | `char(26)` | Tidak |
| `produksi_id` | `char(26)` | Tidak |
| `required_note` | `text` | Tidak |
| `state` | `varchar(40)` | Tidak |
| `assigned_by` | `bigint` | Ya |
| `assigned_at` | `timestamp` | Ya |
| `completed_by` | `bigint` | Ya |
| `completed_at` | `timestamp` | Ya |
| `verified_by` | `bigint` | Ya |
| `verified_at` | `timestamp` | Ya |
| `completion_source` | `varchar(30)` | Ya |
| `completed_planning_hash` | `char(64)` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_produksi_bundle_revision_round`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `bundle_id` | `char(26)` | Tidak |
| `reviewed_version_id` | `char(26)` | Tidak |
| `round_number` | `int` | Tidak |
| `status` | `varchar(20)` | Tidak |
| `open_slot` | `varchar(20)` | Ya |
| `opened_by` | `bigint` | Tidak |
| `opened_at` | `timestamp` | Tidak |
| `completed_by` | `bigint` | Ya |
| `completed_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_produksi_bundle_version`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `bundle_id` | `char(26)` | Tidak |
| `version_number` | `int` | Tidak |
| `previous_version_id` | `char(26)` | Ya |
| `document_number` | `varchar(255)` | Ya |
| `document_date` | `date` | Ya |
| `snapshot_payload` | `json` | Tidak |
| `snapshot_hash` | `char(64)` | Tidak |
| `proposal_pdf_disk` | `varchar(255)` | Tidak |
| `proposal_pdf_path` | `varchar(255)` | Tidak |
| `proposal_pdf_original_name` | `varchar(255)` | Tidak |
| `proposal_pdf_mime` | `varchar(100)` | Tidak |
| `proposal_pdf_size` | `bigint` | Tidak |
| `proposal_pdf_sha256` | `char(64)` | Tidak |
| `published_by` | `bigint` | Tidak |
| `published_at` | `timestamp` | Tidak |
| `created_at` | `timestamp` | Tidak |

### `farhan_new_produksi_instruction`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `nomor` | `varchar(255)` | Tidak |
| `unit` | `varchar(255)` | Tidak |
| `nama_obat_arahan` | `varchar(255)` | Tidak |
| `jumlah_arahan` | `decimal(15` | Tidak |
| `satuan_arahan` | `varchar(50)` | Tidak |
| `catatan` | `text` | Ya |
| `tanggal_instruksi` | `date` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `dibuat_oleh` | `bigint` | Tidak |
| `dibatalkan_oleh` | `bigint` | Ya |
| `dibatalkan_at` | `timestamp` | Ya |
| `alasan_pembatalan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_produksi_material`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `produksi_id` | `char(26)` | Tidak |
| `material_id` | `char(26)` | Tidak |
| `material_batch_id` | `char(26)` | Ya |
| `jumlah_input` | `decimal(15` | Ya |
| `satuan_input` | `varchar(255)` | Ya |
| `conversion_multiplier` | `decimal(18` | Ya |
| `jumlah_plan` | `decimal(15` | Tidak |
| `jumlah_realisasi` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |

### `farhan_new_produksi_tahap`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `produksi_id` | `char(26)` | Tidak |
| `kategori` | `varchar(255)` | Tidak |
| `tahap` | `varchar(255)` | Tidak |
| `dilakukan_oleh` | `bigint` | Tidak |
| `catatan` | `text` | Ya |
| `created_at` | `timestamp` | Tidak |

### `farhan_new_produksi_workflow_audit`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `subject_type` | `varchar(255)` | Tidak |
| `subject_id` | `char(26)` | Tidak |
| `event` | `varchar(255)` | Tidak |
| `actor_id` | `bigint` | Ya |
| `unit` | `varchar(255)` | Ya |
| `produksi_id` | `char(26)` | Ya |
| `bundle_id` | `char(26)` | Ya |
| `bundle_version_id` | `char(26)` | Ya |
| `payload` | `json` | Ya |
| `occurred_at` | `timestamp` | Tidak |
| `created_at` | `timestamp` | Tidak |

### `farhan_new_puslola_cpob`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `lafi_unit` | `varchar(255)` | Tidak |
| `nomor_sertifikat` | `varchar(255)` | Ya |
| `jenis_pengurusan` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `tahap` | `varchar(255)` | Tidak |
| `tanggal_terbit` | `date` | Ya |
| `berlaku_sampai` | `date` | Ya |
| `renbut_id` | `varchar(255)` | Ya |
| `catatan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_cpob_documents`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `cpob_id` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `file_path` | `varchar(255)` | Ya |
| `catatan_lafi` | `text` | Ya |
| `catatan_puslola` | `text` | Ya |
| `uploaded_by` | `bigint` | Ya |
| `reviewed_by` | `bigint` | Ya |
| `uploaded_at` | `timestamp` | Ya |
| `reviewed_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_distribution_items`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `distribution_id` | `varchar(255)` | Tidak |
| `produk_id` | `varchar(255)` | Ya |
| `produk_nama` | `varchar(255)` | Tidak |
| `kode_batch` | `varchar(255)` | Ya |
| `expired_date` | `date` | Ya |
| `jumlah` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Ya |
| `keterangan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_distributions`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `nomor` | `varchar(255)` | Tidak |
| `jenis` | `varchar(255)` | Tidak |
| `tujuan` | `varchar(255)` | Tidak |
| `tujuan_kategori` | `varchar(255)` | Ya |
| `lembaga_kesehatan_id` | `char(26)` | Ya |
| `mou_id` | `varchar(255)` | Ya |
| `pks_id` | `varchar(255)` | Ya |
| `tanggal` | `date` | Ya |
| `status` | `varchar(255)` | Tidak |
| `catatan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_mou`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `judul` | `varchar(255)` | Tidak |
| `mitra` | `varchar(255)` | Tidak |
| `mitra_lembaga_kesehatan_id` | `char(26)` | Ya |
| `tanggal_mulai` | `date` | Ya |
| `tanggal_selesai` | `date` | Ya |
| `status` | `varchar(255)` | Tidak |
| `file_path` | `varchar(255)` | Ya |
| `catatan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_nie`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `produk_id` | `varchar(255)` | Ya |
| `produk_nama` | `varchar(255)` | Tidak |
| `kegunaan` | `varchar(255)` | Ya |
| `nomor_izin_edar` | `varchar(255)` | Ya |
| `lafi_unit` | `varchar(255)` | Tidak |
| `jenis_pengurusan` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `tahap` | `varchar(255)` | Tidak |
| `tanggal_terbit` | `date` | Ya |
| `berlaku_sampai` | `date` | Ya |
| `harga_tertinggi` | `bigint` | Ya |
| `harga_terendah` | `bigint` | Ya |
| `harga` | `bigint` | Ya |
| `renbut_id` | `varchar(255)` | Ya |
| `catatan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_nie_documents`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `nie_id` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `status` | `varchar(255)` | Tidak |
| `file_path` | `varchar(255)` | Ya |
| `catatan_lafi` | `text` | Ya |
| `catatan_puslola` | `text` | Ya |
| `uploaded_by` | `bigint` | Ya |
| `reviewed_by` | `bigint` | Ya |
| `uploaded_at` | `timestamp` | Ya |
| `reviewed_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_pks`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `mou_id` | `varchar(255)` | Ya |
| `lembaga_kesehatan_id` | `char(26)` | Ya |
| `kode` | `varchar(255)` | Tidak |
| `judul` | `varchar(255)` | Tidak |
| `mitra` | `varchar(255)` | Tidak |
| `tanggal_mulai` | `date` | Ya |
| `tanggal_selesai` | `date` | Ya |
| `status` | `varchar(255)` | Tidak |
| `file_path` | `varchar(255)` | Ya |
| `catatan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_renbut`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `judul` | `varchar(255)` | Tidak |
| `tahun` | `varchar(255)` | Tidak |
| `bidang` | `varchar(255)` | Ya |
| `sumber` | `varchar(255)` | Ya |
| `referensi_id` | `varchar(255)` | Ya |
| `status` | `varchar(255)` | Tidak |
| `catatan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_puslola_renbut_items`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `renbut_id` | `varchar(255)` | Tidak |
| `uraian` | `varchar(255)` | Tidak |
| `jumlah` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Ya |
| `harga_satuan` | `bigint` | Tidak |
| `total` | `bigint` | Tidak |
| `keterangan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `farhan_new_stok_ledger`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `material_id` | `char(26)` | Tidak |
| `material_batch_id` | `char(26)` | Ya |
| `unit` | `varchar(255)` | Tidak |
| `jenis` | `varchar(255)` | Tidak |
| `sumber` | `varchar(255)` | Tidak |
| `referensi_id` | `varchar(255)` | Ya |
| `jumlah` | `decimal(15` | Tidak |
| `stok_sebelum` | `decimal(15` | Tidak |
| `stok_sesudah` | `decimal(15` | Tidak |
| `keterangan` | `text` | Ya |
| `dibuat_oleh` | `bigint` | Tidak |
| `created_at` | `timestamp` | Tidak |
| `bundle_id` | `char(26)` | Ya |
| `bundle_version_id` | `char(26)` | Ya |
| `workflow_identity` | `varchar(255)` | Ya |
| `status` | `varchar(255)` | Ya |
| `satuan` | `varchar(50)` | Ya |

### `farhan_new_suppliers`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `jenis` | `varchar(255)` | Ya |
| `kontak` | `varchar(255)` | Ya |
| `alamat` | `text` | Ya |
| `status` | `varchar(255)` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |