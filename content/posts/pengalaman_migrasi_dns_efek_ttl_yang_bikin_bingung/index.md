+++
title = "Pengalaman Migrasi DNS: Efek TTL yang Bikin Bingung"
date = "2026-04-01"
+++

Cerita singkat waktu saya migrasi server dan panik karena DNS terasa tidak berubah-ubah.
<!--more-->

## Penyebab

Kemarin saya lagi migrasi dari server A ke server B. Secara teknis, rencananya sederhana: ganti IP DNS ke server baru, lalu semua berjalan normal.

Masalahnya mulai muncul ketika saya cek hasil resolve domain. Meski IP sudah diganti, hasil dari `nslookup` dan `dig` masih menunjukkan IP lama.

Saya sempat mengira ada yang salah di konfigurasi, karena rasanya sudah saya cek berulang kali. Flush DNS di komputer juga sudah dilakukan, tapi hasilnya tetap sama.

Setelah cari-cari, saya baru sadar sumber masalahnya ada di TTL (time to live). Nilai TTL sebelumnya adalah 14400 detik, alias 4 jam.

Artinya, resolver DNS di banyak tempat masih menyimpan data lama sampai masa TTL habis. Jadi bukan karena setting saya gagal, tapi karena cache DNS memang belum kedaluwarsa.

## Pelajaran yang saya dapat

Kalau mau melakukan perubahan DNS, idealnya turunkan TTL dulu beberapa waktu sebelumnya, misalnya ke 900 detik (15 menit). Dengan begitu waktu propagasi jadi jauh lebih cepat dan kita tidak menunggu terlalu lama.

Beberapa penyedia DNS juga punya opsi TTL `Auto`, contohnya di Cloudflare. Tetap bagus dipahami nilainya, supaya kita bisa memperkirakan dampak saat migrasi.

Jujur ini kesalahan kecil yang bikin saya buang waktu, tapi dari sini saya belajar satu hal: urusan DNS itu bukan cuma benar atau salah, tapi juga soal timing.

Semoga catatan ini membantu kamu yang lagi migrasi server, terutama kalau merasa DNS sudah diubah tapi hasilnya belum ikut berubah.