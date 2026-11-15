# MATERI KONFIGURASI MIKROTIK DAN UBUNTU SERVER

---

## BAB I – DASAR-DASAR JARINGAN KOMPUTER

### 1. Pengertian Jaringan Komputer
Jaringan komputer merupakan sekumpulan perangkat komputer yang saling terhubung untuk berkomunikasi dan bertukar data. Dengan adanya jaringan komputer, perangkat dapat berbagi sumber daya seperti koneksi internet, file, printer, maupun layanan server.

Dalam penerapannya, jaringan komputer membutuhkan beberapa komponen, seperti router, switch, access point, server, dan client. Setiap perangkat memiliki fungsi masing-masing agar komunikasi data dapat berjalan dengan baik.

Pada praktik konfigurasi MikroTik dan Ubuntu Server, jaringan digunakan untuk menghubungkan komputer client, perangkat wireless, dan server sehingga setiap perangkat dapat saling berkomunikasi serta mengakses layanan yang tersedia.

### 2. Pengertian IP Address
IP Address (*Internet Protocol Address*) merupakan alamat berupa angka yang diberikan kepada setiap perangkat dalam jaringan komputer. IP Address berfungsi sebagai identitas perangkat agar dapat dikenali dan melakukan komunikasi dengan perangkat lainnya.

IP Address versi 4 (IPv4) memiliki panjang 32 bit dan terdiri dari empat bagian angka yang dipisahkan oleh tanda titik.

**Contoh:**
`192.168.2.1`

Setiap bagian angka disebut **oktet**, dengan nilai antara `0` sampai `255`. IP Address terdiri dari dua bagian utama:
- **Network ID:** Bagian alamat yang menunjukkan identitas jaringan.
- **Host ID:** Bagian alamat yang menunjukkan identitas perangkat dalam jaringan tersebut.

Sebagai contoh, pada alamat `192.168.2.1/24`, bagian jaringan adalah `192.168.2.0`, sedangkan angka `1` menunjukkan alamat perangkat dalam jaringan tersebut.

### 3. Jenis-Jenis IP Address
Berdasarkan penggunaannya, IP Address dibagi menjadi beberapa jenis:

#### a. IP Private
IP Private merupakan alamat IP yang digunakan dalam jaringan lokal (LAN) dan tidak dapat diakses secara langsung dari internet publik.

| Kelas | Rentang IP |
| :--- | :--- |
| **Class A** | `10.0.0.0 – 10.255.255.255` |
| **Class B** | `172.16.0.0 – 172.31.255.255` |
| **Class C** | `192.168.0.0 – 192.168.255.255` |

#### b. IP Public
IP Public merupakan alamat IP yang digunakan untuk berkomunikasi melalui jaringan internet. IP ini harus dialokasikan melalui penyedia layanan internet atau ISP.

#### c. IP Static
IP Static merupakan alamat IP yang ditentukan secara manual dan tidak berubah secara otomatis. Biasanya digunakan pada router, server, printer jaringan, dan perangkat yang membutuhkan alamat tetap.

#### d. IP Dynamic
IP Dynamic merupakan alamat IP yang diberikan secara otomatis oleh DHCP Server. Alamat ini dapat berubah sesuai dengan pengaturan dan masa sewa (*lease time*).

### 4. Pengertian Subnet Mask dan Prefix
Subnet Mask merupakan nilai yang digunakan untuk membedakan bagian Network ID dan Host ID pada suatu IP Address. Prefix adalah penulisan singkat dari subnet mask menggunakan tanda garis miring, seperti `/24`, `/28`, dan `/30`.

| Prefix | Subnet Mask |
| :--- | :--- |
| `/24` | `255.255.255.0` |
| `/25` | `255.255.255.128` |
| `/26` | `255.255.255.192` |
| `/27` | `255.255.255.224` |
| `/28` | `255.255.255.240` |
| `/29` | `255.255.255.248` |
| `/30` | `255.255.255.252` |

Semakin besar angka prefix, semakin sedikit jumlah alamat host yang tersedia dalam satu jaringan.

### 5. Pengertian Subnetting
Subnetting merupakan proses membagi sebuah jaringan IP menjadi beberapa jaringan yang lebih kecil. Tujuannya adalah untuk mengatur penggunaan alamat IP agar lebih efisien, memisahkan jaringan berdasarkan fungsi, serta mengurangi domain broadcast.

