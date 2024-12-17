# Query untuk Dump Data ke `sync_master`

Berikut adalah serangkaian query untuk mendump data untuk di input ke cloud table masing-masing (database toko)
`database_toko.osls` => `unionweb.osls`
...
`database_toko.opoi` => `unionweb.opoi`

### Note : Jalankan query pada database toko
## **1. Tabel OSLS**

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
LEFT JOIN ocrd c ON o.CardCode = c.OutletCardCode
WHERE o.DocDate BETWEEN '[start_date]' AND '[end_date]';
```

## **2. Tabel SLS1**

```sql
SELECT "[store_code]" as StoreCode, DocEntry,
LineId, ItemCode, SerialNum, BatchNo,
ExpDate, Price, Quantity, DiscPrcnt,
AddDisc1, AddDisc2, VatSum, SalesSum,
LineTotal, PromoCode, PromoNo, CashBackAmount,
CashBackCode, PendingStock,
"Y" as Sync2SAP, VAT, "" as U_StaCon, FindMode
FROM sls1
WHERE sls1.DocEntry IN (
    SELECT os.DocEntry
    FROM osls os
    WHERE os.DocDate BETWEEN '[start_date]' AND '[end_date]'
);
```

## **3. Tabel OPAY**

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
FROM opay
WHERE opay.DocDate BETWEEN '[start_date]' AND '[end_date]';
```

## **4. Tabel PAY1**

```sql
SELECT "[store_code]" AS StoreCode,
       p.DocNum,
       p.LineId,
       p.CashSum,
       p.CashChange,
       p.CashTotal,
       "Y" AS Sync2SAP
FROM pay1 p
WHERE DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
);
```

## **5. Tabel PAY2**

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
FROM pay2
WHERE DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
);
```

## **6. Tabel PAY3**

```sql
SELECT
"[store_code]" as StoreCode,
DocNum,
LineId,
VoucherId,
VoucherSum
FROM pay3
WHERE DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
);
```

## **7. Tabel PAY4**

```sql
SELECT
"[store_code]" as StoreCode,
DocNum,
LineId,
BalType,
BalanceSum
FROM pay4
WHERE DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
);
```

## **8. Tabel PAY5**

```sql
SELECT
"[store_code]" as StoreCode,
DocNum,
LineId,
CreditCard,
BankNo,
TrfNo,
TrfSum
FROM pay5
WHERE DocNum IN (
    SELECT op.DocNum
    FROM opay op
    WHERE op.DocDate BETWEEN '[start_date]' AND '[end_date]'
);
```

## **9. Tabel OPOI**

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
FROM opoi
WHERE opoi.DocDate  BETWEEN '[start_date]' AND '[end_date]';
```
