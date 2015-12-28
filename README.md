# manajemen_jaringan_zenoss_security
Kelompok : Firdauz Fanani [15160] - Adhika Wisaksono [15261] - Andhika Kurnia [15465] - Carolus Gaza [15250] - Goldi Rillo [15315]

## INSTALASI ZENOSS

1. Mulai dengan menginstall centOs. Lalu disable SELinux
2. Hapus library mysql-libs package yang sudah terinstall pada centOs sebelumnya,Karena saat installasi zenoss akan muncul pesan version conflict

*It appears that the distro-supplied version of MySQL is at least partially installed,or a prior installation attempt failed.*
*Please remove these packages, as well as their dependencies (often postfix), and then retry this script:*

` mysql-libs-5.1.69-1.el6_4.x86_64
