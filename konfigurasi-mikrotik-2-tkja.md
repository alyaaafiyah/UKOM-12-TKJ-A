# Konfigurasi MikroTik-2 TKJA

**Nama:** Alya Afiyah\
**Nomor Absen:** 2\
**Materi:** Konfigurasi MikroTik, Hotspot, DHCP Server, Simple Queue,
dan Ubuntu Server

------------------------------------------------------------------------

## 1. Persiapan Router MikroTik

### A. Mengatur IP Address pada Laptop

Sebelum melakukan konfigurasi router, atur alamat IP pada laptop melalui
Control Panel Windows. Gunakan pengaturan berikut:

-   **IP Address:** 192.168.88.2
-   **Subnet Mask:** 255.255.255.0

Pengaturan ini digunakan agar laptop dapat terhubung ke router MikroTik
saat proses konfigurasi awal.

### B. Reset Configuration MikroTik

Buka aplikasi **WinBox** dan hubungkan laptop ke router MikroTik.
Setelah berhasil masuk, lakukan **Reset Configuration** agar konfigurasi
router kembali ke kondisi awal dan proses pengaturan dapat dimulai dari
awal.

### C. Mengatur System Identity

Buka menu **System → Identity**, kemudian ubah nama router menjadi
**R-UKOM2**. Nama ini berfungsi sebagai identitas router agar lebih
mudah dikenali. Nama identity dapat disesuaikan dengan kebutuhan.

### D. Membuat DHCP Client

Masuk ke menu **IP → DHCP Client**, klik **New**, lalu pilih interface
**ether1**. Tunggu hingga status berubah menjadi **bound**. Status
tersebut menunjukkan bahwa MikroTik telah berhasil memperoleh alamat IP
secara otomatis dari jaringan yang terhubung ke ether1.

### E. Mengubah Nama Interface

Atur nama interface agar fungsi setiap port lebih mudah dikenali:

  Interface   Nama
  ----------- ---------
  ether1      ISP
  ether2      Client
  ether3      Server
  wlan1       Hotspot

### F. Menambahkan IP Address

Masuk ke menu **IP → Addresses**, kemudian tambahkan alamat IP pada
interface sesuai pembagian berikut:

  Interface   IP Address       Keterangan
  ----------- ---------------- ------------------
  ether2      10.10.2.1/28     Jaringan Client
  ether3      100.100.2.1/30   Jaringan Server
  wlan1       192.168.2.1/24   Jaringan Hotspot

Pemberian IP Address bertujuan agar setiap interface memiliki alamat
jaringan sebagai jalur komunikasi dengan perangkat yang terhubung.

### G. Konfigurasi Firewall NAT

Buka menu **IP → Firewall → NAT**, kemudian klik **New**. Pada tab
**General**, atur **Out. Interface** menjadi **ether1-ISP**. Setelah
itu, buka tab **Action** dan pilih **masquerade**, lalu klik **Apply**
dan **OK**.

Konfigurasi NAT digunakan agar perangkat pada jaringan lokal dapat
mengakses jaringan luar melalui koneksi ISP yang terhubung ke ether1.

### H. Konfigurasi DNS

Masuk ke menu **IP → DNS**, kemudian isi alamat DNS Server dengan:

-   **8.8.8.8**
-   **1.1.1.1**

Selanjutnya, centang **Allow Remote Requests** dan simpan pengaturan.
DNS berfungsi menerjemahkan nama domain menjadi alamat IP, sedangkan
Allow Remote Requests memungkinkan MikroTik menerima permintaan DNS dari
perangkat client.

### I. Konfigurasi Wireless Access Point

Buka menu **Wireless → Interfaces**, pilih **wlan1**, lalu masuk ke tab
**Wireless**. Atur pengaturan berikut:

-   **Mode:** ap bridge
-   **Band:** 2GHz-B/G/N
-   **SSID:** ukomtkj-2

Mode **ap bridge** digunakan agar MikroTik berfungsi sebagai pemancar
WiFi. Sementara itu, **SSID** merupakan nama jaringan WiFi yang akan
terlihat oleh perangkat pengguna.

### J. Mengaktifkan Hotspot Server

Masuk ke menu **IP → Hotspot**, buka tab **Servers**, lalu pilih
**Hotspot Setup**. Pada bagian **Hotspot Interface**, pilih
**wlan1-Hotspot** dan lanjutkan proses pengaturan hingga selesai.

Langkah ini digunakan untuk mengaktifkan layanan Hotspot pada interface
WiFi sehingga pengguna dapat terhubung melalui halaman login.

### K. Mengatur Hotspot User Profile

Masuk ke menu **IP → Hotspot → User Profiles**, kemudian buka profil
**default** dan pilih tab **General**. Atur beberapa bagian berikut:

