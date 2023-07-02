# **SETUP ARCH INUX**

**Warning**: Don’t blindly use my settings unless you know what that entails. Use at your own risk!

## Table Of Contents

- Install Arch Linux
- Install Editor
- Install Window Manager
- Install Picom
- Touchpad Error
- AUR Helper
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
- Manage Font
- Cara Screenschoot

## Install Arch Linux

- [Dwonload Arch Linux](https://geo.mirror.pkgbuild.com/iso/2023.06.01/) - Download iso file
- [Balena Eatcher](https://etcher.balena.io/) - Creating bootable

Silahkan install sesuai dengan kebutuhan anda, tutorial ini untuk instalasi full, tidak dual boot jika mau dual boot silahkan carai cara instalasi di google atau youtube.

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

# jadi nanri isi dari directory src ini adalah ada dwm, st, dmenu dan dwmblocks
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

## AUR Helper

Sebenarnya ada beberapa AUR Helper yang dapat digunakan tapi saya lebih rekomendasikan untuk menggunakan paru selengkapanya baca [https://wiki.archlinux.org/title/AUR_helpers](https://wiki.archlinux.org/title/AUR_helpers)

```shell
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

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

## Cara Screenschoot

Sebenarnya banyak opsi yang bisa digunkan untuk mengambil tangkapan layar namu disini saya menggunakan `maim` dan sekaligus install package sxhkd untuk mengatur key binddings untuk melakukan screenshoot.

```shell
sudo pacman -S maim sxhkd

# menggunakan maim, perintah ini digunakan untuk mengambil tangkapan layar dan akan di simpan di directory hoem dengan nama screenshoot.png
maim ~/screenshot.png

# mengambil screenshoot dengan key binds
# buat sebuah file 
sudo nvim .config/sxhkd/sxhkdrc

# copy dan paste code ini pada file sxhkdrc, modifikasi sesuai kebutuhan
super + shift + p
    maim ~/pictures/screenshoot/$(date +%s).png

# save dan jalankan 
sxhkd &
# dan tambahkan sxhkd & pada file .xinitrc sebelum exac dwm
# screenshoot dengan menggunakan key binds super + shift + p dan otomatis akan tersimpan di directory ~pictures/screenshot
```
