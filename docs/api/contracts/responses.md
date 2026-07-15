# Kontrak API — Standar Respons (Responses)

Dokumen ini mendefinisikan struktur amplop JSON (*JSON envelope*) standar untuk setiap respons REST API di seluruh endpoint platform Sysinfo Harwat.

---

## 1. Respons Berhasil (HTTP 2xx)

Semua respons berhasil harus dibungkus dalam properti `success: true` dengan data diletakkan di dalam properti `data`, serta menyertakan `request_id` untuk kebutuhan pelacakan log produksi.

### Skema Respons Objek Tunggal:
```json
{
  "success": true,
  "data": {},
  "meta": {
    "request_id": "req-9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```

### Skema Respons Daftar/Array (Dengan Paginasi):
```json
{
  "success": true,
  "data": [],
  "meta": {
    "pagination": {
      "page": 1,
      "limit": 10,
      "total_items": 100,
      "total_pages": 10
    },
    "request_id": "req-9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```

---

## 2. Respons Gagal (HTTP 4xx/5xx)

Semua respons gagal harus mengembalikan `success: false` dan menyertakan objek `error` yang berisi kode error dan deskripsi pesan kesalahan.

### Skema Respons Error Umum (HTTP 400, 401, 403, 404, 500):
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Deskripsi kesalahan teknis dalam bahasa Indonesia.",
    "details": null
  },
  "meta": {
    "request_id": "req-9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```

### Skema Respons Error Validasi Input (HTTP 422 Unprocessable Entity):
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Input data tidak valid.",
    "details": [
      {
        "field": "nama_kolom",
        "rule": "nama_aturan",
        "message": "Pesan kegagalan kolom dalam bahasa Indonesia."
      }
    ]
  },
  "meta": {
    "request_id": "req-9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```
