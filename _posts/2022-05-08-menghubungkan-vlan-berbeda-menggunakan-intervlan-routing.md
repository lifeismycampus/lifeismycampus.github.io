---
layout: post
title: Menghubungkan VLAN Berbeda dengan InterVLAN Routing
date: 2022-05-08 00:00 +0000
author: angga
categories: [Konfigurasi]
tags: [cisco, routing, vlan]
---

## Pengertian

Routing adalah proses pengiriman data dari satu jaringan ke jaringan lain. Dalam hal ini (InterVLAN Routing), yang disebut sebagai jaringan yaitu suatu VLAN (pembagian jaringan secara logical).

![Life is My Campus](/assets/img/2022-05-08-menghubungkan-vlan-berbeda-menggunakan-intervlan-routing/04.png){: .normal }

Maka bisa disimpulkan bahwa sederhananya InterVLAN Routing adalah proses pengiriman data dari suatu VLAN ke VLAN lain, sehingga end device yang terdaftar sebagai anggota VLAN ID yang berbeda dapat saling terhubung.

Dalam jaringan yang lebih kompleks, proses routing akan menggunakan tabel routing (database) untuk menentukan rute terbaik berdasarkan alamat tujuan dalam paket data.

## Perangkat

Umumnya perangkat yang dapat digunakan untuk mendukung proses routing pada jaringan yaitu router.

![Cisco Router](/assets/img/2022-05-08-menghubungkan-vlan-berbeda-menggunakan-intervlan-routing/05.jpeg){: .normal }

Namun, untuk teknik InterVLAN routing dapat juga menggunakan perangkat multi-layer switch (switch dengan kemampuan layer 3).

![Cisco Switch](/assets/img/2022-05-08-menghubungkan-vlan-berbeda-menggunakan-intervlan-routing/06.jpg){: .normal }

Router akan digunakan sebagai gateway yang berperan sebagai gerbang keluar masuknya traffic data dari dan ke suatu VLAN ID.

![Perangkat pada Cisco Packet Tracer](/assets/img/2022-05-08-menghubungkan-vlan-berbeda-menggunakan-intervlan-routing/07.jpg){: .normal }

## Analogi

Beberapa contoh analogi sederhana suatu proses routing yaitu sebagai berikut.

1. Sistem Pemetaan Global/Global Positioning System (GPS):

   1. Anda ingin pergi dari titik A ke titik B.
   1. GPS di mobil Anda adalah router virtual yang mencari jalan tercepat untuk mencapai tujuan Anda.
   1. GPS memeriksa peta jalan dan informasi lalu lintas (analogi tabel routing) dan memandu Anda melalui jalan-jalan yang benar.

2. Kantor Pos:

   1. Bayangkan sebuah kantor pos yang menerima surat dari banyak pengirim dan harus mengirimnya ke berbagai tujuan.
   2. Petugas kantor pos (router) mengambil setiap surat dan memilih rute terbaik untuk mencapai alamat tujuan.
   3. Setelah surat berada di rute, setiap pos ronda (switch) membantu mengirimnya ke alamat yang benar.

3. Peta Transportasi Umum:

   1. Peta sistem transportasi umum di kota besar adalah seperti tabel routing yang digunakan oleh sistem jaringan.
   2. Peta tersebut menunjukkan berbagai rute bus dan kereta yang dapat Anda gunakan untuk mencapai berbagai tujuan di kota.
   3. Ketika Anda ingin pergi ke suatu tempat, Anda mengacu pada peta tersebut untuk menentukan rute terbaik yang harus diambil.

## Kebutuhan

Ada beberapa alasan mengapa perlu melakukan routing antara VLAN, antara lain:

1. Untuk menghubungkan perangkat di VLAN yang berbeda.

   Misalnya, Anda memiliki VLAN untuk karyawan dan VLAN untuk tamu. Untuk memungkinkan karyawan dan tamu saling berkomunikasi, Anda perlu melakukan routing antara VLAN tersebut.

2. Untuk mengakses sumber daya di VLAN yang berbeda.

   Misalnya, Anda memiliki VLAN untuk server dan VLAN untuk workstation. Untuk memungkinkan workstation mengakses sumber daya di server, Anda perlu melakukan routing antara VLAN tersebut.

3. Untuk menerapkan kebijakan keamanan.

   Misalnya, Anda ingin membatasi akses dari VLAN tertentu ke VLAN lainnya. Untuk menerapkan kebijakan keamanan ini, Anda perlu melakukan routing antara VLAN tersebut.

