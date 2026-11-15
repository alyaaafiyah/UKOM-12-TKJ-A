# 🛠️ Solusi Troubleshooting NSLOOKUP: Memperbaiki Status `Server: Unknown` pada BIND9

Dokumentasi ini berisi panduan lengkap langkah demi langkah untuk mengatasi masalah status **`Server: Unknown`** saat melakukan pengujian DNS menggunakan perintah `nslookup` pada Ubuntu Server (BIND9) dan *client* Windows.

---

## 📋 Daftar Isi

- [Bagian I – Tahapan Utama Perbaikan](#bagian-i--tahapan-utama-perbaikan)
- [Bagian II – Troubleshooting / Pemecahan Masalah](#bagian-ii--troubleshooting--pemecahan-masalah)
- [Bagian III – Checklist Pengujian Akhir](#bagian-iii--checklist-pengujian-akhir)
- [Kesimpulan](#kesimpulan)

---

## Bagian I – Tahapan Utama Perbaikan

Bagian ini menjelaskan langkah-langkah untuk mengatasi status `Server: Unknown` pada pengujian DNS menggunakan BIND9.

### STEP 1 – Memeriksa Konfigurasi Forward Zone dan Reverse Zone

Buka file konfigurasi zona DNS menggunakan perintah:

```bash
sudo nano /etc/bind/named.conf.local
```

Pastikan konfigurasi di dalamnya sesuai:

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

**Penjelasan:**
* File `named.conf.local` digunakan untuk mendaftarkan zona DNS yang dikelola BIND9.
* **Forward Zone** menerjemahkan nama domain menjadi IP Address.
* **Reverse Zone** menerjemahkan IP Address menjadi nama domain.
* `type master` menunjukkan bahwa server bertindak sebagai DNS Master.
* `file` menentukan lokasi file database masing-masing zona.

> Simpan perubahan dengan menekan **`CTRL + O`**, **`Enter`**, kemudian **`CTRL + X`**.

---

### STEP 2 – Memperbaiki Konfigurasi Forward Zone

Buka file Forward Zone:

```bash
sudo nano /etc/bind/db.ukomtkj-2
```

Sesuaikan konfigurasi menjadi:

```bind
$TTL 604800
@   IN  SOA ukomtkj-2.sch.id. root.ukomtkj-2.sch.id. (
        2026111201
        604800
        86400
        2419200
        604800
)

@       IN  NS  ukomtkj-2.sch.id.
@       IN  A   100.100.2.2
www     IN  A   100.100.2.2
```

**Penjelasan:**
* **Forward Zone** menghubungkan nama domain dengan alamat IP DNS Server.
* **`SOA`**: Menyimpan informasi utama pengelolaan zona DNS.
* **`NS`**: Menentukan DNS Server yang bertanggung jawab terhadap zona.
* **`A`**: Menghubungkan nama domain dengan alamat IPv4.
* **`@`**: Mewakili nama domain utama (`ukomtkj-2.sch.id`).
* **`www`**: Subdomain yang dapat diakses melalui `www.ukomtkj-2.sch.id`.
* **Serial `2026111201`**: Nomor versi zona. Setiap kali mengubah isi file zona, naikkan angka serialnya.

> Simpan perubahan dengan menekan **`CTRL + O`**, **`Enter`**, kemudian **`CTRL + X`**.

---

### STEP 3 – Memperbaiki Konfigurasi Reverse Zone

Buka file Reverse Zone:

```bash
sudo nano /etc/bind/db.100.100.2
```

Sesuaikan konfigurasi menjadi:

```bind
$TTL 604800
@   IN  SOA ukomtkj-2.sch.id. root.ukomtkj-2.sch.id. (
        2026111201
        604800
        86400
        2419200
        604800
)

@   IN  NS  ukomtkj-2.sch.id.
2   IN  PTR ukomtkj-2.sch.id.
```

*(Catatan: Pastikan angka oktet terakhir pada baris `PTR` sesuai dengan IP DNS Server Anda, yaitu `2` untuk `100.100.2.2`, atau `3` jika IP server adalah `100.100.2.3`).*

**Penjelasan:**
* **Reverse Zone** digunakan untuk menerjemahkan IP Address menjadi nama domain melalui **PTR Record**.
* Angka `2` merupakan oktet terakhir dari IP `100.100.2.2`, sedangkan nama zona `2.100.100.in-addr.arpa` mewakili tiga oktet sebelumnya.
* Jika digabungkan, alamat Reverse DNS menjadi: `2.2.100.100.in-addr.arpa`, yang mengarahkan IP `100.100.2.2` ke nama domain `ukomtkj-2.sch.id`.

> Simpan perubahan dengan **`CTRL + O`**, **`Enter`**, kemudian **`CTRL + X`**. *(Jangan lupa menaikkan nomor serial SOA setiap kali isi file zona diubah).*

---

### STEP 4 – Memeriksa Konfigurasi DNS

Setelah Forward Zone dan Reverse Zone diperbaiki, lakukan pemeriksaan sintaks sebelum memuat ulang layanan BIND9.

**A. Memeriksa konfigurasi utama BIND9**
```bash
sudo named-checkconf
```
*Digunakan untuk memeriksa kesalahan sintaks pada konfigurasi utama BIND9. Jika tidak ada kesalahan, terminal akan kembali ke prompt tanpa menampilkan pesan error.*

**B. Memeriksa Forward Zone**
```bash
sudo named-checkzone ukomtkj-2.sch.id /etc/bind/db.ukomtkj-2
```
Hasil yang diharapkan:
```text
zone ukomtkj-2.sch.id/IN: loaded serial 2026111201
OK
```

**C. Memeriksa Reverse Zone**
```bash
sudo named-checkzone 2.100.100.in-addr.arpa /etc/bind/db.100.100.2
```
Hasil yang diharapkan:
```text
zone 2.100.100.in-addr.arpa/IN: loaded serial 2026111201
OK
```

---

### STEP 5 – Memuat Ulang Layanan BIND9

Setelah seluruh konfigurasi berhasil diperiksa, mulai ulang layanan BIND9:

```bash
sudo systemctl restart bind9.service
```

Selanjutnya, periksa status layanan:

```bash
sudo systemctl status bind9.service
```

Pastikan muncul keterangan:
```text
Active: active (running)
```
> *Tekan **`Q`** untuk keluar dari tampilan status.*

---

### STEP 6 – Mengatur DNS Server pada Laptop Windows

Setelah konfigurasi Ubuntu Server selesai, arahkan pengaturan DNS pada laptop Windows ke DNS Server yang telah dibuat:

1. Buka **Control Panel**.
2. Pilih **Network and Internet** > **Network and Sharing Center**.
3. Klik **Change adapter settings**.
4. Klik kanan pada adapter jaringan yang sedang digunakan, lalu pilih **Properties**.
5. Pilih **Internet Protocol Version 4 (TCP/IPv4)**, kemudian klik **Properties**.
6. Pilih **Use the following DNS server addresses**, lalu masukkan:

| Kolom | Alamat |
| :--- | :--- |
| **Preferred DNS server** | `100.100.2.2` |
| **Alternate DNS server** | *(Kosongkan dahulu)* |

7. Klik **OK**, kemudian **Close**.

---

### STEP 7 – Membersihkan Cache DNS Windows

Buka **Command Prompt (CMD)** sebagai Administrator, lalu jalankan perintah berikut untuk menghapus *cache* DNS lama:

```cmd
ipconfig /flushdns
```

Periksa konfigurasi jaringan untuk memastikan DNS Server sudah terpasang:

```cmd
ipconfig /all
```

Pastikan pada adapter jaringan yang aktif tertera:
```text
DNS Servers . . . . . . . . . . : 100.100.2.2
```

---

### STEP 8 – Melakukan Pengujian NSLOOKUP

Lakukan pengujian menggunakan perintah `nslookup` di CMD Windows:

**A. Pengujian dengan menentukan IP DNS Server**
```cmd
nslookup ukomtkj-2.sch.id 100.100.2.2
```

**B. Pengujian tanpa menambahkan IP DNS Server**
```cmd
nslookup ukomtkj-2.sch.id
```

> *Jika kedua pengujian berhasil dan bagian **`Server`** menampilkan nama domain (`ukomtkj-2.sch.id`), maka masalah `Server: Unknown` telah berhasil diatasi.*

---

## Bagian II – Troubleshooting / Pemecahan Masalah

Gunakan panduan di bawah ini apabila hasil pengujian masih belum sesuai atau muncul pesan kesalahan.

### 1. Server Masih Menampilkan `Unknown`
* **Gejala:**
  ```text
  Server:  Unknown
  Address:  100.100.2.2
  ```
* **Kemungkinan Penyebab:** PTR Record belum dikonfigurasi dengan benar, Reverse Zone gagal dimuat, atau penulisan domain pada PTR Record salah.
* **Solusi:**
  1. Periksa zona Reverse:
     ```bash
     sudo named-checkzone 2.100.100.in-addr.arpa /etc/bind/db.100.100.2
     ```
  2. Uji Reverse DNS secara lokal di server:
     ```bash
     dig @127.0.0.1 -x 100.100.2.2
     ```
  3. Pastikan baris PTR di `/etc/bind/db.100.100.2` mengarah ke `ukomtkj-2.sch.id.` (diakhiri tanda titik). Naikkan nomor serial SOA, lalu jalankan `sudo systemctl restart bind9.service`.

---

### 2. Forward DNS Berhasil, tetapi Reverse DNS Gagal
* **Gejala:** Domain berhasil diterjemahkan ke IP Address, tetapi nama DNS Server tidak dikenali.
* **Solusi:** Jalankan pengujian PTR langsung dari CMD Windows:
  ```cmd
  nslookup -type=PTR 2.2.100.100.in-addr.arpa 100.100.2.2
  ```
  Jika belum mengembalikan nama domain, periksa kembali oktet IP dan penulisan domain pada file `/etc/bind/db.100.100.2`.

---

### 3. NSLOOKUP Mengalami `Request Timed Out`
* **Gejala:**
  ```text
  DNS request timed out.
  timeout was 2 seconds.
  ```
* **Kemungkinan Penyebab:** Layanan BIND9 mati, port 53 tertutup, atau terblokir *firewall*.
* **Solusi:**
  * Cek status BIND9: `sudo systemctl status bind9.service`
  * Cek port 53: `sudo ss -tulpn | grep ':53'`
  * Uji koneksi dari Windows: `ping 100.100.2.2` *(Catatan: Jika ping gagal karena ICMP diblokir firewall, tetap lakukan pengujian port/DNS secara langsung).*

---

### 4. NSLOOKUP Masih Menggunakan DNS Server Lain
* **Gejala:** Saat menjalankan `nslookup` tanpa IP server, bagian `Server` masih menampilkan DNS router/ISP lain.
* **Solusi:** Jalankan `ipconfig /all` di Windows dan pastikan `DNS Servers` hanya berisi `100.100.2.2`. Jika menggunakan DHCP, pastikan pengaturan DHCP Server tidak membagikan alamat DNS lain.

---

### 5. BIND9 Gagal Berjalan Setelah Konfigurasi Diubah
* **Gejala:** Status layanan menunjukkan `Active: failed`.
* **Solusi:**
  1. Cek kesalahan sintaks dengan `sudo named-checkconf`, `sudo named-checkzone` untuk Forward dan Reverse Zone.
  2. Lihat log detail *error* layanan BIND9:
     ```bash
     sudo journalctl -u bind9.service -n 30 --no-pager
     ```
  3. Perbaiki baris yang salah, lalu jalankan `sudo systemctl restart bind9.service`.

---

### 6. DNS Server Tidak Menerima Permintaan dari Laptop
* **Gejala:** DNS berhasil diuji di dalam Ubuntu Server, tetapi gagal saat diakses dari laptop *client*.
* **Solusi:**
  1. Buka `/etc/bind/named.conf.options` dan pastikan `listen-on` menyertakan IP server (`100.100.2.2`) serta `allow-query` mengizinkan jaringan *client*.
  2. Jika firewall UFW aktif, izinkan akses port 53 (UDP dan TCP):
     ```bash
     sudo ufw allow from 100.100.2.0/28 to any port 53 proto udp
     sudo ufw allow from 100.100.2.0/28 to any port 53 proto tcp
     ```

---

## Bagian III – Checklist Pengujian Akhir

Sebelum menyatakan konfigurasi selesai, pastikan seluruh poin berikut telah terpenuhi:

- [x] Forward Zone sudah terdaftar pada `named.conf.local`.
- [x] Reverse Zone sudah terdaftar pada `named.conf.local`.
- [x] A Record mengarah ke IP `100.100.2.2`.
- [x] PTR Record mengarah ke `ukomtkj-2.sch.id.`.
- [x] `named-checkconf` tidak menghasilkan error.
- [x] Pemeriksaan Forward Zone menghasilkan `OK`.
- [x] Pemeriksaan Reverse Zone menghasilkan `OK`.
- [x] Layanan BIND9 berstatus `active (running)`.
- [x] Laptop dapat menjangkau DNS Server `100.100.2.2`.
- [x] DNS Server pada Windows menunjukkan IP `100.100.2.2`.
- [x] Pengujian `nslookup` dengan tambahan IP berhasil.
- [x] Pengujian `nslookup` tanpa tambahan IP berhasil.
- [x] Bagian `Server` menampilkan nama domain, bukan `Unknown`.

---

## Kesimpulan

Permasalahan **`Server: Unknown`** pada pengujian DNS menggunakan perintah `nslookup` dapat diatasi dengan memastikan konfigurasi **Reverse DNS** berjalan dengan baik, khususnya melalui penambahan **PTR Record** yang menghubungkan alamat IP DNS Server dengan nama domainnya. Selain itu, konfigurasi Forward DNS dan pengaturan alamat DNS pada komputer *client* harus diarahkan dengan tepat ke IP DNS Server.

Setelah seluruh tahapan perbaikan dilakukan, pengujian dengan perintah:
```cmd
nslookup ukomtkj-2.sch.id
```
akan menampilkan nama domain DNS Server pada bagian `Server` beserta alamat IP `100.100.2.2` pada bagian `Address`.