**Rumus dasar subnetting IPv4:**
1. Jumlah bit host = $32 - \text{prefix}$
2. Jumlah seluruh alamat IP = $2^{\text{jumlah bit host}}$
3. Jumlah alamat host yang dapat digunakan = $2^{\text{jumlah bit host}} - 2$
4. **Network Address** merupakan alamat pertama dalam subnet.
5. **Broadcast Address** merupakan alamat terakhir dalam subnet.

Pengurangan dua alamat dilakukan karena satu alamat digunakan sebagai Network Address dan satu alamat lainnya digunakan sebagai Broadcast Address.

### 6. Cara Menghitung Subnetting pada Jaringan yang Digunakan
Pada praktik ini terdapat tiga jaringan yang memiliki kebutuhan berbeda, yaitu jaringan client, jaringan server, dan jaringan hotspot:
- `ether2` (Client): `10.10.2.1/28`
- `ether3` (Server): `100.100.2.1/30`
- `wlan1` (Hotspot): `192.168.2.1/24`

#### A. Subnetting IP `10.10.2.1/28` (Jaringan Client)
- Jumlah bit host: $32 - 28 = 4\text{ bit}$
- Jumlah seluruh alamat IP: $2^4 = 16\text{ alamat IP}$
- Jumlah host yang dapat digunakan: $16 - 2 = 14\text{ host}$
- Kenaikan subnet: $256 - 240 = 16$ (dimulai dari `10.10.2.0` sampai `10.10.2.15`)

| Keterangan | Hasil |
| :--- | :--- |
| **IP Router** | `10.10.2.1` |
| **Prefix** | `/28` |
| **Subnet Mask** | `255.255.255.240` |
| **Network Address** | `10.10.2.0` |
| **First Host** | `10.10.2.1` |
| **Last Host** | `10.10.2.14` |
| **Broadcast** | `10.10.2.15` |
| **Jumlah Host** | `14` |

**Mengapa menggunakan `/28`?**
Prefix `/28` dipilih karena jaringan client membutuhkan ruang alamat untuk sejumlah perangkat, tetapi tidak memerlukan kapasitas sebesar jaringan `/24`. IP `10.10.2.1` digunakan sebagai gateway dan alamat client dapat dibagikan melalui DHCP Server mulai dari `10.10.2.2` sampai `10.10.2.14`.

#### B. Subnetting IP `100.100.2.1/30` (Jaringan Server)
- Jumlah bit host: $32 - 30 = 2\text{ bit}$
- Jumlah seluruh alamat IP: $2^2 = 4\text{ alamat IP}$
- Jumlah host yang dapat digunakan: $4 - 2 = 2\text{ host}$
- Kenaikan subnet: $256 - 252 = 4$ (rentang `100.100.2.0` sampai `100.100.2.3`)

| Keterangan | Hasil |
| :--- | :--- |
| **IP Router** | `100.100.2.1` |
| **Prefix** | `/30` |
| **Subnet Mask** | `255.255.255.252` |
| **Network Address** | `100.100.2.0` |
| **First Host** | `100.100.2.1` |
| **Last Host** | `100.100.2.2` |
| **Broadcast** | `100.100.2.3` |
| **Jumlah Host** | `2` |

**Mengapa menggunakan `/30`?**
Prefix `/30` digunakan karena jaringan ini hanya menghubungkan MikroTik (`100.100.2.1/30`) dengan satu Ubuntu Server (`100.100.2.2/30`), sehingga hanya dibutuhkan tepat dua alamat IP host.

#### C. Subnetting IP `192.168.2.1/24` (Jaringan Hotspot)
- Jumlah bit host: $32 - 24 = 8\text{ bit}$
- Jumlah seluruh alamat IP: $2^8 = 256\text{ alamat IP}$
- Jumlah host yang dapat digunakan: $256 - 2 = 254\text{ host}$
- Kenaikan subnet: $256 - 0 = 256$ (rentang `192.168.2.0` sampai `192.168.2.255`)

