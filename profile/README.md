# BAHARWAT — Badan Pemeliharaan dan Perawatan

BAHARWAT (Baharwathan) adalah komando inspektorat dan otoritas logistik pertahanan yang bertugas mengoordinasikan pemeliharaan, perawatan, perbaikan, dan kelaikan alat peralatan pertahanan dan keamanan serta sarana-prasarana militer lintas matra.

Organisasi ini mengelola ekosistem **Sysinfo Harwat**, sebuah platform manajemen MRO (*Maintenance, Repair, and Overhaul*) terintegrasi yang terbagi dalam arsitektur terdekopel (decoupled).

---

## Portofolio Repositori

Ekosistem Sysinfo Harwat dibagi menjadi tiga repositori utama untuk mendukung skalabilitas, kemudahan pemeliharaan, dan performa tinggi skala nasional:

1. **[.github](https://github.com/BAHARWAT/.github)** (Repositori Ini)
   * Berisi profil organisasi dan template standar.
2. **[backend-app](https://github.com/BAHARWAT/backend-app)**
   * REST API Backend stateless yang dibangun menggunakan **NestJS** dengan **Fastify adapter** untuk konkurensi tinggi, optimasi performa, serta manajemen data modular.
3. **[frontend-app](https://github.com/BAHARWAT/frontend-app)**
   * Web client interaktif yang dibangun menggunakan **Next.js** (App Router & TypeScript) untuk menyajikan dashboard pemantauan, form wizard, dan analitik MRO.
