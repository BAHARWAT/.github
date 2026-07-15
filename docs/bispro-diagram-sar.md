# Diagram Proses Bisnis (BISPRO) — Domain SARHAN (SAR)

Dokumen ini mendokumentasikan diagram alur proses bisnis (BISPRO), verifikasi berjenjang, dan logika penilaian risiko untuk domain **SARHAN MRO (SAR)** berdasarkan skema database terdesentralisasi yang baru.

---

## 1. Alur Penilaian Aset oleh Kotama

Proses penilaian kondisi fisik aset oleh Kotama bertujuan untuk menentukan tingkat risiko dan prioritas aset (P1/P2/P3) secara berjenjang dari bawah ke atas.

```mermaid
sequenceDiagram
    autonumber
    actor Satker as Operator Satker
    actor Kotama as Operator Kotama
    participant Sistem as Sistem (SAR Engine)
    participant DB as Database (sar_*)

    %% Registrasi Aset
    Satker->>Sistem: 1. Registrasi Aset & input Kondisi Fisik<br/>(LF/RR/RS/RB/TED)
    activate Sistem
    Sistem->>Sistem: 2. Konversi Kondisi Fisik menjadi<br/>Skor Kemungkinan Awal (1 - 5)
    Sistem->>DB: 3. Simpan Aset (sar_aset) & buat penilaian<br/>(status = menunggu_penilaian_kotama)
    Sistem-->>Kotama: Alert antrean penilaian aset baru
    deactivate Sistem

    %% Penilaian Kotama
    Note over Kotama, Sistem: Kotama meninjau antrean aset di bawah naungan teritorinya
    Kotama->>Sistem: 4. Lakukan penilaian risiko aset
    activate Sistem
    alt Kotama setujui penilaian
        Kotama->>Sistem: Input 4 skor dampak (1-5) & skor kemungkinan final
        Sistem->>Sistem: 5. Hitung Nilai Risiko (Dampak Final x Kemungkinan Final)<br/>dan tentukan prioritas (P1/P2/P3)
        Sistem->>DB: 6. Simpan Penilaian (status = disetujui_kotama) & update sar_aset
        Sistem-->>Kotama: Penilaian berhasil disimpan
    else Kotama minta revisi
        Kotama->>Sistem: Input catatan revisi
        Sistem->>DB: 7. Update Penilaian (status = revisi_satker)
        Sistem-->>Satker: Alert revisi penilaian aset
        Satker->>Sistem: 8. Operator Satker perbaiki data & kirim kembali
        Sistem->>DB: 9. Reset status = menunggu_penilaian_kotama
    else Kotama menolak penilaian
        Kotama->>Sistem: Input catatan penolakan
        Sistem->>DB: 10. Update Penilaian (status = ditolak_kotama)
    end
    deactivate Sistem
```

---

## 2. Alur Pengajuan Pemeliharaan & Verifikasi RAB Berjenjang

Proses pengajuan pemeliharaan sarana prasarana menggunakan alur persetujuan sangat ketat dari Operator Satker -> Kotama -> Mabes UO -> Pusat (Baharwathan), termasuk pemutakhiran versi RAB di setiap jenjang.

