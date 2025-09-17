+++
title = "L860-GL Protokol Intel XMM Cellular di OpenWRT"
date = "2025-09-17"
+++

Setting modem Fibocom L860-GL dengan Protokol Intel XMM Cellular di OpenWRT

<!--more-->

Untuk cara settingnya cukup gampang, karena pada kebanyakan firmware sudah tersedia protokol **Intel XMM Cellular**. Jika belum, tambahkan paket `xmm-modem` dan `luci-proto-xmm`, langsung saja ke tutorialnya.

### Interface

-   Name: x
-   Protocol: Intel XMM Cellular

### General Settings

-   Bring up on boot: ✅
-   Modem port: sesuaikan (umumnya `/dev/ttyACM0` atau `/dev/ttyACM2`)
-   APN: `internet` (sesuaikan dengan provider SIM)
-   PDP Type: `IPv4/IPv6`

### Firewall Settings

-   Create/Assign firewall-zone: `wan`

Untuk konfigurasi yang lain bisa didefaultkan saja.

![result](https://github.com/rohmnnn/b/blob/main/content/posts/setting-modem-fibocom-l860-protocol-intel-xmm-celullar-openwrt/result.png?raw=true)

#  Cara Auto Reconnect

Script sederhana untuk menjaga koneksi dan otomatis reconnect jika ping gagal.

Simpan sebagai ***/etc/hotplug.d/iface/99-restart-interface***

```sh
#!/bin/sh
# Check if the action is 'ifdown' (interface down)
# Youtube Tutorial: https://bit.ly/aryochannel

if [ "$ACTION" = "ifdown" ]; then
  # ganti 'wan_6' sesuai interface ipv6 yang ingin di-restart
  if [ "$INTERFACE" = "wan_6" ]; then
    logger -t hotplug "Interface $INTERFACE is down. Attempting to restart."
    # ganti 'wan' sesuai interface yang ingin di-restart
    /sbin/ifup wan
  fi
fi
```

Pastikan untuk nama interface sudah sesuai dan wajib aktifkan ipv6 pada PDP Type (IPv4/IPv6) 

sumber: 
[aryo brokolly -  L860 AUTO GANTI IP TANPA TAMBAHAN SCRIPT](https://www.youtube.com/watch?v=N-1k3fx3vgg&t=729s)


