# Diagram Proses Bisnis (BISPRO) — Domain ALPALHAN (ALP)

Dokumen ini mendokumentasikan diagram alur proses bisnis (BISPRO), transisi status alur kerja (*state machine*), dan interaksi aktor untuk domain **ALPALHAN MRO (ALP)** berdasarkan skema database terdesentralisasi yang baru.

---

## 1. Alur Pengajuan MMS & Pemisahan Jalur (SPK vs Renbut)

Diagram urutan (*sequence diagram*) di bawah ini menggambarkan alur dari inisiasi pengajuan perawatan (MMS) oleh Satker, klasifikasi jalur otomatis oleh Sistem (berdasarkan ketersediaan suku cadang di gudang), proses persetujuan Mabes UO, hingga pemisahan ke Jalur SPK (Pembuatan WO) atau Jalur Renbut (Pengumpulan Anggaran di Baharwathan).

```mermaid
sequenceDiagram
    autonumber
    actor Satker
    actor Mabes as Mabes UO (Strategis)
    actor Inspektorat as Baharwathan (Inspektorat)
    participant Sistem as Sistem (ALP Engine)
    participant DB as Database (alp_* / shared_*)

    %% Inisiasi Pengajuan
    Satker->>Sistem: 1. Isi Form Pengajuan MMS & Suku Cadang (alp_pengajuan_parts)
    activate Sistem
    Sistem->>DB: 2. Periksa ketersediaan stok (alp_supply_items.stok)
    DB-->>Sistem: Kembalikan status stok (TERSEDIA / KURANG / KOSONG)
    Sistem->>Sistem: 3. Tentukan estimasi biaya & Klasifikasi Jalur otomatis<br/>(Semua parts TERSEDIA ? Jalur = SPK : Jalur = Renbut)
    Sistem->>DB: 4. Simpan status pengajuan (status = DRAFT / SUBMITTED)
    deactivate Sistem
    
    %% Proses Review Mabes UO
    Note over Mabes, Sistem: Hanya pengajuan berstatus SUBMITTED sematra yang ditinjau oleh Mabes UO
    Mabes->>Sistem: 5. Review Pengajuan MMS
    activate Sistem
    alt Pengajuan Ditolak
        Sistem->>DB: Update status = REJECTED, simpan alasan_reject
        Sistem-->>Satker: Notifikasi penolakan (Pengajuan Selesai - Gagal)
    else Pengajuan Disetujui (Jalur == SPK)
        Sistem->>DB: 6. Jalankan Transaksi DB (Atomic Lock)
        activate DB
        DB->>DB: 6.1. Kurangi stok suku cadang (alp_supply_items.stok)
        DB->>DB: 6.2. Buat surat perintah baru (alp_work_orders status = PLANNED)
        DB->>DB: 6.3. Catat status pengajuan = SPK_CREATED
        DB-->>Sistem: Transaksi Sukses
        deactivate DB
        Sistem-->>Mabes: Berhasil menerbitkan SPK/WO
        Sistem-->>Satker: Notifikasi WO diterbitkan (Bengkel siap bekerja)
    else Pengajuan Disetujui (Jalur == Renbut)
        Sistem->>DB: 7. Ubah status = SUBMITTED_BAHARWATHAN
        Sistem-->>Mabes: Berhasil diteruskan ke Baharwathan
        Sistem-->>Inspektorat: Alert antrean usulan anggaran Renbut
    end
    deactivate Sistem

    %% Proses Review Baharwathan (Khusus Jalur Renbut)
    activate Inspektorat
    Inspektorat->>Sistem: 8. Tinjau usulan anggaran (SUBMITTED_BAHARWATHAN)
    activate Sistem
    alt Anggaran Ditolak/Revisi
        Sistem->>DB: Update status = REJECTED / REVISION, catat alasan
        Sistem-->>Satker: Notifikasi revisi/penolakan
    else Anggaran Disetujui
        Sistem->>DB: 9. Cari/Buat Bundel Renbut aktif (alp_renbut)
        DB->>DB: 9.1. Asosiasikan pengajuan ke alp_renbut.id
        DB->>DB: 9.2. Update status pengajuan = RENBUT_FINAL
        DB->>DB: 9.3. Hitung ulang total anggaran (alp_renbut.total_nilai)
        Sistem-->>Inspektorat: Anggaran disetujui & dibundel ke Renbut
        Sistem-->>Satker: Notifikasi alokasi anggaran disetujui (Menunggu pencairan)
    end
    deactivate Sistem
    deactivate Inspektorat
```

