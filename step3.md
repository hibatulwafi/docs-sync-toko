# Query untuk Dump Data ke `sync_master`

Berikut adalah serangkaian query untuk mendump data untuk di input ke cloud table masing-masing (database toko)
`database_toko.osls` => `unionweb.osls`
...
`database_toko.opoi` => `unionweb.opoi`

### Note : Jalankan query pada database toko
## **1. Tabel OSLS**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocEntry,'", ',
'key_name_union ="baseentry",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="osls";') as query_update
FROM osls o
WHERE o.StoreCode = '0001270'
ORDER BY o.DocEntry ASC;
```
