+++
title = "Merubah package manager dari NPM ke PNPM di Laravel"
date = "2026-11-16"
+++

Kalau kamu lebih nyaman pakai PNPM daripada NPM, nggak perlu ribet. Di Laravel, prosesnya cukup sederhana.
<!--more-->

Sebenarnya pilihan package manager itu fleksibel, tergantung kebutuhan masing-masing. Tapi kalau kamu ingin pindah dari NPM ke PNPM, berikut cara yang paling gampang.

## Langkah-langkah

Pertama, hapus file lock dan folder dependencies yang masih dipakai oleh NPM:

```bash
rm -rf node_modules package-lock.json
```

Setelah itu, install ulang dependency dengan PNPM:

```bash
pnpm install
```

Kalau sudah, kamu bisa mengecek apakah konfigurasi package manager sudah berubah dengan perintah:

```bash
php artisan dev:list
```

Berikut hasil sebelum dan sesudah perubahan:

![Sebelum pindah ke PNPM](https://github.com/rohmnnn/b/blob/main/content/posts/merubah-package-manager-di-laravel-php-artisan-dev/before.png?raw=true)

![Sesudah pindah ke PNPM](https://github.com/rohmnnn/b/blob/main/content/posts/merubah-package-manager-di-laravel-php-artisan-dev/after.png?raw=true)

Jika muncul daftar script yang sesuai dan project tetap berjalan normal, berarti proses penggantian package manager sudah berhasil.

## Catatan

Yang perlu diingat, mengganti package manager biasanya tidak mengubah logika aplikasi. Yang berubah hanya cara dependency di-install dan dikelola di local project. Jadi prosesnya lebih ke migrasi tooling, bukan perubahan kode aplikasi itu sendiri.

Jadi, kalau kamu pengen pakai PNPM di Laravel, cukup hapus `package-lock.json` dan `node_modules`, lalu jalankan `pnpm install`. Simpel, cepat, dan aman.