```mermaid
sequenceDiagram
    autonumber
    actor Operator as Operator Satker
    actor Kotama as Verifikator Kotama
    actor Mabes as Validator Mabes UO
    actor Pusat as Approval Pusat (Baharwathan)
    participant Sistem as Sistem (SAR Engine)
    participant DB as Database (sar_*)

    %% Pengajuan Awal
    Operator->>Sistem: 1. Buat Pengajuan, lampiran, & RAB v1 (sar_rab_revisi)
    activate Sistem
    Sistem->>DB: 2. Simpan Pengajuan (status = masuk)
    deactivate Sistem

    %% Verifikasi Kotama
    Kotama->>Sistem: 3. Tinjau Pengajuan status "masuk"
    activate Sistem
    alt Kotama minta revisi
        Kotama->>Sistem: Input catatan revisi & kembalikan
        Sistem->>DB: Update status = revisi
        Sistem-->>Operator: Notifikasi perbaikan RAB
        Operator->>Sistem: Edit RAB & kirim ulang (Submit revisi)
        Sistem->>DB: Reset status = masuk
    else Kotama Tolak
        Sistem->>DB: Update status = ditolak (Terminal)
    else Kotama Setuju
        Kotama->>Sistem: (Opsional) Sesuaikan nilai RAB item
        Sistem->>DB: Update status = diverifikasi_kotama, simpan revisi RAB baru
        Sistem-->>Mabes: Diteruskan ke Mabes UO
    end
    deactivate Sistem

    %% Validasi Mabes UO
    Mabes->>Sistem: 4. Tinjau Pengajuan "diverifikasi_kotama"
    activate Sistem
    alt Mabes minta revisi ke Satker
        Sistem->>DB: Update status = revisi
        Sistem-->>Operator: Notifikasi perbaikan
    else Mabes Tolak
        Sistem->>DB: Update status = ditolak (Terminal)
    else Mabes Validasi
        Mabes->>Sistem: (Opsional) Sesuaikan nilai RAB item
        Sistem->>DB: Update status = divalidasi, simpan revisi RAB baru
        Sistem-->>Pusat: Diteruskan ke Pusat
    end
    deactivate Sistem

    %% Approval Pusat
    Pusat->>Sistem: 5. Tinjau Pengajuan "divalidasi"
    activate Sistem
    alt Pusat Tolak
        Sistem->>DB: Update status = ditolak (Terminal)
    else Pusat Approve (Disetujui)
        Sistem->>DB: 6. Jalankan Transaksi Materialisasi Renbut
        activate DB
        DB->>DB: 6.1. Cari/buat sar_renbut untuk matra & periode terkait
        DB->>DB: 6.2. Salin item RAB versi aktif ke sar_renbut_items
        DB->>DB: 6.3. Hitung ulang total anggaran sar_renbut.total_nilai
        DB->>DB: 6.4. Update status pengajuan = disetujui
        DB-->>Sistem: Transaksi Sukses
        deactivate DB
        Sistem-->>Pusat: Anggaran disetujui & masuk ke Renbut
        Sistem-->>Operator: Notifikasi dana pemeliharaan disetujui
    end
    deactivate Sistem
```

---

## 3. Alur Impor Excel TNI AU & Auto-Scoring dengan Impact Cap

Proses impor data Renbut massal milik TNI AU (AU) menggunakan logika khusus (*Impact Cap*) untuk membatasi tingkat prioritas berdasarkan tingkat dampak operasional.

```mermaid
flowchart TD
    Start([1. Pusat Mengunggah Excel AU]) --> ValidateFile[2. Validasi Struktur & Baris Excel]
    ValidateFile --> WipedExisting[3. Hapus seluruh data sar_renbut_items AU untuk periode terkait]
    WipedExisting --> LoopRows[4. Iterasi tiap baris Excel]
    
    LoopRows --> ExtractMetrics[5. Ekstrak Nilai Dampak & Kemungkinan Risiko]
    ExtractMetrics --> ApplyImpactCap{6. Terapkan Aturan Impact Cap}
    
    ApplyImpactCap -- Skor Dampak >= 4 --> NoCap[Prioritas ditentukan murni dari skor risiko final]
    ApplyImpactCap -- Skor Dampak == 3 --> CapP2[Prioritas dibatasi Maksimal P2]
    ApplyImpactCap -- Skor Dampak <= 2 --> CapP3[Prioritas dibatasi Maksimal P3]
    
    NoCap --> SaveDB[7. Simpan Aset, Penilaian, Pengajuan, & Item Renbut ke DB]
    CapP2 --> SaveDB
    CapP3 --> SaveDB
    
    SaveDB --> CheckNext{8. Ada baris berikutnya?}
    CheckNext -- Ya --> LoopRows
    CheckNext -- Tidak --> RecalculateTotal[9. Hitung ulang total nilai sar_renbut.total_nilai]
    RecalculateTotal --> End([10. Impor Selesai])
```
