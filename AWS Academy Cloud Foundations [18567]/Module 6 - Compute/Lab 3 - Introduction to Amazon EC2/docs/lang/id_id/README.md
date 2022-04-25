# Lab 3: Pengantar Amazon EC2

<!-- Note to translators: This is based on SPL-200. Copy the translation from there. Do not re-translate the whole document. -->

**Versi 1.1.7 (spl200)**

## Ikhtisar

<img src="images/lab-scenario.jpeg" alt="diagram arsitektur" width="400">

&nbsp;

Lab ini memberi Anda ikhtisar dasar dalam meluncurkan, mengubah ukuran, mengelola, dan memantau instans Amazon EC2.

**Amazon Elastic Compute Cloud (Amazon EC2)** adalah layanan web yang menyediakan kapasitas komputasi yang ukurannya dapat diubah di cloud. Layanan ini dirancang untuk mempermudah komputasi cloud berskala web untuk pengembang.

Antarmuka layanan web sederhana Amazon EC2 ini memungkinkan Anda memperoleh dan mengonfigurasi kapasitas dengan sedikit gesekan. Ini menyediakan Anda dengan kontrol penuh dari sumber daya komputasi Anda dan memungkinkan Anda berjalan di lingkungan komputasi Amazon yang terbukti. Amazon EC2 mengurangi waktu yang diperlukan untuk mendapatkan dan melakukan boot instans server baru ke menit, memungkinkan Anda meningkatkan atau mengurangi skala kapasitas, ketika kebutuhan komputasi Anda berubah.

Amazon EC2 mengubah ekonomi komputasi dengan memungkinkan Anda membayar hanya untuk kapasitas yang benar-benar Anda gunakan. Amazon EC2 menyediakan pengembang alat untuk membangun aplikasi yang tahan terhadap kegagalan dan mengisolasinya dari skenario kegagalan umum.

&nbsp;
### Topik yang dibahas

Pada akhir lab ini, Anda akan mampu:

* Meluncurkan server web dengan perlindungan penghentian diaktifkan
* Memantau instans EC2 Anda
* Memodifikasi grup keamanan yang menggunakan server web Anda untuk mengizinkan akses HTTP
* Mengubah ukuran instans Amazon EC2 Anda agar dapat diskalakan
* Menjelajahi batas EC2
* Menguji perlindungan penghentian
* Menghentikan instans EC2 Anda

&nbsp;

### Durasi

Diperlukan sekitar **35 menit ** untuk menyelesaikan lab ini.

&nbsp;
&nbsp;
## Mengakses AWS Management Console

1. Di bagian atas instruksi ini, klik <span id="ssb_voc_grey">Start Lab</span> (Mulai Lab) untuk meluncurkan lab Anda.

   Panel Start Lab (Mulai Lab) terbuka dan menampilkan status lab.

2. Tunggu hingga Anda melihat pesan "**Lab status: ready**" (Status lab: siap), kemudian klik **X** untuk menutup panel Start Lab (Mulai Lab).

3. Di bagian atas instruksi ini, klik <span id="ssb_voc_grey">AWS</span>

   Tindakan ini akan membuka AWS Management Console di tab browser baru. Anda akan secara otomatis masuk ke sistem.

   **Tips**: Jika tab browser baru tidak terbuka, biasanya akan ada banner atau ikon di bagian atas browser Anda yang menunjukkan bahwa browser Anda mencegah situs membuka jendela pop-up. Klik pada banner atau ikon dan pilih "Allow pop ups" ("Izinkan pop ups")

4. Atur tab AWS Management Console agar tampil di sebelah petunjuk ini. Idealnya, Anda akan dapat melihat kedua tab browser pada saat bersamaan, agar lebih mudah mengikuti langkah-langkah lab.

&nbsp;
&nbsp;
## Tugas 1: Meluncurkan Instans Amazon EC2 Anda

Dalam tugas ini, Anda akan meluncurkan instans Amazon EC2 dengan _perlindungan penghentian_. Perlindungan penghentian mencegah Anda dari dengan sengaja mengakhiri instans EC2. Anda akan menerapkan instans Anda dengan skrip Data Pengguna yang akan memungkinkan Anda menerapkan web server sederhana.

5. Di **AWS Management Console** pada menu **Services** (Layanan), klik **EC2**.

