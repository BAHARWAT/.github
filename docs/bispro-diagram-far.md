# Diagram Proses Bisnis (BISPRO) — Domain FARHAN (FAR)

Dokumen ini mendokumentasikan diagram alur proses bisnis (BISPRO), otorisasi produksi obat militer, dan penjaminan mutu untuk domain **FARHAN MRO (FAR)** berdasarkan skema database terdesentralisasi yang baru.

---

## 1. Alur Rencana Produksi & Bundling (Workbench Puslola)

Proses pengajuan rencana produksi dari Kalafi Unit (LAFI) yang diperiksa dan dibundel oleh Puslola. Proses persetujuan akhir mendukung dua jalur: **Jalur Aplikasi Langsung (Path A)** atau **Jalur Luar Aplikasi (Path B - Event Kabadan)**.

```mermaid
sequenceDiagram
    autonumber
    actor Kalafi as Kalafi Unit (LAFI)
    actor Puslola as Operator Puslola
    actor Kabadan as Kabadan (Menteri/Otoritas)
    participant Sistem as Sistem (FAR Engine)
    participant DB as Database (far_*)

    %% Inisiasi Instruksi
    Puslola->>Sistem: 1. Kirim Instruksi Produksi (far_produksi_instruction status = dikirim)
    activate Sistem
    Sistem-->>Kalafi: Alert instruksi produksi baru
    deactivate Sistem
    
    %% Penyusunan Rencana
    Kalafi->>Sistem: 2. Susun proposal rencana bahan & operator (far_produksi)
    activate Sistem
    Sistem->>DB: Simpan usulan (status = menunggu_pemeriksaan, instruksi = ditindaklanjuti)
    deactivate Sistem

    %% Workbench Puslola
    Puslola->>Sistem: 3. Periksa usulan di Workbench
    activate Sistem
    alt Puslola Minta Koreksi
        Puslola->>Sistem: Input catatan koreksi
        Sistem->>DB: Update usulan (status = perlu_revisi)
        Sistem-->>Kalafi: Kembalikan usulan ke LAFI untuk diperbaiki
    else Puslola Setujui & Masukkan ke Bundel
        Puslola->>Sistem: 4. Buat Bundel Baru (far_produksi_bundle)
        Sistem->>DB: 5. Kunci anggota usulan, hitung snapshot/hash & unggah draf PDF<br/>(far_produksi_bundle status = menunggu_keputusan)
        Sistem-->>Puslola: Bundel terbit (Siap diajukan ke Kabadan)
    end
    deactivate Sistem

    %% Pengambilan Keputusan (Decision Path)
    Note over Puslola, Kabadan: Penentuan Jalur Keputusan Akhir oleh Puslola
    alt PATH A: Persetujuan Langsung dalam Aplikasi (In-App)
        Puslola->>Sistem: 6a. Unggah bukti persetujuan Kabadan langsung
        activate Sistem
        Sistem->>DB: 7a. Jalankan Transaksi Database (Atomic)
        activate DB
        DB->>DB: Buat keputusan (far_produksi_bundle_decision status = setuju)
        DB->>DB: Lakukan alokasi stok bahan baku (far_produksi_bundle_decision_allocation)
        DB->>DB: Inisialisasi CPB Digital (far_cpb_batch status = draft)
        DB->>DB: Update bundel (status = disetujui) & usulan (status = disetujui)
        DB-->>Sistem: Transaksi Sukses
        deactivate DB
        Sistem-->>Puslola: Produksi resmi berjalan (CPB aktif)
    else PATH B: Pencatatan Event Kabadan (Luar Aplikasi)
        Puslola->>Sistem: 6b. Catat keputusan Kabadan di luar sistem
        activate Sistem
        Sistem->>DB: Update bundel (status = menunggu_surat_final)
        deactivate Sistem
        Note over Puslola, Sistem: Setelah Surat Final Fisik diterbitkan
        Puslola->>Sistem: 7b. Validasi nomor & tanggal Surat Final
        activate Sistem
        Sistem->>DB: 8b. Jalankan Transaksi Database (Surat Valid)
        activate DB
        DB->>DB: Lakukan alokasi stok bahan baku (far_produksi_material)
        DB->>DB: Inisialisasi CPB Digital (far_cpb_batch status = draft)
        DB->>DB: Update bundel (status = final) & usulan (status = disetujui)
        DB-->>Sistem: Transaksi Sukses
        deactivate DB
        Sistem-->>Puslola: Surat final terarsip, produksi berjalan
        deactivate Sistem
    end
```

