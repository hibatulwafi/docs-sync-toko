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
    - Create table `sync_master` jika belum exist
    - [Query Create Table](query-create-sync_union-toko.md)
    - Ambil **Primary Key** dari tabel awal (masing-masing tabel master seperti `osls`, `sls1`, `pay1`, dll.) untuk dimasukkan ke dalam tabel `sync_master` sebagai penanda (mark) di database toko.
    - [Query Step 1](step1.md)

2. **[Dump unionweb] - Ambil Data asli `toko` untuk di input ke `cloud table` masing-masing**
    - Insert data ke `sync_master`
    - [Query Insert sync_master](query-insert-sync_union-web.md)
    - Insert data dari `database toko` kedalam masing masing table `osls` , `sls1` .... `opoi`.
    - [Query Step 2](step2.md)
    - 

3. **Update `sync_master` dengan Primary Key dari `unionretaildb`**
    - Ambil **primary_key_value_union** dari database `unionretaildb` untuk diperbarui di tabel `sync_master` pada database `unionweb`.
    - [Query Step 2](step3.md)
---

## Catatan

-   Pastikan parameter `store_code`, `start_date`, dan `end_date` sudah benar sebelum menjalankan proses dump data.
-   Proses ini memerlukan hak akses ke masing-masing database (toko, web, dan cloud).
-   Gunakan prosedur ini dengan hati-hati untuk menjaga integritas data.

---