6. Pilih <span id="ssb_orange">Launch Instance <i class="fas fa-caret-down"></i></span> (Luncurkan Instans), lalu pilih <span id="ssb_white">Launch Instance</span> (Luncurkan Instans)

### Langkah 1: Memilih Amazon Machine Image (AMI)

<i class="fas fa-info-circle"></i>**Amazon Machine Image (AMI)** menyediakan informasi yang diperlukan untuk meluncurkan instans yang merupakan server virtual di cloud. AMI mencakup:

* Template untuk volume root pada instans (misalnya, sistem operasi, server aplikasi dengan aplikasi).
* Izin peluncuran yang mengontrol akun AWS mana yang dapat menggunakan AMI untuk meluncurkan instans
* Pemetaan perangkat blok yang menentukan volume yang akan dipasang ke instans saat diluncurkan

Daftar **Quick Start** (Mulai Cepat) berisi AMI yang paling umum digunakan. Anda juga dapat membuat AMI Anda sendiri atau memilih AMI dari AWS Marketplace, toko online tempat Anda dapat menjual atau membeli perangkat lunak yang berjalan pada AWS.

7. Klik <span id="ssb_blue">Select</span> (Pilih) di sebelah **Amazon Linux 2 AMI** (di bagian atas daftar).

&nbsp;

### Langkah 2: Memilih Jenis Instans

<i class="fas fa-info-circle"></i> Amazon EC2 menyediakan berbagai pilihan _jenis instans_ yang dioptimalkan agar sesuai dengan kasus penggunaan yang berbeda. Jenis instans terdiri dari berbagai kombinasi CPU, memori, penyimpanan, dan kapasitas jaringan dan memberi Anda fleksibilitas untuk memilih campuran sumber daya yang sesuai untuk aplikasi Anda. Setiap jenis instans mencakup satu atau beberapa _ukuran instans_, yang memungkinkan Anda menskalakan sumber daya sesuai kebutuhan beban kerja target Anda.

Anda akan menggunakan instans **t2.micro** yang harus dipilih <i class="fas fa-square" style="color:blue"></i> secara default. Jenis instans ini memiliki 1 CPU virtual dan memori 1 GiB. **CATATAN**: Anda mungkin dibatasi untuk menggunakan jenis instans lain di lab ini.

8. Klik <span id="ssb_grey">Next: Configure Instance Details</span> (Selanjutnya: Konfigurasikan Detail Instans)

&nbsp;
### Langkah 3: Mengonfigurasi Detail Instans

Halaman ini digunakan untuk mengonfigurasi instans yang sesuai dengan kebutuhan Anda. Ini termasuk pengaturan jaringan dan pemantauan.

**Network** (Jaringan) menunjukkan Virtual Private Cloud (VPC) mana yang ingin Anda luncurkan ke instans. Anda dapat memiliki beberapa jaringan, misalnya, jaringan yang berbeda untuk pengembangan, pengujian, dan produksi.

9. Untuk **Network** (Jaringan), pilih **Lab VPC**.

   Lab VPC dibuat menggunakan template AWS CloudFormation selama proses penyiapan lab Anda. VPC ini mencakup dua subnet publik dalam dua Availability Zone yang berbeda.

10. Untuk **Enable termination protection** (Aktifkan perlindungan penghentian), pilih <i class="far fa-check-square"></i> **Protect against accidental termination** (Lindungi terhadap penghentian yang tidak disengaja).

<i class="fas fa-info-circle"></i> Ketika instans Amazon EC2 tidak lagi diperlukan, instans bisa _dihentikan_, ini berarti bahwa instans dihentikan dan sumber dayanya dirilis. Instans yang dihentikan tidak bisa dimulai lagi. Jika Anda ingin mencegah instans dari dihentikan secara tidak sengaja, Anda dapat mengaktifkan _perlindungan penghentian_ untuk instans, yang mencegahnya dari dihentikan.

11. Gulir ke bawah, lalu buka <i class="fas fa-caret-right"></i> **Advanced Details** (Detail Lanjutan).

Bidang untuk **User data** (Data pengguna) akan muncul.

<i class="fas fa-info-circle"></i> Ketika Anda meluncurkan instans, Anda bisa meloloskan _data pengguna_ ke instans yang dapat digunakan untuk melakukan tugas konfigurasi otomatis umum dan bahkan menjalankan skrip setelah instans dimulai.

