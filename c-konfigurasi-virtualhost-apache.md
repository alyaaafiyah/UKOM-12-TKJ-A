# C. Konfigurasi VirtualHost Apache

VirtualHost digunakan untuk mengatur agar Apache dapat menampilkan website ketika diakses menggunakan nama domain tertentu. Pada bagian ini, kita akan membuat konfigurasi untuk domain `ukomtkj-2.sch.id`, lalu mengaktifkannya agar dapat digunakan oleh Apache.

## 1. Membuat File Konfigurasi VirtualHost

Pertama, buat file konfigurasi VirtualHost menggunakan perintah berikut:

```bash
sudo nano /etc/apache2/sites-available/ukomtkj-2.sch.id.conf
```

Setelah file terbuka, masukkan konfigurasi berikut:

```apache
<VirtualHost *:80>
    ServerName ukomtkj-2.sch.id
    ServerAlias www.ukomtkj-2.sch.id

    DocumentRoot /var/www/html
    DirectoryIndex index.html index.php

    <Directory /var/www/html>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/ukom-error.log
    CustomLog ${APACHE_LOG_DIR}/ukom-access.log combined
</VirtualHost>
```

Keterangan singkat:
- `ServerName` menentukan nama domain utama website.
- `ServerAlias` menambahkan nama domain alternatif, yaitu versi `www`.
- `DocumentRoot` menentukan folder tempat file website disimpan.
- `DirectoryIndex` menentukan file yang akan dibuka pertama kali.
- `ErrorLog` dan `CustomLog` digunakan untuk menyimpan catatan kesalahan dan aktivitas akses website.

Setelah selesai, simpan file dengan menekan **Ctrl + O**, lalu **Enter**. Keluar dari editor dengan menekan **Ctrl + X**.

## 2. Mengaktifkan VirtualHost

Selanjutnya, aktifkan konfigurasi VirtualHost yang sudah dibuat dengan perintah berikut:

```bash
sudo a2ensite ukomtkj-2.sch.id.conf
```

Perintah tersebut digunakan agar Apache mulai menggunakan konfigurasi website yang telah dibuat.

## 3. Memeriksa Konfigurasi Apache

Sebelum menerapkan konfigurasi, periksa terlebih dahulu apakah terdapat kesalahan penulisan dengan perintah:

```bash
sudo apache2ctl configtest
```

Jika konfigurasi sudah benar, akan muncul hasil:

```text
Syntax OK
```

Jika muncul pesan kesalahan, periksa kembali isi file VirtualHost dan perbaiki bagian yang salah.

## 4. Memuat Ulang Apache

Jika hasil pemeriksaan menunjukkan `Syntax OK`, jalankan perintah berikut untuk memuat ulang Apache:

```bash
sudo systemctl reload apache2
```

Perintah ini membuat Apache membaca konfigurasi VirtualHost yang baru tanpa perlu mematikan layanan Apache.

## 5. Melakukan Pengujian

Terakhir, pastikan domain `ukomtkj-2.sch.id` sudah diarahkan ke alamat IP Ubuntu Server melalui konfigurasi DNS. Setelah itu, buka browser pada komputer client dan masukkan alamat domain tersebut.

Jika konfigurasi DNS dan Apache sudah benar serta komputer client dapat terhubung ke server, website akan tampil melalui domain tersebut.

**Catatan:** Gunakan nama domain yang sama pada konfigurasi VirtualHost dan DNS. Jika domain yang ditentukan saat pengujian berbeda, sesuaikan nama domain pada file konfigurasi sebelum melakukan pengujian.