4. Untuk memenuhi kebutuhan aplikasi tertentu.

   Misalnya, Anda menggunakan aplikasi seperti VoIP (Voice over IP), memerlukan komunikasi antara perangkat dalam VLAN yang berbeda. Dengan Inter-VLAN Routing, maka melakukan panggilan dengan perangkat berbeda VLAN akan bisa dilakukan.

Contoh situasi suatu kantor atau perusahaan di mana teknik konfigurasi InterVLAN routing butuh diimplementasi:

1. Komunikasi Antar Departemen:

   Departemen pemasaran perlu berbagi informasi dan file dengan departemen pengembangan. Tanpa Inter-VLAN Routing, keduanya akan terisolasi, dan komunikasi antara mereka tidak akan mungkin.

2. Server Bersama:

   Jika ada server bersama seperti server file atau server basis data yang harus diakses oleh kedua departemen, Inter-VLAN Routing diperlukan agar perangkat di kedua VLAN dapat mengakses server tersebut.

3. VoIP

   Jika kantor mengimplementasikan sistem VoIP dan perangkat telepon terhubung ke satu VLAN, sedangkan server VoIP terhubung ke VLAN lain, Inter-VLAN Routing memungkinkan perangkat telepon untuk berkomunikasi dengan server VoIP.

4. Kepatuhan dan Keamanan

   Dalam beberapa kasus, Inter-VLAN Routing digunakan untuk mengimplementasikan kebijakan keamanan, pengawasan, dan kepatuhan yang berbeda untuk setiap departemen atau kelompok perangkat.

## Cara Kerja

Berikut adalah objektif cara kerja intervlan routing:

1. Perangkat di suatu VLAN yang berbeda akan mengirimkan paket data ke router.

   Perangkat di VLAN yang berbeda akan mengirimkan paket data ke router, Paket data tersebut akan berisi alamat IP tujuan.

2. Router akan memeriksa tabel routing untuk menentukan jalur terbaik untuk mengirimkan paket data tersebut.

   Router akan memeriksa tabel routing untuk menentukan jalur terbaik untuk mengirimkan paket data tersebut. Tabel routing berisi informasi tentang jaringan yang terhubung ke router, serta jalur terbaik untuk mencapai jaringan tersebut.

3. Router akan meneruskan paket data tersebut melalui jalur yang ditentukan.

   Router akan meneruskan paket data tersebut melalui jalur yang ditentukan oleh tabel routing.

4. Perangkat di VLAN tujuan akan menerima paket data tersebut.

   Paket data tersebut akan diterima oleh perangkat di VLAN tujuan dan dapat digunakan untuk berbagai tujuan, seperti mengakses sumber daya, berkomunikasi dengan perangkat lain, atau mengunduh file.

## Metode

Berikut beberapa metode konfigurasi yang bisa diimplementasikan untuk membangun jaringan routing antar VLAN:

1. Metode 1: Multi-Layer Switching

   ![Life is My Campus](/assets/img/2022-05-08-menghubungkan-vlan-berbeda-menggunakan-intervlan-routing/01.png){: .normal }

   Multi-Layer Switching adalah metode routing antara VLAN yang menggunakan perangkat layer 3 switch. Alamat ip akan diinputkan pada suatu Interface VLAN (Switch Virtual Interface/SVI) sehingga dapat digunakan sebagai gateway suatu jaringan.

   1. Kelebihan

      1. Scalable untuk jaringan yang besar.
      2. Cukup satu perangkat switch.

   2. Kekurangan

      1. Kompleksitas konfigurasi dibanding konfig VLAN biasa.
      1. Membutuhkan switch yang mendukung routing (layer 3).

   3. Langkah Konfigurasi

      1. Buat VLAN pada switch.
      2. Hubungkan switch ke router.
      3. Konfigurasi switch sebagai perangkat layer 3.
      4. Konfigurasi IP Address dan subnet mask pada VLAN.
      5. Konfigurasi routing pada switch.

2. Metode 2: Legacy InterVLAN Routing

   ![Life is My Campus](/assets/img/2022-05-08-menghubungkan-vlan-berbeda-menggunakan-intervlan-routing/02.png){: .normal }

   Legacy Inter-VLAN Routing adalah metode routing antara VLAN yang menggunakan banyak interface router, masing-masing terhubung ke port switch di VLAN yang berbeda. Interface router ini berfungsi sebagai gateway, yang membutuhkan kabel tambahan jika jaringan harus diperluas.

   1. Kelebihan

      1. Sederhana dan mudah untuk diimplementasi.

   2. Kekurangan

      1. Tidak scalable untuk jaringan yang besar.
      2. Biaya tinggi karena membutuhkan banyak perangkat.

   3. Langkah Konfigurasi

      1. Buat VLAN pada switch.
      2. Hubungkan router ke switch.
      3. Konfigurasi IP Address dan subnet mask untuk setiap interface router yang terhubung ke VLAN.
      4. Konfigurasi routing protocol pada router.