Instans Anda menjalankan Amazon Linux, sehingga Anda akan menyediakan _skrip shell_ yang akan berjalan ketika instans dimulai.

12. Salin perintah berikut dan tempel di bidang **User data** (Data pengguna):

    ```plain
       #!/bin/bash
       yum -y install httpd
       systemctl enable httpd
       systemctl start httpd
       echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
       ```

    Skrip akan:

    * Menginstal server web Apache (httpd)
    * Mengonfigurasi server web untuk secara otomatis dimulai saat boot
    * Mengaktifkan server Web
    * Membuat halaman web sederhana


13. Klik <span id="ssb_grey">Next: Add Storage</span> (Selanjutnya: Menambahkan Penyimpanan)


&nbsp;
### Langkah 4: Menambahkan Penyimpanan

<i class="fas fa-info-circle"></i> Amazon EC2 menyimpan data pada disk virtual yang terpasang di jaringan yang disebut *Elastic Block Store*.

Anda akan meluncurkan instans Amazon EC2 menggunakan volume disk 8 GiB default. Ini akan menjadi volume root Anda (juga disebut sebagai volume 'boot').

14. Klik <span id="ssb_grey">Next: Add Tags</span> (Selanjutnya: Menambahkan Tag)

&nbsp;
### Langkah 5: Menambahkan Tag

<i class="fas fa-info-circle"></i> Tag memungkinkan Anda mengategorikan sumber daya AWS Anda dengan cara yang berbeda, misalnya, berdasarkan tujuan, pemilik, atau lingkungan. Tag berguna ketika Anda memiliki banyak sumber daya dari jenis yang sama - Anda dapat dengan cepat mengidentifikasi sumber daya tertentu berdasarkan tag yang telah Anda tetapkan pada sumber daya tersebut. Setiap tag terdiri dari Kunci dan Nilai, yang keduanya Anda tentukan.

15. Klik <span id="ssb_grey">Add Tag</span> (Tambah Tag), lalu konfigurasikan:

    * **Key:** (Kunci:) `Name` (Nama)
    * **Value:** (Nilai:) `Web Server` (Server Web)

16. Klik <span id="ssb_grey">Next: Configure Security Group</span> (Selanjutnya: Mengonfigurasikan Grup Keamanan)

&nbsp;
### Langkah 6: Mengonfigurasikan Grup Keamanan

<i class="fas fa-info-circle"></i> _Grup keamanan_ bertindak sebagai firewall virtual yang mengontrol lalu lintas untuk satu atau beberapa instans. Ketika Anda meluncurkan instans, Anda mengaitkan satu atau beberapa grup keamanan dengan instans. Anda menambahkan _aturan_ ke setiap grup keamanan yang mengizinkan lalu lintas ke atau dari instans yang terkait. Anda dapat mengubah aturan untuk grup keamanan setiap saat; aturan baru secara otomatis diterapkan ke semua instans yang dikaitkan dengan grup keamanan.

17. Pada **Langkah 6: Konfigurasikan Grup Keamanan**, konfigurasikan:

    * **Security group name:** (Nama grup keamanan:) `Web Server security group` (Grup keamanan Server Web)
    * **Description:** (Deskripsi:) `Security group for my web server` (Grup keamanan untuk server web saya)

    Di lab ini, Anda tidak akan masuk ke instans Anda menggunakan SSH. Menghapus akses SSH akan meningkatkan keamanan instans.

18. Hapus <i class="fas fa-times-circle"></i> aturan SSH yang ada.

19. Klik <span id="ssb_blue">Review and Launch</span> (Tinjau dan Luncurkan)

&nbsp;
### Langkah 7: Meninjau Peluncuran Instans

Halaman Tinjauan menampilkan konfigurasi untuk instans yang akan Anda luncurkan.

20. Klik <span id="ssb_blue">Launch</span> (Luncurkan)

Jendela **Select an existing key pair or create a new key pair** (Pilih key pair yang ada atau buat key pair baru) akan muncul.

<i class="fas fa-info-circle"></i> Amazon EC2 menggunakan kriptografi kunci publik untuk mengenkripsi dan mendekripsi informasi login. Untuk masuk ke instans Anda, Anda harus membuat key pair, menentukan nama key pair ketika Anda meluncurkan instans, dan menyediakan kunci privat ketika Anda terhubung ke instance.

