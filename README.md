# Dokumentasi Proses Dump Data

## Parameter

-   **store_code**: eg. `[store_code]`
-   **start_date**: eg. `2024-01-01`
-   **end_date**: eg. `2024-11-30`
-   **list table**:
    -   `osls`, `sls1`, `opay`, `pay1`, `pay2`, `pay3`, `pay4`, `pay5`,
    -   `opoi`, `otrw`, `trw1`, `oign`, `ign1`

---

## Proses Dump Data

1. **Create Table & Dump Data dari Table `sync_master` (toko)**

    - Ambil **Primary Key** dari tabel awal (masing-masing tabel master seperti `osls`, `sls1`, `pay1`, dll.) untuk dimasukkan ke dalam tabel `sync_master` sebagai penanda (mark) di database toko.

2. **Dump Data dari `sync_master` (web)**

    - Ekspor data dari tabel `sync_master` pada database web untuk kebutuhan sinkronisasi.

3. **Dump Data dari Masing-masing Table (web)**

    - Ambil data asli dari setiap tabel di database web untuk diinput ke cloud table masing-masing di database toko.

4. **Update `sync_master` dengan Primary Key dari `unionretaildb`**
    - Ambil **primary_key_value_union** dari database `unionretaildb` untuk diperbarui di tabel `sync_master`.

---

## Catatan

-   Pastikan parameter `store_code`, `start_date`, dan `end_date` sudah benar sebelum menjalankan proses dump data.
-   Proses ini memerlukan hak akses ke masing-masing database (toko, web, dan cloud).
-   Gunakan prosedur ini dengan hati-hati untuk menjaga integritas data.

---

## Step 1 (database toko): Dump Data dari Masing-masing Table Master

### **1. Tabel OSLS**

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

### **2. Tabel SLS1**

```sql
SELECT "[store_code]" AS store_code,
o.DocEntry AS key_value,
"sls1" AS table_name,
"docentry" AS key_name,
1 AS is_sync,
CURRENT_TIMESTAMP() AS sync_date
FROM sls1 o
WHERE NOT EXISTS
(SELECT 1 FROM sync_master s
WHERE s.key_value = o.DocEntry
AND s.table_name = 'sls1')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
GROUP BY o.DocEntry
ORDER BY key_value ASC;
```

### **3. Tabel OPAY**

```sql
SELECT "[store_code]" AS store_code,
o.DocEntry AS key_value,
"opay" AS table_name,
"docentry" AS key_name,
1 AS is_sync,
CURRENT_TIMESTAMP() AS sync_date
FROM opay o
WHERE NOT EXISTS
(SELECT 1 FROM sync_master s
WHERE s.key_value = o.DocEntry
AND s.table_name = 'opay')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
GROUP BY o.DocEntry
ORDER BY key_value ASC;
```

### **4. Tabel PAY1**

```sql
SELECT "[store_code]" AS store_code,
o.DocNum AS key_value,
"pay1" AS table_name,
"docnum" AS key_name,
1 AS is_sync,
CURRENT_TIMESTAMP() AS sync_date
FROM pay1 o
WHERE NOT EXISTS
(SELECT 1 FROM sync_master s
WHERE s.key_value = o.DocNum
AND s.table_name = 'pay1')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

### **5. Tabel PAY2**

```sql
SELECT "[store_code]" AS store_code,
o.DocNum AS key_value,
"pay2" AS table_name,
"docnum" AS key_name,
1 AS is_sync,
CURRENT_TIMESTAMP() AS sync_date
FROM pay2 o
WHERE NOT EXISTS
(SELECT 1 FROM sync_master s
WHERE s.key_value = o.DocNum
AND s.table_name = 'pay2')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

### **6. Tabel PAY3**

```sql
SELECT "[store_code]" AS store_code,
o.DocNum AS key_value,
"pay3" AS table_name,
"docnum" AS key_name,
1 AS is_sync,
CURRENT_TIMESTAMP() AS sync_date
FROM pay3 o
WHERE NOT EXISTS
(SELECT 1 FROM sync_master s
WHERE s.key_value = o.DocNum
AND s.table_name = 'pay3')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

### **7. Tabel PAY4**

```sql
SELECT "[store_code]" AS store_code,
o.DocNum AS key_value,
"pay4" AS table_name,
"docnum" AS key_name,
1 AS is_sync,
CURRENT_TIMESTAMP() AS sync_date
FROM pay4 o
WHERE NOT EXISTS
(SELECT 1 FROM sync_master s
WHERE s.key_value = o.DocNum
AND s.table_name = 'pay4')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

