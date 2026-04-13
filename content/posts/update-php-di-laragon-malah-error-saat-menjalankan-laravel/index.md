+++
title = "Update PHP di Laragon Malah Error Saat Menjalankan Laravel"
date = "2026-04-13"
+++

Pengalaman saat update PHP manual di Laragon, tetapi malah muncul error ketika menjalankan Laravel.
<!--more-->

Beberapa waktu lalu saya mencoba update PHP ke versi terbaru secara manual. Caranya cukup biasa: download file ZIP, lalu ekstrak ke folder Laragon.

Saya menggunakan Laragon versi lama, yaitu Laragon Full 6.0 220916. Setelah proses update selesai, saya langsung coba menjalankan project Laravel dengan perintah `php artisan serve`.

Ternyata bukannya jalan normal, malah muncul error seperti ini:

```text
Failed to listen on 127/0.0.1:8000 (reason: ?)
Failed to listen on 127/0.0.1:8001 (reason: ?)
Failed to listen on 127/0.0.1:8002 (reason: ?)
Failed to listen on 127/0.0.1:8003 (reason: ?)
Failed to listen on 127/0.0.1:8004 (reason: ?)
Failed to listen on 127/0.0.1:8005 (reason: ?)
```

Awalnya saya kira ada masalah di port atau konflik dengan service lain. Tapi setelah dicek lagi, penyebabnya ternyata ada di konfigurasi PHP.

## Solusi

Buka file konfigurasi `php.ini`, lalu cari bagian `variables_order`.

Kalau nilainya masih seperti ini:

```ini
variables_order = "EGPCS"
```

Ubah menjadi:

```ini
variables_order = "GPCS"
```

Setelah itu restart Laragon, lalu coba jalankan lagi project Laravel dengan `php artisan serve`.

Di kasus saya, setelah konfigurasi itu diubah, Laravel bisa berjalan normal kembali.

## Catatan

Kalau kamu update PHP secara manual di Laragon, terutama pada versi Laragon yang cukup lama, ada baiknya cek dulu file `php.ini` setelah proses update. Kadang masalahnya bukan di Laravel atau port, tetapi di konfigurasi PHP yang ikut berubah.

Semoga catatan singkat ini membantu kalau kamu mengalami error serupa setelah update PHP.