Di lab ini Anda tidak akan masuk ke instans Anda, sehingga Anda tidak memerlukan key pair.

21. Klik drop-down **Choose an existing key pair** (Pilih key pair yang ada) <i class="fas fa-angle-down"></i> dan pilih *Proceed without a key pair* (Lanjutkan tanpa key pair).

22. Pilih <i class="far fa-check-square"></i> **I acknowledge that ...** (Saya mengakui bahwa ....)

23. Klik <span id="ssb_blue">Launch Instances</span> (Luncurkan Instans)

    Instans Anda sekarang akan diluncurkan.

24. Klik <span id="ssb_blue">View Instances</span> (Tampilkan Instans)

    Instans akan muncul dengan status _tertunda_ , yang berarti sedang diluncurkan. Status kemudian akan berubah menjadi _berjalan_, yang menunjukkan bahwa instans telah memulai boot. Anda akan memerlukan beberapa saat sebelum dapat mengakses instans.

    Instans menerima _nama DNS publik_ yang dapat Anda gunakan untuk menghubungi instans dari Internet.

    <i class="fas fa-square" style="color:blue"></i> Anda **Web Server** (Server Web) harus dipilih. Tab **Description** (Deskripsi) menampilkan informasi terperinci tentang instans Anda.

    <i class="fas fa-comment"></i> Untuk melihat informasi selengkapnya di tab Deskripsi, seret pembagi jendela ke atas.

    Tinjau informasi yang ditampilkan di tab **Description** (Deskripsi). Informasi ini mencakup jenis instans, pengaturan keamanan, dan pengaturan jaringan.

25. Tunggu instans Anda untuk menampilkan berikut:

* **Instance State:** (Status Instans:) <span style="color:green"><i class="fas fa-circle"></i></span> running (berjalan)
* **Status Checks:** (Pemeriksaan Status:) <span style="color:green"><i class="fas fa-check-circle"></i></span> 2/2 checks passed (lolos pemeriksaan 2/2)

<span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Selamat!** Anda telah berhasil meluncurkan instans Amazon EC2 pertama Anda.

&nbsp;
&nbsp;
## Tugas 2: Memantau Instans Anda

Pemantauan merupakan bagian penting dari menjaga keandalan, ketersediaan, dan kinerja dari instans Anda Amazon Elastic Compute Cloud (Amazon EC2) dan solusi AWS Anda.

26. Klik tab **Status Checks** (Pemeriksaan Status).

    <i class="fas fa-info-circle"></i> Dengan pemantauan status instans, Anda dapat dengan cepat menentukan apakah Amazon EC2 telah mendeteksi masalah yang mungkin mencegah instans Anda dari menjalankan aplikasi. Amazon EC2 menjalankan pemeriksaan otomatis pada setiap instans EC2 berjalan untuk mengidentifikasi masalah perangkat keras dan perangkat lunak.

    Perhatikan bahwa pemeriksaan **System reachability** (Jangkauan sistem) dan **Instance reachability** (Jangkauan instans) sudah lulus.

27. Klik tab **Monitoring** (Pemantauan).

    Tab ini menampilkan metrik Amazon CloudWatch untuk instans Anda. Saat ini, tidak banyak metrik untuk ditampilkan karena instans baru saja diluncurkan.

    Anda dapat mengklik grafik untuk memperluas tampilan.

    <i class="fas fa-info-circle"></i> Amazon EC2 mengirimkan metrik ke Amazon CloudWatch untuk instans EC2 Anda. Pemantauan dasar (lima menit) diaktifkan secara default. Anda dapat mengaktifkan pemantauan terperinci (satu menit).

28. Di menu <span id="ssb_grey_square">Actions <i class="fas fa-caret-down"></i></span> (Tindakan), pilih **Monitor and troubleshoot** <i class="fas fa-caret-right"></i> (Pemantauan dan Pemecahan Masalah) **Get System Log** (Dapatkan Log Sistem).

   Log Sistem menampilkan output konsol dari instans, yang merupakan alat yang berguna untuk diagnosis masalah. Alat ini terutama berguna untuk memecahkan masalah kernel dan masalah konfigurasi layanan yang dapat menyebabkan instans dihentikan atau menjadi tidak dapat dijangkau sebelum daemon SSH-nya dapat dimulai. Jika Anda tidak melihat log sistem, tunggu beberapa menit, lalu coba lagi.

