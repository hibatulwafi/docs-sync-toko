# Query Create Table `sync_master`

Dokumen ini berisi query SQL untuk membuat tabel `sync_master` pada database `toko`
---

## Catatan

- Run pada database toko

---

## Query SQL untuk Membuat Tabel `sync_master`

```sql
CREATE TABLE IF NOT EXISTS `sync_master` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `store_code` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `key_value` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `table_name` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `key_name` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `is_sync` int NOT NULL DEFAULT '0' COMMENT '0',
  `sync_date` datetime DEFAULT NULL,
  `is_sync_unionweb` int NOT NULL DEFAULT '0',
  `sync_date_unionweb` datetime DEFAULT NULL,
  `created_by` bigint unsigned NOT NULL DEFAULT '0',
  `updated_by` bigint unsigned DEFAULT '0',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci ROW_FORMAT=DYNAMIC;
