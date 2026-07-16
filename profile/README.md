# BAHARWAT — Badan Pemeliharaan dan Perawatan

BAHARWAT (Baharwathan) adalah komando inspektorat dan otoritas logistik pertahanan yang bertugas mengoordinasikan pemeliharaan, perawatan, perbaikan, dan kelaikan alat peralatan pertahanan dan keamanan.

Organisasi ini mengelola ekosistem **Sysinfo Harwat**, sebuah platform manajemen MRO (*Maintenance, Repair, and Overhaul*) terintegrasi yang terbagi dalam arsitektur terdekopel (decoupled).

---

## Struktur Repositori

Ekosistem Sysinfo Harwat dibagi menjadi empat repositori utama untuk mendukung skalabilitas, kemudahan pemeliharaan, dan performa tinggi skala nasional:

1. **[.github](https://github.com/BAHARWAT/.github)** (Repositori Ini)
   * Berisi profil organisasi dan template standar.
2. **[backend-app](https://github.com/BAHARWAT/backend-app)**
   * REST API Backend stateless yang dibangun menggunakan **NestJS** dengan **Fastify adapter** untuk konkurensi tinggi, optimasi performa, serta manajemen data modular.
3. **[frontend-app](https://github.com/BAHARWAT/frontend-app)**
   * Web client interaktif yang dibangun menggunakan **Next.js** (App Router & TypeScript) untuk menyajikan dashboard pemantauan, form wizard, dan analitik MRO.
4. **[baharwat-workspace](https://github.com/BAHARWAT/baharwat-workspace)**
   * Konfigurasi workspace yang mengintegrasikan semua repositori utama dalam satu session pengembangan.

---

## BAHARWAT Workspace

**baharwat-workspace** adalah konfigurasi workspace yang mengintegrasikan semua repositori utama ekosistem Sysinfo Harwat dalam satu session pengembangan. Kompatibel dengan berbagai IDE modern.

### Menjalankan Workspace

Untuk membuka workspace di IDE pilihan Anda, gunakan salah satu cara berikut:

#### Cara 1: Double-Click File (Rekomendasi)
Cukup double-click file `sysinfo-harwat.code-workspace` di file explorer. IDE Anda akan secara otomatis mengenali dan membuka workspace dengan semua folder dan konfigurasi yang telah ditetapkan.

**IDE Kompatibel:**
- **VS Code** (.code-workspace)
- **Antigravity IDE** (.code-workspace)
- **JetBrains IDEs** (IntelliJ IDEA, WebStorm, dll)
- **Sublime Text**

#### Cara 2: Command Line
Jalankan perintah berikut di terminal sesuai IDE yang Anda gunakan:

**VS Code:**
```bash
code sysinfo-harwat.code-workspace
```

**Antigravity IDE:**
```bash
antigravity-ide sysinfo-harwat.code-workspace
```

**JetBrains IDEs (IntelliJ IDEA):**
```bash
idea sysinfo-harwat.code-workspace
```

**Catatan**: Pastikan IDE sudah ditambahkan ke PATH sistem atau command sudah tersedia di terminal Anda.

---

File workspace ini telah dikonfigurasi dengan:
- **Folder Structure**: Mengintegrasikan `backend-app` dan `frontend-app`
- **Extensions/Plugins**: Rekomendasi untuk development yang optimal
- **Settings**: Konfigurasi standar untuk konsistensi tim
