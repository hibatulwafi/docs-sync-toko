# Query untuk Dump Data ke `sync_master`

Berikut adalah serangkaian query untuk mendump data dari berbagai tabel ke dalam tabel `sync_master`. Query-query ini memastikan bahwa data yang belum ada di `sync_master` akan dimasukkan dengan kondisi tertentu, seperti rentang tanggal yang ditentukan.

## 1. Dump Data dari Tabel `osls`

```sql
SELECT "[store_code]" AS store_code,
	o.DocEntry AS key_value,
       "osls" AS table_name,
       "docentry" AS key_name,
       1 AS is_sync,
       CURRENT_TIMESTAMP() AS sync_date
FROM osls o
WHERE NOT EXISTS (
	SELECT 1
	FROM sync_master s
	WHERE s.key_value = o.DocEntry
	AND s.table_name = 'osls'
)
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
ORDER BY key_value ASC;
```

## 2. Dump Data dari Tabel `sls1`

```sql
SELECT "[store_code]" AS store_code,
	o.DocEntry AS key_value,
       "sls1" AS table_name,
       "docentry" AS key_name,
       1 AS is_sync,
       CURRENT_TIMESTAMP() AS sync_date
FROM sls1 o
WHERE NOT EXISTS (
	SELECT 1
	FROM sync_master s
	WHERE s.key_value = o.DocEntry
	AND s.table_name = 'sls1'
)
AND o.DocEntry IN (
	SELECT os.DocEntry
	FROM osls os
	WHERE os.DocDate BETWEEN '[start_date]' AND '[end_date]'
)
GROUP BY o.DocEntry
ORDER BY key_value ASC;
```

## 3. Dump Data dari Tabel `opay`

```sql
SELECT "[store_code]" AS store_code,
	o.DocNum AS key_value,
	"opay" AS table_name,
	"docnum" AS key_name,
	1 AS is_sync,
	CURRENT_TIMESTAMP() AS sync_date
FROM opay o
WHERE NOT EXISTS
(SELECT 1 FROM sync_master s
	WHERE s.key_value = o.DocNum
	AND s.table_name = 'opay')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

## 4. Dump Data dari Tabel `pay1`

```sql
SELECT "[store_code]" AS store_code,
	o.DocNum AS key_value,
       "pay1" AS table_name,
       "docnum" AS key_name,
       1 AS is_sync,
       CURRENT_TIMESTAMP() AS sync_date
FROM pay1 o
WHERE NOT EXISTS (
	SELECT 1 
	FROM sync_master s
	WHERE s.key_value = o.DocNum
	AND s.table_name = 'pay1'
)
AND o.DocNum IN (
	SELECT op.DocNum
	FROM opay op
	WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
)
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

## 5. Dump Data dari Tabel `pay2`

```sql
SELECT "[store_code]" AS store_code,
       o.DocNum AS key_value,
       "pay2" AS table_name,
       "docnum" AS key_name,
       1 AS is_sync,
       CURRENT_TIMESTAMP() AS sync_date
FROM pay2 o
WHERE NOT EXISTS (
    SELECT 1
    FROM sync_master s
    WHERE s.key_value = o.DocNum
      AND s.table_name = 'pay2'
)
AND o.DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
)
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

## 6. Dump Data dari Tabel `pay3`

```sql
SELECT "[store_code]" AS store_code,
       o.DocNum AS key_value,
       "pay3" AS table_name,
       "docnum" AS key_name,
       1 AS is_sync,
       CURRENT_TIMESTAMP() AS sync_date
FROM pay3 o
WHERE NOT EXISTS (
    SELECT 1
    FROM sync_master s
    WHERE s.key_value = o.DocNum
      AND s.table_name = 'pay3'
)
AND o.DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
)
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

## 7. Dump Data dari Tabel `pay4`

```sql
SELECT "[store_code]" AS store_code,
       o.DocNum AS key_value,
       "pay4" AS table_name,
       "docnum" AS key_name,
       1 AS is_sync,
       CURRENT_TIMESTAMP() AS sync_date
FROM pay4 o
WHERE NOT EXISTS (
    SELECT 1
    FROM sync_master s
    WHERE s.key_value = o.DocNum
      AND s.table_name = 'pay4'
)

AND o.DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
)
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

## 8. Dump Data dari Tabel `pay5`

```sql
SELECT "[store_code]" AS store_code,
       o.DocNum AS key_value,
       "pay5" AS table_name,
       "docnum" AS key_name,
       1 AS is_sync,
       CURRENT_TIMESTAMP() AS sync_date
FROM pay5 o
WHERE NOT EXISTS (
    SELECT 1
    FROM sync_master s
    WHERE s.key_value = o.DocNum
      AND s.table_name = 'pay5'
)

AND o.DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
)
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

## 9. Dump Data dari Tabel `opoi`

```sql
SELECT "[store_code]" AS store_code, 
	o.DocEntry AS key_value,
	"opoi" AS table_name,
	"docentry" AS key_name,
	1 AS is_sync,
	CURRENT_TIMESTAMP() AS sync_date
FROM opoi o
WHERE NOT EXISTS
	 (SELECT 1 FROM sync_master s
	 WHERE s.key_value = o.DocEntry
	 AND s.table_name = 'opoi')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
ORDER BY key_value ASC;
```

# Query untuk update data `is_sync_unionweb` `sync_date_unionweb` pada `sync_master`

```sql
UPDATE sync_master
SET is_sync_unionweb = 1,
sync_date_unionweb = CURRENT_TIMESTAMP()
WHERE `table_name` = 'osls';
```