---

## 2. Siklus Hidup Transisi Status Work Order (WO)

Diagram status (*state machine*) berikut mendefinisikan transisi status Work Order di bengkel pemeliharaan secara ketat di backend. Perubahan status dari `BACKLOG` hingga `DONE` wajib divalidasi kebenarannya.

```mermaid
stateDiagram-v2
    [*] --> BACKLOG : WO Dibuat dari Pengajuan SPK
    [*] --> PLANNED : WO Dibuat dari Preventive Maintenance
    
    BACKLOG --> PLANNED : Persiapan Suku Cadang & Kebutuhan (alp_work_order_requirements)
    
    state PLANNED {
        [*] --> Material_Check
        Material_Check --> Ready : Semua requirement status = ready
        Material_Check --> Pending : Ada requirement status = pending/not_available
    }
    
    PLANNED --> IN_PROGRESS : Operator Memulai Pekerjaan (Trigger: Start Task)
    
    state IN_PROGRESS {
        [*] --> Checklist_Steps
        Checklist_Steps --> Step_Execution : Update status step per baris<br/>(pending -> in_progress -> done)
        Step_Execution --> Checklist_Steps : Hitung akumulasi progres (%)
    }
    
    IN_PROGRESS --> QC : Progres Checklist = 100% (Semua step = done / skipped)
    
    state QC {
        [*] --> Quality_Control
        Quality_Control --> Approve_Inspection : Inspektur menyetujui kelaikan
        Quality_Control --> Reject_Inspection : Inspektur menolak kelaikan (Ditemukan defect/degradasi baru)
    }
    
    Reject_Inspection --> IN_PROGRESS : WO Dikembalikan ke Teknisi untuk Perbaikan (Status Step reset ke pending)
    Approve_Inspection --> DONE : Pekerjaan Selesai sepenuhnya
    
    DONE --> [*]
```

---

## 3. Siklus Pemeliharaan Preventif (Preventive Maintenance)

Diagram alur di bawah menggambarkan bagaimana sistem menjadwalkan pekerjaan pemeliharaan preventif secara otomatis berdasarkan template manual teknis (kalender, jam putar, atau kejadian eksternal).

```mermaid
flowchart TD
    Start([1. Sistem Memindai Aset]) --> ScanTemplates[2. Ambil Template Tugas Pemeliharaan Aktif<br/>alp_task_templates]
    ScanTemplates --> FetchAssetData[3. Ambil data Running Hours & Terakhir Kalibrasi<br/>alp_fleets]
    FetchAssetData --> CheckTrigger{4. Periksa Kriteria Trigger?}
    
    CheckTrigger -- Kalender / Interval Waktu --> TriggerCal[Jadwal Berikutnya <= Tanggal Hari Ini]
    CheckTrigger -- Jam Operasional / Running Hours --> TriggerHours[Remaining Hours <= 0]
    CheckTrigger -- Event Spesifik --> TriggerEvent[Kejadian Khusus Terlapor]
    
    TriggerCal --> CreateWO[5. Buat Work Order Baru]
    TriggerHours --> CreateWO
    TriggerEvent --> CreateWO
    
    CreateWO --> SetupWO[6. Inisialisasi Data WO]
    SetupWO --> GenerateReqs[6.1. Salin Kebutuhan Suku Cadang ke<br/>alp_work_order_requirements]
    SetupWO --> GenerateSteps[6.2. Salin Langkah Teknis Prosedur ke<br/>alp_work_order_steps]
    GenerateSteps --> Complete[7. WO Terbit status PLANNED]
```
