# Peta Manajemen State Frontend

Dokumen ini mendefinisikan strategi manajemen state untuk aplikasi Next.js pada frontend-app.

---

## 1. Pembagian Scope State

Untuk menjamin performa dan pemisahan modularitas yang bersih antara domain bisnis, state dibagi menjadi tiga tingkat:

### A. State Global (Shared)
* **Penggunaan:** Autentikasi pengguna (`User Context`), preferensi tema, status koneksi API, dan notifikasi global.
* **Teknologi:** React Context API atau Zustand.

### B. State Domain Bisnis (Feature-scoped)
* **Penggunaan:** Cache data list/detail per domain bisnis (ALP, SAR, FAR) untuk menghindari pemuatan ulang (*refetching*) data yang tidak perlu saat navigasi antar-halaman dalam satu domain.
* **Teknologi:** React Query (TanStack Query) untuk sinkronisasi server state dengan auto-caching.

### C. State Lokal (Component-scoped)
* **Penggunaan:** Status UI komponen individual seperti status modal terbuka/tutup, filter pencarian tabel aktif, atau input form wizard yang belum disubmit.
* **Teknologi:** React hook standar (`useState`, `useReducer`).