---

## 2. Siklus Hidup CPB (Catatan Pengolahan Batch) Digital

Diagram status (*state machine*) di bawah mendefinisikan transisi pelaksanaan produksi obat secara digital dari inisialisasi awal hingga perilisan produk obat jadi ke pasar.

```mermaid
stateDiagram-v2
    [*] --> Draft : Di-bootstrap dari Template (far_cpb_template) setelah Rencana Disetujui
    
    Draft --> Aktif : Operator Menekan "Mulai Batch" (Trigger: Konsumsi Stok BBO)
    
    state Aktif {
        [*] --> Tahap_Produksi
        Tahap_Produksi --> Isi_Data_Tahap : Isi parameter DDL form (far_cpb_batch_tahap)
        Isi_Data_Tahap --> Tahap_Produksi : Reopen data sebelum batch selesai
        
        [*] --> Langkah_QC
        Langkah_QC --> QC_Testing : Operator/QC mengisi data (far_cpb_batch_qc_step)
        QC_Testing --> Langkah_QC : Reopen QC-step sebelum batch selesai
    }
    
    Aktif --> Completed : Semua production-stage & QC-step berstatus selesai
    
    Completed --> Aktif : Reopen Stage/QC-Step oleh Kalafi Unit (Membatalkan status completed)
    
    Completed --> QA_Verification : Mulai verifikasi akhir oleh QA (Trigger: QA-start & sampling)
    
    state QA_Verification {
        [*] --> Final_Check
        Final_Check --> Input_Signatures : Unggah TTD Digital / Scan Basah (far_cpb_batch_signature)
        Input_Signatures --> Reconciliation : Lakukan Rekonsiliasi Kemasan (far_cpb_batch_kemas_reconciliation)
    }
    
    Reconciliation --> Finalized : Verifikator menekan tombol "Finalize CPB" (Terminal)
    
    Finalized --> QA_Release : Trigger sistem (Execution = selesai, Quality State = QA Released)
    QA_Release --> Released_Product : Obat Jadi masuk Gudang (far_obat_jadi_batch status = released)
    
    Released_Product --> [*]
```

---

## 3. Siklus Mutu Bahan Baku Obat (Quality State BBO)

Bahan baku obat (BBO) yang masuk ke LAFI harus melalui pengawasan mutu yang ketat sebelum diizinkan dikonsumsi dalam proses produksi.

```mermaid
stateDiagram-v2
    [*] --> Diterima : Bahan Baku masuk Gudang (Quality = diterima)
    
    Diterima --> QC_Proses : Memulai pengujian laboratorium (Trigger: Mulai QC)
    
    QC_Proses --> QA_Proses : Lulus pengujian QC (Trigger: Lulus QC)
    QC_Proses --> Ditolak : Gagal pengujian QC (Trigger: Tolak)
    
    QA_Proses --> Released : Lulus verifikasi akhir dokumen QA (Quality = released)
    QA_Proses --> Ditolak : Gagal verifikasi QA (Trigger: Tolak)
    
    Released --> Perlu_Retest : Masa berlaku mendekati jadwal uji ulang (Next Retest Due)
    Perlu_Retest --> QC_Proses : Uji ulang laboratorium
    
    Released --> Expired : Melewati masa kadaluarsa (Expired Date)
    Perlu_Retest --> Expired : Melewati masa kadaluarsa
    
    Ditolak --> [*] : Bahan baku dimusnahkan / dikembalikan
    Expired --> [*] : Bahan baku dibuang secara aman
```
