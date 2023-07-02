# **SETUP ARCH LINUX**

Copyright © 2023 [04burhanuddin](https://github.com/04burhanuddin)

**Warning**: Don't blindly use configurations on dwm, st, dmenu and dwmblocks, unless you know what they require. Use at your own risk!.

## Table Of Contents

- Install Arch Linux
- Install Github-CLI
- Install Editor
- Install Window Manager
- AUR Helper
- Install ZSH
- Install Browser
- Install Picom
- Touchpad Error
- Install Firewall
- Install Apache
- Install Mysql
- Install PHP
- Install Composer
- Laravel Global Installer
- Install Node JS Using NVM
- Install Java
- Install Android Studio
- Install Flutter
- Install Visual Studio Code
- Configure Input & Output Audio
- Install Bluetooth
- Manage Font
- Set Walpaper
- Install Neofetch
- Setup Neovim
- Cara Screenshoot
- Meggunakan SSH key passphrases
- Others

## **Install Arch Linux**

> **You can try installing on VirtualBox**

- [Dwonload Arch Linux](https://geo.mirror.pkgbuild.com/iso/2023.06.01/) - Download iso file
- [Balena Eatcher](https://etcher.balena.io/) - Creating bootable
- Sebelum lanjut instalasi pastikan kamu memiliki koneksi internet yang cukup stabil. **wajib konek ke internet**

**PANDUAN INSTALASI:**

- Masuk ke menu boot seperti biasanya, pada boot option pilih usb penginstalan tekan enter
- Next pilih menu `Arch Linux Install Medium...` dan tunggu sampai masuk ke `tty1`
- Setelah masuk ke tty1 silahkan konek ke `wifi` dengan cara berikut ini:

    ```shell
    # ketikkan perintah ini pada terminal tty1 dan nanti masuk ke mode iwctl
    iwctl

    # untuk mendapatkan device list contoh wlan0
    device list

    # menampilkan semua list wifi yang tersedia
    station wlan0 get-networks
    
    # konek ke wifi
    station wlan0 connect ENTER_SSID
    
    # atau
    iwctl --passphrase=PASSPHRASE station wlan0 connect SSID

    # exit iwctl
    CTRL + D / exit
    ```

- Setelah itu uji coba koneksi dengan cara

    ```shell
    ping google.com
    # exit CTRL + C
    ping google.com -c5
    ```

- Lakukan update dan install archinstall, archlinux-keyring

    ```shell
    pacman -Syu archinstall archlinux-keyring
    ```

- Setelah semua update dan instalasi selesai tinggal install menggunakan perintah

    ```shell
    archinstall
    ```

    > tunggu sampai masuk ke menu instalasi

    **Konfigurasi Instalasi**
  - Arch language (default English) tidak perlu di ubah
  - Keyboar layout US (skip tidak perlu di ubah)
  - Mirror region bebas mau di atur atau tidak (saran tidak usah)
  - Locale language (skip tidak perlu di ubah en_US default)
  - Locale encoding (skip tidak perlu di ubah utf-8 default)
  - Drive(s)
    > Pemilihan lokasi instalasi pastikan kamu memilih hardrive/ssd jangan pilih flas
    - Tab to select
  - Disk layout
    > Note: instalasi full jadi pilih
    - wipe all selected...
    - pilih ext4 untuk filesystem
  - Bootloader (skip saja)
  - Swap (skip default true)
  - Hostname (atur sesui ke inginan)
  - Root Passwor (gunakan password yang mudah kamu ingat sendiri dan susah di tebak oleh orang
  - User account (silahkan tambahkan user baru, user dan paddword jangan sampai lupa)
  - Profile (atur Pilih xorg: install a minimal ... dan untuk `graphics driver` select all
  - Audio (pilih `pipewire`)
  - Kernals (skip saja default `linux`)
  - Addtional package (skip saja biar tidak lama saat proses instalasi)
  - Network configuration (pilih Use network manager....)
  - Timezone (disesuaikan saja, saya menggunakan asia/jakarta)
  - Automatic time sync (skip default true)
  - Optional repsository (skip)
  - **Terakhir pilih install** and enter continue
- Tunggu sampai selesai dan seteleh itu reboot dan masuk kemabali login dengan user yang telah dibuat.
- Test koneksi internet apakah masih terhubung dengan cara lakukan `ping google.com`
- Jika tidak konek gunakan cara ini untuk konek kw `wifi` terlebih dahulu

    ```shell
    # kamu bisa mempelajari dulu nmcli ini apa sih?
    nmcli

    # mendapatkan list wifi
    nmcli dev wifi list
    
    # konek ke wifi
    sudo nmcli dev wifi connect NETWORK_SSID password "NETWORK_PASSWORD"
    # Note: jika ssid nya menggunakan spasi gunakan tanda "NETWORK_SSID" atau SSID\ ...
    # melihat koneksi wifi
    
    nmcli connection show
    # uji coba dengan test ping ke google.com -c5
    
    ping google.com -c5
    ```

## Install Github-CLI

```shell
sudo pacman -S git github-cli

# silahkan login dengan akun anda, dengan cara ketik token manual, tidak ada cara lain selain ini, agar bisa clone repository github.
gh auth login
# pilih github.com
# jika tidak punya token buat dulu di pengaturan akun github settings -> developer settings -> personal acces token -> token classic, silahkan di centang semau permission yang dibutuhkan (centang semua saja).
```

## Install Editor

Setelah penginstalan Arch Linux tidak ada editor bawaan, dibawah ini beberapa opsi yang dapat saya berikan, rekomended gunakan `neovim` dan pastikan kamu sudah paham soal cara penggunaanya

```shell
# install sesuai kebutuhan
sudo pacman -S neovim 
sudo pacman -S emacs
sudo pacman -S vim
sudo pacman -S nano
```

## Install Window Manager

Sebelum mendownload semua resource pastikan sudah membuat directory untuk menampung semua resources ini dengan menggunakan perintah berikut ini

```shell
mkdir -p .local/src

# jadi isi dari directory src ini adalah ada dwm, st, dmenu dan dwmblocks
# ikuti langkah di bawah untuk menginstall
```

Sebeleum melakukan instlasi di bawah agar tidak terjadi error terlebih dahulu install package ini dan lakukan konfigurasi berikut ini

``` shell
sudo pacman -S libxft libxinerama

# kemudian copy 
cp /etc/X11/xinit/xinitrc ~/.xinitrc

# kemudian modifikasi file .xinitrc seperti dibawah ini
#!/bin/sh
userresources=$HOME/.Xresources
usermodmap=$HOME/.Xmodmap
sysresources=/etc/X11/xinit/.Xresources
sysmodmap=/etc/X11/xinit/.Xmodmap
# merge in defaults and keymaps
if [ -f $sysresources ]; then
    xrdb -merge $sysresources
fi
if [ -f $sysmodmap ]; then
    xmodmap $sysmodmap
fi
if [ -f "$userresources" ]; then
    xrdb -merge "$userresources"
fi
if [ -f "$usermodmap" ]; then
    xmodmap "$usermodmap"
fi
# start some nice programs
if [ -d /etc/X11/xinit/xinitrc.d ] ; then
 for f in /etc/X11/xinit/xinitrc.d/?*.sh ; do
  [ -x "$f" ] && . "$f"
 done
 unset f
fi

dwmblocks &
exec dwm
```

- [dwm.suckless.org](https://dwm.suckless.org/) - **Dynamic Window Manager**

    Installation

    ```shell
    cd .local/src/
    git clone https://github.com/04burhanuddin/dwm.git
    cd dwm
    sudo make clean install
    ```

- [st.suckless.org](https://st.suckless.org/) - **Simple Terminal**

    Installation

    ```shell
    cd .local/src/
    git clone https://github.com/Bugswriter/st.git
    cd st
    sudo make clean install
    ```

- [tools.suckless.org](https://tools.suckless.org/dmenu/) - **Dynamic Menu**

    Installation

    ```shell
    cd .local/src/
    git clone git clone https://git.suckless.org/dmenu
    cd dmenu
    sudo make clean install
    ```

- [Dwmblocks](https://) - **Dwmblocks**

    Installation

    ```shell
    cd .local/src/
    git clone git clone https://github.com/Bugswriter/dwmblocks.git
    cd dwmblocks
    sudo make clean install
    ```

    Cara menampilkan informasi pada dwmblocks

**Next Terakhir** Install font terlebih dahulu `sudo pacman -S ttf-jetbrains-mono` agar bisa di jalankan, setelah itu jalankan perintah `startx`

## AUR Helper

Sebenarnya ada beberapa AUR Helper yang dapat digunakan tapi saya lebih rekomendasikan untuk menggunakan paru selengkapanya baca [https://wiki.archlinux.org/title/AUR_helpers](https://wiki.archlinux.org/title/AUR_helpers)

```shell
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

## Install ZSH

```shell
sudo pacman -S zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# jika belum punya curl 
sudo pacman -S curl

# stelah itu restart
```

Install plugin ZSH

```shell
# untuk memasang clone smua pligun ini
# autosuggesions plugin
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions

# zsh-syntax-highlighting plugin
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

# zsh-fast-syntax-highlighting plugin
git clone https://github.com/zdharma-continuum/fast-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/fast-syntax-highlighting

#zsh-autocomplete plugin
git clone --depth 1 -- https://github.com/marlonrichert/zsh-autocomplete.git $ZSH_CUSTOM/plugins/zsh-autocomplete

# implementasi
# Open ~/.zshrc
# cari bagian plugins=(git)
# copy dan paste code do bawah ini kedalam plugins menjadi seperti ini
plugins=(git zsh-autosuggestions zsh-syntax-highlighting fast-syntax-highlighting zsh-autocomplete)
```

Baca juga [https://github.com/romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k)

> Kamu bisa explore lagi plugins yang lainnya.

## Install Browser

Bebas saja mau pake browser apa ada `qutebrowser`, `Brave`, `Google Chrome`, `Firefox`, `Microsoft Edge`, `surf` dll

```shell
sudo pacman -S qutebrowser
sudo pacman -S firefox

# yang lainnya bisa di install menggunakan aur helper
# atau bisa dengan tanpa aur helper dengan cara clone dari aur package
paru -S google-chrome
paru -S microsoft-edge-stable-bin
paru -S brave-bin

# surf dapat kamu lihat pada https://surf.suckless.org/
```

> Jangan di install semua, sesuai kebutuhan saja.

## Install Picom

Jadai DWM tidak menyediakan compositing maka dari itu akan aneh rasanya seperti patah-patah dll, dan `picom` adalah solusi yang paling tepat untuk mengatasi window manager yang tidak menyediakan compositor. baca lengkap [https://github.com/yshui/picom/wiki/](https://github.com/yshui/picom/wiki/) or [https://wiki.archlinux.org/title/picom](https://wiki.archlinux.org/title/picom)

```shell
sudo pacman -S picom

# buat konfigurasi baru
# buat directory config yang baru terlebih dahulu
sudo mkdir .config/picom/picom.conf

# buat link menuju ke config baru yang kita buat tadi
picom --config .config/picom/picom.conf

# kemudian modifikasi config sesuai kebutuhan, animasi, rounded corners dll.
sudo nvim .config/picom/picom.conf
```

## Touchpad Error

Sebenarnya tidak error cuman tidak berfungsi seperti touchapad normal biasanya, double tap tidak berfungsi

```shell
# buat file baru di directory xorg..
sudo nvim /etc/X11/xorg.conf.d/30-touchpad.conf

# copy code ini kedalam file 30-touchapad.conf save dan restart.
Section "InputClass"
Identifier "devname"
Driver "libinput"
    Option "Tapping" "on"
    Option "NaturalScrolling" "true"
EndSection
```

**Cara lain** gunakan Touchpad Synaptics [https://wiki.archlinux.org/title/Touchpad_Synaptics](https://wiki.archlinux.org/title/Touchpad_Synaptics)

## Install Firewall

Baca lengkap disni [https://wiki.archlinux.org/title/Uncomplicated_Firewall](https://wiki.archlinux.org/title/Uncomplicated_Firewall)

```shell
sudo pacman -S ufw
sudo systemctl start ufw.service
```

## Install Apache

Konfigurasi lengkapnya baca di [Apache HTTP Server](https://wiki.archlinux.org/title/Apache_HTTP_Server) atau jika ingin menggunakan [Nginix](https://wiki.archlinux.org/title/nginx)

```shell
sudo pacman -S apache

# manage apache
sudo systemctl start httpd.service
sudo systemctl status httpd.service
```

## Install Mysql

```shell
sudo pacman -S mysql
# select 1 Mariadb

# install database
sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql

# manage mysql
sudo systemctl start mariadb.service
sudo systemctl start mysqld
sudo systemctl status mysqld

# sequre mysql
sudo mysql_secure_installation

# menambahkan user baru
sudo mysql -u root -p

# switch ke mysql
MariaDB [(none)]> USE mysql;

# menampilkan semua user
MariaDB [mysql]> SELECT user, host FROM mysql.user;

# pastikan sudah change ke mysql seperti ini kemudian buat user baru dengan
MariaDB [mysql]> create user 'YOUR_USERNAME'@'localhost' identified by 'YOUR_PASSWORD';

# mengatur hak akses user
MariaDB [mysql]> GRANT ALL PRIVILEGES ON *.* TO 'YOUR_USERNAME'@'localhost' WITH GRANT OPTION;
MariaDB [mysql]> FLUSH PRIVILEGES;

    # cek kembali
MariaDB [mysql]> SELECT user, host FROM mysql.user;
MariaDB [mysql]>quit; 
```

## Install PHP

Perlu di ingat perintah ini digunakan untuk menginstall php dengan versi terbaru yang ada di package Arch Linux, jika ingin menginstall PHP versi lain gunakan AUR Helper. baca lengkap [https://wiki.archlinux.org/title/PHP](https://wiki.archlinux.org/title/PHP)

```shell
sudo pacman -S php

# enable module php, ektifakn module yang dibutuhkan saja
sudo nvim /etc/php/php.ini
```

## Install Composer

Selengkapnya [https://getcomposer.org/download/](https://getcomposer.org/download/)

```shell
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

# cek instalasi
composer --version
```

## Install Node JS Using NVM

Baca lengkap [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

Path otomatis akan di tambahkan pada bash profile, jika menggunakan `ZSH` pastikan baris ini ada pada baris bagian bawah, simplenya jika bisa menjalankan `nvm --version` keluar versi nvm, berarti barisnya sudah ada jika tidak bisa tambahkan secara manual, copy code di bawah ini dan paste pada bash profile yang diunakan.

```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Next install `Node JS`

```shell
# untuk melihat semua version node js
nvm list-remote

# install node js
nvm install VERSI_NODEJS

# perintah ini untuk menginstall node js yan long time support
nvm install -lts

# manage node js, jika menginstall beberapa versi
nvm use VERSI_NODEJS
nvm alias default VERSI_NODEJS
```

## Install Java

Baca lengkap [https://wiki.archlinux.org/title/java](https://wiki.archlinux.org/title/java)

```shell
# menggunakan java 17
sudo pacman -S jre17-openjdk

# cek java default profile
sudo archlinux-java status

# mengganti default java profile
sudo archlinux-java set YOUR_JAVA_VERSION
```

## Install Android Studio

Install Android Studio menggunakan AUR Helper

```shell
paru -S android-studio
```

Install Android Studio tanpa AUR Helper

```shell
git clone https://aur.archlinux.org/android-studio.git
cd android-studio
makepkg -si
```

**Untuk Flutter** perlu SDK manager, buka android studio yang sudah di install pilih -> SDK Manager -> SDK Tools -> centang `Android SDK Command-line Tools` download saja dan exit.

## Install Visual Studio Code

Install VS Code menggunakan AUR Helper

```shell
paru -S visual-studio-code-bin
```

Install VS Code tanpa AUR Helper

```shell
git clone https://aur.archlinux.org/android-studio.git
cd android-studio
makepkg -si
```

## Install Flutter

[https://docs.flutter.dev/get-started/install/linux](https://docs.flutter.dev/get-started/install/linux) - Download flutter

```shell
# buat directory baru sebenarnya bebas saja, tapi di sini buat directory baru saja biar rapih
# menggunakan titik biar tidak terlihat
mkdir .development

# pindahkan file flutter .tar.xz yang telah di download kedalam directory .development

cd .development
# extract file .tar.xz setelah di extract hapus file flutter .tar.xz
tar xf flutter_linux_3.10.5-stable.tar.xz
rm flutter_linux_3.10.5-stable.tar.xz

# buat path pada bash profile dengan menggunakan perintah ini
export PATH="$PATH:`pwd`/flutter/bin"
# atau tambahkan manual
export PATH="$PATH:/home/YUR_USER/.development/flutter/bin"

# cek dependensi yang diperlukan
flutter doctor -v
# ikuti semua yang disarankan untuk di execute misal android license dll.

# manage flutter, rekomended disable saja yang tidak diperlukan
flutter config --no-enable-web
flutter config --no-enable-macos-desktop
flutter config --no-enable-windows-desktop
flutter config --no-enable-linux-desktop
```

## Install Cisco Packet Tracer

Untuk menginstall `packet tracer` hanya bisa menggunakan package dari AUR Helper, jangan install menggunakan AUR Helper dikarenakan akan terjadi error berikut adalah caranya

[https://www.netacad.com/portal/resources/packet-tracer](https://www.netacad.com/portal/resources/packet-tracer) - Download packet tracer

```shell
# pertama clone dulu
git clone https://aur.archlinux.org/packettracer.git

# gunakan link di atas untuk mendownload packet tracer jika tidak punya akun silahkan buat dulu
# kemudian pindahkan file packet tracer ...deb yang telah di download kedalam directory yang di clone tadi.

# masuk kedalam directory packettracer dan install
cd packettracer

# pastikan file packet tracer yang .deb ada di dalam directory packettracer kemudian install
ls
makepkg -si
```

## Laravel Global Installer

```shell
composer global require laravel/installer

# tambahkan path ini pada bash profile
export PATH="$HOME/.config/composer/vendor/bin:$PATH"

# check laravel globl version
laravel --version

# create project laravel
laravel new PROJECT_NAME
```

## Konfigure Input & Output Audio

Jangan gunakan `pulseaudio` akan terjadi masalah, gunakan `pipewire` agar tidak terjadi masalah di audio input. Biar tidak ribet gunakan `pipewire`

```shell
sudo pacman -S pipewire pipewire-pulse

# kemudian copy code ini dan paste kedalam file .xinitrc sebelum exect dwm  
/usr/bin/pipewire &
/usr/bin/pipewire-pulse &
/usr/bin/pipewire-media-session &

# atau
pipewire &
pipewire-pulse &
pipewire-media-session &
```

Install package `pavucontrol` untuk emngontroll audio profile dengan GUI

```shell
sudo pacman -S pavucontrol
```

Jika sudah tinggal buka pavucontrol

## Install Bluetooth

Baca lengkap [https://wiki.archlinux.org/title/bluetooth](https://wiki.archlinux.org/title/bluetooth)

```shell
sudo pacman -S bluez bluez-utils

# manage bluetooth
systemctl enable bluetooth.service
systemctl start bluetooth.service
```

## Manage Font

[https://wiki.archlinux.org/title/fonts](https://wiki.archlinux.org/title/fonts) - Panduan menambahakna font yang lainnya

```shell
# fix emoji
sudo pacman -S noto-fonts-emoji

# buat file baru
sudo nvim /etc/fonts/local.conf

# copy dan paste code ini kedalam local.conf save dan restart
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
 <alias>
   <family>sans-serif</family>
   <prefer>
     <family>Noto Sans</family>
     <family>Noto Color Emoji</family>
     <family>Noto Emoji</family>
     <family>DejaVu Sans</family>
   </prefer> 
 </alias>

 <alias>
   <family>serif</family>
   <prefer>
     <family>Noto Serif</family>
     <family>Noto Color Emoji</family>
     <family>Noto Emoji</family>
     <family>DejaVu Serif</family>
   </prefer>
 </alias>

 <alias>
  <family>monospace</family>
  <prefer>
    <family>Noto Mono</family>
    <family>Noto Color Emoji</family>
    <family>Noto Emoji</family>
   </prefer>
 </alias>
</fontconfig>
```

Lalu bagaimana cara mengatur default font interface install package ini

```shell
sudo pacman -S lxappearcnce-gtk3
```

Kemudian buka `lxappearcnce` untuk menagatur font pastikan kamu sudah menambahakan font di `.local/share/fonts/` ikuti laing di atas untuk menginstall font yang lainnya

## Set Walpaper

Baca selengkapnya [https://wiki.archlinux.org/title/feh](https://wiki.archlinux.org/title/feh)

```shell
Sudo pacman -S feh

# contoh mengatur walpaper click link di atas untuk detail
feh --bg-scale /path/to/image.file

# tambahkan baris ini pada .xinitrc
~/.fehbg &
```

## Cara Screenshoot

Sebenarnya banyak opsi yang bisa digunkan untuk mengambil tangkapan layar namu disini saya menggunakan `maim` dan sekaligus install package sxhkd untuk mengatur key binddings untuk melakukan screenshoot. Baca Selengkapnya [https://man.archlinux.org/man/maim.1.en](https://man.archlinux.org/man/maim.1.en) - untuk screenshoot opsi lainnya.

```shell
sudo pacman -S maim sxhkd

# menggunakan maim, perintah ini digunakan untuk mengambil tangkapan layar dan akan di simpan di directory hoem dengan nama screenshoot.png
maim ~/screenshot.png

# mengambil screenshoot dengan key binds
# buat sebuah file 
sudo nvim .config/sxhkd/sxhkdrc

# copy dan paste code ini pada file sxhkdrc, modifikasi sesuai kebutuhan
super + shift + p
    maim -s ~/pictures/screenshoot/$(date +%s).png

# -s adalah select jika tidak mau langsung saja tanpa -s click link di atas untuk opsi lainnya
# save dan jalankan 
sxhkd &
# dan tambahkan sxhkd & pada file .xinitrc sebelum exac dwm
# screenshoot dengan menggunakan key binds super + shift + p dan otomatis akan tersimpan di directory ~pictures/screenshot
```

## Mneggunakan SSH Key untuk Github

Jadi sebelumnya menggunaakan github-cli dan saya lebih suka menggunakan ssh key berikut ini ada cara settingnya. sebelumnya remove dulu `sudo pacman -Rsuc github-cli` karena kita tidak menggunakan nya lagi
> [referensi here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

```shell
cd ~/.ssh
ssh-keygen -t rsa -b 4096 -C "contoh@gmail.com"

Generating public/private rsa key pair.
Enter file in which to save the key (/home/dev/.ssh/id_rsa): YOUR_ID
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in ....
Your public key has been saved in ....pub
The key fingerprint is:
SHA256:SJ6tYaXowqW0CB..............
The key's randomart image is:
+---[RSA 4096]----+
|==o.o. .. .      |
|B ++o..  . .     |
|o+.=o.. .   .    |
|.oooo+ *   .     |
|. +.o B S   o   E|
|.+ = . o   . . +.|
|. = . .       o =|
|   .          .o+|
|             ...o|
+----[SHA256]-----+

cat ~/.ssh/YOUR_ID.pub
# copy semua dan simpan ke ssh keys github settings -> SSH and GPG Keys -> new ssh

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/YOUR_ID HERE

# pastikan sudah sukses seperti ini
ssh -T git@github.com
Hi ....! You've successfully authenticated, but GitHub does not provide shell access.
```

## Install Neofetch

[https://github.com/Chick2D/neofetch-themes](https://github.com/Chick2D/neofetch-themes) - Thme Neofetch

```shell
sudo pacman -S neofetch
```

Run `neofetch` pada terminal dan foto kemudian upload di sosmed kamu dengan caption `I USE ARCH BTW` 🤣

## Setup Neovim

[Tutorial Neovim](https://www.youtube.com/watch?v=vdn_pKJUda8&t=2363s) - Sangat rekomended implementasi sesuai kebutuhan saja.

## Others

- Saran saya sering-saering baca wiki arch linux karena di sana sudah lengkap banget untuk dokumetasi dari setiap package-package yang tersedia pada package arch maupun AUR Package.
- Jangan takut untuk explore lebih dalam lagi tentang Arch Linux, tapi ingat hati-hati karena bisa beresiko fatal jika kebanyakan explore package, gunakan sesuai kebutuhan saja, jangan sampai memberatkan sistem kamu sendiri
- Modifikasi key binddings kamu sesuai ke inginan kamu di setiap file `config.h` pada **dem**, **dmenu**, **st** dan jika kamu mau melakukan patch, saran patch sesuai kebutuhan saja dan pastikan kamu sudah membackup file utama sebelum di otak-atik dan cari tutorial cara patch yang baik dan benar.

## Butuh Bantuan 😂

[Discord](https://discordapp.com/users/04burhanuddin) - Contact
