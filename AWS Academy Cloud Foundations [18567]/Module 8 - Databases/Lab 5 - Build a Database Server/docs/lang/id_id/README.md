# Lab 5: Membangun Server DB Anda dan Berinteraksi dengan DB Anda Menggunakan Aplikasi

<!-- Note to translators: This is based on Technical Essentials Lab 2. Copy the translation from there. Do not re-translate the whole document. -->

&nbsp;&nbsp;

**Versi 4.6.6 (TESS2)**

Lab ini dirancang untuk memperkuat konsep memanfaatkan instans basis data yang dikelola AWS untuk menyelesaikan kebutuhan basis data relasional.

***Amazon Relational Database Service*** (Amazon RDS) memudahkan penyiapan, pengoperasian, dan penskalaan basis data relasional di cloud. Layanan ini menyediakan kapasitas yang hemat biaya dan dapat diubah ukurannya sambil mengelola tugas administrasi basis data yang memakan waktu, sehingga Anda dapat berfokus pada aplikasi dan bisnis Anda. Amazon RDS memberi Anda enam opsi mesin basis data umum: Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL, dan MariaDB.

&nbsp;

**Tujuan**

Setelah menyelesaikan lab ini, Anda dapat:

- Meluncurkan instans DB Amazon RDS dengan ketersediaan tinggi.
- Mengonfigurasi instans DB untuk mengizinkan koneksi dari server web Anda.
- Membuka aplikasi web dan berinteraksi dengan basis data Anda.

&nbsp;

**Durasi**

Diperlukan sekitar **30 menit** untuk menyelesaikan lab ini.

&nbsp;

**Skenario**

Anda mulai dengan infrastruktur berikut:
<img src="../../images/lab-5-starting-lab-architecture.png" width="800">

&nbsp;&nbsp;


Di akhir lab, ini adalah infrastrukturnya:

&nbsp;

<img src="../../images/lab-5-final-lab-architecture.png" width="800">

&nbsp;&nbsp;
___
## Mengakses AWS Management Console

1. Di bagian atas instruksi ini, klik <span id="ssb_voc_grey">Start Lab</span> (Mulai Lab) untuk meluncurkan lab Anda.

   Panel Mulai Lab terbuka dan menampilkan status lab.

2. Tunggu hingga Anda melihat pesan "**Lab status: ready** (Status lab: siap)", lalu klik **X** untuk menutup panel Mulai Lab.

3. Di bagian atas instruksi ini, klik <span id="ssb_voc_grey">AWS</span>

   Tindakan ini akan membuka AWS Management Console di tab browser baru. Anda akan masuk ke sistem secara otomatis.

   **Tips**: Jika tab browser baru tidak terbuka, biasanya akan ada banner atau ikon di bagian atas browser Anda yang menunjukkan bahwa browser Anda mencegah situs membuka jendela pop-up. Klik banner atau ikon dan pilih "Allow pop ups" ("Izinkan pop up").

4. Atur tab AWS Management Console agar ditampilkan bersama instruksi ini. Idealnya, Anda akan dapat melihat kedua tab browser sekaligus, untuk mempermudah mengikuti langkah-langkah lab.

&nbsp;
___
## Tugas 1: Membuat Grup Keamanan untuk Instans DB RDS

Dalam tugas ini, Anda akan membuat grup keamanan untuk mengizinkan server web Anda mengakses instans DB RDS Anda. Grup keamanan akan digunakan ketika Anda meluncurkan instans basis data.

5. Di **AWS Management Console**, pada menu <span id="ssb_services">Services <i class="fas fa-angle-down"></i></span> (Layanan), klik **VPC**.

6. Di panel navigasi kiri, klik **Security Groups** (Grup Keamanan).

7. Klik <span id="ssb_blue">Create security group</span> (Buat grup keamanan), lalu konfigurasi:

   - **Security group name:** (Nama grup keamanan:) `DB Security Group` (Grup Keamanan DB)
   - **Description:** (Deskripsi:) `Permit access from Web Security Group` (Izinkan akses dari Grup Keamanan Web)
   - **VPC:** _Lab VPC_

