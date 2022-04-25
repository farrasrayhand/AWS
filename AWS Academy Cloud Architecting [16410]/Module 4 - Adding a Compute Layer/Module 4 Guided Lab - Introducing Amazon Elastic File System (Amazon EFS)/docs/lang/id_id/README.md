# Modul 4 - Lab Terpandu: Memperkenalkan Amazon Elastic File System (Amazon EFS)
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: SPL-151"

## Gambaran umum dan tujuan lab
Lab ini memperkenalkan Anda ke Amazon Elastic File System (Amazon EFS) menggunakan Konsol Manajemen AWS.

Setelah menyelesaikan lab ini, Anda akan mampu:

- Masuk ke Konsol Manajemen AWS

- Buat sistem file Amazon EFS.

- Login ke instans Amazon Elastic Compute Cloud (Amazon EC2) yang menjalankan Amazon Linux

- Pasang sistem file Anda ke instans EC2

- Periksa dan pantau kinerja sistem file Anda

<br/>
## Durasi

Dibutuhkan sekitar **20 menit** untuk menyelesaikan lab ini.

<br/>
## Pembatasan layanan AWS

Dalam lingkungan lab ini, akses ke layanan AWS dan tindakan layanan mungkin dibatasi untuk orang-orang yang diperlukan untuk menyelesaikan instruksi lab. Anda mungkin akan mengalami error jika mencoba mengakses layanan lain atau melakukan tindakan di luar yang dijelaskan di lab ini.

<br/>

## Mengakses Konsol Manajemen AWS

1. Di bagian atas instruksi ini, pilih <span id="ssb_voc_grey">Start Lab</span> (Mulai Lab) untuk meluncurkan lab Anda.

   Panel **Start Lab** (Mulai Lab) terbuka dan menampilkan status lab.

   <i class="fas fa-info-circle"></i> **Tip**: Jika Anda memerlukan lebih banyak waktu untuk menyelesaikan lab, mulai ulang pengatur waktu untuk lingkungan dengan memilih tombol <span id="ssb_voc_grey">Start Lab</span> (Mulai Lab) lagi.

2. Tunggu hingga panel **Start Lab** (Mulai Lab) menampilkan pesan *Lab status: ready* (Status lab: siap), lalu tutup panel dengan memilih **X**.

3. Di bagian atas instruksi ini, pilih <span id="ssb_voc_grey">AWS</span>.

   Tindakan ini akan membuka Konsol Manajemen AWS di tab browser baru. Anda akan masuk ke sistem secara otomatis.

   <i class="fas fa-exclamation-triangle"></i> **Tip**: Jika tab browser baru tidak terbuka, banner atau ikon biasanya berada di bagian atas browser Anda dengan pesan bahwa browser Anda mencegah situs membuka jendela sembulan. Pilih banner atau ikon, lalu pilih **Allow pop-ups** (Izinkan sembulan).

4. Atur tab **AWS Management Console**(Konsol Manajemen AWS) agar ditampilkan bersama instruksi ini. Idealnya, Anda perlu membuka kedua tab browser secara bersamaan, sehingga Anda dapat mengikuti langkah-langkah lab dengan lebih mudah.

   <i class="fas fa-exclamation-triangle"></i> **Jangan mengubah Wilayah kecuali secara khusus diperintahkan untuk melakukannya**.

<br/>

## Tugas 1: Membuat grup keamanan untuk mengakses sistem file EFS

Grup keamanan yang Anda kaitkan dengan target pemasangan *harus mengizinkan akses masuk untuk TCP pada port 2049 untuk Network File System (NFS)*. Ini adalah grup keamanan yang akan Anda buat, konfigurasi, dan lampirkan ke target pemasangan EFS.


5. Di **AWS Management Console** (Konsol Manajemen AWS), pada menu <span id="ssb_services">Services</span> (Layanan), pilih **EC2**.

6. Di panel navigasi sebelah kiri, pilih **Security Groups** (Grup Keamanan).

