+++
title = "Asus Max Pro M2 Untuk Modem"
date = "2025-08-30"
+++

Hal-hal simple yang kulakukan untuk menjadikan X01BD sebagai modem.

<!--more-->

Oh iya ini hanya menulis pengalaman saya menggunakan ini selama kurang lebih hampir satu tahun, hingga akhir nya mesin dari HP ini meninggal (kena write protected). Jadi teman-teman bisa jadikan ini sebagai referensi, karena jika mengalami hal yang sama itu menjadi tanggung jawab kalian masing masing HAHAHAHAHAHAA

Oke saya bagi beberapa tahapan.

- Kustomisasi ROM X01BD
- Unlock CA (Optional)
- Bypass Charging
- Internet
- Modul Magisk

### Kustomisasi ROM X01BD

Pertama tentukan ROM yang akan digunakan, saya mencari ROM yang sangat minim bloatware dengan versi android 10 atau 11, kenapa harus versi tersebut karena itu yang paling smooth diHP ini yang menggunakan sdm660 dan varian ram 4GB.

ROM yang saya gunakan adalah [CarbonROM](https://carbonrom.org/), setelah memasang rom saya lakukan rooting dengan [Magisk](https://github.com/topjohnwu/Magisk) saya gunakan versi v28.

### Unlock CA (Optional)
Ini tujuannya supaya kita mendapatkan sinyal dual Band, misal Band 1 dan Band 3 atau Band 3 dan Band 8 atau Band 1 dan Band 8, bisa juga dalam Band yang sama misal Band 1 dan Band 1 atau Band 3 dan Band 3 tergantung smartphone kita support yang mana dan BTS Provider menyediakan yang mana. Nah dalam kasus kita dengan device X01BD kita hanya memasukan [CACOMBO](https://cacombos.com/device/ZB634KL) ke dalam file emmc kita, file sudah ada di forum xda/telegram silahkan cari sendiri. hehehe

### Bypass Charging
Bypass charging yang saya lakukan ada dua pertama menggunakan software Baterai Advanced Charging Controller [ACC](https://github.com/VR-25/acc) karena kan HP ini disambungkan dengan USB Tethring, maka akan terdeteksi charging juga maka kita lakukan limitasi saat baterai <= 70 kita lakukan charging, dan saat baterai >= 80 kita stop ya memang tujuannya untuk memperpanjang umur baterai.

Lalu yang kedua menggunakan aliran listrik langsung tanpa baterai dengan memanfaatkan modul stepdown dan juga modul BMS dari baterai bekas sebagai soket masuk tanpa harus melakukan jumper kabel. diHP ini saya set voltase di 4V-4,2V dengan input REAL 12V 2A (ini harus pakai adaptor ori, jika tidak maka HP perlu bantuan charging supaya bisa hidup).

![bypass-charging](https://github.com/rohmnnn/b/blob/main/content/posts/asus-max-pro-m2-untuk-modem/bypass-charging.jpeg?raw=true)

### Internet

Di sini sumber internet yang saya gunakan adalah dari Inject menggunakan [Box for root](https://github.com/taamarin/box_for_magisk) saya gunakan veris 1.7.0.

### Modul Magisk

Saya membutuhkan modul untuk mengubah state jika device kondisi mati dan ada input dari USB maka device akan melakukan reboot, dimana defaultnya adalah charging. Jadi setiap mati listrik device akan mati lalu pas listrik hidup kembali devicenya akan otomatis hidup kembali, sebetulnya lebih gampang jika memiliki sebuah UPS.
