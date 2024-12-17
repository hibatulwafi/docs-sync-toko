# Query Pengecekan Data

## 1. Pengecekan Data synchronize

```sql
SELECT 
    -- Group: Doc Entries
    o.DocEntry AS doc_entry_osls,
    oi.DocEntry AS doc_entry_opoi,
    oi.BaseEntry AS base_entry_opoi,
    oi.StoreEntry AS store_entry_opoi,
    -- Group: Document Numbers
    o.DocNum AS doc_num_osls,
    op.DocNum AS doc_num_opay,
    p1.DocNum AS doc_num_pay1,
    p2.DocNum AS doc_num_pay2,
    p3.DocNum AS doc_num_pay3,
    p4.DocNum AS doc_num_pay4,
    p5.DocNum AS doc_num_pay5,
    
    -- Group: Store Codes
    o.StoreCode AS store_code_osls,
    op.StoreCode AS store_code_opay,
    oi.StoreCode AS store_code_opoi,
    
    -- Group: Credit Sums
    o.CreditSum AS credit_sum_osls,
    op.CreditSum AS credit_sum_opay,
    p1.CashSum AS credit_sum_pay1,
    p2.CreditSum AS credit_sum_pay2,
    p3.VoucherSum AS vc_sum_pay3,
    p4.BalanceSum AS balance_sum_pay4,
    p5.TrfSum AS credit_sum_pay2,
    
    -- Group: Doc Date
    o.DocDate AS doc_date_osls,
    op.DocDate AS doc_date_osls,
    oi.DocDate AS doc_date_osls
  
FROM 
    osls o
JOIN 
    opay op ON op.DocNum = o.DocNum AND op.StoreCode = o.StoreCode
LEFT JOIN 
    pay1 p1 ON p1.DocNum = o.DocNum AND p1.StoreCode = o.StoreCode
LEFT JOIN 
    pay2 p2 ON p2.DocNum = o.DocNum AND p2.StoreCode = o.StoreCode
LEFT JOIN 
    pay3 p3 ON p3.DocNum = o.DocNum AND p3.StoreCode = o.StoreCode
LEFT JOIN 
    pay4 p4 ON p4.DocNum = o.DocNum AND p4.StoreCode = o.StoreCode
LEFT JOIN 
    pay5 p5 ON p5.DocNum = o.DocNum AND p5.StoreCode = o.StoreCode
JOIN 
    opoi oi ON oi.BaseEntry = o.DocEntry AND oi.StoreCode = o.StoreCode;
```

## 2. Pengecekan Data synchronize (spesifik)

```sql
SELECT * FROM osls o WHERE o.DocEntry = '9178' AND o.DocNum = '2401000003' AND o.StoreCode = '0000005';
SELECT * FROM opay o WHERE o.DocNum = '2401000003' AND o.StoreCode = '0000005';
SELECT * FROM pay2 p WHERE p.DocNum = '2401000003' AND p.StoreCode = '0000005';
SELECT * FROM pay3 p WHERE p.DocNum = '2401000003' AND p.StoreCode = '0000005';
SELECT * FROM pay4 p WHERE p.DocNum = '2401000003' AND p.StoreCode = '0000005';
SELECT * FROM pay5 p WHERE p.DocNum = '2401000003' AND p.StoreCode = '0000005';
SELECT * FROM opoi o WHERE o.BaseEntry = '9178' AND o.StoreCode = '0000005';
```


## Bisa juga tambahkan where untuk spesifik entry

```sql
WHERE 
    o.DocEntry = '9178' 
    AND o.DocNum = '2401000003' 
    AND o.StoreCode = '0000005';
```
