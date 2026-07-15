# Token Komponen UI

Dokumen ini mendefinisikan sistem token desain dasar yang digunakan untuk menyusun komponen UI pada aplikasi frontend Next.js agar memiliki estetika premium dan konsisten.

---

## 1. Skema Warna (Color Palette)

* **Primary (Militer/Otoritas):**
  * `Deep Olive`: HSL (120, 25%, 15%) — Warna dasar untuk identitas instansi.
  * `Muted Bronze`: HSL (40, 30%, 45%) — Warna aksen untuk status/navigasi sekunder.
* **Secondary (Aksen & Status):**
  * `Success Green`: HSL (145, 60%, 35%) — Untuk status FMC atau WO Selesai.
  * `Warning Amber`: HSL (35, 80%, 45%) — Untuk status PMC atau revisi.
  * `Danger Red`: HSL (0, 75%, 45%) — Untuk status NMC atau pengajuan ditolak.
* **Backgrounds (Sleek Dark Mode):**
  * `Surface Dark`: HSL (220, 15%, 8%) — Latar belakang aplikasi utama.
  * `Card Dark`: HSL (220, 15%, 12%) — Latar belakang kartu, form, dan panel.

---

## 2. Tipografi (Typography)

* **Font Utama:** Google Fonts `Outfit` atau `Inter` untuk kejelasan teks.
* **Font Monospace:** `JetBrains Mono` atau `Fira Code` khusus untuk data numerik, ID (ULID), dan kode status.

---

## 3. Efek & Mikro-Animasi (Aesthetics)

* **Glassmorphism:**
  * Gunakan properti CSS `backdrop-filter: blur(8px)` dengan border semi-transparan `rgba(255,255,255,0.05)` untuk menghasilkan efek kaca premium pada panel dashboard.
* **Hover State:**
  * Transisi warna dan elevasi yang halus menggunakan `transition: all 0.2s ease-in-out` pada tombol, baris tabel interaktif, dan kartu status.
