# Query untuk Dump Data ke `sync_master`

Berikut adalah serangkaian query untuk mendump data untuk di input ke cloud table masing-masing (database toko)
`database_toko.osls` => `unionweb.osls`
*note : sesuaikan dulu StoreCode nya.

### Note : Jalankan query pada database unionretaildb
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
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```

## **2. Tabel SLS1**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocEntry,'", ',
'key_name_union ="docentry",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="sls1";') as query_update
FROM osls o
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```

## **3. Tabel OPAY**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocNum,'", ',
'key_name_union ="docnum",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="opay";') as query_update
FROM osls o
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```

## **4. Tabel PAY1**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocNum,'", ',
'key_name_union ="docnum",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="pay1";') as query_update
FROM osls o
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```

## **5. Tabel PAY2**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocNum,'", ',
'key_name_union ="docnum",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="pay2";') as query_update
FROM osls o
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```

## **6. Tabel PAY3**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocNum,'", ',
'key_name_union ="docnum",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="pay3";') as query_update
FROM osls o
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```

## **7. Tabel PAY4**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocNum,'", ',
'key_name_union ="docnum",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="pay4";') as query_update
FROM osls o
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```

## **8. Tabel PAY5**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocNum,'", ',
'key_name_union ="docnum",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="pay5";') as query_update
FROM osls o
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```

## **9. Tabel OPOI**
```sql
SELECT CONCAT('UPDATE sync_master SET is_sync_union = 1, ',
'key_value_union ="',o.DocNum,'", ',
'key_name_union ="docnum",  ',
'sync_date_union = CURRENT_TIMESTAMP() ',
'WHERE key_value ="',o.BaseEntry,'"',
'AND store_code ="',o.StoreCode,'"',
'AND table_name ="opay";') as query_update
FROM osls o
WHERE o.StoreCode = '[store_code]'
ORDER BY o.DocEntry ASC;
```
