# Registri Layanan Bersama (Shared Services)

Dokumen ini mencatat layanan-layanan bersama (*shared services*) yang digunakan lintas modul pada backend NestJS.

---

## 1. Layanan Autentikasi (`AuthService`)
* **Deskripsi:** Layanan untuk memverifikasi kredensial pengguna, menerbitkan token JWT (*Access Token* & *Refresh Token*), dan melakukan otorisasi berbasis peran/matra/badan.
* **Modul:** `SharedModule`

---

## 2. Publisher Notifikasi (`NotificationPublisher`)
* **Deskripsi:** Layanan terpusat untuk mempublikasikan notifikasi sistem ke database (`shared_notifications`) setelah transaksi database berhasil berkomitmen (`afterCommit`).
* **Modul:** `SharedModule`

---

## 3. Logger Audit Log (`AuditLogger`)
* **Deskripsi:** Layanan untuk mencatat log audit transaksi penting secara asinkron ke database guna memenuhi standar keamanan pelacakan aktivitas pengguna.
* **Modul:** `SharedModule`
