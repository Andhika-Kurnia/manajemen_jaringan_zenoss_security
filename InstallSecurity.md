## INSTALASI Security Plugin on zenOss


1. Zenpack tidak mempunyai installasi yang spesial. Berdasarkan versi dari zenOss yang kita install maka kita menginstall zenPacknya. Kita akan membutuhkan (.egg) untuk memverifikasi package yang kita punya untuk menginstall zenPack.

   -Zenoss 4.1 dan seterusnya: file The ZenPack harus berakhiran dengan -py2.7.egg.

   -Zenoss 3.0 - 4.0: file ZenPack harus berakhiran dengan -py2.6.egg.
2. Untuk menginstall zenPack kita harus mengcopy .egg ke server zenOss master dan menjalankan command sebagai user zenOss

  ```
  $ sudo su - zenoss
  ```
3. Install ZenPack:

  ```
  $ zenpack --install <filename.egg>
  ```
4. Setelah installasi kita harus merestart zenOss dengan menjalankan command berikut sebaga user pada zenOss:

  ```
  $ zenoss restart
  ```