7. Salin **ID grup keamanan** dari grup keamanan *EFSClient* ke editor teks Anda.

   ID Grup harus terlihat serupa dengan *sg-03727965651b6659b*.

8. Pilih <span id="ssb_orange">Create security group</span> (Buat grup keamanan), lalu konfigurasikan:

   <a id='securitygroup'></a>

   * **Security group name:** `EFS Mount Target` (Target Pemasangan EFS)
   * **Description:** `Inbound NFS access from EFS clients` (Akses NFS masuk dari klien EFS)
   * **VPC:** *Lab VPC*

9. Di bawah bagian **Inbound rules** (Aturan masuk), pilih <span id="ssb_white">Add rule</span> (Tambah aturan), lalu konfigurasikan:

   * **Type:** *NFS*
   * **Source** (Sumber):
      * *Custom* (Kustom)
      * Di kotak *Custom* (Kustom), tempel **ID grup keamanan** dari grup keamanan yang disalin ke editor teks Anda
   * Pilih <span id="ssb_orange">Create security group</span> (Buat grup keamanan).

<br/>

## Tugas 2: Membuat sistem file EFS

Sistem file EFS dapat dipasang ke beberapa instans EC2 yang berjalan di Availability Zone yang berbeda di Wilayah yang sama. Instans ini menggunakan *target pemasangan* yang dibuat di setiap *Availability Zone* untuk memasang sistem file dengan menggunakan semantik NFSv4.1 standar. Anda dapat memasang sistem file pada instans hanya dalam satu virtual private cloud (VPC) pada satu waktu. Sistem file dan VPC harus berada di Wilayah yang sama.


10. Pada menu <span id="ssb_services">Services</span> (Layanan), pilih **EFS**.

11. Pilih <span id="ssb_orange">Create file system</span> (Buat sistem file)

12. Di jendela **Create file system** (Buat sistem file), pilih <span id="ssb_white">Customize</span> (Kustomisasi)

13. Pada **Step 1** (Langkah 1):

   - Hapus centang <i class="far fa-square"></i> Enable automatic backups (Aktifkan cadangan otomatis).
   - **Lifecycle management:** Pilih *None* (Tidak Ada)
   - Di bagian **Tags** (Tag), konfigurasikan:
      - **Key** (Kunci): `Name` (Nama)
      - **Value:** `My First EFS File System` (Sistem File EFS Pertama Saya)

14. Pilih <span id="ssb_orange">Next</span> (Berikutnya)

15. Untuk **VPC**, pilih *Lab VPC*.

16. Lepaskan grup keamanan default dari setiap target pemasangan *Availability Zone* dengan mencentang kotak <i class="fas fa-times"></i> pada setiap grup keamanan default.

17. Lampirkan grup keamanan **Target Pemasangan EFS** ke setiap target pemasangan *Availability Zone* dengan:

* Mencentang setiap kotak **Security groups** (Grup keamanan).
* Memilih **EFS Mount Target** (Target Pemasangan EFS).

   Target pemasangan dibuat untuk setiap subnet.

   Target pemasangan Anda akan terlihat seperti contoh berikut. Diagram menunjukkan dua target pemasangan di **Lab VPC** yang menggunakan grup keamanan **EFS Mount Target** (Target Pemasangan EFS). Di lab ini, Anda harus menggunakan Lab VPC.

<img src="images/mount-targets-security-groups.png" alt="Grup Keamanan Target" width="600" >

18. Pilih <span id="ssb_orange">Next</span> (Berikutnya)

19. Pada **Step 3** (Langkah 3), pilih <span id="ssb_orange">Next</span> (Berikutnya)

20. Pada **Step 4:**

* Tinjau konfigurasi Anda.
* Pilih <span id="ssb_orange">Create</span> (Buat)

<i class="far fa-thumbs-up"></i> Selamat! Anda telah menciptakan sistem file EFS baru di Lab VPC dan target pemasangan di setiap subnet Lab VPC. Dalam beberapa detik, **File system state** (Status sistem file) dari sistem file akan berubah menjadi *Available* (Tersedia), diikuti oleh target pemasangan 2-3 menit kemudian.

