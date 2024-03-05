---
layout: post
title: Konfigurasi InterVLAN Routing Menggunakan MikroTik dengan Bridge VLAN Table
date: 2022-05-29 00:00 +0000
author: angga
categories: [Konfigurasi]
tags: [mikrotik, routing, vlan, subnetting]
---

## Ulasan

InterVLAN Routing menggunakan MikroTik dapat dilakukan dengan beberapa metode, yaitu

1. Bridge VLAN Table
2. Switch Chip (hanya untuk interface port ethernet)

Praktikum berikut menggunakan port wlan selain ether, oleh karena itu akan menggunakan metode bridge vlan table.

## Alat dan Bahan

1. Jaringan router internet (disediakan instruktur)
   - SSID: internet
   - Security (Password): 1Sampai8
   - IP; DHCP Client dari internet
   - DNS Server; Allow Remote Request
2. 2 buah Komputer; 1 sebagai remote sekaligus client pada pengujian
3. 2 buah MikroTik Router; 1 sebagai router dan 1 sebagai switch
   - minimal 4 port eth dan 1 port wlan
   - reset configuration
   - no default configuration
4. 3 buah kabel UTP; 1 untuk trunk link dan 2 untuk access link
5. MikroTik WinBox; remote client untuk router

## Langkah Kerja

Pada praktikum kali ini akan terbagi menjadi beberapa tahapan yaitu

1. Persiapan
2. Konfigurasi Router
3. Konfigurasi Switch
4. Konfigurasi Client dan Pengujian
5. Berlatih

### 1. Persiapan

#### Topologi jaringan

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/top1.jpg){: .normal }

#### Spesifikasi

1. Router

   - Hostname: router-yy (yy diganti nama masing-masing, pada contoh menggunakan angga)
   - eht1: trunk
     - VLAN ID: 10; Name: public; dhcp server,
     - VLAN ID: 20; Name: private; dhcp server,
     - VLAN ID: 30; Name: wireless; dhcp server
   - eht2: -
   - eht3: -
   - eht4: remote
   - wlan1:
     - mode: station bridge,
     - ssid: internet,
     - security (password): 1Sampai8

2. Switch

   - Hostname: switch-yy (yy diganti nama masing-masing, pada contoh menggunakan angga)
   - eht1: trunk (VLAN 10, VLAN 20, VLAN 30)
   - eht2: VLAN 10
   - eht3: VLAN 20
   - eht4: remote
   - wlan1: VLAN 30
     - mode: ap bridge,
     - ssid: vlan30-yy (yy diganti nama masing-masing, pada contoh menggunakan angga),
     - security (password): sandikuxx (xx diganti no presensi masing-masing, pada contoh menggunakan 99)

#### Tabel pengalamatan

| Device    | Interface | VLAN       | IP Addr      | Subnet Mask | Gateway     |
| --------- | --------- | ---------- | ------------ | ----------- | ----------- |
| router-yy | eth1      | 10, 20, 30 | -            | -           | -           |
| router-yy | eth1      | 10         | 10.1xx.10.1  | /24         | -           |
| router-yy | eth1      | 20         | 10.1xx.20.1  | /24         | -           |
| router-yy | eth1      | 30         | 10.1xx.30.1  | /24         | -           |
| router-yy | eth2      | -          | -            | -           | -           |
| router-yy | eth3      | -          | -            | -           | -           |
| router-yy | eth4      | -          | -            | -           | -           |
| router-yy | wlan1     | -          | 172.16.1.1xx | /24         | 172.16.1.1  |
| switch-yy | eth1      | 10, 20, 30 | -            | -           | -           |
| switch-yy | eth2      | 10         | -            | -           | -           |
| switch-yy | eth3      | 20         | -            | -           | -           |
| switch-yy | eth4      | -          | -            | -           | -           |
| switch-yy | wlan1     | 30         | -            | -           | -           |
| client1   | nic       | 10         | dhcp client  | /24         | 10.1xx.10.1 |
| client2   | nic       | 20         | dhcp client  | /24         | 10.1xx.20.1 |
| client3a  | wifi      | 30         | dhcp client  | /24         | 10.1xx.30.1 |
| client3b  | wifi      | 30         | dhcp client  | /24         | 10.1xx.30.1 |

### 2. Konfigurasi Router

#### Langkah ke-01: pasang kabel pada port eth4 untuk remote

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r1.jpg){: .normal }

#### Langkah ke-02: login pada mikrotik

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r2a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r2b.png){: .normal }

#### Langkah ke-03: ganti hostname

> Menu: System > Identity

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r3.png){: .normal }

#### Langkah ke-04: tambah interface vlan

> Menu: Interfaces > Interface > + > VLAN

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r4.png){: .normal }

#### Langkah ke-05: buat interface vlan sesuai spesifikasi

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r5a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r5b.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r5c.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r5d.png){: .normal }

#### Langkah ke-06: konfigurasi alamat ip pada vlan (nantinya sebagai gateway)

> Menu: IP > Addresses > Address List > +

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r6a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r6b.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r6c.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r6d.png){: .normal }

