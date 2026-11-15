# Konfigurasi IP Ubuntu Server

**Nama:** Alya Afiyah  
**Nomor Absen:** 2  
**Materi:** Instalasi Ubuntu Server dan Konfigurasi IP Address

## A. Langkah Awal Instalasi Ubuntu Server

### 1. Membuat Virtual Machine

Buat virtual machine baru dengan pengaturan berikut:

- **ISO:** Ubuntu Server 24.04
- **RAM:** 4096 MB
- **CPU:** 2 Core
- **Penyimpanan:** 25 GB
- **Network:** Bridged Adapter

**Catatan:** Jika proses jaringan atau instalasi berjalan lambat, mode **Host-Only Adapter** dapat digunakan sesuai kebutuhan praktik.

### 2. Proses Instalasi Ubuntu Server

Ikuti pengaturan awal instalasi berikut:

1. Pilih bahasa **English**, lalu pilih **Done**.
2. Atur alamat IP secara manual:
   - **Subnet:** `100.100.2.0/30`
   - **Address:** `100.100.2.2`
   - **Gateway:** `100.100.2.1`
3. Pilih **Custom storage layout**.
4. Pada bagian penyimpanan, gunakan **Free Space** sebesar 10 GB, lalu pilih **Continue**.
5. Isi informasi pengguna:
   - **Your Name:** `alya`
   - **Name Server:** `alya`
   - **Username:** `alya`
   - **Password:** isi sesuai kebutuhan praktik
   - **Confirm Password:** masukkan kembali password yang sama
6. Centang **Install OpenSSH Server** agar server dapat diakses melalui SSH.
7. Pilih **Done**, lanjutkan instalasi, lalu lakukan reboot setelah proses selesai.

### 3. Login ke Ubuntu Server

Setelah server melakukan reboot, login menggunakan **username** dan **password** yang telah dibuat saat instalasi.

## B. Konfigurasi IP Ubuntu Server

### 1. Melihat Informasi IP Address

Jalankan perintah berikut untuk melihat informasi jaringan:

```bash
ip a
```

### 2. Melihat Status Interface Jaringan

Gunakan perintah berikut agar nama interface dan statusnya terlihat lebih ringkas:

```bash
ip -br a
```

Contoh interface yang muncul:

- `lo` — interface loopback.
- `enp0s3` — interface jaringan virtual yang digunakan pada praktik ini.

### 3. Melihat File Konfigurasi Netplan

Periksa file konfigurasi Netplan yang tersedia dengan perintah:

```bash
ls /etc/netplan/
```

### 4. Mengubah Konfigurasi IP Address

Buka file konfigurasi Netplan menggunakan perintah:

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Sesuaikan isi konfigurasi menjadi seperti berikut. Ganti huruf **X** dengan nomor absen yang digunakan pada praktik.

```yaml
network:
  ethernets:
    enp0s3:
      addresses:
        - 100.100.X.2/30
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
        search: []
      routes:
        - to: default
          via: 100.100.X.1
  version: 2
```

Keterangan:

- **enp0s3:** nama interface jaringan Ubuntu Server.
- **addresses:** alamat IP yang digunakan Ubuntu Server.
- **nameservers:** alamat DNS untuk membantu menerjemahkan nama domain.
- **routes:** pengaturan jalur jaringan default.
- **via:** alamat gateway untuk menuju jaringan lain.
- **X:** nomor absen yang digunakan sebagai bagian dari alamat IP.

Setelah selesai mengedit, simpan file dengan menekan **Ctrl + O**, lalu **Enter**. Setelah itu, tekan **Ctrl + X** untuk keluar dari editor Nano.

### 5. Menguji Konfigurasi Netplan

Jalankan perintah berikut untuk menguji konfigurasi jaringan:

```bash
sudo netplan try
```

Jika konfigurasi sudah benar, konfirmasi pengaturan sesuai petunjuk yang muncul di terminal.

### 6. Menerapkan Konfigurasi Netplan

Setelah konfigurasi berhasil diuji, jalankan perintah berikut:

```bash
sudo netplan apply
```

## C. Pengujian Koneksi Jaringan

### 1. Memeriksa IP Address

Periksa kembali alamat IP yang terpasang dengan perintah:

```bash
ip a
```

Pastikan interface `enp0s3` sudah menggunakan alamat IP `100.100.X.2/30`, sesuai nomor absen yang digunakan.

### 2. Menguji Koneksi ke MikroTik

Uji koneksi dari Ubuntu Server menuju gateway MikroTik pada interface Ether3 dengan perintah:

```bash
ping 100.100.X.1
```

### 3. Menguji Koneksi ke MikroTik

Uji koneksi dari Ubuntu Server menuju gateway MikroTik pada interface Ether3 dengan perintah:

```bash
ping google.com
```

Note : Ganti **X** dengan nomor absen yang digunakan. Jika muncul balasan seperti `64 bytes from ...`, berarti Ubuntu Server berhasil berkomunikasi dengan interface **Ether3 MikroTik**.

## D. Kesimpulan

Pada praktik ini, Ubuntu Server diinstal dan dikonfigurasi menggunakan alamat IP statis melalui Netplan. Setelah konfigurasi diterapkan, koneksi diuji dengan melakukan ping ke gateway MikroTik. Pengujian ini bertujuan memastikan Ubuntu Server dapat terhubung dengan jaringan pada interface Ether3 MikroTik.

---

**Catatan keamanan:** Jangan mencantumkan password asli pada dokumentasi GitHub yang bersifat Public.