Lanjutkan ke langkah berikutnya setelah **Mount target state** (Status target pemasangan) untuk setiap perubahan target pemasangan ke *Available* (Tersedia). Pilih tombol segarkan layar setelah 2-3 menit untuk memeriksa kemajuannya.

<br/>

## Tugas 3: Menghubungkan ke instans EC2 Anda melalui SSH

Dalam tugas ini, Anda akan terhubung ke instans EC2 Anda dengan menggunakan Secure Shell (SSH).

### <i class="fab fa-windows"></i> Pengguna Microsoft Windows

<i class="fas fa-comment"></i> Instruksi ini khusus untuk pengguna Microsoft Windows. Jika Anda menggunakan macOS atau Linux, <a href="#ssh-MACLinux">lompat ke bagian berikutnya</a>.
​

21. Di atas instruksi yang sedang Anda baca ini, pilih menu tarik-turun <span id="ssb_voc_grey">Details</span> (Detail), lalu pilih <span id="ssb_voc_grey">Show</span> (Tampilkan)

Jendela **Credentials** (Kredensial) akan terbuka.

22. Pilih tombol **Download PPK** (Unduh PPK), dan simpan file **labsuser.ppk**.

**Catatan:** Biasanya, browser Anda akan menyimpan file ke direktori **Downloads** (Unduhan).

23. Catatan alamat **EC2PublicIP**, jika ditampilkan.

24. Keluar dari panel **Details** (Detail) dengan memilih **X**.

25. Untuk menggunakan SSH untuk mengakses instans EC2, Anda harus menggunakan ***\*PuTTY\****. Jika Anda belum menginstal PuTTY di komputer, <a href="https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe">download PuTTY</a> (unduh PuTTY).

26. Buka **putty.exe**.

27. Konfigurasikan waktu henti PuTTY agar sesi PuTTY tetap terbuka dalam jangka waktu yang lebih lama:

* Pilih **Connection** (Koneksi)
* **Seconds between keepalives** (Detik antara keepalive): `30`

28. Konfigurasikan sesi PuTTY Anda dengan menggunakan pengaturan berikut.

* Pilih **Session** (Sesi)
* **Host Name(or IP address):** Tempel **EC2PublicIP** untuk instans yang Anda catat sebelumnya
   * Atau, kembali ke konsol Amazon EC2 dan pilih **Instances** (Instans)
   * Pilih instans yang ingin Anda sambungkan
   * Di tab *Description* (Deskripsi), salin nilai **IPv4 Public IP**
* Kembali ke PuTTY, dalam daftar **Connection** (Koneksi), perluas <i class="far fa-plus-square"></i> **SSH**
* Pilih **Auth** (tapi jangan memperluasnya)
* Pilih **Browse** (Jelajahi)
* Jelajahi file *labsuser.ppk* yang Anda unduh, pilih file tersebut, lalu pilih **Open** (Buka)
* Pilih **Open** (Buka) lagi


29. Untuk memercayai dan terhubung ke host, pilih **Yes** (Ya).

30. Saat muncul pertanyaan **login as** (login sebagai), masukkan: `ec2-user`

   Tindakan ini menghubungkan Anda ke instans EC2.

31. Pengguna Microsoft Windows: <a href="#ssh-after">Pilih tautan ini untuk lompat ke tugas berikutnya.</a>



<a id='ssh-MACLinux'></a>

### Pengguna macOS <span style="font-size: 30px; color: #808080;"><i class="fab fa-apple"></i></span> dan Linux <span style="font-size: 30px; "><i class="fab fa-linux"></i></span>

Instruksi ini khusus untuk pengguna macOS atau Linux. Jika Anda pengguna Windows, <a href="#ssh-after">lompat ke tugas berikutnya.</a>