8. Klik <span id="ssb_blue">Create</span> (Buat), lalu klik <span id="ssb_blue">Close</span> (Tutup)

   Sekarang Anda akan menambahkan aturan ke grup keamanan untuk mengizinkan permintaan basis data masuk.

9. Pilih <i class="far fa-check-square"></i> **DB Security Group** (Grup Keamanan DB).

10. Klik tab **Inbound Rules** (Aturan Masuk).

   Grup keamanan saat ini tidak memiliki aturan. Anda akan menambahkan aturan untuk mengizinkan akses dari _Web Security Group_ (Grup Keamanan Web).

11. Klik <span id="ssb_grey">Edit rules</span> (Edit aturan)

12. Klik <span id="ssb_grey">Add Rule</span> (Tambahkan aturan), lalu konfigurasikan:

   - **Type:** (Jenis) _MySQL/Aurora (3306)_
   - **CIDR, IP, Security Group or Prefix List:** (CIDR, IP, Grup Keamanan, atau Daftar Prefiks) Ketik `sg`, lalu pilih _Web Security Group_ (Grup Keamanan Web).

   Ini mengonfigurasi Grup keamanan basis data untuk mengizinkan lalu lintas masuk pada port 3306 dari instans EC2 apa pun yang terkait dengan _Web Security Group_ (Grup Keamanan Web).

13. Klik <span id="ssb_blue">Save rules</span> (Simpan aturan), lalu klik <span id="ssb_blue">Close</span> (Tutup)

   Anda akan menggunakan grup keamanan ini ketika meluncurkan basis data Amazon RDS.

&nbsp;
___
## Tugas 2: Membuat Grup Subnet DB

Dalam tugas ini, Anda akan membuat _grup subnet DB_ yang digunakan untuk memberi tahu RDS subnet mana yang dapat digunakan untuk basis data. Setiap grup subnet DB memerlukan subnet dalam setidaknya dua Availability Zone.

14. Pada menu <span id="ssb_services">Services <i class="fas fa-angle-down"></i></span> (Layanan), klik **RDS**.

15. Di panel navigasi kiri, klik **Subnet groups** (Grup subnet).

   <i class="fas fa-exclamation-triangle"></i> Jika panel navigasi tidak terlihat, klik ikon menu <i class="fas fa-bars"></i> di sudut kiri atas.

16. Klik <span id="ssb_orange">Create DB Subnet Group</span> (Buat Grup Subnet DB), lalu konfigurasikan:

   - **Name:** (Nama:) `DB-Subnet-Group`
   - **Description:** (Deskripsi:) `DB Subnet Group` (Grup Subnet DB)
   - **VPC:** _Lab VPC_

17. Gulir ke bawah ke bagian **Add Subnets** (Tambahkan Subnet).

18. Perluas daftar nilai di bawah **Availability Zone** dan pilih dua zona pertama: **us-east-1a** dan **us-east-1b**.

19. Perluas daftar nilai di bawah **Subnets** (Subnet) dan pilih subnet yang terkait dengan rentang CIDR **10.0.1.0/24** dan **10.0.3.0/24**.

   Subnet ini seharusnya sekarang ditampilkan di tabel **Subnets selected** (**Subnet yang dipilih**).

20. Klik <span id="ssb_orange">Create</span> (Buat)

   Anda akan menggunakan grup subnet DB ini ketika membuat basis data di tugas selanjutnya.

&nbsp;
___
## Tugas 3: Membuat Instans DB Amazon RDS

Dalam tugas ini, Anda akan mengonfigurasi dan meluncurkan instans basis data Amazon RDS for MySQL Multi-AZ.

***Multi-AZ*** deployment Amazon RDS memberikan peningkatan ketersediaan dan daya tahan untuk instans Basis data (DB), menjadikan instans tersebut sesuai untuk beban kerja basis data produksi. Ketika Anda menyediakan Instans DB Multi-AZ, Amazon RDS secara otomatis membuat instans DB primer dan secara serentak mereplikasi data ke instans siaga di Availability Zone (AZ) yang berbeda.

21. Di panel navigasi kiri, klik **Databases** (Basis data).

