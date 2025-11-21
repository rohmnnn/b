+++
title = "Scroll Reverser di Mac (macOS)"
date = "2025-11-21"
+++

Scroll Reverser adalah aplikasi ringan untuk membalik arah scroll khusus perangkat tertentu (misalnya mouse) tanpa mengubah perilaku trackpad. Cocok jika Anda ingin scroll mouse seperti di Windows, sementara trackpad tetap default macOS.

<!--more-->

- Situs resmi: https://pilotmoon.com/scrollreverser/

## Instalasi (Homebrew)

```sh
brew install --cask scroll-reverser
```

## Pengaturan yang Disarankan

- Buka aplikasi Scroll Reverser (dari Launchpad/Spotlight)
- Beri izin Accessibility: System Settings → Privacy & Security → Accessibility → centang Scroll Reverser
- Aktifkan “Reverse Scrolling” (Horizontal & Vertical) untuk `Mouse` saja; biarkan `Trackpad` tidak dicentang
- Centang “Start at login” agar aktif otomatis saat boot

## Uninstall

```sh
brew uninstall --cask scroll-reverser
```