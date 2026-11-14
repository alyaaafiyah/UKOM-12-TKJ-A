# B. Instalasi Web Server Apache dan PHP

Pada bagian ini, kita akan menginstal Apache dan PHP pada Ubuntu Server. Apache berfungsi untuk menjalankan website, sedangkan PHP digunakan agar website dapat memproses data dan terhubung ke database MariaDB yang sudah dibuat pada Bagian A.

## 1. Menginstal Apache dan PHP

Langkah pertama adalah menginstal Apache, PHP, dan beberapa paket pendukung agar PHP dapat berjalan bersama Apache serta terhubung ke MariaDB.

Jalankan perintah berikut pada terminal Ubuntu Server:

```bash
sudo apt install apache2 php libapache2-mod-php php-mysql -y
```

Tunggu sampai proses instalasi selesai.

## 2. Mengaktifkan dan Memeriksa Layanan Apache

Setelah instalasi selesai, aktifkan layanan Apache agar langsung berjalan dan otomatis aktif ketika Ubuntu Server dinyalakan kembali.

Jalankan perintah berikut:

```bash
sudo systemctl enable --now apache2
```

Untuk memeriksa apakah Apache sudah berjalan, gunakan perintah:

```bash
sudo systemctl status apache2
```

Pastikan status layanan menunjukkan **active (running)**. Jika sudah, berarti Apache berhasil dijalankan. Tekan tombol `q` untuk keluar dari tampilan status.

## 3. Membuat Halaman Utama Website

Apache menggunakan file `index.html` sebagai halaman utama website. File ini berada di direktori `/var/www/html/`.

Buka file tersebut menggunakan perintah:

```bash
sudo nano /var/www/html/index.html
```

Isi file dengan kode HTML landing page yang sudah disiapkan. Jika ingin menggunakan desain website sendiri, kode di dalam file dapat disesuaikan atau diganti.

**Catatan:** Kode landing page dapat disimpan atau dibuat secara terpisah. Pastikan file utama website tetap bernama `index.html` dan berada di direktori `/var/www/html/`.

Setelah selesai mengedit, simpan file dengan menekan `Ctrl + O`, lalu tekan `Enter`. Untuk keluar dari editor Nano, tekan `Ctrl + X`.

## 4. Membuat Halaman untuk Menampilkan Data Database

Selanjutnya, buat file PHP yang digunakan untuk menghubungkan website dengan database MariaDB. File ini akan mengambil data dari tabel `informasi`, kemudian menampilkannya melalui browser.

Buat file bernama `database.php` dengan perintah:

```bash
sudo nano /var/www/html/database.php
```

Masukkan kode PHP berikut ke dalam file:

```php
<?php
$host = "localhost";
$user = "ukom2";
$password = "<PASSWORD_DATABASE>";
$database = "ukom2";

$conn = new mysqli($host, $user, $password, $database);

if ($conn->connect_error) {
    http_response_code(500);
    die("Koneksi database gagal.");
}

$conn->set_charset("utf8mb4");

echo "<h1>Koneksi Database Berhasil</h1>";
echo "<h2>Data Informasi UKOM TKJ</h2>";

$sql = "SELECT id, nama, keterangan, created_at FROM informasi";
$result = $conn->query($sql);

if ($result && $result->num_rows > 0) {
    while ($row = $result->fetch_assoc()) {
        echo "<hr>";
        echo "ID: " . htmlspecialchars((string)$row["id"]) . "<br>";
        echo "Nama: " . htmlspecialchars($row["nama"]) . "<br>";
        echo "Keterangan: " .
             htmlspecialchars($row["keterangan"]) . "<br>";
        echo "Dibuat: " .
             htmlspecialchars($row["created_at"]) . "<br>";
    }
} else {
    echo "Belum ada data informasi.";
}

$conn->close();
?>
```

Pada bagian `$password`, ganti `<PASSWORD_DATABASE>` dengan password user MariaDB yang dibuat pada Bagian A. Jangan menggunakan placeholder tersebut sebagai password sungguhan.

Kode di atas memiliki fungsi sebagai berikut:

- Mengatur alamat server, nama user, password, dan nama database.
- Menghubungkan PHP ke database MariaDB.
- Memeriksa apakah koneksi database berhasil.
- Mengambil data dari tabel `informasi`.
- Menampilkan data seperti ID, nama, keterangan, dan waktu data dibuat pada halaman website.

Setelah kode dimasukkan, simpan file dengan menekan `Ctrl + O`, lalu tekan `Enter`. Setelah itu, tekan `Ctrl + X` untuk keluar dari editor Nano.

## 5. Memulai Ulang Apache

Setelah file website dan PHP selesai dibuat, mulai ulang layanan Apache agar konfigurasi dan file yang dibuat dapat digunakan.

Jalankan perintah berikut:

```bash
sudo systemctl restart apache2
```

## 6. Menguji Website Melalui Browser Client

Langkah terakhir adalah menguji website melalui browser pada komputer client yang terhubung ke jaringan server.

Buka alamat berikut pada browser:

**Untuk menampilkan landing page:**

```text
http://ukomtkj-2.sch.id/
```

**Untuk menguji koneksi PHP ke MariaDB:**

```text
http://ukomtkj-2.sch.id/database.php
```

Sesuaikan nama domain tersebut dengan domain yang digunakan pada konfigurasi DNS server.

Jika DNS belum dapat digunakan, website juga dapat diakses menggunakan alamat IP Ubuntu Server. Contohnya:

```text
http://IP-UBUNTU-SERVER/
```

Untuk menguji halaman database:

```text
http://IP-UBUNTU-SERVER/database.php
```

Jika landing page berhasil ditampilkan dan halaman `database.php` menampilkan pesan **Koneksi Database Berhasil** beserta data dari tabel `informasi`, berarti Apache, PHP, dan koneksi ke MariaDB sudah berjalan dengan baik.

## Catatan Penting

1. Pastikan Apache sudah berstatus **active (running)** sebelum melakukan pengujian melalui browser.
2. File website harus disimpan di direktori `/var/www/html/`.
3. Pastikan nama database, user, dan tabel sesuai dengan konfigurasi pada Bagian A, yaitu database `ukom2`, user `ukom2`, dan tabel `informasi`.
4. Ganti `<PASSWORD_DATABASE>` pada file PHP dengan password MariaDB yang benar pada server.
5. Jangan mengunggah password asli ke repository GitHub yang bersifat Public. Gunakan placeholder pada dokumentasi dan hindari mengunggah file konfigurasi yang berisi password asli.
6. Jika menggunakan domain, pastikan DNS sudah mengarah ke alamat IP Ubuntu Server yang benar.
