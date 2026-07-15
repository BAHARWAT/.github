# Standarisasi API REST

Dokumen ini mendefinisikan standar respons dan format error untuk REST API Sysinfo Harwat. Semua endpoint backend NestJS wajib mematuhi panduan ini.

---

## 1. Format Respons Sukses (Standard Success Response)

Respons sukses wajib mengembalikan status HTTP 2xx dengan struktur JSON sebagai berikut:

### Respons Objek Tunggal (Single Object Response)
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

### Respons Daftar/Array (List/Array Response dengan Paginasi)
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

## 2. Format Respons Error (Standard Error Response)

Respons error mengembalikan status HTTP 4xx/5xx dengan struktur seragam:

### Respons Error Umum (HTTP 400, 401, 403, 404, 500)
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE_STRING",
    "message": "Deskripsi pesan kesalahan dalam Bahasa Indonesia.",
    "details": null
  },
  "meta": {
    "request_id": "req-9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```

### Respons Error Validasi Input (HTTP 422 Unprocessable Entity)
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Input data tidak valid.",
    "details": [
      {
        "field": "email",
        "rule": "isEmail",
        "message": "Format alamat email tidak valid."
      },
      {
        "field": "password",
        "rule": "minLength",
        "message": "Password minimal harus memiliki 8 karakter."
      }
    ]
  },
  "meta": {
    "request_id": "req-9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```

---

## 3. Daftar Kode Error Standar
* `UNAUTHORIZED`: Kredensial tidak valid atau token JWT kadaluarsa (HTTP 401).
* `FORBIDDEN`: Aktor tidak memiliki hak akses untuk tindakan terkait (HTTP 403).
* `NOT_FOUND`: Resource tidak ditemukan (HTTP 404).
* `VALIDATION_ERROR`: Payload input tidak lolos validasi (HTTP 422).
* `INTERNAL_SERVER_ERROR`: Kesalahan internal sistem backend atau database (HTTP 500).