32. Di atas instruksi yang sedang Anda baca ini, pilih menu tarik-turun <span id="ssb_voc_grey">Details</span> (Detail), lalu pilih <span id="ssb_voc_grey">Show</span> (Tampilkan)

   Jendela **Credentials** (Kredensial) akan terbuka.

33. Pilih tombol **Download PEM** (Unduh PEM) dan simpan file **labsuser.pem**.

34. Catatan alamat **EC2PublicIP**, jika ditampilkan.

35. Keluar dari panel **Details** (Detail) dengan memilih **X**.

36. Buka jendela terminal, dan ubah direktori ke direktori tempat file *labsuser.pem* diunduh dengan menggunakan perintah `cd`.

    Misalnya, jika file *labsuser.pem* disimpan ke direktori **Downloads** (Unduhan), jalankan perintah ini:

    ```bash
    cd ~/Downloads
    ```

37. Ubah izin pada kunci menjadi hanya baca, dengan menjalankan perintah ini:

    ```bash
    chmod 400 labsuser.pem
    ```

38. Jalankan perintah berikut (ganti **<public-ip\>** dengan alamat **EC2PublicIP** yang Anda disalin sebelumnya).

   * Atau, untuk menemukan alamat IP instans on premise, kembali ke konsol Amazon EC2 dan pilih **Instances** (Instans)
   * Pilih instans yang ingin Anda sambungkan
   * Di tab **Description** (Deskripsi), salin nilai **IPv4 Public IP**

     ```bash
     ssh -i labsuser.pem ec2-user@<public-ip>
     ```

39. Saat Anda diminta untuk mengizinkan koneksi pertama ke server SSH jarak jauh ini, masukkan: `yes` (ya)

   Karena Anda menggunakan pasangan kunci untuk autentikasi, Anda tidak akan diminta memasukkan sandi.

<a id='ssh-after'></a>

<br/>
## Tugas 4: Membuat direktori baru dan memasang sistem file EFS

<i class="fas fa-info-circle" aria-hidden="true"></i> Amazon EFS mendukung protokol NFSv4.1 dan NFSv4.0 ketika memasang sistem file Anda pada instans EC2. Meskipun NFSv4.0 didukung, kami sarankan Anda menggunakan NFSv4.1. Ketika memasang sistem file EFS pada instans EC2, Anda juga harus menggunakan klien NFS yang mendukung protokol NFSv4 pilihan Anda. Instans EC2 yang diluncurkan sebagai bagian dari lab ini mencakup klien NFSv4.1, yang sudah terinstal di dalamnya.


40. Kembali ke **Konsol Manajemen AWS**, pada menu <span id="ssb_services">Services</span> (Layanan), klik **EFS**.

41. Pilih **My First EFS File System** (Sistem File EFS Pertama Saya).

42. Di **Konsole Amazon EFS**, di sudut kanan atas halaman, pilih <span id="ssb_orange">Attach</span> (Lampirkan) untuk membuka instruksi pemasangan Amazon EC2.

43. Di sesi SSH Anda, ikuti instruksi dalam bagian **To set up your EC2 instance** (Menyiapkan instans EC2). Salin dan tempel (atau ketik secara manual) arahan perintah untuk menginstal utilitas yang diperlukan.

    Perintah harus terlihat serupa dengan contoh ini:

    ```
    sudo yum install -y amazon-efs-utils
    ```

    *Tip:* untuk menempelkan ke terminal di browser, tempatkan kursor Anda tepat di sebelah kanan command prompt dan klik kanan untuk melihat opsi tempel.

44. Gulir ke bawah ke bagian **Mounting your file system** (Memasang sistem file Anda).

45. Di sesi SSH Anda, buat direktori baru dengan memasukkan `sudo mkdir efs`

46. Salin seluruh perintah di bagian **Using the NFS client** (Menggunakan klien NFS).

   Perintah pasang harus terlihat serupa dengan contoh ini:

    `sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-bce57914.efs.us-west-2.amazonaws.com:/ efs`

    <i class="fas fa-comment" aria-hidden="true"></i> Perintah `sudo mount...` yang disediakan menggunakan opsi pemasangan Linux default.

