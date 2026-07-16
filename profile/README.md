# BAHARWAT — Badan Pemeliharaan dan Perawatan

BAHARWAT (Baharwathan) adalah komando inspektorat dan otoritas logistik pertahanan yang bertugas mengoordinasikan pemeliharaan, perawatan, perbaikan, dan kelaikan alat peralatan pertahanan dan keamanan.

Organisasi ini mengelola ekosistem **Sysinfo Harwat**, sebuah platform manajemen MRO (*Maintenance, Repair, and Overhaul*) terintegrasi yang terbagi dalam arsitektur terdekopel (decoupled).

---

## Struktur Repositori

Ekosistem Sysinfo Harwat dibagi menjadi tiga repositori utama untuk mendukung skalabilitas, kemudahan pemeliharaan, dan performa tinggi skala nasional:

1. **[.github](https://github.com/BAHARWAT/.github)** (Repositori Ini)
   * Berisi profil organisasi dan template standar.
2. **[backend-app](https://github.com/BAHARWAT/backend-app)**
   * REST API Backend stateless yang dibangun menggunakan **NestJS** dengan **Fastify adapter** untuk konkurensi tinggi, optimasi performa, serta manajemen data modular.
3. **[frontend-app](https://github.com/BAHARWAT/frontend-app)**
   * Web client interaktif yang dibangun menggunakan **Next.js** (App Router & TypeScript) untuk menyajikan dashboard pemantauan, form wizard, dan analitik MRO.
4. **[baharwat-workspace](https://github.com/BAHARWAT/baharwat-workspace)**
   * Konfigurasi VS Code workspace yang mengintegrasikan semua repositori utama dalam satu session pengembangan.

---

## BAHARWAT Workspace

**baharwat-workspace** adalah konfigurasi VS Code workspace yang mengintegrasikan semua repositori utama ekosistem Sysinfo Harwat dalam satu session pengembangan.

### Menjalankan Workspace

Untuk membuka workspace di VS Code, gunakan file konfigurasi `sysinfo-harwat.code-workspace`:

```bash
code sysinfo-harwat.code-workspace
```

File workspace ini telah dikonfigurasi dengan:
- **Folder Structure**: Mengintegrasikan `backend-app`, dan `frontend-app`
- **Extensions**: Rekomendasi extension untuk development yang optimal
- **Settings**: Konfigurasi standar untuk konsistensi tim