29. Gulir seluruh output dan perhatikan bahwa paket HTTP sudah diinstal dari **data pengguna** yang ditambahkan ketika Anda membuat instans.

<img src="images/Console-output.png" alt="Konsol-Output" width="600">

30. Pilih **Cancel** (Batalkan).

31. Di menu <span id="ssb_grey_square">Actions <i class="fas fa-caret-down"></i></span> (Tindakan), pilih **Monitor and troubleshoot** <i class="fas fa-caret-right"></i> (Pemantauan dan Pemecahan Masalah) **Get Instance Screenshot** (Dapatkan Tangkapan Layar Instans).

    Ini menunjukkan bagaimana tampilan konsol instans Amazon EC2 Anda jika layar ditambahkan pada instans tersebut.

    <img src="images/Screen-shot.png" alt="Tangkapan Layar" width="600">

    <i class="fas fa-info-circle"></i> Jika Anda tidak dapat menjangkau instans Anda melalui SSH atau RDP, Anda dapat melakukan tangkapan layar untuk instans Anda dan menampilkannya sebagai gambar. Ini memberikan visibilitas mengenai status instans, dan memungkinkan pemecahan masalah yang lebih cepat.

32. Pilih **Cancel** (Batalkan).

    <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Selamat!** Anda telah menjelajahi beberapa cara untuk memantau  instans Anda.

&nbsp;
&nbsp;
## Tugas 3: Memperbarui Grup Keamanan Anda dan Mengakses Server Web

Ketika Anda meluncurkan instans EC2, Anda menyediakan skrip yang menginstal server web dan membuat halaman web sederhana. Dalam tugas ini, Anda akan mengakses konten dari server web.


33. Klik tab **Details** (Detail).

34. Salin **IP Publik IPv4** dari instans ke clipboard Anda.

35. Buka tab baru di browser web Anda, tempel alamat IP yang baru saja Anda salin, lalu tekan **Enter**.

    **Pertanyaan:** Apakah Anda dapat mengakses server web Anda? Mengapa tidak?

    Anda saat ini **tidak** dapat mengakses server web Anda karena _grup keamanan_ tidak mengizinkan lalu lintas masuk pada port 80, yang digunakan untuk permintaan web HTTP. Ini adalah demonstrasi penggunaan grup keamanan sebagai firewall untuk membatasi lalu lintas jaringan yang diizinkan masuk dan keluar dari instans.

    Untuk memperbaiki ini, Anda sekarang akan memperbarui grup keamanan untuk mengizinkan lalu lintas web pada port 80.

36. Tetap buka tab browser, tetapi kembali ke tab **EC2 Management Console**.

37. Di panel navigasi kiri, klik **Security Groups** (Grup Keamanan).

38. Pilih <i class="fas fa-square" style="color:blue"></i> **Web Server security group** (Grup keamanan Server Web).

39. Klik tab **Inbound** (Masuk).

    Grup keamanan saat ini tidak memiliki aturan.

40. Klik <span id="ssb_grey_square">Edit inboud rules</span> (Edit aturan masuk), lalu konfigurasikan:

    * **Type:** (Jenis:) _HTTP_
    * **Source:** (Sumber:) _Anywhere_ (Mana Saja)
    * Klik <span id="ssb_orange">Save rules</span> (Simpan Aturan)

41. Kembali ke tab server web yang sebelumnya Anda buka dan segarkan <i class="fas fa-sync"></i> halaman.

    Anda akan melihat pesan _Hello From Your Web Server!_ (Halo dari Server Web Anda!)

    <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Selamat!** Anda telah berhasil mengubah grup keamanan Anda untuk mengizinkan lalu lintas HTTP ke dalam instans Amazon EC2 Anda.

&nbsp;
&nbsp;
## Tugas 4: Mengubah Ukuran Instans Anda: Jenis Instans dan Volume EBS

Ketika kebutuhan Anda berubah, Anda mungkin mendapati bahwa instans Anda terlalu banyak digunakan (terlalu kecil) atau kurang dimanfaatkan (terlalu besar). Jika demikian, Anda dapat mengubah _jenis instance_. Misalnya, jika instans _t2.micro_ terlalu kecil untuk beban kerjanya, Anda dapat mengubahnya ke instans _m5.medium_. Demikian pula, Anda dapat mengubah ukuran disk.


### Menghentikan Instans Anda

