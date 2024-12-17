# Dump Data dari Tabel `sync_master` untuk UNIONWEB

Query ini digunakan untuk mengekspor data dari tabel `sync_master` database toko ke tabel `sync_master` pada sistem UNIONWEB.

## Cara Penggunaan

1. **Ganti Placeholder `[store_code]`**  
   Gantilah `[store_code]` dengan kode toko yang ingin Anda ambil datanya. Misalnya, jika kode toko adalah `0001270`, maka query Anda akan menjadi:
   ```sql
   SELECT
       '[store_code]' AS store_code,
       key_value,
       table_name,
       key_name,
       null AS key_name_union,
       null AS key_value_union,
       '1' AS is_sync,
       sync_date,
       '1' AS is_sync_union,
       CURRENT_TIMESTAMP() AS sync_date_union,
       CASE
           WHEN table_name IN ('owtr', 'wtr1', 'ocrd') THEN '2'
           ELSE '1'
       END AS is_client_to_server,
       '0' AS created_by,
       '0' AS updated_by,
       CURRENT_TIMESTAMP() AS created_at,
       CURRENT_TIMESTAMP() AS updated_at
   FROM sync_master;
   ```
2. **Run query di database toko**
3. **Pada Heidi, `Copy as > SQL Inserts`**
4. **Paste query, dan run pada database `unionweb`**
