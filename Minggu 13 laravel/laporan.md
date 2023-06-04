# Laporan Minggu 13
![](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)
![](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![](https://img.shields.io/badge/VirtualBox-21416b?style=for-the-badge&logo=VirtualBox&logoColor=white)
![](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![](https://img.shields.io/badge/Apache-D22128?style=for-the-badge&logo=Apache&logoColor=white)
![](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)

## Instalasi Composer dan Laravel

## Kelompok 5
1. Emha Aji Putra Zaman (3121600042)
2. Marits Ikmal Yasin (3121600047)
3. Rizka Dwi Fajriyah (3121600050)

#
1. Install Composer dengan curl :
   ```
   curl -sS https://getcomposer.org/installer | php
   ```
2. Jalankan perintah ini secara berurutan agar composer dapat dijalankan secara global :
   ```
   sudo mv composer.phar /usr/local/bin/composer
   ```
   ```
   sudo chmod +x /usr/local/bin/composer
   ```
3. Cek apakah composer sudah terinstall dengan perintah :
   ```
   composer
   ```
   Gambar : <br>
   ![](image/composer.png)
4. Install Laravel :
   ```
   composer create-project laravel/laravel kelompok_5
   ```
5. Jalankan laravel dengan perintah :
   ```
   php artisan serve --host=IP --port=8000
   ```
   Di sini menggunakan IP : 192.168.5.21
   Gambar : <br>
   ![](image/laravel_work.png)
6.  Setting allow-transfer :
   ```
   sudo nano /etc/bind/named.conf.local
   ```
   Gambar : <br>
   ![](image/allow-transfer.png)
