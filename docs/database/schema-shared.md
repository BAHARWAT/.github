# Schema Existing — Shared Tables

Source: `harwatdb` MySQL 8.4.7. Snapshot 15 Juli 2026.

Total tabel: 10

### `cache`

| Kolom | Tipe | Nullable |
|---|---|---|
| `key` | `varchar(255)` | Tidak |
| `value` | `mediumtext` | Tidak |
| `expiration` | `int` | Tidak |

### `cache_locks`

| Kolom | Tipe | Nullable |
|---|---|---|
| `key` | `varchar(255)` | Tidak |
| `owner` | `varchar(255)` | Tidak |
| `expiration` | `int` | Tidak |

### `failed_jobs`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `bigint` | Tidak |
| `uuid` | `varchar(255)` | Tidak |
| `connection` | `text` | Tidak |
| `queue` | `text` | Tidak |
| `payload` | `longtext` | Tidak |
| `exception` | `longtext` | Tidak |
| `failed_at` | `timestamp` | Tidak |

### `job_batches`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `varchar(255)` | Tidak |
| `name` | `varchar(255)` | Tidak |
| `total_jobs` | `int` | Tidak |
| `pending_jobs` | `int` | Tidak |
| `failed_jobs` | `int` | Tidak |
| `failed_job_ids` | `longtext` | Tidak |
| `options` | `mediumtext` | Ya |
| `cancelled_at` | `int` | Ya |
| `created_at` | `int` | Tidak |
| `finished_at` | `int` | Ya |

### `jobs`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `bigint` | Tidak |
| `queue` | `varchar(255)` | Tidak |
| `payload` | `longtext` | Tidak |
| `attempts` | `tinyint` | Tidak |
| `reserved_at` | `int` | Ya |
| `available_at` | `int` | Tidak |
| `created_at` | `int` | Tidak |

### `migrations`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `int` | Tidak |
| `migration` | `varchar(255)` | Tidak |
| `batch` | `int` | Tidak |

### `notifications`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `char(36)` | Tidak |
| `type` | `varchar(255)` | Tidak |
| `notifiable_type` | `varchar(255)` | Tidak |
| `notifiable_id` | `bigint` | Tidak |
| `data` | `text` | Tidak |
| `read_at` | `timestamp` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |

### `password_reset_tokens`

| Kolom | Tipe | Nullable |
|---|---|---|
| `email` | `varchar(255)` | Tidak |
| `token` | `varchar(255)` | Tidak |
| `created_at` | `timestamp` | Ya |

### `sessions`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `varchar(255)` | Tidak |
| `user_id` | `bigint` | Ya |
| `ip_address` | `varchar(45)` | Ya |
| `user_agent` | `text` | Ya |
| `payload` | `longtext` | Tidak |
| `last_activity` | `int` | Tidak |

### `users`

| Kolom | Tipe | Nullable |
|---|---|---|
| `id` | `bigint` | Tidak |
| `name` | `varchar(255)` | Tidak |
| `email` | `varchar(255)` | Tidak |
| `badan` | `enum('SuperAdmin','Alpalhankam','Farhan','Sarhan')` | Tidak |
| `role` | `enum('operator','mabes','kotama','pusat')` | Ya |
| `sarhan_org_unit_id` | `bigint` | Ya |
| `alpalhan_role` | `varchar(255)` | Ya |
| `alpalhan_unit` | `varchar(255)` | Ya |
| `matra` | `enum('AD','AU','AL')` | Ya |
| `email_verified_at` | `timestamp` | Ya |
| `password` | `varchar(255)` | Tidak |
| `remember_token` | `varchar(100)` | Ya |
| `created_at` | `timestamp` | Ya |
| `updated_at` | `timestamp` | Ya |
| `farhan_role` | `varchar(255)` | Ya |
| `farhan_unit` | `varchar(255)` | Ya |