22. Klik <span id="ssb_orange">Create database</span> (Buat basis data)

   <i class="fas fa-exclamation-triangle"></i> Jika Anda melihat **Switch to the new database creation flow** (Alihkan ke alur pembuatan basis data baru) di bagian atas layar, silakan klik.

23. Pilih <i class="far fa-dot-circle"></i> **MySQL**.

24. Dalam **Settings** (Pengaturan), konfigurasikan:

   - **DB instance identifier:** (Pengidentifikasi instans DB) `lab-db`
   - **Master username:** (Nama pengguna master) `main` (utama)
   - **Master password:** (Kata sandi master) `lab-password`
   - **Confirm password:** (Konfirmasi kata sandi) `lab-password`

25. Dalam **DB instance size** (Ukuran instans DB), konfigurasikan:

   - Pilih <i class="far fa-dot-circle"></i> **Burstable classes (includes t classes)** (Kelas Burstable (termasuk kelas t)).
   - Pilih _db.t3.micro_

26. Dalam **Storage** (Penyimpanan), konfigurasikan:

   - **Storage type:** (Jenis penyimpanan) _General Purpose (SSD)_ (Tujuan Umum (SSD))
   - **Allocated storage:** (Alokasi penyimpanan) _20_

27. Dalam **Connectivity** (Konektivitas), konfigurasikan:

   - **Virtual Private Cloud (VPC):** _Lab VPC_

28. Perluas <i class="fas fa-caret-right"></i> **Additional connectivity configuration** (Konfigurasi konektivitas tambahan), lalu konfigurasikan:

   - Untuk **Existing VPC security groups:** (Grup keamanan VPC yang ada) klik _DB Security Group_ (Grup Keamanan DB) untuk menyorotnya dengan warna biru.

29. Perluas <i class="fas fa-caret-right"></i> **Additional configuration** (Konfigurasi tambahan), lalu konfigurasikan:

   - **Initial database name:** (Nama basis data awal:) `lab`
   - Hapus centang **Enable automatic backups** (Aktifkan pencadangan otomatis).
   - Hapus centang **Enable Enhanced monitoring** (Aktifkan Peningkatan pemantauan).

   <i class="fas fa-comment"></i> Ini akan menonaktifkan pencadangan, yang biasanya tidak disarankan, tetapi akan membuat basis data diterapkan dengan lebih cepat untuk lab ini.

