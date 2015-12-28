# ANGGOTA KELOMPOK
- Firdauz Fanani [15160]
- Carolus Gaza [15250]
- Adhika Wisaksono [15261]
- Goldi Rillo [15315]
- Andhika Kurnia [15465]

## INSTALASI ZENOSS

1. Mulai dengan menginstall centOs. Lalu disable SELinux
2. Hapus library mysql-libs package yang sudah terinstall pada centOs sebelumnya,Karena saat installasi zenoss akan muncul pesan version conflict
	*It appears that the distro-supplied version of MySQL is at least partially installed,or a prior installation attempt failed.*
	*Please remove these packages, as well as their dependencies (often postfix), and then retry this script:*
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
9. Pada beberapa bagian saat installasi, akan ditanya apakah kita ingin mengatur password root pada mysql, ketikkan “Y” untuk memilih password
```
MySQL is configured with a blank root password.
Configure a secure MySQL root password? [Yn]: Y
```
10.	Pilih password dan confirm .
11.	Installasi memerlukan waktu yang lumayan lama.
```