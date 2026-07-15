# Kontrak API — Autentikasi (Auth)

Dokumen ini mendefinisikan rute, payload permintaan, dan format respons untuk modul Autentikasi.

---

## 1. Login Pengguna

Menyediakan akses autentikasi ke dalam sistem menggunakan kredensial email dan password.

* **Metode:** `POST`
* **Endpoint:** `/api/v1/auth/login`
* **Headers:**
  * `Content-Type: application/json`

### Payload Permintaan (Request Body):
```json
{
  "email": "user@baharwat.go.id",
  "password": "securepassword123"
}
```

### Respons Sukses (HTTP 200 OK):
```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "def456...",
    "expires_in": 3600,
    "user": {
      "id": 1,
      "name": "Operator Alpalhan",
      "email": "user@baharwat.go.id",
      "badan": "Alpalhankam",
      "role": "satker"
    }
  },
  "meta": {
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```
