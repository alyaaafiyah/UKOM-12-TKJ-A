# A. Instalasi dan Konfigurasi Database MariaDB

Pada bagian ini, kita akan mempelajari cara menginstal dan
mengkonfigurasi MariaDB pada Ubuntu Server. Mulai dari memperbarui
paket, menginstal MariaDB, membuat database dan user, membuat tabel,
memasukkan data, hingga melakukan pengujian login menggunakan user yang
sudah dibuat.

## 1. Memperbarui Daftar Paket

Langkah pertama adalah memperbarui daftar paket pada Ubuntu Server.
Tujuannya agar sistem mendapatkan informasi paket terbaru sebelum
melakukan instalasi.

Jalankan perintah berikut:

``` bash
sudo apt update
```

## 2. Menginstal MariaDB

Setelah proses pembaruan selesai, selanjutnya instal MariaDB Server dan
MariaDB Client. MariaDB digunakan untuk menyimpan dan mengelola data
pada server.

Jalankan perintah berikut:

``` bash
sudo apt install mariadb-server mariadb-client -y
```

Tunggu hingga proses instalasi selesai.

## 3. Mengaktifkan dan Memeriksa Layanan MariaDB

Setelah MariaDB berhasil diinstal, aktifkan layanan agar MariaDB
langsung berjalan dan otomatis aktif ketika Ubuntu Server dinyalakan
kembali.

Jalankan perintah berikut:

``` bash
sudo systemctl enable --now mariadb
```

Untuk memeriksa apakah MariaDB sudah berjalan, gunakan perintah:

``` bash
sudo systemctl status mariadb
```

Pastikan status layanan menunjukkan **active (running)**. Jika sudah,
berarti MariaDB berhasil dijalankan. Tekan tombol `q` untuk keluar dari
tampilan status.

## 4. Menjalankan Pengamanan Awal MariaDB

Langkah selanjutnya adalah melakukan pengamanan awal MariaDB. Proses ini
bertujuan untuk mengatur beberapa pengaturan keamanan agar database
lebih aman digunakan.

Jalankan perintah berikut:

``` bash
sudo mariadb-secure-installation
```

Ikuti pertanyaan yang muncul pada terminal sesuai kebutuhan konfigurasi
server.

Setelah selesai, masuk ke konsol MariaDB menggunakan perintah:

``` bash
sudo mariadb
```

## 5. Membuat Database dan User

Setelah berhasil masuk ke konsol MariaDB, langkah berikutnya adalah
membuat database yang akan digunakan untuk menyimpan data.

Buat database dengan nama `ukom2` menggunakan perintah berikut:

``` sql
CREATE DATABASE ukom2
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

Selanjutnya, buat user khusus yang akan digunakan untuk mengakses
database tersebut:

``` sql
CREATE USER 'ukom2'@'localhost'
IDENTIFIED BY '<PASSWORD_DATABASE>';
```

Ganti `<PASSWORD_DATABASE>` dengan password yang ingin digunakan.

Berikan hak akses kepada user agar dapat mengelola database `ukom2`:

``` sql
GRANT ALL PRIVILEGES ON ukom2.* TO 'ukom2'@'localhost';
```

Kemudian jalankan perintah berikut untuk menerapkan perubahan hak akses:

``` sql
FLUSH PRIVILEGES;
```

Untuk memastikan database sudah berhasil dibuat, tampilkan daftar
database menggunakan perintah:

``` sql
SHOW DATABASES;
```

## 6. Memilih Database dan Membuat Tabel

Setelah database berhasil dibuat, pilih database `ukom2` yang akan
digunakan:

``` sql
USE ukom2;
```

Selanjutnya, buat tabel dengan nama `informasi`. Tabel ini digunakan
untuk menyimpan beberapa informasi yang dibutuhkan dalam konfigurasi
UKOM.

Jalankan perintah berikut:

``` sql
CREATE TABLE informasi (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    keterangan TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Tabel tersebut memiliki beberapa kolom, yaitu:

-   `id`: Nomor identitas data yang dibuat secara otomatis.
-   `nama`: Menyimpan nama informasi.
-   `keterangan`: Menyimpan penjelasan atau keterangan informasi.
-   `created_at`: Menyimpan waktu ketika data ditambahkan.

Untuk memastikan tabel berhasil dibuat, jalankan:

``` sql
SHOW TABLES;
```

## 7. Memasukkan Data ke Dalam Tabel

Setelah tabel berhasil dibuat, selanjutnya masukkan beberapa data contoh
ke dalam tabel `informasi`.

Jalankan perintah berikut:

``` sql
INSERT INTO informasi (nama, keterangan)
VALUES
('UKOM TKJ', 'Web Server dan Database Server'),
('Absen 2', 'Konfigurasi jaringan UKOM'),
('MikroTik', 'Gateway dan HotSpot'),
('Server', 'Ubuntu Server (100.100.X.2)');
```

Untuk melihat data yang sudah berhasil disimpan, gunakan perintah:

``` sql
SELECT * FROM informasi;
```

Jika data muncul pada terminal, berarti proses penambahan data berhasil
dilakukan.

Setelah selesai, keluar dari konsol MariaDB menggunakan perintah:

``` sql
EXIT;
```

## 8. Menguji Login Menggunakan User Database

Langkah terakhir adalah melakukan pengujian untuk memastikan user yang
sudah dibuat dapat digunakan untuk mengakses database.

Pada terminal Ubuntu Server, jalankan perintah berikut:

``` bash
mariadb -u ukom2 -p ukom2
```

Masukkan password yang sudah dibuat sebelumnya. Saat mengetik password,
karakter yang dimasukkan tidak akan terlihat pada terminal. Hal tersebut
merupakan hal yang normal.

Jika berhasil masuk, jalankan perintah berikut untuk melihat data yang
sudah disimpan:

``` sql
SELECT * FROM informasi;
```

Apabila seluruh data berhasil ditampilkan, berarti database, tabel,
data, dan hak akses user sudah berhasil dikonfigurasi.

Untuk keluar dari MariaDB, gunakan perintah:

``` sql
EXIT;
```

## Catatan Penting

1.  Pastikan nama database yang digunakan selalu konsisten, yaitu
    `ukom2`.
2.  Gunakan `USE ukom2;` untuk memilih database yang sudah dibuat.
3.  Ganti alamat `100.100.X.2` dengan IP Ubuntu Server yang digunakan
    jika diperlukan.
4.  Jangan mencantumkan password database asli pada dokumentasi GitHub
    yang bersifat Public. Gunakan placeholder seperti
    `<PASSWORD_DATABASE>` agar informasi login tidak diketahui orang
    lain.
5.  Pastikan setiap perintah dijalankan sesuai urutan agar proses
    konfigurasi berjalan dengan baik.