| Keterangan | Hasil |
| :--- | :--- |
| **IP Router** | `192.168.2.1` |
| **Prefix** | `/24` |
| **Subnet Mask** | `255.255.255.0` |
| **Network Address** | `192.168.2.0` |
| **First Host** | `192.168.2.1` |
| **Last Host** | `192.168.2.254` |
| **Broadcast** | `192.168.2.255` |
| **Jumlah Host** | `254` |

**Mengapa menggunakan `/24`?**
Prefix `/24` digunakan karena jaringan hotspot dapat melayani banyak perangkat wireless (laptop dan smartphone) sekaligus.

### 7. Kesimpulan Pembagian IP Address

| Interface | Fungsi | IP Address | Kapasitas Host |
| :--- | :--- | :--- | :--- |
| `ether2` | Client | `10.10.2.1/28` | `14` |
| `ether3` | Server | `100.100.2.1/30` | `2` |
| `wlan1` | Hotspot | `192.168.2.1/24` | `254` |

---

## BAB II – KONFIGURASI MIKROTIK

### 1. Pengertian MikroTik
MikroTik merupakan perangkat jaringan yang digunakan untuk mengatur dan mengelola lalu lintas data dalam jaringan komputer. MikroTik menggunakan sistem operasi bernama **RouterOS** dan dapat dikelola melalui WinBox, WebFig, maupun Command Line Interface (CLI).

### 2. Pengertian Router
Router merupakan perangkat jaringan yang berfungsi menghubungkan dua atau lebih jaringan yang memiliki Network Address berbeda dengan menentukan jalur pengiriman paket data berdasarkan tabel routing.

### 3. Fungsi Interface pada MikroTik
- **`ether2-client`:** Menghubungkan MikroTik dengan jaringan komputer client (IP `10.10.2.1/28`).
- **`ether3-server`:** Menghubungkan MikroTik dengan Ubuntu Server (IP `100.100.2.1/30`).
- **`wlan1-hotspot`:** Jalur komunikasi wireless untuk layanan Wi-Fi pengguna hotspot (IP `192.168.2.1/24`).

### 4. Konfigurasi IP Address pada MikroTik
Dikonfigurasi melalui menu **IP → Addresses**. Setelah alamat IP dipasang, MikroTik secara otomatis mengenali jaringan yang terhubung langsung (*Dynamic Connected*) dan memasukkannya ke dalam tabel routing.

### 5. Konsep Gateway
Gateway merupakan perangkat atau alamat IP yang menjadi jalur keluar dari suatu jaringan menuju jaringan lain. Gateway harus berada dalam satu subnet dengan perangkat yang menggunakannya.

### 6. Konsep NAT (*Network Address Translation*)
NAT digunakan untuk menerjemahkan alamat IP Private lokal menjadi IP Public pada interface keluar agar perangkat lokal dapat mengakses internet. Dikonfigurasi melalui **IP → Firewall → NAT**:
- **Chain:** `srcnat`
- **Out. Interface:** Interface yang mengarah ke internet (ISP)
- **Action:** `masquerade`

---

## BAB III – DHCP SERVER

### 1. Pengertian DHCP Server
DHCP (*Dynamic Host Configuration Protocol*) Server adalah layanan jaringan yang memberikan konfigurasi IP Address, Subnet Mask, Gateway, DNS Server, dan Lease Time secara otomatis kepada perangkat client.

### 2. Cara Kerja DHCP Server (Proses DORA)
1. **Discover:** Client mengirimkan pesan broadcast untuk mencari DHCP Server.
2. **Offer:** DHCP Server menawarkan alamat IP dan konfigurasi jaringan kepada client.
3. **Request:** Client meminta untuk menggunakan alamat IP yang ditawarkan tersebut.
4. **Acknowledge:** DHCP Server mengonfirmasi pemberian alamat IP sehingga siap digunakan client.

### 3. Konfigurasi DHCP Server pada Jaringan Client
| Pengaturan | Nilai |
| :--- | :--- |
| **Interface** | `ether2-client` |
| **Gateway** | `10.10.2.1` |
| **Network** | `10.10.2.0/28` |
| **IP Pool** | `10.10.2.2–10.10.2.14` |
| **Subnet Mask** | `255.255.255.240` |

