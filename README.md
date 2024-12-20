# Dokumentasi Sync Toko (Dump data)
---

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
    - [Query Create Table](query-create-sync_master-toko.md)
    - Ambil **Primary Key** dari tabel awal (masing-masing tabel master seperti `osls`, `sls1`, `pay1`, dll.) untuk dimasukkan ke dalam tabel `sync_master` sebagai penanda (mark) di database toko.
    - [Query Step 1](step1.md)

2. **[Dump unionweb] - Ambil Data asli `toko` untuk di input ke `cloud table` masing-masing**
    - Insert data ke `sync_master`
    - [Query Insert sync_master](query-insert-sync_master-web.md)
    - Insert data dari `database toko` kedalam masing masing table `osls` , `sls1` .... `opoi`.
    - [Query Step 2](step2.md)
      

3. **Update `sync_master` dengan Primary Key dari `unionretaildb`**
    - Ambil **primary_key_value_union** dari database `unionretaildb` untuk diperbarui di tabel `sync_master` pada database `unionweb`.
    - [Query Step 3](step3.md)
      
4. **Cek data**
    - [Query Cek Data](query-cek-data.md)

# **Panduan Instalasi NFDash V2**

## Requirement System (Pada Toko)
- PHP 7.4.x
- Node.js
- Git
- Composer
- XAMPP
---

### **1. Download & Install Kebutuhan Software**
Pastikan Anda telah mengunduh dan menginstal perangkat lunak berikut sebelum memulai:  
- **Node.js:** [Download Node.js](https://nodejs.org/en/download/prebuilt-installer).  
- **Composer:** [Download Composer](https://getcomposer.org/download/).  
- **Git:** [Download Git](https://git-scm.com/downloads).  

---

### **2. Clone Repository**
Clone repository dari GitHub dan pindah ke branch yang sesuai:  
```bash
git clone https://github.com/rizaals/nfdashv2.git
git checkout origin/minisync-toko
```

---

### **3. Install Project**
Ikuti langkah-langkah berikut untuk menginstal project:  

1. Salin file `.env` contoh:  
   ```bash
   cp .env.example .env
   ```  

2. Generate application key:  
   ```bash
   php artisan key:generate
   ```

3. Install dependensi backend menggunakan Composer:  
   ```bash
   composer install
   ```

4. Install dependensi frontend menggunakan npm:  
   ```bash
   npm install
   ```

---

### **4. Konfigurasi `.env`**
Sesuaikan file `.env` Anda dengan kebutuhan, seperti contoh berikut:  

- **STORE_CODE:** Sesuaikan dengan kode toko Anda:  
  ```plaintext
  STORE_CODE=[kode_toko]
  ```

- **Konfigurasi Database:**  
  ```plaintext
  DB_CONNECTION=mysql
  DB_HOST=localhost
  DB_PORT=3306
  DB_DATABASE=vs_centralpark
  DB_USERNAME=root
  DB_PASSWORD=12345678
  ```

- **Alamat Sinkronisasi:**  
  ```plaintext
  ALAMAT_SYNC="https://dashv2.naturalfarm.id"
  ```

---

### **5. Install Scheduler**
Bagian ini akan ditentukan kemudian (TBD).

---


## Catatan

-   Pastikan parameter `store_code`, `start_date`, dan `end_date` sudah benar sebelum menjalankan proses dump data.
-   Proses ini memerlukan hak akses ke masing-masing database (toko, web, dan cloud).
-   Gunakan prosedur ini dengan hati-hati untuk menjaga integritas data.

---
