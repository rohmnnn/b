+++
title = "Fix WiFi iPhone Mati Saat Layar Terkunci"
date = "2025-12-30"
+++

WiFi di iPhone terputus saat layar terkunci, tapi langsung nyambung lagi ketika layar dibuka.

<!--more-->

## Penyebab

Masalah ini disebabkan oleh DHCP lease time di router. Saat layar terkunci dan lease time habis, iPhone akan kehilangan koneksi WiFi. Ketika layar dibuka, iPhone meminta IP baru dan WiFi tersambung kembali.

## Solusi

### 1. Perpanjang Lease Time di Router

Jika memiliki akses ke router:

- Login ke admin panel router
- Cari pengaturan DHCP Lease Time
- Ubah menjadi 12 jam atau 24 jam
- Simpan dan restart router

Catatan: Lease time yang panjang akan membuat IP address terkunci lebih lama. IP baru bisa digunakan device lain setelah lease time habis.

### 2. Set IP Manual di iPhone

Jika tidak bisa akses router:

1. Buka Settings > WiFi
2. Tap icon (i) di samping nama WiFi
3. Tap Configure IP
4. Ubah dari Automatic ke Manual
5. Isi konfigurasi:
   - IP Address: misalnya 192.168.1.50
   - Subnet Mask: 255.255.255.0
   - Router: IP gateway (misalnya 192.168.1.1)
   - DNS: 8.8.8.8, 8.8.4.4
6. Simpan

Dengan cara ini, iPhone tidak perlu meminta IP baru sehingga koneksi tetap stabil meskipun layar terkunci.

