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
6. Sekarang kita punya kofigurasi SNMP daemon, kita membutuhkan restart service untuk mengimplementasikan perubahannya:

  ```
  sudo service snmpd restart
  ```
7. Client sekarang akan merespon ke polling request.

##Konfigurasi SSH Client
Untuk client yang lain, kita akan memperbolehkan zenoss untuk mendapatkan informasi melalui SSH. Kita akan menkonfigurasi pada zenoss , bukan pada SSH client.

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

##Konfigurasi zenOss
1. Hampir semua konfigurasi zenoss di lakukan dalam web. Buka browser dan buka :

  ```
  Your.Zenoss.IP.Address:8080
  ```
2. Jika kita pertama kali mengaksesnya maka akan muncul zenoss setup page :
3. Lalu klik “Get Started!” untuk lanjut. Kita akan dibawa ke halaman “Set Up Initial Users”. 
4. Pilih secure password untuk akun admin, dimana digunakan untuk tugas administrative, juga tambahkan regular username dan password untuk digunakan pada operasi biasa.
5. Klik “Next” untuk lanjut, kita akan dibawa ke halaman “Specify or Discover Devices to Monitor” 
6. Disini, kita akan menambahkan SNMP client VPS kita. Ketikkan IP addressnya ke kolom “Hostname/IP Address”. Pilih device type as Linux Server(SNMP) dan click “Save”. Click “Finish or Skip to Dashboard”
7. Kita akan melihat dashboar dari zenoss. Click pada “Infrasturcture” . Kita akan dibawa ke halaman “Devices”

##Menambahkan SSH Client
1. Klik pada icon yang terlihat seperti monitor computer dengan “plus” ditengah. Pilih “Add a Single Device”
2. Ketikkan pada IP Address pada SSH client kita dan pilih nama untuk mengidentifikasi yang ada pada “Title” field.
3. Pilih “/Server/SSH/Linux” untuk Device Class dan uncheck Model Device CheckBox.
4. Klik “Add” pada button.
5. Refresh halaman , maka SSH client akan muncul. Klik nama mesinya untuk membuka Device Overview. Pada bagian kiri, klik pada “Configuration Properties”.
6. Cari untik properti “zCommandUsername” dan double-click pada kolom “value”. Masukkan username yang digunakan pada SSH 
7. Cari “zKeyPath” dan double klik pada kolom “value”.Masukkan full pathname pada kunci RSA kita. 

  ```
  /home/zenoss/.ssh/id_rsa
  ```
8. Pada dibawah window, klik pada gambar gear dan pilih “Model Device”
9. Klik pada “X” di atas kanan setelah selesai.

##Konfigurasi Localhost
Konfigurasi untuk localhost tidak bekerja secara benar secara default.  Ini artinya VPS zenoss kita tidak di buat model secara benar.

1. Untuk membenarkan polling SNMP, log in ke mesin sebagai root, pindah pada directory konfigurasi SNMP dan pindah konfigurasi default SNMP daemon ke tempat yang aman.

  ```
  cd /etc/snmp/
  mv snmpd.conf snmpd.conf.bak
  ```
2. Sekarang buat snmpd.conf.file sederhana, seperti saat SNMP client sebelumnya
  
  ```
  nano snmpd.conf
  rocommunity public
  ```
3. Restart sevice :

  ```
  service snmpd restart
  ```
4. Kembali ke interface web, klik pada “Infrastructure” dan pilih “Devices”. Klik pada “Localhost” link untuk membuka konfigurasinya.
5. Sekarang klik pada gambar gear. Pilih “Reset/Change IP Address”
6. Pada dialog yang akan muncul, ketikkan “127.0.0.1” untuk menggunakan loopback network device
7. Klik kembali pada gambar gear dan pilih “Model Device”untuk membetulkan masalah sebelumya. Klik “X” ketika log sudah selesai.