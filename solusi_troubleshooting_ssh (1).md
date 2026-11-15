# 🔐 Solusi Troubleshooting SSH (Ubuntu Server & Windows Client)

![Ilustrasi CEO Alya Afiyah](assets/ilustrasi_ceo_alya.jpg)

> **Catatan:** Pastikan Anda menyimpan file gambar ilustrasi ke dalam folder `assets/` di repositori GitHub Anda dengan nama `ilustrasi_ceo_alya.jpg` (atau sesuaikan *path* gambarnya di atas agar tampil di halaman GitHub).

Dokumentasi ini berisi panduan lengkap konfigurasi **SSH (Secure Shell) Server** pada Ubuntu, pengujian koneksi dari *client* Windows menggunakan Command Prompt, serta solusi mengatasi berbagai kendala (*troubleshooting*) yang sering terjadi.

> **Info Jaringan:** Pada panduan ini, Ubuntu Server bertindak sebagai SSH Server dengan alamat IP `100.100.2.x` (sesuaikan `x` dengan IP server Anda) dan Windows bertindak sebagai *client*.

---

## 📋 Daftar Isi
- [A. Konfigurasi di Ubuntu Server](#a-konfigurasi-di-ubuntu-server)
- [B. Pengujian SSH dari Windows](#b-pengujian-ssh-dari-windows)
- [C. Solusi Troubleshooting SSH](#c-solusi-troubleshooting-ssh)

---

## A. Konfigurasi di Ubuntu Server

### 1. Memeriksa Alamat IP Ubuntu
Periksa informasi *interface* jaringan untuk memastikan alamat IP yang digunakan oleh Ubuntu Server:
```bash
ip a
```
*Pastikan server sudah mendapatkan alamat IP (misalnya: `100.100.2.x`).*

### 2. Memperbarui Daftar Paket
Perbarui daftar paket repositori agar sistem mendapatkan informasi paket terbaru sebelum instalasi:
```bash
sudo apt update
```

### 3. Menginstal OpenSSH Server
Instal paket `openssh-server` agar Ubuntu dapat menerima koneksi SSH dari komputer *client*:
```bash
sudo apt install openssh-server -y
```

### 4. Membuat SSH Host Key
Buat *SSH Host Key* yang berfungsi sebagai identitas kriptografi server saat menerima koneksi dari *client*:
```bash
sudo ssh-keygen -A
```

### 5. Mengaktifkan Layanan SSH
Aktifkan layanan SSH agar berjalan otomatis saat *booting* dan mulai ulang layanan untuk menerapkan konfigurasi:
```bash
sudo systemctl enable ssh
sudo systemctl restart ssh
```

### 6. Memeriksa Status SSH
Pastikan layanan SSH sudah aktif dan berjalan dengan baik:
```bash
sudo systemctl status ssh
```
*Pastikan muncul keterangan **`active (running)`**. Tekan tombol **`Q`** pada keyboard untuk keluar dari tampilan status.*

### 7. Memeriksa Port SSH
Pastikan layanan SSH sudah berjalan dan mendengarkan (*listening*) pada port default **22**:
```bash
sudo ss -tlnp | grep :22
```
*Pastikan output menampilkan status **`LISTEN`**.*

### 8. Mengatur Firewall (UFW)
Jika *firewall* UFW aktif, izinkan lalu lintas SSH melalui port 22:
```bash
sudo ufw allow 22/tcp
```
Periksa kembali status dan aturan *firewall*:
```bash
sudo ufw status
```
*Pastikan aturan untuk port `22/tcp` berstatus **ALLOW**.*

---

## B. Pengujian SSH dari Windows

### 1. Membuka Command Prompt (CMD)
Buka aplikasi **Command Prompt (CMD)** atau **PowerShell** pada komputer Windows yang digunakan sebagai *client*.

### 2. Menguji Koneksi Jaringan
Lakukan *ping* ke alamat IP Ubuntu Server untuk memastikan kedua perangkat saling terhubung dalam jaringan:
```cmd
ping 100.100.2.x
```
*Jika muncul balasan `Reply from 100.100.2.x`, berarti koneksi jaringan antara Windows dan Ubuntu berhasil.*

### 3. Menghapus Host Key Lama (Opsional)
Jika sebelumnya pernah terhubung ke IP yang sama namun sistem server baru saja diinstal ulang, hapus catatan *Host Key* lama agar tidak terjadi konflik:
```cmd
ssh-keygen -R 100.100.2.x
```

### 4. Menghubungkan Windows ke Ubuntu
Jalankan perintah berikut untuk memulai sesi SSH (ganti `alya` dengan *username* Ubuntu Anda):
```cmd
ssh alya@100.100.2.x
```

### 5. Mengonfirmasi Host Key Baru
Saat pertama kali terhubung atau setelah *Host Key* berubah, akan muncul pesan konfirmasi keamanan berikut:
```text
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
Ketik **`yes`**, lalu tekan **Enter** untuk menyimpan identitas server.

### 6. Memasukkan Password Ubuntu
Masukkan *password* akun Ubuntu Anda, lalu tekan **Enter**.
> *Catatan: Demi keamanan, karakter password memang tidak akan terlihat di layar terminal saat diketik.*

### 7. Memastikan Koneksi Berhasil
Jika proses koneksi berhasil, tampilan *prompt* CMD akan berubah menjadi terminal Ubuntu seperti berikut:
```text
alya@ubuntu:~$
```

---

## C. Solusi Troubleshooting SSH

Berikut adalah daftar pesan *error* yang umum muncul beserta langkah penyelesaiannya:

| No | Pesan Error | Penyebab Umum | Solusi / Langkah Perbaikan |
| :--- | :--- | :--- | :--- |
| **1** | `Connection refused` | Layanan SSH belum aktif atau port 22 tertutup. | Jalankan `sudo systemctl status ssh` di Ubuntu. Pastikan statusnya `active (running)` dan port 22 dalam keadaan `LISTEN`. |
| **2** | `Host key verification failed` | Identitas *Host Key* server berubah dan tidak cocok dengan catatan di Windows. | Jalankan perintah `ssh-keygen -R 100.100.2.x` di CMD Windows untuk menghapus *key* lama, lalu coba hubungkan kembali. |
| **3** | `Permission denied` | Kesalahan pada *username* atau *password* yang dimasukkan. | Periksa kembali penulisan *username* (perhatikan huruf besar/kecil) dan *password* akun Ubuntu Anda. |
| **4** | `Connection timed out` | Koneksi terblokir *firewall* atau salah alamat IP/jaringan. | Pastikan kedua perangkat satu jaringan, cek ulang IP Ubuntu dengan `ip a`, dan pastikan UFW mengizinkan port `22/tcp`. |
| **5** | `No hostkeys available` | File *Host Key* belum dibuat atau hilang di server. | Jalankan `sudo ssh-keygen -A` pada Ubuntu untuk membuat *key* baru, lalu jalankan `sudo systemctl restart ssh`. |

> **Tips Tambahan:** Jika Ubuntu Server menggunakan IP dinamis (DHCP), pastikan untuk selalu mengecek ulang alamat IP terbaru menggunakan perintah `ip a` sebelum melakukan koneksi dari Windows.