#### Langkah ke-07: konfigurasi dhcp server untuk vlan

> Menu: IP > DHCP Server > DHCP > DHCP Setup

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r7a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r7b.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r7c.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r7d.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r7e.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r7f.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r7g.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r7h.png){: .normal }

#### Langkah ke-08: definisikan security profile

> Menu: Wireless > Security Profile > +

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r8.png){: .normal }

#### Langkah ke-09: aktivasi wlan1

> Menu: Wireless > WiFi Interfaces > wlan1 > ✔

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r9.png){: .normal }

#### Langkah ke-10: koneksikan router vlan dengan router internet

> Klik ganda wlan1, pindah ke tab Wireless

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r10.png){: .normal }

#### Langkah ke-11: konfigurasi alamat ip pada wlan

> Menu: IP > Addresses > +

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r11.png){: .normal }

#### Langkah ke-12: konfirmasi koneksi router vlan dengan router internet

> Menu: Terminal > ping 172.16.1.1

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r12.png){: .normal }

#### Langkah ke-13: dapatkan koneksi internet untuk router vlan

Menu: IP > Firewall > NAT > +

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r13a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r13b.png){: .normal }

#### Langkah ke-14: konfigurasi default route

> Menu: IP > Routes > +

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r14.png){: .normal }

#### Langkah ke-15: konfigurasi dns server

Menu: IP > DNS > tambahkan alamat router internet

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r15.png){: .normal }

#### Langkah ke-16: konfirmasi koneksi router vlan dengan internet

Menu: Terminal > ping 8.8.8.8 dan ping google.com

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/r16.png){: .normal }

### 3. Konfigurasi Switch

#### Langkah ke-01: pindah kabel dari eth4 router ke eth4 switch untuk remote

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s1.jpg){: .normal }

#### Langkah ke-02: pasang kabel pada port eth1 untuk trunk, penghubung dari router ke switch

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s2.jpg){: .normal }

#### Langkah ke-03: login pada mikrotik

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s3.png){: .normal }

#### Langkah ke-04: ganti hostname

> Menu: System > Identity

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s4.png){: .normal }

#### Langkah ke-05: aktifkan interface bridge

> Menu: Bridge > Bridge > +

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s5.png){: .normal }

#### Langkah ke-06: tambahkan port eth1 yang digunakan sebagai trunk

> Menu: Bridge > Ports > +

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s6.png){: .normal }

#### Langkah ke-07: tambahkan port eth2 yang digunakan sebagai anggota vlan 10

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s7a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s7b.png){: .normal }

#### Langkah ke-08: tambahkan port eth3 yang digunakan sebagai anggota vlan 20

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s8a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s8b.png){: .normal }

#### Langkah ke-09: tambahkan port wlan1 yang digunakan sebagai anggota vlan 30

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s9a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s9b.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s9c.png){: .normal }

#### Langkah ke-10: mengatur mode link: tagged untuk trunk dan untagged untuk access

> Menu: Bridge > VLANs > +

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s10a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s10b.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s10c.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s10d.png){: .normal }

#### Langkah ke-11: aktifkan VLAN filtering

> Menu: Bridge > Bridge
>
> Klik ganda bridge1, pindah ke tab VLAN, klik ✔️ pada VLAN Filtering

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s11.png){: .normal }

#### Langkah ke-12: aktivasi interface wlan1

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s12a.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s12b.png){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/s12c.png){: .normal }

### 4. Konfigurasi Client dan Pengujian

#### Langkah ke-01: konfigurasi DHCP Client pada Client1

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c1a.jpg){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c1b.jpg){: .normal }

#### Langkah ke-02: konfirmasi routing ke VLAN lain (VLAN 20) dan internet pada Client1

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c2.jpg){: .normal }

#### Langkah ke-03: konfigurasi DHCP Client pada Client2

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c3.png){: .normal }

#### Langkah ke-04: konfirmasi routing ke VLAN lain (VLAN 10) dan internet pada Client2

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c4.png){: .normal }

#### Langkah ke-05: konfigurasi DHCP Client pada Client3a, koneksikan perangkat via wifi

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c5a.jpg){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c5b.jpg){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c5c.jpg){: .normal }

#### Langkah ke-06: konfigurasi DHCP Client pada Client3b, koneksikan perangkat via wifi

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c6a.jpg){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c6b.jpg){: .normal }

#### Langkah ke-07: konfirmasi routing ke VLAN lain (VLAN 10 dan VLAN 20) ke VLAN 30

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c7a.jpg){: .normal }
![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/c7b.png){: .normal }

### 5. Berlatih

#### Topologi Jaringan

![Life is My Campus](/assets/img/2022-05-29-konfigurasi-intervlan-routing-menggunakan-mikrotik-bridge-vlan-table/top2.jpg){: .normal }

#### Instruksi

1. Reservasi alamat server pada DHCP sebanyak 5 alamat pertama masing-masing jaringan
2. Konfigurasikan jaringan InterVLAN di atas sesuai ketentuan
3. Lakukan pengamatan dan pengujian pada jaringan terkonfigurasi
4. Susun dokumentasi sesuai lembar kerja tersedia