47. Di sesi Linux SSH Anda, pasang sistem file Amazon EFS Anda dengan:

   * Menempelkan perintah
   * Menekan ENTER


48. Dapatkan rangkuman lengkap mengenai penggunaan ruang disk yang tersedia dan yang digunakan dengan memasukkan:

    `sudo df -hT`

    Tangkapan layar berikut ini adalah contoh output dari perintah *disk filesystem* berikut: 

    `df -hT`

    Perhatikan *Jenis* dan *Ukuran* dari sistem file EFS terpasang Anda.

<img src="images/disk-space.png" alt="ruang disk" width="600">


<br/>
## Tugas 5: Memeriksa perilaku kinerja sistem file EFS baru


### Memeriksa kinerja dengan menggunakan Fleksibel IO

<i class="fas fa-info-circle"></i> Fleksibel IO (fio) adalah utilitas tolok ukur I/O sintetis untuk Linux. Hal ini digunakan sebagai patokan dan untuk menguji subsistem I/O Linux. Selama boot, *fio* secara otomatis diinstal pada instans EC2 Anda.

49. Periksa karakteristik kinerja tulis dari sistem file Anda dengan memasukkan:

    ```
    sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio
    ```

    <i class="fas fa-comment"></i> Perintah `fio` memerlukan waktu 5-10 menit untuk diselesaikan. Output akan terlihat seperti contoh dalam tangkapan layar berikut: Pastikan bahwa Anda memeriksa output perintah `fio` Anda, khususnya informasi status rangkuman untuk tes WRITE ini.
    
    <img src="images/fio.png" alt="fio" width="600" >

<br/>
## Memantau kinerja dengan menggunakan Amazon CloudWatch

50. Di **Konsol Manajemen AWS**, di menu <span id="ssb_services">Services</span> (Layanan), pilih **CloudWatch**.

51. Di panel navigasi di sebelah kiri, pilih **Metrics** (Metrik).

52. Di tab **All metrics** (Semua metrik), pilih **EFS**.

53. Pilih **File System Metrics** (Metrik Sistem File).

54. Pilih baris yang memiliki Nama Metrik **PermittedThroughput**.

   <i class="fas fa-comment"></i> Anda mungkin harus menunggu 2-3 menit dan menyegarkan layar beberapa kali sebelum semua metrik yang tersedia, termasuk **PermittedThroughput**, menghitung dan mengisi.

55. Pada grafik, pilih dan seret di sekitar baris data. Jika Anda tidak melihat grafik garis, sesuaikan rentang waktu grafik untuk menampilkan periode selama Anda menjalankan perintah `fio`.

<img src="images/graph.png" alt="pilih seret" width="600" >


56. Jeda pointer Anda pada baris data dalam grafik. Nilai harus *105M*.

<img src="images/throughput.png" alt="Throughput" width="600" >


   Throughput Amazon EFS meningkat saat sistem file tumbuh. Beban kerja berbasis file biasanya berubah-ubah. Hasil tersebut mendorong tingkat throughput yang tinggi untuk jangka waktu yang singkat, dan tingkat throughput yang rendah selama sisa waktu. Karena perilaku ini, Amazon EFS dirancang untuk melonjak ke tingkat throughput tinggi selama jangka waktu tertentu. Semua sistem file, terlepas dari ukurannya, dapat mengalami lonjakan throughput hingga 100 MiB/s. Untuk informasi lebih lanjut tentang karakteristik kinerja sistem file EFS Anda, baca <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">dokumentasi Amazon Elastic File System</a> yang resmi.

57. Di tab **All metrics** (Semua metrik), *hapus centang* pada kotak **PermittedThroughput**.

