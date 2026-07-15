# Schema Existing — Domain SH (Sarhan MRO)

Source: `harwatdb` MySQL 8.4.7. Snapshot 15 Juli 2026.

Total tabel: 12

### `sarhan_new_aset`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `nup` | `varchar(255)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `kategori_satker` | `varchar(255)` | Tidak |
| `sarhan_org_unit_id` | `bigint` | Ya |
| `sarhan_parent_kotama_id` | `bigint` | Ya |
| `gps` | `varchar(255)` | Ya |
| `status` | `varchar(255)` | Ya |
| `matra` | `enum('KEMHAN','MABES','AD','AL','AU')` | Ya |
| `jenis_bangunan` | `varchar(255)` | Ya |
| `life_cycle` | `int` | Tidak |
| `kondisi_persen` | `int` | Tidak |
| `kondisi_desc` | `text` | Ya |
| `tahun_perolehan` | `int` | Ya |
| `nilai_perolehan` | `bigint` | Tidak |
| `data_kib` | `json` | Ya |
| `data_teknis` | `json` | Ya |
| `lokasi` | `json` | Ya |
| `pengadaan` | `json` | Ya |
| `administrasi` | `json` | Ya |
| `galeri` | `json` | Ya |
| `denah_koordinat` | `json` | Ya |
| `skor_dampak_operasional` | `tinyint` | Ya |
| `skor_dampak_keselamatan` | `tinyint` | Ya |
| `skor_dampak_keuangan` | `tinyint` | Ya |
| `skor_dampak_final` | `tinyint` | Ya |
| `skor_kemungkinan` | `tinyint` | Ya |
| `nilai_risiko` | `tinyint` | Ya |
| `tingkat_prioritas` | `enum('P1','P2','P3')` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_aset_penilaian`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `bigint` | Tidak |
| `aset_id` | `char(26)` | Tidak |
| `satker_user_id` | `bigint` | Ya |
| `satker_org_unit_id` | `bigint` | Ya |
| `kotama_user_id` | `bigint` | Ya |
| `kotama_org_unit_id` | `bigint` | Ya |
| `status` | `enum('draft','menunggu_penilaian_kotama','revisi_satker','disetujui_kotama','ditolak_kotama')` | Tidak |
| `skor_kemungkinan_awal` | `tinyint` | Ya |
| `skor_kemungkinan_final` | `tinyint` | Ya |
| `skor_dampak_operasional` | `tinyint` | Ya |
| `skor_dampak_keselamatan` | `tinyint` | Ya |
| `skor_dampak_keuangan` | `tinyint` | Ya |
| `skor_dampak_final` | `tinyint` | Ya |
| `nilai_risiko` | `tinyint` | Ya |
| `tingkat_prioritas` | `enum('P1','P2','P3')` | Ya |
| `catatan_operator` | `text` | Ya |
| `catatan_kotama` | `text` | Ya |
| `submitted_at` | `timestamp` | Ya |
| `reviewed_at` | `timestamp` | Ya |
| `approved_at` | `timestamp` | Ya |
| `rejected_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_histori`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `pengajuan_id` | `char(26)` | Tidak |
| `user_id` | `bigint` | Ya |
| `aksi` | `varchar(255)` | Tidak |
| `detail` | `text` | Tidak |
| `data_snapshot` | `json` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_lampiran`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `pengajuan_id` | `char(26)` | Tidak |
| `nama` | `varchar(255)` | Tidak |
| `type` | `enum('pdf','image')` | Tidak |
| `path` | `varchar(255)` | Tidak |
| `uploaded_by` | `bigint` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_org_units`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `bigint` | Tidak |
| `code` | `varchar(255)` | Tidak |
| `name` | `varchar(255)` | Tidak |
| `type` | `varchar(255)` | Tidak |
| `parent_id` | `bigint` | Ya |
| `matra` | `varchar(255)` | Ya |
| `kota` | `varchar(255)` | Ya |
| `provinsi` | `varchar(255)` | Ya |
| `meta` | `json` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_pemeliharaan`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `aset_id` | `char(26)` | Tidak |
| `nama_kegiatan` | `varchar(255)` | Tidak |
| `interval_tahun` | `int` | Tidak |
| `terakhir_dilakukan` | `date` | Ya |
| `jadwal_berikutnya` | `date` | Ya |
| `estimasi_biaya` | `bigint` | Tidak |
| `catatan` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_pengajuan`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `renbut_id` | `char(26)` | Ya |
| `id_usulan` | `varchar(255)` | Tidak |
| `user_id` | `bigint` | Ya |
| `nama` | `varchar(255)` | Tidak |
| `kategori` | `varchar(255)` | Tidak |
| `satker` | `varchar(255)` | Tidak |
| `deskripsi_teknis` | `text` | Ya |
| `metode_pelaksanaan` | `varchar(255)` | Ya |
| `tahun_anggaran` | `int` | Ya |
| `estimasi_biaya` | `bigint` | Tidak |
| `status` | `enum('masuk','revisi','diverifikasi_kotama','divalidasi','disetujui','ditolak')` | Tidak |
| `progres_verifikasi` | `int` | Tidak |
| `skor_otomatis` | `decimal(5` | Ya |
| `dampak_kerusakan` | `int` | Ya |
| `urgensi_operasional` | `int` | Ya |
| `skor_akhir` | `decimal(5` | Ya |
| `tingkat_prioritas` | `enum('P1','P2','P3')` | Ya |
| `catatan` | `text` | Ya |
| `validasi_oleh` | `bigint` | Ya |
| `validasi_at` | `timestamp` | Ya |
| `approval_oleh` | `bigint` | Ya |
| `approval_at` | `timestamp` | Ya |
| `source_type` | `varchar(50)` | Ya |
| `source_reference` | `varchar(255)` | Ya |
| `source_unit_subtotal` | `bigint` | Ya |
| `source_payload` | `json` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_pengajuan_aset`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `pengajuan_id` | `char(26)` | Tidak |
| `aset_id` | `char(26)` | Tidak |
| `prioritas` | `varchar(255)` | Tidak |

### `sarhan_new_rab_item`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `rab_revisi_id` | `char(26)` | Tidak |
| `uraian` | `varchar(255)` | Tidak |
| `volume` | `decimal(15` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `harga_satuan` | `bigint` | Tidak |
| `total` | `bigint` | Tidak |
| `urutan` | `int` | Tidak |

### `sarhan_new_rab_revisi`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `pengajuan_id` | `char(26)` | Tidak |
| `versi` | `int` | Tidak |
| `label` | `varchar(255)` | Tidak |
| `total` | `bigint` | Tidak |
| `dibuat_oleh` | `bigint` | Ya |
| `catatan_revisi` | `text` | Ya |
| `is_active` | `tinyint(1)` | Tidak |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_renbut`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `kode` | `varchar(255)` | Tidak |
| `judul` | `varchar(255)` | Tidak |
| `periode` | `varchar(255)` | Tidak |
| `matra` | `enum('KEMHAN','MABES','AD','AL','AU')` | Tidak |
| `user_id` | `bigint` | Ya |
| `status` | `enum('DRAFT','SUBMITTED','REVIEW','REVISION','APPROVED','REJECTED')` | Tidak |
| `total_nilai` | `bigint` | Tidak |
| `catatan_mabes` | `text` | Ya |
| `alasan_revisi` | `text` | Ya |
| `ttd_nama` | `varchar(255)` | Ya |
| `ttd_jabatan` | `varchar(255)` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `sarhan_new_renbut_items`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(26)` | Tidak |
| `renbut_id` | `char(26)` | Tidak |
| `kelompok` | `varchar(255)` | Ya |
| `unit` | `varchar(255)` | Ya |
| `kode` | `varchar(255)` | Ya |
| `uraian` | `text` | Tidak |
| `volume` | `decimal(12` | Tidak |
| `satuan` | `varchar(255)` | Tidak |
| `harga_satuan` | `bigint` | Tidak |
| `total` | `bigint` | Tidak |
| `unit_subtotal` | `bigint` | Ya |
| `keterangan` | `varchar(255)` | Ya |
| `klasifikasi` | `varchar(20)` | Ya |
| `status_kondisi` | `varchar(10)` | Ya |
| `skor_dampak` | `tinyint` | Ya |
| `skor_kemungkinan` | `tinyint` | Ya |
| `nilai_risiko` | `tinyint` | Ya |
| `tingkat_prioritas` | `enum('P1','P2','P3')` | Ya |
| `scoring_source` | `varchar(30)` | Ya |
| `scoring_reason` | `json` | Ya |
| `scoring_tier` | `varchar(30)` | Ya |
| `source_page` | `int` | Ya |
| `raw_text` | `text` | Ya |
| `import_status` | `varchar(255)` | Tidak |
| `validation_notes` | `text` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |