# Laporan Minggu 16
![](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)
![](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![](https://img.shields.io/badge/VirtualBox-21416b?style=for-the-badge&logo=VirtualBox&logoColor=white)


## Mail Server

## Kelompok 5
1. Emha Aji Putra Zaman (3121600042)
2. Marits Ikmal Yasin (3121600047)
3. Rizka Dwi Fajriyah (3121600050)

Pastikan sebelum kita melakukan konfigurasi kita sudah menambahkan konfigurasi MX pada db.kampus-05.takehome.com
<br>
![](gambar/dbkampus-05-mx.PNG)
### **Konfigurasi Postfix dan Dovecot**
#
Sebelum memulai install mail server, ada baiknya siapkan domain khusus yang akan digunakan untuk konfigurasi mail server. Dalam contoh konfigurasi kali ini akan menggunakan nama domain mail.kampus-05.takehome.com yang dibuat menggunakan bind9 secara lokal. 
1. Update repository dan install package postfix.
    ```
    apt update
    apt install postfix dovecot-imapd dovecot-pop3d
    ```
2. **Konfigurasi Postfix** <br>
    Setelah installasi selesai akan muncul message box, kemudian pilih internet site agar komunikasi email menggunakan protokol SMTP secara langsung.<br>
    <img src="./gambar/1.png"/><br>
    Selanjutnya masukkan nama domain yang akan digunakan.
    <img src="./gambar/2.png"/><br>
    Setelah itu, postfix akan menyelesaikan installasinya. Setelah Installasi selesai, edit file di /etc/postfix/main.cf dan tambahkan home_mailbox = Maildir/ pada baris paling bawah.
    ```
    sudo nano /etc/postfix/main.cf
    ```

    ```
    inet_interfaces = all
    inet_protocols = all

    #tambahkan pada baris mynetworks
    0.0.0.0/0 

    #Berikan comment bagian relayhost
    #relayhost =

    #tambahkan baris berikut pada baris paling bawah
    home_mailbox = Maildir/
    ```

    buat mail directory di directory /etc/skel
    ```
    maildirmake.dovecot /etc/skel/Maildir
    ```

    Setelah itu masukkan perintah berikut
    ```
    dpkg-reconfigure postfix
    ```
    Pilih beberapa pilihan dan isikan beberapa input yang akan muncul, sesuaikan dengan topology/konfigurasi sistem dan kebutuhan.
    <img src="./gambar/1.png"/><br>
    <img src="./gambar/2.png"/><br>
    <img src="./gambar/3.png"/><br>
    <img src="./gambar/4.png"/><br>
    <img src="./gambar/5.png"/><br>
    <img src="./gambar/6.png"/><br>
    <img src="./gambar/7.png"/><br>
    <img src="./gambar/8.png"/><br>
    <img src="./gambar/9.png"/><br>

    Restart postfix service.
    ```
    systemctl restart postfix
    ```

3. **Konfigurasi Dovecot** <br>
   Edit file konfigurasi /etc/dovecot/dovecot.conf.
   ```
    sudo nano /etc/dovecot/dovecot.conf
    ```

    Uncomment dan edit baris berikut.
    ```
    ...
    # If you want to specify non-default ports or anything more complex,
    # edit conf.d/master.conf.
    listen = *
    ...
    ```
    Edit file konfigurasi /etc/dovecot/conf.d/10-auth.conf.
    ```
    sudo nano /etc/dovecot/conf.d/10-auth.conf
    ```
    Uncomment dan ganti dari yes ke no.
     ```
    ...
    # connection is considered secure and plaintext authentication is allowed.
    # See also ssl=required setting.
    disable_plaintext_auth = no
    ...
    ```

    Edit file konfigurasi /etc/dovecot/conf.d/10-mail.conf.
     ```
    sudo nano /etc/dovecot/conf.d/10-mail.conf
    ```
    Uncomment pada baris berikut.
     ```
    ...
    mail_location = maildir:~/Maildir
    ...
    ```
    Beri comment pada baris berikut.
     ```
    ...
    # mail_location = mbox:~/mail:INBOX=/var/mail/%u
    ...
    ```

    Restart dovecot service.
     ```
    systemctl restart dovecot
    ```


    
### **Konfigurasi RoundCube**
#
1. **Install Mariadb dan Roundcube** <br>
    Install roundcube sebagai webmail yang akan digunakan oleh client, dan package mariadb yang nantinya akan digunakan sebagai database dari roundcube.
     ```
    apt install mariadb-server roundcube
    ```
    Pilih yes untuk membuat database secara otomatis oleh roundcube.
    <img src="./gambar/10.png"/><br>
    Masukkan password database roundcube.
    <img src="./gambar/11.png"/><br>
    <img src="./gambar/12.png"/><br>
    Edit file /etc/roundcube/config.inc.php.
     ```
    sudo nano /etc/roundcube/config.inc.php
    ```
    Isikan default host dengan nama domain mail server.
     ```
    ...
    // For example %n = mail.domain.tld, %t = domain.tld
    $config['default_host'] = 'mail.kampus-05.takehome.com';
    ...
    ```
    Ganti smtp server dengan nama domain mail server.
    ```
    ...
    // For example %n = mail.domain.tld, %t = domain.tld
    $config['smtp_server'] = 'mail.kampus-05.takehome.com';
    ...
    ```
    Ganti smtp port dari 587 ke 25.
    ```
    ...
    // SMTP port. Use 25 for cleartext, 465 for Implicit TLS, or 587 for STARTTLS (default)
    $config['smtp_port'] = 25;
    ...
    ```
    Kosongkan value dari smtp user.
    ```
    ...
    // will use the current username for login
    $config['smtp_user'] = '';
    ...
    ```
    Kosongkan value dari smtp password
    ```
    ....
    // will use the current user's password for login
    $config['smtp_pass'] = '';
    ...
    ```
    Configure ulang roundcube (langkah ini bisa dilewati).
    ```
    dpkg-reconfigure roundcube-core
    ```

    Kosongkan karena kita tidak menggunakan tls.
    <img src="./gambar/13.png"/><br>

    Pilih bahasa untuk roundcube.
    <img src="./gambar/14.png"/><br>

    Pilih no jika tidak ingin reinstall database yang telah dibuat.
    <img src="./gambar/15.png"/><br>

    Check pada pilihan apache dan uncheck lighttpd.
    <img src="./gambar/16.png"/><br>

    Pilih yes untuk merestart web server.
    <img src="./gambar/17.png"/><br>

    Keep local version jika tidak ingin merubah versi roundcube ke yang lebih terbaru.
    <img src="./gambar/18.png"/><br>

    Edit apache config untuk memasukkan konfigurasi tambahan dari roundcube ke apache config.
    ```
    sudo nano /etc/apache2/apache2.conf
    ```

    Tambahkan pada baris paling bawah.
    ```
    Include /etc/roundcube/apache.conf
    ```

    Selanjutnya, masuk ke directory website apache dan tambahkan file baru untuk mail server.
    ```
    cd /etc/apache2/sites-available
    touch mail.conf
    vi mail.conf
    ```

    ```
    <VirtualHost *:80>
    ServerName mail.kampus-05.takehome.com
    DocumentRoot /usr/share/roundcube
    </VirtualHost>
    ```

    Disable apache default config dan enable kan mail config.
    ```
    a2dissite 000-default.conf
    a2ensite mail.conf
    ```

    Restart apache service.
    ```
    sudo systemctl restart apache2
    ```

### **Testing**
#### **Testing Lokal**

- buat user untuk mail terlebih dahulu
    ```
    adduser satu
    adduser dua
    ```
    jangan lupa restart postfix dan dovecot
    ```
    systemctl restart postfix dovecot
    ```
- Testing dengan **telnet**
    ```
    apt install telnet
    ```

    Test kirim file menggunakan perintah telnet <nama domain> <port> dengan menggunakan port 25 (SMTP). Masukkan nama alamat pengirim menggunakan mail from:. Masukkan nama alamat penerima menggunakan rcpt to:. Ketikkan data lalu enter. Isikan subject dengan megetikkan Subject: <isi subject>. Lalu isikan pesan yang akan dikirim kemudian isikan titik (.) untuk mengakhiri pesan.
    ```
    telnet mail.kampus-05.takehome.com 25
    ```
    <img src="./gambar/telnet sent.png"/><br>

    Melihat pesan menggunakan perintah telnet <nama domain> <port>. Login user menggunakan user <nama user>. Dan masukkan password menggunakan pass <password>. Untuk melihat list pesan yang diterima menggunakan perintah list. Dan untuk membuka pesan yang diterima menggunakan perintah retr <nomer pesan>.<br> Perintah quit untuk keluar dari telnet.

    ```
    telnet mail.kampus-05.takehome.com 110
    ```
    <img src="./gambar/telnet received.png"/><br>
   
- Selanjutnya buka web browser pada sisi client dan masukkan domain dari mail server (**mail.kampus-05.takehome.com**), maka akan muncul interface dari roundcube. Lalu login menggunakan salah satu user yang telah dibuat.
  <img src="./gambar/login student1.png"/><br>

  Klik pada compose dan isikan pesan untuk user lainnya. Lalu klik send.
  <img src="./gambar/send mail.png"/><br>

  Logout dan login ke user penerima, maka akan muncul pesan yang dikirim.
  <img src="./gambar/user student2.png"/><br>


#### **Testing Eksternal**
- Buka web mail roundcube di 'mail.kampus-05.takehome.com'
- Selanjutnya login masukkan username dan password
- Mengirim pesan kepada kelompok 6 dengan email `nicho@mail.kampus-06.takehome.com`
  <br>
  ![](gambar/postfix-ke-kelompok-6.PNG)
- Menerima pesan dari kelompok 2 dari `alan@mail.kampus-02.takehome.com`
  <br>
  ![](gambar/mendapat%20massage%20dari%20kelompok%202.PNG)