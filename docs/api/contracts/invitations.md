# Kontrak API — Undangan Pengguna (Invitations)

Dokumen ini mendefinisikan rute dan format payload untuk mengundang pengguna baru ke dalam platform oleh administrator Pusat.

---

## 1. Kirim Undangan Pengguna Baru

Mengirim email undangan pendaftaran bagi pengguna baru dengan peran dan instansi tertentu.

* **Metode:** `POST`
* **Endpoint:** `/api/v1/auth/invitations`
* **Headers:**
  * `Authorization: Bearer <access_token>`
  * `Content-Type: application/json`

### Payload Permintaan (Request Body):
```json
{
  "email": "petugas.baru@baharwat.go.id",
  "name": "Petugas Baru",
  "badan": "Sarhan",
  "role": "operator",
  "sarhan_org_unit_id": 10
}
```

### Respons Sukses (HTTP 201 Created):
```json
{
  "success": true,
  "data": {
    "invitation_id": "01J2PAW0M1S9...",
    "email": "petugas.baru@baharwat.go.id",
    "status": "pending",
    "sent_at": "2026-07-16T00:00:00Z"
  },
  "meta": {
    "timestamp": "2026-07-16T00:00:00Z"
  }
}
```
