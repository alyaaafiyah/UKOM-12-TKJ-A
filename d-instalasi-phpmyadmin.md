# D. Instalasi phpMyAdmin

phpMyAdmin adalah aplikasi berbasis web yang digunakan untuk mengelola database MariaDB melalui browser. Dengan phpMyAdmin, pengguna dapat melihat database, membuat tabel, menambahkan data, dan mengatur database tanpa harus selalu menggunakan perintah terminal.

## 1. Memperbarui Daftar Paket dan Menginstal phpMyAdmin

Sebelum menginstal phpMyAdmin, perbarui terlebih dahulu daftar paket Ubuntu Server. Setelah itu, instal phpMyAdmin beserta paket PHP yang dibutuhkan agar dapat terhubung dengan MariaDB.

Jalankan perintah berikut:

```bash
sudo apt update
```

```bash
sudo apt install phpmyadmin php-mysql -y
```

Ikuti pilihan konfigurasi yang muncul selama proses instalasi. Jika diminta memilih web server, pilih **Apache2** apabila tersedia. Gunakan tombol spasi untuk memilih, lalu tekan Enter untuk melanjutkan.

## 2. Memeriksa dan Mengaktifkan Konfigurasi Apache

Setelah instalasi selesai, periksa apakah file konfigurasi phpMyAdmin sudah tersedia di Apache. Perintah berikut digunakan untuk mencari konfigurasi tersebut:

```bash
ls -l /etc/apache2/conf-available/ | grep -i phpmyadmin
```

Jika konfigurasi phpMyAdmin ditemukan, aktifkan konfigurasi tersebut dengan perintah:

```bash
sudo a2enconf phpmyadmin
```

Selanjutnya, periksa apakah terdapat kesalahan pada konfigurasi Apache:

```bash
sudo apache2ctl configtest
```

Pastikan hasil pemeriksaan menampilkan:

```text
Syntax OK
```

Jika tidak ada kesalahan, mulai ulang layanan Apache agar perubahan konfigurasi diterapkan:

```bash
sudo systemctl restart apache2
```

## 3. Menguji phpMyAdmin Melalui Browser

Setelah Apache berhasil dijalankan kembali, buka browser pada komputer client yang terhubung dan dapat menjangkau Ubuntu Server.

Masukkan alamat berikut pada kolom URL browser:

```text
http://100.100.x.x/phpmyadmin
```

Ganti `100.100.x.x` dengan alamat IP Ubuntu Server yang sebenarnya. Jika halaman login phpMyAdmin berhasil ditampilkan, berarti phpMyAdmin sudah dapat diakses melalui browser.

Jika halaman belum bisa dibuka, lakukan pemeriksaan pada bagian troubleshooting berikut.

## 4. Troubleshooting Jika phpMyAdmin Tidak Bisa Diakses

Jika phpMyAdmin belum dapat dibuka, periksa terlebih dahulu apakah paket phpMyAdmin sudah terpasang di Ubuntu Server.

Jalankan perintah:

```bash
dpkg -l | grep phpmyadmin
```

Selanjutnya, cari lokasi file konfigurasi Apache yang disediakan oleh paket phpMyAdmin:

```bash
dpkg -L phpmyadmin | grep -E 'apache.conf|phpmyadmin.conf'
```

Jika hasil pencarian menunjukkan file `/etc/phpmyadmin/apache.conf`, tetapi konfigurasi belum tersedia di folder konfigurasi Apache, buat tautan menggunakan perintah berikut:

```bash
sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
```

Perintah tersebut menghubungkan file konfigurasi phpMyAdmin dengan folder konfigurasi Apache agar dapat diaktifkan.

**Catatan:** Jika muncul pesan bahwa file tujuan sudah ada, jangan jalankan perintah tersebut berulang kali. Periksa konfigurasi yang sudah tersedia terlebih dahulu.

Setelah itu, aktifkan konfigurasi phpMyAdmin:

```bash
sudo a2enconf phpmyadmin
```

Periksa kembali konfigurasi Apache:

```bash
sudo apache2ctl configtest
```

Pastikan hasilnya menampilkan `Syntax OK`. Jika konfigurasi sudah benar, mulai ulang Apache:

```bash
sudo systemctl restart apache2
```

## 5. Pengujian Ulang phpMyAdmin

Setelah proses troubleshooting selesai, buka kembali browser dan coba akses phpMyAdmin menggunakan alamat IP Ubuntu Server:

```text
http://100.100.x.x/phpmyadmin
```

Jika DNS domain sudah diarahkan dengan benar ke Ubuntu Server, phpMyAdmin juga dapat diakses melalui nama domain:

```text
http://ukomtkj-2.sch.id/phpmyadmin
```

Apabila halaman login berhasil ditampilkan, masukkan username dan password akun MariaDB yang memiliki hak akses ke database.

## Catatan Penting

- Alamat `100.100.x.x` merupakan contoh alamat IP. Sesuaikan dengan IP Ubuntu Server yang sebenarnya.
- Domain yang digunakan dalam dokumentasi ini adalah `ukomtkj-2.sch.id`.
- Akun yang digunakan untuk login phpMyAdmin harus merupakan akun MariaDB yang memiliki hak akses ke database.
- Jangan mencantumkan password asli pada dokumentasi yang diunggah ke repository GitHub Public.
