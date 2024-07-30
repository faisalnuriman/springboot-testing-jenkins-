# Spring Boot CI/CD Pipeline

## Deskripsi
Proyek ini menggunakan Jenkins untuk mengautomasi pipeline CI/CD. Pipeline ini mencakup proses build, test, dan deployment menggunakan Docker.

## Struktur Proyek
- **Dockerfile**: Konfigurasi untuk membangun image Docker.
- **Jenkinsfile**: File pipeline Jenkins yang mendefinisikan proses CI/CD.
- **src/**: Kode sumber aplikasi Spring Boot.
- **pom.xml**: File konfigurasi Maven untuk manajemen dependensi.

## Jenkins Pipeline

Pipeline Jenkins dikonfigurasi sebagai `Multibranch Pipeline` untuk proyek ini. Pipeline secara otomatis menjalankan build, test, dan deploy setiap kali ada perubahan pada cabang `main`.

### Konfigurasi Webhook
- **Webhook URL**: [URL Webhook Jenkins]
- **Event yang Dipicu**: Push ke cabang `main`

### Proses Pipeline
1. **Build**: Menggunakan Maven untuk membersihkan dan membangun aplikasi.
2. **Test**: Menjalankan unit tests.
3. **Build Docker Image**: Membangun image Docker untuk aplikasi.
4. **Deploy**: Menjalankan container Docker dan mengekspos port 8081.

### Polling SCM
Jenkins juga dikonfigurasi untuk memeriksa perubahan pada repositori setiap 5 menit.

## Cara Menambahkan Perubahan
1. Lakukan perubahan pada proyek Anda.
2. Commit perubahan menggunakan:
    ```bash
    git add .
    git commit -m "Deskripsi perubahan"
    git push origin main
    ```
3. Jenkins akan otomatis mendeteksi perubahan dan menjalankan pipeline.

## Troubleshooting
- Jika pipeline tidak berjalan otomatis, pastikan webhook di GitHub sudah benar.
- Periksa konfigurasi polling SCM di Jenkins jika menggunakan polling.

## Referensi
- [Dokumentasi Jenkins](https://www.jenkins.io/doc/)
- [Panduan Docker](https://docs.docker.com/get-started/)

