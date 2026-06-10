# Laporan Workshop Administrasi dan Jaringan
## Web Service Docker

<br>

<div align="center">
  <img src="assets/logo.png" alt="Logo PENS" width="400">
</div>

<br>

| Disusun Oleh                     |              |
| -------------------------------- | ------------ |
| Rizal Maulana Airlangga          | - 3124600033 |
| Muhammad Fajrul Fatih Abul 'Ilmi | - 3124600040 |
| Nur Aini Agusthina               | - 3124600050 |

| Kelas        | 2 S.Tr. Teknik Informatika B  |
| ------------ | ----------------------------- |
| **Kelompok** | **B4**                        |


<br>

### Dosen Pengampu
**Dr. Ferry Astika Saputra, S.T., M.Sc.**

<br>

## PROGRAM STUDI D4 TEKNIK INFORMATIKA
## DEPARTEMEN TEKNIK INFORMATIKA DAN KOMPUTER
## POLITEKNIK ELEKTRONIKA NEGERI SURABAYA
## 2026

<br><br>

# Pre-Lab
1. Apa keuntungan menjalankan web server di container dibandingkan langsung di host?
> Container memberikan isolasi environment, deployment lebih konsisten, mudah dipindahkan antar server, dan dependency aplikasi tidak bercampur dengan host.
2. Jelaskan perbedaan document root Apache vs Nginx.
> Apache menggunakan default document root /usr/local/apache2/htdocs/, sedangkan Nginx menggunakan /usr/share/nginx/html/. Folder tersebut adalah lokasi file website yang dilayani web server.
3. Apa itu SSL Termination dan mengapa dilakukan di reverse proxy?
> SSL Termination adalah proses decrypt HTTPS di reverse proxy sebelum request diteruskan ke backend. Hal ini mempermudah manajemen sertifikat dan mengurangi beban backend.
4. Apa perbedaan name-based dan IP-based virtual hosting?
> Name-based virtual hosting menggunakan satu IP untuk banyak domain berdasarkan hostname. IP-based virtual hosting menggunakan IP berbeda untuk setiap website.
5. Mengapa self-signed certificate menghasilkan warning di browser?
>Karena sertifikat ditandatangani sendiri dan tidak diverifikasi oleh Certificate Authority terpercaya.

<br>

# Screenshot Wajib
## docker compose ps: 4 Service Running
## curl -k https://site1.lab:8443: Halaman Site 1
![](assets/modul-3/1.png)
![](assets/modul-3/2.png)
## curl -k https://site2.lab:8443: Halaman Site 2
![](assets/modul-3/3.png)
## curl -I http://site1.lab:8080: HTTP → HTTPS Redirect 301
![](assets/modul-3/4.png)
## openssl s_client output: Detail Certificate
![](assets/modul-3/5-1.png)
![](assets/modul-3/5-2.png)
![](assets/modul-3/5-3.png)
## curl -k https://app.lab:8443/api/health: JSON database connected
![](assets/modul-3/6-1.png)
![](assets/modul-3/6-2.png)
## POST /api/visitors: Response 201
![](assets/modul-3/7-1.png)
![](assets/modul-3/7-2.png)
## GET /api/visitors: Daftar Visitor
![](assets/modul-3/8.png)
## Log nginx per-site
![](assets/modul-3/9.png)
## Log Apache per-site
![](assets/modul-3/10.png)

<br>

# Post-Lab
1. Bandingkan response header dari Apache vs Nginx. Header apa yang menunjukkan software web server?
> Perbedaan terlihat pada header:
> - nginx (host atau browser) Server: nginx/1.29.8 menunjukkan request ditangani nginx (reverse proxy)
> ![](assets/modul-3/post-1-1.png)
> ![](assets/modul-3/post-1-2.png)
> - Apache (lokal) Server: Apache/2.4.66 (Unix) menunjukkan backend yang digunakan adalah Apache.
> ![](assets/modul-3/post-1-3.png)
> Header yang menunjukkan software web server adalah Server, karena berisi informasi jenis dan versi web server yang menangani request.
2. Jika Nginx proxy down, apakah Apache masih bisa diakses langsung? Bagaimana cara testnya?
> ![](assets/modul-3/post-2-1.png)
> Tidak bisa diakses langsung, namun bisa diakses melalui Apache
> ![](assets/modul-3/post-2-2.png)
3. Tunjukkan bahwa X-Real-IP header diteruskan dengan benar dari Nginx ke Flask.
> ![](assets/modul-3/post-3.png)
> Header X-Real-IP berhasil diteruskan dan flask menerima IP dari client melalui nginx.
4. Jelaskan mengapa Flask app perlu terhubung ke dua network (web-net dan db-net).
> Flask butuh 2 network, karena satu untuk menerima request dari nginx (web layer) dan satu lagi untuk mengakses database (data layer). Ini bertujuan untuk memisahkan traffic agar lebih aman dan terstruktur.
5. Apa yang terjadi jika file server.key atau server.crt dihapus saat container running?
> ![](assets/modul-3/post-5.png)
> Jika file server.key atau server.crt dihapus saat container sedang berjalan, layanan HTTPS masih dapat berjalan sementara. Namun, jika container di-restart, maka nginx gagal memulai layanan HTTPS karena file sertifikat tidak ditemukan.