Sebelum Anda dapat mengubah ukuran instans, Anda harus _menghentikannya_.

<i class="fas fa-info-circle"></i> Ketika Anda menghentikan instans, instans akan dimatikan. Tidak ada biaya untuk instans EC2 yang dihentikan, tetapi biaya penyimpanan untuk volume Amazon EBS yang dipasang tetap Anda.

42. Pada **EC2 Management Console**, di panel kiri, klik **Instances** (Instans).

    <i class="fas fa-square" style="color:blue"></i> **Web Server** (Server Web) harus sudah dipilih.

43. Di menu <span id="ssb_grey_square">Instance State <i class="fas fa-caret-down"></i></span> (Status Instans), pilih **Stop instance** (Hentikan instans).

44. Pilih <span id="ssb_orange">Stop</span> (Hentikan)

    Instans Anda akan melakukan penghentian normal, lalu akan berhenti berjalan.

45. Tunggu **Instance State** (Status Instans) menampilkan: <span style="color:red"><i class="fas fa-circle"></i></span> stopped (berhenti)

### Mengubah Jenis Instans

46. Di menu <span id="ssb_grey_square">Actions <i class="fas fa-caret-down"></i></span> (Tindakan), pilih **Instance Settings** (Pengaturan Instans) <i class="fas fa-caret-right"></i> **Change Instance Type** (Ubah Jenis Instans), lalu konfigurasikan:

    * **Instance Type:** (Jenis Instans:) *t2.small*
    * Pilih <span id="ssb_orange">Apply</span> (Terapkan)

    Ketika instans dimulai lagi, maka akan menjadi _t2.small_, yang memiliki memori dua kali dari instans _t2.micro_. **CATATAN**: Anda mungkin dibatasi untuk menggunakan jenis instans lain di lab in.

### Mengubah Ukuran Volume EBS

47. Di menu navigasi kiri, klik **Volumes** (Volume).

48. Di menu <span id="ssb_grey">Actions <i class="fas fa-caret-down"></i></span> (Tindakan), pilih **Modify Volume** (Ubah Volume).

    Volume disk saat ini memiliki ukuran 8 GiB. Sekarang Anda akan meningkatkan ukuran disk ini.

49. Ubah ukuran menjadi: `10` **CATATAN**: Anda mungkin dibatasi untuk membuat volume Amazon EBS yang besar di lab ini.

50. Pilih <span id="ssb_grey">Modify</span> (Ubah)

51. Pilih <span id="ssb_blue">Yes</span> (Ya) untuk mengonfirmasi dan meningkatkan ukuran volume.

52. Pilih <span id="ssb_blue">Close</span> (Tutup)

### Mulai Instance yang Ukurannya Diubah

Anda sekarang akan memulai instans lagi, yang sekarang akan memiliki lebih banyak memori dan lebih banyak ruang disk.

53. Di panel navigasi kiri, klik **Instances** (Instans).

54. Di menu <span id="ssb_grey_square">Instance State <i class="fas fa-caret-down"></i></span> (Status Instans), pilih **Start instance** (Mulai instans).

55. Pilih <span id="ssb_orange">Start</span> (Mulai)

    <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Selamat!** Anda telah berhasil mengubah ukuran Instans Amazon EC2 Anda. Dalam tugas ini, Anda mengubah jenis instans Anda dari _t2.micro_ ke _t2.small_. Anda juga memodifikasi volume disk root dari 8 GiB menjadi 10 GiB.

&nbsp;
&nbsp;
## Tugas 5: Menjelajahi Batas EC2

Amazon EC2 menyediakan sumber daya yang berbeda yang dapat Anda gunakan. Sumber daya ini mencakup gambar, instans, volume, dan snapshot. Ketika Anda membuat akun AWS, ada batas default pada sumber daya ini per wilayah.

56. Di panel navigasi kiri, klik **Limits** (Batas).

57. Dari daftar drop down, pilih **Running instances** (Instans yang Berjalan).

    Perhatikan bahwa ada batas pada jumlah instans yang dapat Anda luncurkan di wilayah ini. Ketika meluncurkan instans, permintaan tidak harus membuat penggunaan Anda melebihi batas instans saat ini di wilayah tersebut.

    Anda dapat meminta peningkatan untuk berbagai batas ini.

&nbsp;
&nbsp;
## Tugas 6: Menguji Perlindungan Penghentian

