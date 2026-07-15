# Pemetaan Batas Modul (Module Boundary Map)

Dokumen ini mendefinisikan aturan pemisahan modul (*decoupling rules*) antara frontend, backend, dan antar-domain bisnis.

---

## 1. Batas Frontend dan Backend

* **Stateless API:** Komunikasi sepenuhnya dilakukan melalui HTTP REST JSON secara asinkron. Frontend dilarang bergantung pada state atau session server backend.
* **Format Data Kontrak:** Payload request dan response wajib divalidasi menggunakan skema JSON tetap yang didefinisikan dalam API Contract. Perubahan skema wajib didiskusikan terlebih dahulu sebelum diimplementasikan.

---

## 2. Batas Antar-Domain Bisnis (ALP, SAR, FAR)

* **Isolasi Database:** Domain ALP, SAR, dan FAR tidak boleh melakukan query join lintas prefix database. Jika diperlukan data dari domain lain, akses harus melalui pemanggilan API internal atau modul pembagi di core/shared.
* **Isolasi Codebase:**
  * Di NestJS: Kode modul `AlpModule`, `SarModule`, dan `FarModule` harus bersifat independen secara struktural. Modul-modul ini hanya diizinkan mengimpor modul dari `SharedModule` (seperti koneksi database, layanan autentikasi) dan dilarang saling mengimpor satu sama lain.
  * Di Next.js: Halaman dan fitur di dalam `features/alp/` dilarang mengimpor komponen atau logika khusus domain dari `features/sar/` atau `features/far/`. Komponen yang digunakan bersama wajib dipindahkan ke direktori global `components/` atau `core/`.
