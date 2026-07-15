# Kontrak API — Pengaturan Tema (Themes)

Dokumen ini mendefinisikan rute dan format payload untuk menyimpan preferensi tema pengguna (misal: mode gelap/sleek dark mode) di database shared.

---

## 1. Perbarui Preferensi Tema

Mengubah pengaturan visual tema bagi pengguna terautentikasi.

* **Metode:** `PUT`
* **Endpoint:** `/api/v1/auth/themes/preference`
* **Headers:**
  * `Authorization: Bearer <access_token>`
  * `Content-Type: application/json`

### Payload Permintaan (Request Body):
```json
{
  "theme": "sleek-dark",
  "density": "comfortable"
}
```

### Respons Sukses (HTTP 200 OK):
```json
{
  "success": true,
  "data": {
    "user_id": 1,
    "theme": "sleek-dark",
    "density": "comfortable",
    "updated_at": "2026-07-16T00:00:00Z"
  },
  "meta": {
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```