Anda dapat menghapus instans Anda ketika Anda tidak lagi membutuhkannya. Hal ini disebut sebagai _mengakhiri_ instans Anda. Anda tidak dapat terhubung ke atau me-restart instans setelah telah dihentikan.

Dalam tugas ini, anda akan belajar cara menggunakan _perlindungan penghentian_.

58. Di panel navigasi kiri, klik **Instances** (Instans).

59. Di menu <span id="ssb_grey_square">Instance State <i class="fas fa-caret-down"></i></span> (Status Instans), pilih **Terminate instance** (Hentikan instans).

60. Kemudian pilih <span id="ssb_orange">Terminate</span> (Hentikan)

    Perhatikan bahwa Anda pesan yang berbunyi: *Failed to terminate the instance i-1234567xxx. The instance 'i-1234567xxx' may not be terminated. Modify its 'disableApiTermination' instance attribute and try again.* (Gagal menghentikan instans i-1234567xxx. Instans 'i-1234567xxx' tidak dapat dihentikan. Ubah atribut instans 'disableApiTermination' dan coba lagi).

    Ini adalah perlindungan untuk mencegah penghentian instans secara tidak sengaja. Jika Anda benar-benar ingin menghentikan instans, Anda harus menonaktifkan perlindungan penghentian.

61. Di menu <span id="ssb_grey_square">Actions <i class="fas fa-caret-down"></i></span> (Tindakan), pilih **Instance Settings** (Pengaturan Instans) <i class="fas fa-caret-right"></i> **Change Termination Protection** (Ubah Perlindungan Penghentian).

62. Hapus tanda centang di sebelah <i class="far fa-square"></i> **Enable** (Aktifkan).

63. Pilih <span id="ssb_orange">Save</span> (Simpan)

    Anda sekarang dapat menghentikan instans.

64. Di menu <span id="ssb_grey_square">Status Instans <i class="fas fa-caret-down"></i></span>, pilih **Terminate** (Hentikan).

65. Pilih <span id="ssb_orange">Terminate</span> (Hentikan)

    <span style="color:blue"><i class="far fa-thumbs-up"></i></span> **Selamat!** Anda telah berhasil menguji perlindungan penghentian dan menghentikan instans Anda.

&nbsp;
&nbsp;

## Lab Selesai

<i class="icon-flag-checkered"></i> Selamat! Anda telah menyelesaikan lab.

66. Klik <span id="ssb_voc_grey">End Lab</span> (Akhiri Lab) di bagian atas halaman ini, kemudian klik <span id="ssb_blue">Yes</span> (Ya) untuk mengonfirmasi bahwa Anda ingin mengakhiri lab.

    Panel akan muncul, yang menunjukkan bahwa "DELETE has been initiated... You may close this message box now" ("DELETE sudah dimulai... Anda dapat menutup kotak pesan ini sekarang".) 

67. Klik **X** di sudut kanan atas untuk menutup panel.

Untuk umpan balik, saran, atau koreksi, silakan kirim email kepada kami di: *aws-course-feedback@amazon.com*

&nbsp;
&nbsp;
## Sumber Daya Tambahan

* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html" target="_blank">Meluncurkan Instans Anda</a>
* <a href="https://aws.amazon.com/ec2/instance-types" target="_blank">Jenis Instans Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html" target="_blank">Amazon Machine Images (AMI)</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html" target="_blank">Amazon EC2 - Data Pengguna dan Skrip Shell</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html" target="_blank">Volume Perangkat Root Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html" target="_blank">Menandai Sumber Daya  Amazon EC2 Anda</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html" target="_blank">Grup Keamanan</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html" target="_blank">Amazon EC2 Key Pair</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html?icmpid=docs_ec2_console" target="_blank">Pemeriksaan Status untuk Instans Anda</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-console.html" target="_blank">Mendapatkan Konsol Output dan Melakukan Boot Instans</a>
* <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ec2-metricscollected.html" target="_blank">Metrik dan Dimensi Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html" target="_blank">Mengubah Ukuran Instans Anda</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html" target="_blank">Menghentikan dan Memulai Instans Anda</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-resource-limits.html" target="_blank">Batas Layanan Amazon EC2</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html" target="_blank">Menghentikan Instans Anda</a>
* <a href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html" target="_blank">Perlindungan Penghentian untuk Instans</a>
