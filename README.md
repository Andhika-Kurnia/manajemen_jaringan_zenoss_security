# ANGGOTA KELOMPOK
- Firdauz Fanani [15160]
- Carolus Gaza [15250]
- Adhika Wisaksono [15261]
- Goldi Rillo [15315]
- Andhika Kurnia [15465]

## INSTALASI ZENOSS

1. Mulai dengan menginstall centOs. Lalu disable SELinux

2. Hapus library mysql-libs package yang sudah terinstall pada centOs sebelumnya,Karena saat installasi zenoss akan muncul pesan version conflict "*It appears that the distro-supplied version of MySQL is at least partially installed,or a prior installation attempt failed. Please remove these packages, as well as their dependencies (often postfix), and then retry this script:*"
  ```
  mysql-libs-5.1.69-1.el6_4.x86_64
  ```

3. Maka kita akan menghapus file-file tersebut sebelum dimulai
  ```
  yum remove mysql-libs
  ```

4. Zenoss menyediakan script installasi yang akan melakukan langkah-langkah dalam penginstallan. Kita akan mendapatkan file Zenoss dari website
  ```
  cd ~
  wget --no-check-certificate https://github.com/zenoss/core-autodeploy/tarball/4.2.4 -O auto.tar.gz
  ```

5. Lalu kita unzip file tersebut, pindahkan ke directory dan jalankan auto-install scriptnya
  ```
  tar xvf auto.tar.gz
  cd zenoss-core-autodeploy-*
  ./core-autodeploy.sh
  ```

6. Tekan “ENTER” untuk melanjutkan.Lalu akan muncul pesan License Agreement, baca lalu tekan “Q” untuk melanjutkan.

7. Lalu akan muncul pesan accept license, ketikkan “yes” untuk melanjutkan.

8. Zenoss akan mulai mendownload dan menkonfigurasi komponen-komponen yang dibutuhkan.

9. Pada beberapa bagian saat installasi, akan ditanya apakah kita ingin mengatur password root pada mysql,ketikkan “Y” untuk memilih password
  ```
  MySQL is configured with a blank root password.
  Configure a secure MySQL root password? [Yn]: Y
  ```

10.	Pilih password dan confirm .
<<<<<<< HEAD
11.	Installasi memerlukan waktu yang lumayan lama.

##Konfigurasi Pada Client

1. Ada beberaa hal yang akan kita konfigurasi untuk memberikan hal yang berguna untuk Zenoss saat diatur.
2. Kita akan menkonfigurasi 2 mesin “client” yang akan zenoss monitor, Client akan dijalankan di Ubuntu pada VPS.

##Konfigurasi SNMP Client
Pada installasi Ubuntu, kita akan menginstall SNMP daemon, dimana akan memperbolehkan Zenoss mendapatkan informasi tentang client.
1. Pada client, ketikkan command :
  ```
  sudo apt-get update
  sudo apt-get install snmpd
  ```
2. Setelah installasi , kita membutuhkan konfigurasi untuk daemon, pertama kita menuju konfigurasi directory dan lalu mengganti konfigurasi defaultnya :
  ```
  cd /etc/snmp/
  sudo mv snmpd.conf snmpd.conf.bak
  ```
3. Sekarang, kita akan membuat baru, konfigurasi file sebagai root 
  ```
  sudo nano snmpd.conf
  ```
4. Copy dan paste line ini pada file konfigurasi 
  ```
  rocommunity public
  ```
5. Simpan dan tutup file tersebut.
6. Sekarang kita punya kofigurasi SNMP daemon, kita membutuhkan restart service untuk mengimplementasikan perubahannya.:
  ```
  sudo service snmpd restart
  ```
7. Client sekarang akan merespon ke polling request.

##Konfigurasi SSH Client
Untuk client yang lain, kita akan memperbolehkan zenoss untuk mendapatkan informasi melalui SSH.
Kita akan menkonfigurasi pada zenoss , bukan pada SSH client
1. Mulai dengan log in pada zenoss user dan membuat kunci RSA :
  ```
  su - zenoss
  ssh-keygen -t rsa
  ```
2. Lalu tekan “ENTER” untuk menerima secara default dan menggunakan no passphrase.
3. Lalu, kita copy kunci SSH ke Komputer client SSH kita. Ubah username dan IP address untuk merefleksinakan konfigurasi SSH kita :
  ```
  ssh-copy-id username@SSH.Client.IP.Address
  ```
4. Kita akan ditanya tentang authentikasi pada remote machine melalui password dan lalu kita akan menambahkan kunci kita pada remote server. Tes kemampuan untuk log in tanpa password dengan mengetikkan :
  ```
  ssh username@SSH.Client.IP.Address
  ```
5. Jika sukses, ketik “exit” untuk kembali ke zenoss
6. Jika gagal ketik “exit” untuk kembali ke root shell


