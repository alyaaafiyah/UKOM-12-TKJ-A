# 🌐 E. Konfigurasi DNS Server BIND9

**DNS (Domain Name System) Server** digunakan untuk menerjemahkan nama domain menjadi alamat IP (dan sebaliknya). Pada konfigurasi ini, layanan **BIND9** digunakan agar domain `ukomtkj-2.sch.id` dapat diarahkan ke alamat IP Ubuntu Server `100.100.2.2` (atau `100.100.2.x` sesuai konfigurasi jaringan Anda).

---

## 📋 Daftar Isi

1. [Instalasi BIND9](#1-instalasi-bind9)
2. [Menambahkan Konfigurasi Zona DNS](#2-menambahkan-konfigurasi-zona-dns)
3. [Membuat File Zona DNS Forward](#3-membuat-file-zona-dns-forward)
4. [Membuat File Zona DNS Reverse](#4-membuat-file-zona-dns-reverse)
5. [Mengatur Opsi BIND9](#5-mengatur-opsi-bind9)
6. [Memeriksa Konfigurasi BIND9](#6-memeriksa-konfigurasi-bind9)
7. [Mengaktifkan dan Memeriksa Layanan BIND9](#7-mengaktifkan-dan-memeriksa-layanan-bind9)
8. [Menguji DNS dari Ubuntu Server](#8-menguji-dns-dari-ubuntu-server)
9. [Menguji DNS dari Komputer Client](#9-menguji-dns-dari-komputer-client)
10. [Pengujian Akses Website Menggunakan Domain](#10-pengujian-akses-website-menggunakan-domain)
11. [Kesimpulan](#11-kesimpulan)

---

## 1. Instalasi BIND9

Jalankan perintah berikut untuk memperbarui daftar paket dan menginstal BIND9 beserta utilitas DNS:

```bash
sudo apt update
sudo apt install bind9 bind9-utils dnsutils -y
```

* **`apt update`**: Memperbarui daftar paket yang tersedia di repositori.
* **`apt install`**: Memasang `bind9` sebagai layanan DNS dan `dnsutils` untuk menyediakan alat pengujian DNS seperti `dig` dan `nslookup`.

---

## 2. Menambahkan Konfigurasi Zona DNS

Buka file konfigurasi zona lokal:

```bash
sudo nano /etc/bind/named.conf.local
```

Tambahkan konfigurasi berikut di bagian paling bawah file:

```bind
zone "ukomtkj-2.sch.id" {
    type master;
    file "/etc/bind/db.ukomtkj-2";
};

zone "2.100.100.in-addr.arpa" {
    type master;
    file "/etc/bind/db.100.100.2";
};
```

**Keterangan:**
* Zona pertama (`ukomtkj-2.sch.id`) digunakan untuk menerjemahkan domain menjadi alamat IP (**Forward Zone**).
* Zona kedua (`2.100.100.in-addr.arpa`) digunakan untuk menerjemahkan alamat IP menjadi nama domain (**Reverse Zone**).
* `type master` menunjukkan bahwa Ubuntu Server bertindak sebagai DNS Master.
* `file` menentukan lokasi file konfigurasi masing-masing zona.

> **Simpan file:** Tekan `Ctrl + O`, lalu `Enter`, kemudian keluar menggunakan `Ctrl + X`.

---

## 3. Membuat File Zona DNS Forward

Salin file zona bawaan sebagai *template*:

```bash
sudo cp /etc/bind/db.local /etc/bind/db.ukomtkj-2
```

Kemudian buka file zona yang sudah dibuat:

```bash
sudo nano /etc/bind/db.ukomtkj-2
```

Sesuaikan isinya menjadi seperti berikut (ganti `x` pada `100.100.2.x` dengan oktet terakhir IP server Anda, misalnya `2`):

```bind
$TTL 604800
@   IN  SOA ns.ukomtkj-2.sch.id. root.ukomtkj-2.sch.id. (
        2026111203
        604800
        86400
        2419200
        604800
)

@   IN  NS  ns.ukomtkj-2.sch.id.
@   IN  A   100.100.2.x
ns  IN  A   100.100.2.x
www IN  A   100.100.2.x
```

**Keterangan:**
* **`SOA`**: Berisi informasi utama mengenai zona DNS.
* **`NS`**: Menentukan server DNS yang melayani zona tersebut.
* **`A`**: Menghubungkan nama domain dengan alamat IPv4.
* **`www`**: Digunakan agar domain dengan awalan `www` juga dapat diakses.
* **Nomor serial** (`2026111203`): Penanda versi file zona. Nomor ini perlu dinaikkan setiap kali file zona diubah.

> **Simpan file:** Tekan `Ctrl + O`, lalu `Enter`, kemudian keluar menggunakan `Ctrl + X`.

---

## 4. Membuat File Zona DNS Reverse

Buat file Reverse DNS dengan menyalin *template* zona bawaan:

```bash
sudo cp /etc/bind/db.127 /etc/bind/db.100.100.2
```

Kemudian buka file tersebut:

```bash
sudo nano /etc/bind/db.100.100.2
```

Sesuaikan isinya menjadi seperti berikut:

```bind
$TTL 604800
@   IN  SOA ns.ukomtkj-2.sch.id. root.ukomtkj-2.sch.id. (
        2026111203
        604800
        86400
        2419200
        604800
)

@   IN  NS  ns.ukomtkj-2.sch.id.
2   IN  PTR ukomtkj-2.sch.id.
```

**Keterangan:**
* File ini digunakan untuk konfigurasi **Reverse DNS**.
* **`PTR`**: Digunakan untuk menghubungkan alamat IP dengan nama domain.
* Angka **`2`** menunjukkan oktet terakhir dari IP server (`100.100.2.2`).
* **`NS`**: Menentukan server DNS yang melayani zona Reverse.

> **Simpan file:** Tekan `Ctrl + O`, tekan `Enter`, lalu keluar dengan `Ctrl + X`.

---

## 5. Mengatur Opsi BIND9

Buka file konfigurasi opsi BIND9:

```bash
sudo nano /etc/bind/named.conf.options
```

Sesuaikan bagian `options` agar memuat konfigurasi berikut. *(Jika sudah terdapat blok `options`, edit blok yang tersedia dan jangan membuat blok kedua).*

### Opsi 1 (Standar)
```bind
options {
    directory "/var/cache/bind";

    listen-on { 127.0.0.1; 100.100.2.x; };
    listen-on-v6 { any; };

    allow-query {
        localhost;
        10.10.2.0/28;
        100.100.2.0/30;
        192.168.2.0/24;
    };

    recursion yes;

    forwarders {
        1.1.1.1;
        8.8.8.8;
    };

    dnssec-validation auto;
};
```

### Opsi 2 (Dengan Pembatasan Recursion & Cache)
```bind
options {
    directory "/var/cache/bind";

    listen-on {
        127.0.0.1;
        100.100.2.x;
    };

    listen-on-v6 { none; };

    allow-query {
        localhost;
        10.10.2.0/28;
        192.168.2.0/24;
    };

    recursion yes;

    allow-recursion {
        localhost;
        10.10.2.0/28;
        192.168.2.0/24;
    };

    allow-query-cache {
        localhost;
        10.10.2.0/28;
        192.168.2.0/24;
    };

    forwarders {
        8.8.8.8;
        1.1.1.1;
    };

    dnssec-validation auto;
};
```

**Keterangan:**
* **`listen-on`**: Menentukan alamat IPv4 yang digunakan BIND9 untuk menerima permintaan DNS.
* **`allow-query`**: Mengizinkan perangkat dari jaringan yang dicantumkan untuk mengirim permintaan DNS.
* **`recursion yes`**: Mengaktifkan pencarian DNS lanjutan.
* **`forwarders`**: Meneruskan permintaan DNS yang tidak dapat dijawab secara lokal ke DNS publik (`1.1.1.1` / `8.8.8.8`).
* **`dnssec-validation auto`**: Mengaktifkan validasi DNSSEC secara otomatis.

> Simpan file setelah selesai mengedit (`Ctrl + O`, `Enter`, lalu `Ctrl + X`).

---

## 6. Memeriksa Konfigurasi BIND9

Periksa sintaks konfigurasi utama BIND9 menggunakan perintah:

```bash
sudo named-checkconf
```

Jika tidak muncul pesan kesalahan, lanjutkan dengan memeriksa file zona **Forward**:

```bash
sudo named-checkzone ukomtkj-2.sch.id /etc/bind/db.ukomtkj-2
```

Kemudian periksa file zona **Reverse**:

```bash
sudo named-checkzone 2.100.100.in-addr.arpa /etc/bind/db.100.100.2
```

> *Pastikan kedua pemeriksaan menampilkan status **`OK`**. Pemeriksaan ini memastikan konfigurasi zona sudah benar sebelum layanan dijalankan.*

---

## 7. Mengaktifkan dan Memeriksa Layanan BIND9

Jalankan perintah berikut untuk mengaktifkan layanan BIND9 agar berjalan otomatis saat server dinyalakan, lalu mulai ulang layanan:

```bash
sudo systemctl enable --now named
sudo systemctl restart bind9
sudo systemctl status bind9
```

* **`enable --now`**: Mengaktifkan layanan sekaligus menjalankannya.
* **`restart`**: Memulai ulang layanan agar konfigurasi terbaru diterapkan.
* **`status`**: Memeriksa kondisi layanan saat ini.

> *Pastikan status layanan menunjukkan **`active (running)`**. Tekan tombol `Q` untuk keluar dari tampilan status.*

---

## 8. Menguji DNS dari Ubuntu Server

Uji domain utama dan subdomain `www` menggunakan DNS lokal:

```bash
dig @127.0.0.1 ukomtkj-2.sch.id
dig @127.0.0.1 www.ukomtkj-2.sch.id
dig @100.100.2.x ukomtkj-2.sch.id
dig @100.100.2.x www.ukomtkj-2.sch.id
```

Kemudian uji **Reverse DNS**:

```bash
dig @127.0.0.1 -x 100.100.2.x
```

> *Periksa bagian **`ANSWER SECTION`**. Hasil Forward DNS harus mengarah ke `100.100.2.x`, sedangkan hasil Reverse DNS harus menampilkan domain `ukomtkj-2.sch.id`.*

---

## 9. Menguji DNS dari Komputer Client

Buka **Command Prompt (CMD)** sebagai Administrator pada Windows, kemudian bersihkan *cache* DNS:

```cmd
ipconfig /flushdns
```

Setelah itu, uji domain menggunakan DNS MikroTik yang sesuai (contoh jika *client* terhubung melalui `ether2` dengan IP `10.10.2.1`):

```cmd
nslookup ukomtkj-2.sch.id 10.10.2.1
nslookup www.ukomtkj-2.sch.id 10.10.2.1
```

Untuk menguji **Reverse DNS**:

```cmd
nslookup 100.100.2.2 10.10.2.1
```

> *Pastikan hasil pengujian menunjukkan alamat IP dan nama domain yang sesuai dengan konfigurasi server.*

---

## 10. Pengujian Akses Website Menggunakan Domain

Setelah DNS berhasil diuji, buka *browser* pada komputer *client* dan akses alamat berikut:

* `http://ukomtkj-2.sch.id`
* `http://www.ukomtkj-2.sch.id`

> *Jika halaman website berhasil ditampilkan, berarti konfigurasi DNS sudah terhubung dengan layanan web server (Apache/Nginx).*

---

## 11. Kesimpulan

Konfigurasi **BIND9** digunakan untuk menghubungkan domain `ukomtkj-2.sch.id` dengan alamat IP Ubuntu Server `100.100.2.x`. Tahapan konfigurasi meliputi pembuatan **Forward Zone**, **Reverse Zone**, pengaturan opsi BIND9, serta pengaktifan layanan DNS.

Selanjutnya, **DNS Static** ditambahkan pada MikroTik agar domain website mengarah ke IP Ubuntu Server. Pengujian Forward DNS dan Reverse DNS menggunakan `dig` serta `nslookup` dilakukan untuk memastikan penerjemahan domain dan alamat IP berjalan sesuai konfigurasi.