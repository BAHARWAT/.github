# Kontrak API — Transaksi (Transactions)

Dokumen ini mendefinisikan rute dan format respons untuk pencatatan riwayat transaksi stok dan mutasi material di domain bisnis MRO.

---

## 1. Ambil Riwayat Mutasi Material

Mengambil mutasi masuk dan keluar dari ledger material (misal: BBO di Farhan, supply items di Alpalhan).

* **Metode:** `GET`
* **Endpoint:** `/api/v1/transactions/ledger`
* **Query Parameters:**
  * `domain`: `alp` | `far` (wajib)
  * `material_id`: string (opsional)
  * `page`: number (default: 1)
  * `limit`: number (default: 10)
* **Headers:**
  * `Authorization: Bearer <access_token>`

### Respons Sukses (HTTP 200 OK):
```json
{
  "success": true,
  "data": [
    {
      "id": "01J2PAW0M1S9...",
      "domain": "alp",
      "material_id": "01J2PAW0M1A1...",
      "type": "issue",
      "quantity": 15,
      "reference_type": "work_order",
      "reference_id": "01J2PAW0M1WO...",
      "user_id": 2,
      "created_at": "2026-07-16T01:00:00Z"
    }
  ],
  "meta": {
    "pagination": {
      "total": 120,
      "count": 1,
      "per_page": 10,
      "current_page": 1,
      "total_pages": 12
    },
    "timestamp": "2026-07-16T01:05:00Z"
  }
}
```
