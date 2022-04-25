# Modul 3 — Lab Terpandu: Meng-hosting Situs Web Statis
[//]: # "SKU: ILT-TF-200-ACACAD-2    Source Course: ILT-TF-100-ARCHIT-6 branch dev_65"

## Gambaran umum dan tujuan lab
Situs web statis memiliki konten tetap tanpa pemrosesan backend. Situs web itu dapat berisi halaman HTML, gambar, style sheet, dan semua file yang diperlukan untuk membuat situs web. Namun, situs web statis tidak menggunakan scripting sisi server atau basis data. Jika ingin halaman web statis Anda menyediakan interaktivitas dan menjalankan logika pemrograman, Anda dapat menggunakan JavaScript yang berjalan di browser web pengguna.

Anda dapat dengan mudah meng-hosting situs web statis di Amazon Simple Storage Service (Amazon S3) dengan mengunggah konten yang diinginkan dan menjadikannya dapat diakses oleh publik. Server tidak diperlukan, Anda dapat menggunakan Amazon S3 untuk menyimpan dan mengambil data dalam jumlah berapa pun, kapan pun, dari mana pun di web.

Setelah menyelesaikan lab ini, Anda akan mampu:

- Buat bucket di Amazon S3
- Unggah konten ke bucket Anda
- Aktifkan akses ke objek bucket
- Memperbarui situs web

<br/>
## Durasi

Diperlukan waktu sekitar **20 menit** untuk menyelesaikan lab ini.

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
## Tugas 1: Membuat bucket di Amazon S3

Dalam tugas ini, Anda akan membuat sebuah bucket S3 dan mengonfigurasinya untuk hosting situs web statis.

5. Di **Konsol Manajemen AWS**, pada menu <span id="ssb_services">Service<i class="fas fa-angle-down"></i></span> (Layanan), klik **S3**.

6. Pilih <span id="ssb_orange">Create bucket</span> (Buat bucket)

   Nama bucket S3 unik secara global, dan namespace-nya digunakan oleh semua akun AWS. Setelah Anda membuat bucket, nama bucket tidak dapat digunakan oleh akun AWS lain di Wilayah AWS mana pun kecuali Anda menghapus bucket.

   Karena itu, untuk lab ini, Anda akan menggunakan nama bucket dengan nomor acak, seperti: _website-123_

7. Untuk **Bucket name** (Nama bucket), masukkan: `website-<123>` (ganti <_123_> dengan nomor acak)

   Akses publik ke bucket diblokir secara default. Karena file di situs web statis Anda harus dapat diakses melalui internet, Anda harus mengizinkan akses publik.

8. Hapus centang **Block all public access** (Blokir semua akses publik), lalu pilih kotak yang menyatakan **I acknowledge that the current settings may result in this bucket and the objects within becoming public** (Saya mengetahui bahwa pengaturan saat ini dapat mengakibatkan bucket ini dan objek di dalamnya menjadi bersifat publik).

9. Pilih <span id="ssb_orange">Create bucket</span> (Buat bucket).

   Anda dapat menggunakan tanda untuk menambahkan informasi tambahan ke bucket, seperti kode proyek, pusat biaya, atau pemilik.

10. Pilih nama bucket baru Anda.

11. Pilih tab **Properties** (Properti).

12. Gulir ke panel **Tags** (Tag).

13. Pilih <span id="ssb_white">Edit</span>, lalu <span id="ssb_white">Add tag</span> (Tambahkan tanda) dan masukkan:

* **Key:** `Departement` (Departemen)
* **Value:** `Marketing` (Pemasaran)

14. Pilih <span id="ssb_orange">Save changes</span> (Simpan perubahan) untuk menyimpan tag.

Sekarang, Anda akan mengonfigurasi bucket untuk hosting situs web statis.

15. Tetap di konsol **Properties** (Properti).

16. Gulir ke panel **Static website hosting** (Hosting situs web statis).

17. Pilih <span id="ssb_white">Edit</span>

18. Konfigurasikan pengaturan berikut ini:

   - **Static web hosting:** Enable (Aktifkan)
   - **Hosting type:** Host a static website (Host situs web statis)
   - **Index document:** `index.html`
      - **Catatan**: Anda harus memasukkan nilai ini, meskipun sudah ditampilkan.
   - **Error document:** `error.html`

19. Pilih <span id="ssb_orange">Save changes</span> (Simpan perubahan)

20. Pilih tautan di bawah **Bucket website endpoint** (Endpoint situs web bucket).

   Anda akan menerima pesan *403 Forbidden* (403 Terlarang) karena izin bucket belum dikonfigurasi. Tetap buka tab ini di browser web, sehingga Anda bisa kembali lagi ke tab tersebut nanti.

   Bucket Anda kini telah dikonfigurasi untuk meng-hosting situs web statis.

<br/>
## Tugas 2: Mengunggah konten ke bucket Anda

Dalam tugas ini, Anda akan mengunggah file statis ke bucket Anda.

21. Klik kanan masing-masing tautan ini dan unduh file tersebut ke komputer Anda:

<i class="fas fa-exclamation-triangle"></i> Pastikan setiap file memiliki nama file yang sama, termasuk ekstensinya.

    - [index.html](../../../scripts/index.html)
    - [script.js](../../../scripts/script.js)
    - [style.css](../../../scripts/style.css)

22. Kembali ke konsol Amazon S3 dan klik tab **Objects** (Objek).

23. Pilih <span id="ssb_orange">Upload</span> (Unggah)

24. Pilih <span id="ssb_white">Add files</span> (Tambah file)

25. Pilih tiga file yang telah Anda unduh.

26. Pilih <i class="far fa-check-square"></i>I acknowledge that existing objects with the same name will be overwritten (Saya mengetahui bahwa objek yang ada dengan nama yang sama akan ditimpa).

27. Pilih <span id="ssb_orange">Upload</span> (Unggah)

File Anda diunggah ke bucket.

<br/>
## Tugas 3: Mengaktifkan akses ke objek

Objek yang disimpan di Amazon S3 secara default bersifat pribadi. Hal ini untuk memastikan bahwa data organisasi Anda tetap aman.

Dalam tugas ini, Anda akan membuat objek yang telah diunggah dapat diakses publik.

Pertama, konfirmasikan bahwa objek tersebut saat ini bersifat pribadi.

28. Kembali ke tab browser yang menunjukkan pesan *403 Forbidden* (403 Terlarang).

29. Segarkan <i class="fas fa-sync"></i>halaman web.

<i class="fas fa-comment"></i> Jika Anda tidak sengaja menutup tab ini, buka tab **Properties** (Properti), dan di panel **Static website hosting** (Hosting situs web statis) pilih tautan **Endpoint** lagi.

Anda akan tetap melihat pesan *403 Forbidden* (403 Terlarang). Tanggapan ini ditunggu! Hal ini menandakan bahwa situs web statis Anda sedang di-hosting oleh Amazon S3, tetapi kontennya bersifat pribadi.

Anda dapat membuat objek Amazon S3 bersifat publik melalui dua cara yang berbeda:

    - Untuk membuat publik baik seluruh bucket maupun direktori tertentu dalam bucket publik, gunakan *kebijakan bucket*.
    - Untuk membuat objek individual dalam bucket publik, gunakan *access control list (ACL)*.

Biasanya akan lebih aman membuat publik _objek individual_ untuk menghindari objek lain dijadikan publik secara tidak sengaja. Namun, jika Anda merasa bahwa seluruh bucket tidak mengandung informasi yang sensitif, Anda dapat menggunakan _kebijakan bucket_.

Sekarang Anda akan mengonfigurasikan objek individual agar bisa diakses publik.

30. Kembali ke tab browser web dengan konsol manajemen Amazon S3 (tetapi jangan tutup tab situs web).

31. Pilih <i class="far fa-check-square"></i>ketiga objek.

32. Di menu <span id="ssb_s3_blue">Actions<i class="fas fa-angle-down"></i></span> (Tindakan), pilih **Make public** (Buat publik).

Daftar ketiga objek ditampilkan.

33. Pilih <span id="ssb_s3_blue">Make public</span> (Buat publik)

Situs web statis Anda kini dapat diakses oleh publik.

34. Kembali ke tab browser web yang memiliki pesan *403 Forbidden* (403 Terlarang).

35. Segarkan <i class="fas fa-sync"></i>halaman web.

Anda akan melihat situs web statis yang di-hosting oleh Amazon S3.

<br/>
## Tugas 4: Memperbarui situs web

Anda dapat mengubah situs web dengan mengedit file HTML dan mengunggahnya lagi ke bucket S3.

Amazon S3 merupakan _layanan penyimpanan objek_, sehingga Anda perlu mengunggah seluruh file. Tindakan ini menggantikan objek yang ada di bucket Anda. Anda tidak dapat mengedit konten sebuah objek—tapi, keseluruhan objek harus diganti.

36. Pada komputer Anda, muat file **index.html** ke dalam editor teks (contohnya, Notepad atau TextEdit).

37. Temukan teks **Served from Amazon S3** (Disajikan dari Amazon S3) dan ganti dengan `Created by<YOUR-NAME>` (Dibuat oleh), yang mengganti <*YOUR-NAME*> dengan nama Anda (contohnya, _Created by Jane_).

38. Simpan file.

39. Kembali ke konsol Amazon S3 dan unggah file **index.html** yang baru saja diedit.

40. Pilih <i class="far fa-check-square"></i>**index.html** dan gunakan menu **Actions** (Tindakan)untuk memilih opsi **Make public** (Buat publik) lagi.

41. Kembali ke tab browser web dengan situs web statis, lalu segarkan <i class="fas fa-sync"></i>halaman.

Nama Anda seharusnya sekarang sudah ada di halaman.



Situs web statis Anda kini dapat diakses di internet. Karena di-hosting di Amazon S3, situs web memiliki ketersediaan tinggi dan mampu melayani volume lalu lintas yang tinggi tanpa menggunakan server apa pun.

Anda juga dapat menggunakan nama domain Anda sendiri untuk mengarahkan pengguna ke situs web statis yang di-hosting di Amazon S3. Untuk mencapai hal ini, Anda bisa menggunakan layanan Domain Name System (DNS) Amazon Route 53 dikombinasikan dengan Amazon S3.

<br/>
## Mengirimkan pekerjaan Anda

42. Di bagian atas instruksi ini, pilih <span id="ssb_blue">Submit</span> (Kirim) untuk merekam kemajuan Anda dan saat diminta, pilih **Yes** (Ya).

43. Jika hasilnya tidak muncul setelah beberapa menit, kembali ke bagian atas instruksi ini dan pilih <span id="ssb_voc_grey">Grades</span> (Nilai)

**Tip**: Anda dapat mengirimkan pekerjaan Anda beberapa kali. Setelah Anda mengubah pekerjaan, pilih **Submit** (Kirim) lagi. Apa yang akan direkam untuk lab ini adalah pengiriman terakhir Anda.

44. Untuk menemukan detail umpan balik tentang pekerjaan Anda, pilih <span id="ssb_voc_grey">Details</span> (Detail) diikuti oleh <i class="fas fa-caret-right"></i> **View Submission Report** (Lihat Laporan Pengiriman).

<br/>

## Lab selesai <i class="fas fa-graduation-cap"></i>

<i class="fas fa-flag-checkered"></i> Selamat! Anda telah menyelesaikan lab.

45. Pilih <span id="ssb_voc_grey">End Lab</span> (Akhiri Lab) di bagian atas halaman ini, lalu pilih <span id="ssb_blue">Yes</span> (Ya) untuk mengonfirmasi bahwa Anda ingin mengakhiri lab.

   Sebuah panel menunjukkan bahwa *DELETE has been initiated... * (Penghapusan sudah dimulai...) *You may close this message box now.* (Anda dapat menutup kotak pesan ini sekarang.)

46. Pilih **X** di sudut kanan atas untuk menutup panel.



*©2020 Amazon Web Services, Inc. dan afiliasinya. Hak cipta dilindungi undang-undang. Karya ini tidak boleh direproduksi atau didistribusikan ulang, seluruhnya atau sebagian, tanpa izin tertulis sebelumnya dari Amazon Web Services, Inc. Dilarang menyalin, meminjamkan, atau menjual secara komersial.*