30. Klik <span id="ssb_orange">Create database</span> (Buat basis data)

   Basis data Anda sekarang akan diluncurkan.

   <i class="fas fa-comment"></i> Jika Anda menerima kesalahan yang menyebutkan "not authorized to perform: iam:CreateRole" ("tidak diizinkan untuk menjalankan: iam:CreateRole", pastikan Anda menghapus centang _Enable Enhanced monitoring_ (Aktifkan peningkatan pemantauan) di langkah sebelumnya.

31. Klik **lab-db** (klik tautan itu sendiri).

   Anda sekarang akan perlu menunggu **sekitar 4 menit** agar basis data tersedia. Proses deployment sedang men-deploy basis data dalam dua Availability zone yang berbeda.

   <i class="fas fa-info-circle"></i> Sambil menunggu, Anda mungkin ingin meninjau [Amazon RDS FAQ](https://aws.amazon.com/rds/faqs/) atau menyeduh secangkir kopi.

32. Tunggu hingga **Info** berubah menjadi **Modifying** (Memodifikasi) atau **Available** (Tersedia).

33. Gulir ke bawah ke bagian **Connectivity & security** (Konektivitas & keamanan) dan salin bidang **Endpoint** (Titik akhir).

   Ini akan mirip dengan: _lab-db.cggq8lhnxvnv.us-west-2.rds.amazonaws.com_

34. Tempel nilai Titik akhir ke editor teks. Anda nanti akan memerlukannya di lab.

&nbsp;
___
## Tugas 4: Berinteraksi dengan Basis Data Anda

Dalam tugas ini, Anda akan membuka aplikasi web yang berjalan pada server web Anda dan mengonfigurasinya untuk menggunakan basis data.

35. Untuk menyalin alamat IP **WebServer**, klik menu drop down <span id="ssb_voc_grey">Details</span> (Detail) di atas instruksi ini, lalu klik <span id="ssb_voc_grey">Show</span> (Tampilkan).

36. Buka tab browser web baru, tempelkan alamat IP _WebServer_, dan tekan Enter.

   Aplikasi web akan ditampilkan, yang menunjukkan informasi tentang instans EC2.

37. Klik tautan **RDS** di bagian atas halaman.

   Sekarang Anda akan mengonfigurasi aplikasi untuk terhubung ke basis data Anda.

38. Konfigurasikan pengaturan berikut ini:

   - **Endpoint:** (Titik akhir:) Tempel Titik akhir yang Anda salin ke editor teks sebelumnya
   - **Database:** (Basis data:) `lab`
   - **Username:** (Nama pengguna:) `main` (utama)
   - **Password:** (Kata sandi:) `lab-password`
   - Klik **Submit** (Kirim)

   Pesan akan muncul dan menjelaskan bahwa aplikasi sedang menjalankan perintah untuk menyalin informasi ke basis data. Setelah beberapa saat, aplikasi akan menampilkan **Address Book** (Buku Alamat).

   Aplikasi Buku Alamat menggunakan basis data RDS untuk menyimpan informasi.

39. Uji aplikasi web dengan menambahkan, mengedit, dan menghapus kontak.

   Data sedang disimpan ke basis data dan secara otomatis mereplikasi ke Availability Zone kedua.

&nbsp;
___
## Lab Selesai

<i class="icon-flag-checkered"></i> Selamat! Anda telah menyelesaikan lab.

40. Klik <span id="ssb_voc_grey">End Lab</span> (Akhiri Lab) di bagian atas halaman ini, lalu klik <span id="ssb_blue">Yes</span> (Ya) untuk mengonfirmasi bahwa Anda ingin mengakhiri lab.

   Panel akan muncul, menampilkan pesan "DELETE has been initiated... You may close this message box now" ("PENGHAPUSAN sudah dimulai... Anda dapat menutup kotak pesan ini sekarang".)

41. Klik **X** di sudut kanan atas untuk menutup panel.

Untuk tanggapan, saran, atau koreksi, silakan kirim email kepada kami di: *aws-course-feedback@amazon.com*

&nbsp;
___
### Atribusi

**Bootstrap v3.3.5 - [http://getbootstrap.com](http://getbootstrap.com "http://getbootstrap.com**/")

Lisensi MIT (MIT)

Hak Cipta (c) 2011-2016 Twitter, Inc.

Dengan ini izin diberikan, secara gratis, kepada siapa pun yang memperoleh salinan perangkat lunak ini dan file dokumentasi terkait ("Perangkat Lunak"), untuk bertransaksi dengan Perangkat Lunak tanpa larangan, termasuk tanpa batasan pada hak untuk menggunakan, menyalin, memodifikasi, menggabungkan, memublikasikan, mendistribusikan, mensublisensikan, dan/atau menjual salinan Perangkat Lunak, dan untuk mengizinkan orang yang diberi Perangkat Lunak tersebut untuk melakukannya, tunduk pada ketentuan berikut:

Pemberitahuan hak cipta di atas dan pemberitahuan izin ini akan disertakan dalam semua salinan atau sebagian besar Perangkat Lunak.

PERANGKAT LUNAK DIBERIKAN "APA ADANYA", TANPA JAMINAN APA PUN, BAIK TERSURAT MAUPUN TERSIRAT, TERMASUK TETAPI TIDAK TERBATAS PADA JAMINAN YANG DAPAT DIPERDAGANGKAN, KESESUAIAN UNTUK TUJUAN DAN NONPELANGGARAN TERTENTU. DALAM KEADAAN APA PUN PENULIS ATAU PEMEGANG HAK CIPTA TIDAK BERTANGGUNG JAWAB ATAS KLAIM, KERUSAKAN, ATAU TANGGUNG JAWAB LAINNYA, BAIK DALAM TINDAKAN KONTRAK, KESALAHAN, MAUPUN SEBALIKNYA, YANG TIMBUL DARI, AKIBAT, ATAU SEHUBUNGAN DENGAN PERANGKAT LUNAK ATAU PENGGUNAAN ATAU PUN TRANSAKSI LAIN DALAM PERANGKAT LUNAK.