### 4. Konfigurasi DHCP Server pada Jaringan Hotspot
| Pengaturan | Nilai |
| :--- | :--- |
| **Interface** | `wlan1-hotspot` |
| **Gateway** | `192.168.2.1` |
| **Network** | `192.168.2.0/24` |
| **IP Pool** | `192.168.2.2–192.168.2.254` |
| **Subnet Mask** | `255.255.255.0` |

---

## BAB IV – HOTSPOT MIKROTIK

### 1. Pengertian Hotspot
Hotspot merupakan fitur pada MikroTik untuk menyediakan akses jaringan dengan sistem autentikasi berbasis halaman login (*captive portal*) sebelum pengguna mendapatkan akses internet.

### 2. Fungsi dan Cara Kerja Hotspot
- **Fungsi:** Autentikasi pengguna, manajemen akun, pembatasan waktu akses, pengaturan bandwidth per pengguna, dan pemantauan pengguna aktif.
- **Cara Kerja:** Perangkat terhubung ke Wi-Fi $\rightarrow$ memperoleh IP dari DHCP $\rightarrow$ membuka browser $\rightarrow$ diarahkan ke *Login Page* $\rightarrow$ memasukkan username & password $\rightarrow$ akses internet dibuka sesuai profil.

### 3. Komponen Utama Hotspot
- **Hotspot Server:** Layanan utama pengatur autentikasi pada interface tertentu (`wlan1`).
- **User Profile:** Pengaturan kebijakan akses (batas kecepatan, waktu sesi, jumlah *shared users*).
- **Hotspot User:** Akun kredensial (username dan password) milik pengguna.
- **Login Page:** Halaman portal autentikasi.
- **Active Users:** Daftar pengguna yang sedang aktif terhubung.

### 4. Perbedaan Wi-Fi Biasa dan Hotspot MikroTik

| Wi-Fi Biasa | Hotspot MikroTik |
| :--- | :--- |
| Menghubungkan perangkat secara wireless | Menyediakan akses jaringan dengan autentikasi |
| Umumnya hanya menggunakan password Wi-Fi | Menggunakan username dan password per akun |
| Tidak membutuhkan halaman login | Menggunakan halaman *captive portal* |
| Pengelolaan pengguna sederhana | Pengelolaan kecepatan & waktu lebih terperinci |

---

## BAB V – SIMPLE QUEUE MIKROTIK

### 1. Pengertian dan Fungsi Simple Queue
Simple Queue adalah fitur manajemen bandwidth pada MikroTik untuk membatasi kecepatan *download* dan *upload* berdasarkan alamat IP atau target tertentu agar pembagian koneksi merata dan stabil.

### 2. Istilah dalam Simple Queue
- **Target:** Alamat IP atau subnet yang dikenai aturan pembatasan.
- **Max Limit:** Batas kecepatan maksimum *upload* dan *download* yang diperbolehkan.
- **Upload:** Proses mengirim data dari perangkat pengguna ke jaringan/internet.
- **Download:** Proses menerima data dari internet ke perangkat pengguna.
- **Queue:** Mekanisme antrean paket data.

### 3. Contoh Konfigurasi Simple Queue
Dikonfigurasi melalui menu **Queues → Simple Queues → New**:

| Pengaturan | Nilai |
| :--- | :--- |
| **Name** | `Client-1` |
| **Target** | `10.10.2.2` |
| **Max Limit Upload** | `2 Mbps` |
| **Max Limit Download** | `5 Mbps` |

---

## BAB VI – UBUNTU SERVER

### 1. Pengertian dan Fungsi Ubuntu Server
Ubuntu Server adalah sistem operasi berbasis Linux tanpa antarmuka grafis (berbasis CLI/terminal) yang dirancang untuk menjalankan berbagai layanan jaringan secara stabil, seperti MariaDB, Apache, PHP, phpMyAdmin, dan BIND9.

### 2. Konfigurasi IP Address Ubuntu Server (Netplan)
Ubuntu Server menggunakan **Netplan** (file konfigurasi berformat `.yaml`) untuk menetapkan IP Static agar alamat server konsisten.

| Pengaturan | Nilai |
| :--- | :--- |
| **Interface Ubuntu** | `enp0s3` |
| **IP Address** | `100.100.2.2/30` |
| **Network** | `100.100.2.0` |
| **Gateway** | `100.100.2.1` |
| **Subnet Mask** | `255.255.255.252` |