3. Metode 3: Router-on-a-Stick (RoaS)

   ![Life is My Campus](/assets/img/2022-05-08-menghubungkan-vlan-berbeda-menggunakan-intervlan-routing/03.png){: .normal }

   RoaS adalah metode yang menggunakan satu interface fisik router untuk menghubungkan ke switch. Interface fisik router tersebut kemudian dibagi menjadi beberapa subinterface, satu untuk setiap VLAN yang ingin dihubungkan. Subinterface inilah yang digunakan sebagai gateway.

   1. Kelebihan

      1. Scalable untuk jaringan yang besar.
      2. Membutuhkan satu interface fisik router.
      3. Mengurangi biaya perangkat keras.

   2. Kekurangan

      1. Kompleksitas konfigurasi.
      2. Memerlukan switch yang mendukung trunk port.

   3. Langkah Konfigurasi

      1. Hubungkan interface fisik router ke switch port yang terhubung ke VLAN yang berbeda.
      2. Konfigurasi interface fisik router sebagai trunk port.
      3. Buat subinterface untuk setiap VLAN yang ingin dihubungkan.
      4. Konfigurasi IP Address dan subnet mask pada subinterface.

## Keamanan

Inter-VLAN routing membutuhkan pengelolaan keamanan untuk memastikan bahwa jaringan aman dan terlindungi dari ancaman keamanan seperti serangan jaringan, akses yang tidak sah, dan lainnya.

Berikut adalah beberapa konsep keamanan yang penting dalam Inter-VLAN routing:

1. Access Control List (ACL)

   ACL adalah daftar kontrol akses yang menentukan bagaimana paket jaringan diterima atau ditolak oleh perangkat jaringan. ACL menggunakan aturan khusus untuk menentukan apakah paket diterima atau ditolak. Ini digunakan untuk memastikan bahwa jaringan terlindung dari serangan dan akses yang tidak sah.

1. Firewall

   Firewall adalah perangkat yang memantau dan mengendalikan akses jaringan. Firewall memiliki kemampuan untuk menganalisis traffic jaringan dan membuat keputusan apakah traffic tersebut harus diterima atau ditolak. Firewall biasanya digunakan untuk memblokir serangan jaringan dan membatasi akses yang tidak sah.

1. VLAN Access Control

   Menentukan VLAN mana yang dapat diakses oleh host atau perangkat tertentu. Ini membatasi akses yang tidak sah dan memastikan bahwa hanya perangkat yang sah yang dapat mengakses jaringan.

1. Port Security

   Menentukan perangkat apa saja yang dapat mengakses switch melalui port tertentu. Ini membatasi akses yang tidak sah dan memastikan bahwa hanya perangkat yang sah yang dapat mengakses jaringan.

1. Virtual Router Redundancy Protocol (VRRP)

   VRRP memastikan bahwa router tetap aktif dan memberikan solusi backup jika salah satu router gagal. Ini memastikan bahwa jaringan tetap berfungsi selama ada gangguan.

1. Encryption

   Menggunakan enkripsi untuk memproteksi data yang dikirimkan melalui jaringan. Ini memastikan bahwa data tidak dapat dicuri atau dibaca oleh pihak yang tidak sah.

1. Virtual Routing and Forwarding (VRF)

   VRF adalah teknologi yang memungkinkan jaringan memiliki beberapa routing domain yang terisolasi secara logika dalam satu perangkat. VRF memastikan bahwa jaringan dapat memiliki routing domain yang terisolasi dan tidak berdampak pada routing domain lainnya. Ini memastikan bahwa jaringan tetap stabil dan aman meskipun ada perubahan yang terjadi dalam jaringan.

## DHCP

Dynamic Host Configuration Protocol (DHCP) adalah protokol jaringan yang digunakan untuk mengotomatisasi proses konfigurasi alamat IP dan pengaturan jaringan lainnya pada perangkat yang terhubung ke jaringan komputer.

Dapat disimpulkan bahwa, DHCP Server adalah komputer atau perangkat jaringan yang menjalankan perangkat lunak server DHCP dan bertanggung jawab untuk memberikan alamat IP serta konfigurasi jaringan lainnya kepada perangkat yang memintanya di dalam jaringan.

Ketika end device seperti komputer, smartphone, atau perangkat jaringan lainnya terhubung ke suatu jaringan, mereka mengirim permintaan ke DHCP Server untuk menerima alamat IP dan informasi konfigurasi jaringan.