### **8. Tabel PAY5**

```sql
SELECT "[store_code]" AS store_code,
o.DocNum AS key_value,
"pay5" AS table_name,
"docnum" AS key_name,
1 AS is_sync,
CURRENT_TIMESTAMP() AS sync_date
FROM pay5 o
WHERE NOT EXISTS
(SELECT 1 FROM sync_master s
WHERE s.key_value = o.DocNum
AND s.table_name = 'pay5')
AND o.DocDate BETWEEN '[start_date]' AND '[end_date]'
GROUP BY o.DocNum
ORDER BY key_value ASC;
```

### **9. Tabel OPOI**

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


## Step 2 (database toko): Dump Data dari Masing-masing Table Master

### **1. Tabel OSLS**

```sql
SELECT DocEntry,
"[store_code]" as StoreCode,
DocNum,
DocDate,
c.CardCode as CardCode,
BoxCode, MbrCode,
CashSum, CreditSum, VatSum, SalesSum, StsPay,
Void,
VoidEffectBalRetur,
VoidReason,
WaiterCode,
UserCode,
Remark1, Remark2, Remark3, Remark4,
DocStatus,
VoucherCode, DiscSum
FROM osls o
LEFT JOIN ocrd c ON o.CardCode = c.OutletCardCode;
```

### **2. Tabel SLS1**

```sql
SELECT "[store_code]" as StoreCode, DocEntry,
LineId, ItemCode, SerialNum, BatchNo,
ExpDate, Price, Quantity, DiscPrcnt,
AddDisc1, AddDisc2, VatSum, SalesSum,
LineTotal, PromoCode, PromoNo, CashBackAmount,
CashBackCode, PendingStock,
"Y" as Sync2SAP, VAT, "" as U_StaCon, FindMode
FROM sls1;
```

### **3. Tabel OPAY**

```sql
SELECT "[store_code]" as StoreCode,
DocEntry,
DocNum,
DocDate,
CashSum,
CreditSum,
VoucherSum,
BalanceSum,
TransferSum,
Rounding,
"Y" as Sync2SAP
FROM opay;
```

### **4. Tabel PAY1**

```sql
SELECT "[store_code]" as StoreCode,
DocNum,
LineId,
CashSum,
CashChange,
CashTotal,
"Y" as Sync2SAP
FROM pay1;
```

### **5. Tabel PAY2**

```sql
SELECT "[store_code]" as StoreCode,
DocNum,
LineId,
CreditNo,
CreditAcct,
CreditSum,
BankCharges,
TotalPaid,
EDC,
MID,
Tenor,
PaymentType,
CashBackCode,
CashBackNo,
CashBackAmount,
ContractNo,
CCPromo,
DCPromo
FROM pay2;
```

### **6. Tabel PAY3**

```sql
SELECT
"[store_code]" as StoreCode,
DocNum,
LineId,
VoucherId,
VoucherSum
FROM pay3;
```

### **7. Tabel PAY4**

```sql
SELECT
"[store_code]" as StoreCode,
DocNum,
LineId,
BalType,
BalanceSum
FROM pay5;
```

### **8. Tabel PAY5**

```sql
SELECT
"[store_code]" as StoreCode,
DocNum,
LineId,
CreditCard,
BankNo,
TrfNo,
TrfSum
FROM pay5;
```

### **9. Tabel OPOI**

```sql
SELECT "[store_code]" AS StoreCode,
DocEntry,
DocDate,
BaseEntry,
BaseType,
DocEntry as StoreEntry,
CardCode,
TotalSales,
PoinValue,
PoinIn,
PoinOut,
PoinBefSales,
PoinAftSales,
"N" as Sync2CRD,
U_CardName,
U_FactorPoin,
U_CardNumber
FROM opoi;
```


## Step 3 (database toko): Dump Data dari Masing-masing Table Master
``` TBD ```
