+++
title = "Backup Restore vnStat di OpenWRT"
date = "2025-11-01"
+++

vnStat adalah utilitas ringan untuk mencatat dan menampilkan trafik jaringan per antarmuka. Dengan vnStat, Anda bisa melihat total bandwidth serta rata-rata kecepatan per menit, hari, bulan, hingga tahun.

<!--more-->

## Prasyarat

- Perangkat OpenWrt dengan akses SSH (root)
- Paket vnStat sudah terpasang (`opkg install vnstat`)
- Mengetahui versi vnStat yang digunakan (penting untuk kompatibilitas basis data)

Cek versi vnStat:

```sh
vnstat --version
```

Contoh: `vnStat 2.13`.

## Lokasi basis data vnStat

Pada vnStat 2.x, berkas basis data umumnya berada di salah satu dari:

- `/var/lib/vnstat/vnstat.db` (paling umum pada OpenWrt baru)
- `/etc/vnstat/vnstat.db` (beberapa build atau konfigurasi lama)

Anda bisa memastikan direktori basis data yang aktif dari konfigurasi:

```sh
grep -i ^DatabaseDir /etc/vnstat.conf || echo "Default: /var/lib/vnstat"
```

## Hentikan layanan sebelum backup/restore

Untuk menghindari korupsi data, hentikan layanan vnStat sebelum menyalin atau menimpa basis data:

```sh
/etc/init.d/vnstat stop
```

## Backup

Langkah-langkah membuat cadangan (backup) basis data vnStat 2.13:

1) Pastikan layanan dihentikan (lihat di atas).
2) Salin berkas `vnstat.db` ke lokasi aman (mis. ke `/root` atau unduh ke komputer lewat SCP).

Contoh menyalin secara lokal (jika database di `/var/lib/vnstat/`):

```sh
cp -a /var/lib/vnstat/vnstat.db /root/backup-vnstat-$(date +%F).db
```

Jika database Anda berada di `/etc/vnstat/`:

```sh
cp -a /etc/vnstat/vnstat.db /root/backup-vnstat-$(date +%F).db
```

Mengunduh ke komputer via SCP (ganti `router` dengan IP/host router Anda):

```sh
scp root@router:/var/lib/vnstat/vnstat.db ./vnstat-$(date +%F).db
```

## Restore

Untuk memulihkan (restore) dari berkas cadangan:

1) Hentikan layanan vnStat jika belum dihentikan.
2) Timpa berkas `vnstat.db` di direktori basis data aktif dengan berkas cadangan.
3) Mulai kembali layanan vnStat.

Contoh (database berada di `/var/lib/vnstat/`):

```sh
/etc/init.d/vnstat stop
cp -a /path/ke/backup-vnstat.db /var/lib/vnstat/vnstat.db
/etc/init.d/vnstat start
```

Jika database Anda berada di `/etc/vnstat/` sesuaikan jalurnya:

```sh
/etc/init.d/vnstat stop
cp -a /path/ke/backup-vnstat.db /etc/vnstat/vnstat.db
/etc/init.d/vnstat start
```

## Verifikasi

Setelah layanan berjalan kembali, verifikasi bahwa data tampil normal:

```sh
vnstat
```

Anda juga bisa melihat dalam format JSON untuk pemeriksaan cepat:

```sh
vnstat --json | head
```

## Catatan & troubleshooting

- Pastikan versi vnStat kompatibel dengan berkas cadangan Anda (database vnStat 1.x tidak kompatibel dengan 2.x).
- Jika vnStat tidak menemukan database, cek `DatabaseDir` pada `/etc/vnstat.conf` lalu sesuaikan jalur dan restart layanan.
- Pastikan ruang penyimpanan cukup dan filesystem tidak dalam keadaan hanya-baca (read-only).
- Pada beberapa sistem, kepemilikan/izin berkas bisa berpengaruh. Umumnya pada OpenWrt, menjalankan sebagai root sudah cukup; jika perlu, set izin konservatif: `chmod 600` pada `vnstat.db`.

Dengan langkah-langkah di atas, proses backup dan restore vnStat di OpenWrt menjadi lebih aman dan bisa direproduksi kapan pun dibutuhkan.