58. Centang kotak untuk **DataWriteIOBytes**.

   <i class="fas fa-comment"></i> Jika Anda tidak melihat *DataWriteIOBytes* dalam daftar metrik, gunakan pencarian **File System Metrics** (Metrik Sistem File) untuk menemukannya.

59. Pilih tab **Graphed metrics** (Metrik dengan grafik).

60. Pada kolom **Statistics** (Statistik), pilih **Sum** (Jumlah).

61. Pada kolom **Period** (Periode), pilih **1 Minute** (1 Menit).

62. Jeda pointer Anda pada puncak grafik garis. Ambil nomor ini (dalam byte) dan bagilah dengan durasi dalam detik (60 detik). Hasilnya memberi Anda throughput tulis (B/s) dari sistem file Anda selama tes Anda.

<img src="images/Sum-1-minute.png" alt="Jumlah 1 Menit" width="600" >

   Throughput yang tersedia untuk sistem file meningkat saat sistem file tumbuh. Semua sistem file memberikan kinerja dasar yang konsisten sebesar 50 MiB/s per TiB penyimpanan. Selain itu, semua sistem file (terlepas dari ukurannya) dapat melonjak hingga 100 MiB/s. Sistem file yang lebih besar dari 1 TB dapat melonjak hingga 100 MiB/s per TiB penyimpanan. Saat Anda menambahkan data ke sistem file, throughput maksimum yang tersedia untuk sistem file akan melakukan meningkat secara linier dan secara otomatis sesuai penyimpanan Anda.

   Throughput sistem file dibagikan ke semua instans EC2 yang tersambung dengan sistem file. Untuk informasi lebih lanjut tentang karakteristik kinerja sistem file EFS Anda, baca <a href="http://docs.aws.amazon.com/efs/latest/ug/performance.html" target="_blank">dokumentasi Amazon Elastic File System</a> yang resmi.

   <i class="far fa-thumbs-up" style="color:blue"></i> Selamat! Anda telah membuat sistem file EFS, memasangnya ke instans EC2, dan menjalankan uji tolok ukur I/O untuk memeriksa karakteristik kinerjanya.


<br/>
## Mengirimkan pekerjaan Anda

63. Di bagian atas instruksi ini, pilih <span id="ssb_blue">Submit</span> (Kirim) untuk merekam kemajuan Anda dan saat diminta, pilih **Yes** (Ya).

64. Jika hasilnya tidak muncul setelah beberapa menit, kembali ke bagian atas instruksi ini dan pilih <span id="ssb_voc_grey">Grades</span> (Nilai)

   **Tip**: Anda dapat mengirimkan pekerjaan Anda beberapa kali. Setelah Anda mengubah pekerjaan, pilih **Submit** (Kirim) lagi. Apa yang akan direkam untuk lab ini adalah pengiriman terakhir Anda.

65. Untuk menemukan detail umpan balik tentang pekerjaan Anda, pilih <span id="ssb_voc_grey">Details</span> (Detail) diikuti oleh <i class="fas fa-caret-right"></i> **View Submission Report** (Lihat Laporan Pengiriman).


<br/>
## Lab selesai <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Selamat! Anda telah menyelesaikan lab.


66. Pilih <span id="ssb_voc_grey">End Lab</span> (Akhiri Lab) di bagian atas halaman ini, lalu pilih <span id="ssb_blue">Yes</span> (Ya) untuk mengonfirmasi bahwa Anda ingin mengakhiri lab.

   Panel akan muncul dengan pesan ini: *DELETE telah dimulai... (Penghapusan telah dimulai...) You may close this message box now.* (Anda dapat menutup kotak pesan ini sekarang.)

67. Pilih **X** di sudut kanan atas untuk menutup panel.





*©2020 Amazon Web Services, Inc. dan afiliasinya. Hak cipta dilindungi undang-undang. Karya ini tidak boleh direproduksi atau didistribusikan ulang, seluruhnya atau sebagian, tanpa izin tertulis sebelumnya dari Amazon Web Services, Inc. Dilarang menyalin, meminjamkan, atau menjual secara komersial.*