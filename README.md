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
    - [Lihat Query Create Table](query-create-sync_union-toko.md)
    - Ambil **Primary Key** dari tabel awal (masing-masing tabel master seperti `osls`, `sls1`, `pay1`, dll.) untuk dimasukkan ke dalam tabel `sync_master` sebagai penanda (mark) di database toko.
    - [Lihat Query Create Table](query-create-sync_union-toko.md)

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