### 3. SSH dan Pengujian Koneksi
- **SSH (*Secure Shell*):** Protokol untuk meremote dan mengelola server dari jarak jauh secara terenkripsi.
- **Pengujian Koneksi:** Menggunakan perintah `ping` dari MikroTik ke `100.100.2.2`, dari Ubuntu Server ke `100.100.2.1`, serta dari client ke server.

---

## BAB VII – INSTALASI DAN KONFIGURASI DATABASE MARIADB

### 1. Pengertian MariaDB
MariaDB adalah sistem manajemen basis data relasional (*RDBMS*) bersifat *open source* yang menggunakan bahasa SQL (*Structured Query Language*) untuk menyimpan, mencari, dan mengelola data aplikasi web.

### 2. Struktur Dasar Database
- **Database:** Wadah penyimpanan kumpulan tabel (contoh: database `ukom2`).
- **Table:** Tempat penyimpanan data dalam bentuk baris dan kolom.
- **Field:** Kolom yang mendefinisikan atribut data (contoh: `id`, `judul`, `isi`, `tanggal`).
- **Record:** Satu baris data lengkap yang tersimpan di dalam tabel.

### 3. Tahapan Instalasi dan Pengujian
1. Memperbarui daftar paket repositori Ubuntu (`apt update`).
2. Menginstal paket `mariadb-server`.
3. Memeriksa status layanan dan menjalankan pengamanan awal (`mysql_secure_installation`).
4. Membuat database, membuat user khusus beserta hak aksesnya, serta membuat tabel dan memasukkan data pengujian.

---

## BAB VIII – INSTALASI WEB SERVER APACHE DAN PHP

### 1. Pengertian Apache Web Server
Apache HTTP Server adalah perangkat lunak web server yang menerima permintaan HTTP/HTTPS dari browser client dan mengirimkan halaman website sebagai respons. Apache mendukung fitur **VirtualHost** untuk menjalankan banyak domain dalam satu server.

### 2. Pengertian dan Fungsi PHP
PHP (*Hypertext Preprocessor*) adalah bahasa pemrograman *server-side scripting* yang digunakan untuk menghasilkan konten web dinamis, memproses formulir HTML, mengelola sesi login, serta menghubungkan halaman website dengan database MariaDB.

### 3. Tahapan Instalasi Apache & PHP
1. Memperbarui repositori sistem.
2. Menginstal paket `apache2`, `php`, dan modul `libapache2-mod-php`.
3. Memeriksa status layanan Apache dan menguji akses halaman default melalui browser di alamat `http://100.100.2.2`.

---

## BAB IX – INSTALASI PHPMyADMIN DAN DNS SERVER (BIND9)

### 1. Pengertian phpMyAdmin
phpMyAdmin adalah aplikasi berbasis web yang ditulis dengan PHP untuk mengelola database MariaDB/MySQL secara visual melalui browser (membuat tabel, mengedit record, serta *import/export* file `.sql`).

### 2. Pengertian DNS Server (BIND9)
DNS (*Domain Name System*) Server menerjemahkan nama domain yang mudah diingat manusia (misalnya `ujikom.local`) menjadi alamat IP Address (`100.100.2.2`). **BIND9** adalah perangkat lunak DNS Server standar di lingkungan Linux yang mengatur zona *forward* (domain ke IP) dan zona *reverse* (IP ke domain).

---

## RANGKUMAN ARSITEKTUR KESELURUHAN PRAKTIK

```text
+-------------------------------------------------------+
|                    MIKROTIK ROUTER                    |
|  - Gateway & Routing Pusat                            |
|  - NAT Masquerade (Internet Sharing)                  |
|  - Hotspot & Simple Queue Management                  |
+-----------+-----------------------+-------------------+
            |                       |
            | (ether2)              | (ether3)
            v                       v
    +---------------+       +------------------+
    | CLIENT        |       | UBUNTU SERVER    |
    | IP: 10.10.2.0 |       | IP: 100.100.2.2  |
    | (DHCP / /28)  |       | - MariaDB        |
    +---------------+       | - Apache & PHP   |
                            | - phpMyAdmin     |
                            | - BIND9 (DNS)    |
                            +------------------+
```