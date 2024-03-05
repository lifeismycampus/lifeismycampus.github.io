---
layout: post
title: Translasi Alamat Local ke Public dengan Network Address Translation (NAT)
date: 2023-03-26 00:00 +0000
author: angga
categories: [Konsep]
tags: [nat]
---

## Pengertian dan Konsep

Network Address Translation (NAT) adalah sebuah teknologi yang digunakan untuk menerjemahkan alamat IP lokal suatu jaringan ke alamat IP publik yang digunakan di Internet. Dengan kata lain, NAT memungkinkan beberapa perangkat dalam jaringan lokal untuk menggunakan satu alamat IP publik untuk berkomunikasi di Internet. Konsep ini mirip dengan pelayanan resepsionis di suatu gedung, di mana banyak orang dari dalam gedung (jaringan lokal) dapat berkomunikasi melalui satu pintu masuk (alamat IP publik).

### Internet Gateway

Internet Gateway, di sisi lain, adalah perangkat keras atau perangkat lunak yang menghubungkan jaringan lokal (misalnya, jaringan kantor atau rumah) ke Internet. Internet Gateway bertindak sebagai gerbang atau pintu masuk yang memungkinkan lalu lintas data masuk dan keluar dari jaringan lokal ke Internet dan sebaliknya. Fungsinya melibatkan proses routing data antara jaringan lokal dan Internet.

### Hubungan NAT dan Internet Gateway

Dengan analogi sederhana, hubungan antara NAT dan Internet Gateway dapat digambarkan sebagai berikut:

1. NAT (Resepsionis)

   Terletak di dalam gedung (jaringan lokal) dan bertanggung jawab untuk menerjemahkan alamat IP lokal ke alamat IP publik agar perangkat di dalam bisa berkomunikasi dengan dunia luar (Internet).

2. Internet Gateway (Pintu Masuk Gedung)

   Merupakan pintu masuk dan keluar dari gedung (jaringan lokal). Melibatkan proses routing data antara gedung dan luar gedung (Internet), memastikan data dapat masuk dan keluar dengan benar.

Jadi, dalam analogi ini, NAT adalah seperti resepsionis di dalam gedung yang menangani interaksi antara ruangan (perangkat) di dalam dengan tamu (dunia luar), sedangkan Internet Gateway adalah pintu masuk yang mengontrol lalu lintas masuk dan keluar dari gedung (jaringan lokal). Keduanya bekerja bersama untuk memastikan komunikasi yang aman dan efisien antara jaringan lokal dan Internet.

## Tujuan Penggunaan

Tujuan utama penggunaan NAT adalah untuk mengatasi keterbatasan jumlah alamat IP publik yang tersedia dan memberikan keamanan tambahan dengan menyembunyikan struktur jaringan internal dari dunia luar.

## Prinsip dan Cara Kerja

Beberapa cara kerja dari NAT yaitu

1. Address Translation,
2. Port Translation, dan
3. Network Address Port Translation

### Address Translation

NAT melakukan translasi alamat IP dari perangkat dalam jaringan lokal ke alamat IP publik. Analoginya, jika setiap perangkat dalam jaringan adalah buku dalam perpustakaan, NAT bertindak sebagai katalog yang menerjemahkan nomor loker buku (alamat IP lokal) ke nomor loker buku yang terdaftar di luar perpustakaan (alamat IP publik).

### Port Translation

Selain menerjemahkan alamat IP, NAT juga melakukan translasi port. Port adalah alamat yang terkait dengan aplikasi atau layanan tertentu pada suatu perangkat. Analoginya, jika alamat IP adalah alamat rumah, port adalah nomor kamar di dalam rumah. NAT memastikan bahwa setiap "kamar" memiliki nomor yang unik saat berkomunikasi dengan dunia luar.

### Network Address Port Translation

NAPT adalah bentuk NAT yang paling umum. Ini melibatkan translasi alamat IP dan port, memungkinkan banyak perangkat di dalam jaringan lokal untuk berbagi satu alamat IP publik. Analoginya, beberapa orang di dalam gedung (jaringan lokal) dapat berbagi pintu masuk (alamat IP publik) dengan setiap orang memiliki nomor kamar (port) yang unik.

## Klasifikasi

NAT dapat dikelompokkan menjadi:

1. Static NAT,
2. Dynamic NAT, dan
3. NAT Overload/Port Address Translation (PAT)

### Static NAT

Static NAT mengalokasikan satu alamat IP publik untuk setiap perangkat di jaringan lokal secara tetap. Analoginya, setiap buku dalam perpustakaan memiliki nomor loker tetap di luar perpustakaan.

### Dynamic NAT

Dynamic NAT mengalokasikan alamat IP publik secara dinamis dari suatu pool ketika diperlukan. Analoginya, buku dalam perpustakaan bisa memiliki nomor loker yang berbeda setiap kali dipinjam.

### NAT Overload/Port Address Translation (PAT)

NAT Overload atau PAT adalah bentuk NAPT yang paling umum. Ini mengizinkan banyak perangkat di jaringan lokal untuk berbagi satu alamat IP publik, dengan menerjemahkan alamat IP dan port. Analoginya, beberapa orang di gedung dapat berbagi pintu masuk (alamat IP publik) dengan setiap orang memiliki nomor kamar (port) yang unik.