-   **Name:** default
-   **Address Pool:** hs-pool-6
-   **Shared Users:** 2
-   **Rate Limit (rx/tx):** 7M/7M
-   **Keepalive Timeout:** 00:02:00

**Address Pool** menentukan kumpulan alamat IP yang digunakan oleh
pengguna Hotspot. **Shared Users** membatasi satu akun agar dapat
digunakan oleh maksimal dua perangkat secara bersamaan. **Rate Limit**
mengatur batas kecepatan upload dan download, sedangkan **Keepalive
Timeout** mengatur waktu pengecekan koneksi pengguna.

### L. Membuat Akun Hotspot

Masuk ke menu **IP → Hotspot → Users**, klik **New**, lalu isi
pengaturan pada tab **General**:

-   **Server:** hotspot1
-   **Name:** Ukomalya
-   **Password:** isi sesuai konfigurasi praktikum
-   **Profile:** default

Pengaturan ini digunakan untuk membuat akun yang akan dipakai pengguna
saat login ke jaringan Hotspot. Profil **default** dipilih agar akun
mengikuti kebijakan penggunaan yang telah diatur sebelumnya.

### M. Mengatur Simple Queue

Buka menu **Queues → Simple Queues**, kemudian klik **New**. Pada tab
**General**, isi pengaturan berikut:

-   **Name:** ukomtkj-2
-   **Target:** ether2 - Client
-   **Max Limit Upload:** 10M
-   **Max Limit Download:** 10M

**Simple Queue** digunakan untuk mengatur dan membatasi penggunaan
bandwidth pada jaringan Client. Pada konfigurasi ini, batas maksimal
upload dan download masing-masing ditetapkan sebesar **10 Mbps**.

### N. Mengatur DHCP Server

Masuk ke menu **IP → DHCP Server**. Pilih **DHCP Setup**, tentukan
interface **ether2-Client**, lalu lanjutkan proses pengaturan hingga
selesai.

Berdasarkan konfigurasi yang digunakan, terdapat dua DHCP Server:

**1. DHCP Server Hotspot**

-   **Name:** dhcp1
-   **Interface:** wlan1 - Hotspot
-   **Lease Time:** 01:00:00
-   **Address Pool:** hs-pool-1

**2. DHCP Server Client**

-   **Name:** dhcp2
-   **Interface:** ether2 - Client
-   **Lease Time:** 00:10:00
-   **Address Pool:** dhcp-pool1

DHCP Server berfungsi memberikan alamat IP secara otomatis kepada
perangkat yang terhubung. Dengan demikian, perangkat pada jaringan
Hotspot dan Client dapat memperoleh alamat IP dari pool masing-masing
tanpa harus mengatur IP secara manual.

### O. Mengatur IP Laptop Menjadi Otomatis

Setelah DHCP Server pada interface Client selesai dikonfigurasi, buka
kembali pengaturan jaringan pada Control Panel Windows. Pada pengaturan
IPv4, pilih:

-   **Obtain an IP address automatically**
-   **Obtain DNS server address automatically**

Pengaturan ini membuat laptop memperoleh alamat IP dan DNS secara
otomatis dari DHCP Server MikroTik saat terhubung ke jaringan Client.
Pengaturan ini menggantikan IP statis yang digunakan pada tahap
persiapan awal.

------------------------------------------------------------------------

## 2. Hasil Konfigurasi dan Pengujian

Setelah seluruh konfigurasi selesai, lakukan pengujian untuk memastikan
setiap layanan berjalan sesuai fungsinya. Pengujian dapat mencakup
pemeriksaan alamat IP yang diterima laptop, koneksi jaringan Client dan
Hotspot, proses login menggunakan akun Hotspot, serta penerapan batas
bandwidth melalui Simple Queue.

Hasil pengujian dan screenshot dapat ditambahkan pada bagian ini sesuai
hasil praktik yang diperoleh.

## 3. Kesimpulan

Melalui praktikum ini, dilakukan konfigurasi dasar MikroTik yang
meliputi pengaturan interface, IP Address, DHCP Client, Firewall NAT,
DNS, Wireless Access Point, Hotspot, User Profile, Simple Queue, dan
DHCP Server. Setiap layanan memiliki fungsi dalam mengatur koneksi,
memberikan alamat IP, menyediakan akses WiFi, serta mengelola penggunaan
bandwidth. Praktikum ini membantu memahami cara membangun dan mengelola
jaringan menggunakan MikroTik.

------------------------------------------------------------------------

**Catatan keamanan:** Repository ini bersifat Public. Jangan
mencantumkan password asli pada dokumentasi atau mengunggah screenshot
yang menampilkan kata sandi. Gunakan placeholder atau samarkan informasi
login sebelum dipublikasikan.
