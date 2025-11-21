+++
title = "Trik Cepat Import File SQL (MySQL/MariaDB)"
date = "2025-11-21"
+++

Ketika ingin melakukan import hasil export (backup) database MySQL/MariaDB, gunakan CLI agar lebih cepat dibandingkan tool GUI.

<!--more-->

## Perintah Dasar

```sh
mysql -u root -p nama_database < /path/ke/backup.sql
```

Setelah menekan enter Anda akan diminta memasukkan password user (contoh: `root`). Pastikan `nama_database` sudah dibuat sebelumnya:

```sh
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS nama_database CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
```

## Import dari File .gz (Kompres)

Jika backup disimpan dalam bentuk terkompresi untuk menghemat ruang:

```sh
gunzip -c backup.sql.gz | mysql -u root -p nama_database
```

Alternatif menggunakan `zcat` (Linux):

```sh
zcat backup.sql.gz | mysql -u root -p nama_database
```

## Cek Hasil Import

```sh
mysql -u root -p -e "USE nama_database; SHOW TABLES;"
```

## Tips Tambahan

- Pastikan charset konsisten (gunakan `utf8mb4` untuk dukungan emoji & multibahasa).
- Gunakan user dengan hak minimal jika hanya melakukan restore (hindari root di produksi).
- Untuk file besar, jalankan di screen/tmux agar tidak terputus.
- Jika terjadi error foreign key, bisa sementara menonaktifkan constraint:

```sh
mysql -u root -p nama_database -e "SET FOREIGN_KEY_CHECKS=0; SOURCE /path/ke/backup.sql; SET FOREIGN_KEY_CHECKS=1;"
```

Dengan cara di atas proses import menjadi lebih cepat, aman, dan terstruktur.