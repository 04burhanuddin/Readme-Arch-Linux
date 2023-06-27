# **EASY RICING ARCH LINUX (NEWBIE)**
> Jika kamu sudah install arch linux

# <h2 style="color:orange">**Install Arch Linux**</h1>
- Download ISO
    > Download [Disini](https://geo.mirror.pkgbuild.com/iso/2023.06.01/) Pilih `archlinux-2023.06.01-x86_64.iso`
- Create Bootable
    > Gunakan ini [Balena/eatcher](https://etcher.balena.io/) untuk membuat Bootable
- Panduan Instalasi
    > Pastikan sudah membuat bootable
    - NOTE: panduan ini menginstall secara full no dual boot jadi hati-hati, silahkan ikuti petunjuk instalasi lain jika ingin dual boot.
    - Masuk ke menu boot seperti biasa, restart press F2 sampai masuk menu boot pilih `USB yang kamu gunakan` pada menu boot optios
    - Pilih menu `Arch Linux Install Medium ...` dan tunggu sampai masuk ke tty1 arch linux
    - Konek ke wifi dengan perintah berikut ini
        > Wajib konek wifi
        ```sh
        # masuk ke iwctl dengan perintah
        iwctl

        # untuk melihat panduan manual
        [ictl] iwctl help

        # mendapatkan list device
        [iwctl] device list

        # disini saya menggunakan wlan0 karna ini device yang ada di list saya

        # gunakan perintah ini untuk scan daftar wifi yg tersedia
        [iwctl] station wlan0 get-networks

        # gunakan perintah ini untuk konek ke wifi 
        # Note jika wifi nya menggunakan spasi gunakan tanda "SSID" 
        [iwctl] station wlan0 connect SSID

        # jika tidak bisa tambahkan security type pada bagian akhir misal
        [iwctl] station wlan0 connect SSID psk

        # atau 
        [iwctl] iwctl --passphrase=PASSPHRASE station wlan0 connect SSID

        # exit iwctl
        CTRL + D 
        
        # atau 
        [iwctl] exit
        ```
    - Next
        > posisi ada di tty1 arch linux bukan di iwctl tadi
        - Pastikan sudah konek ke internet bia test menggunakan `ping google.com`
            ```st
            ping google.com
            ```
    - Install package archinstall dan upgrade kyring terlebih dahulu
        ```sh
        pacman -Syu archinstall archlinux-keyring
        ```
    - Selanjutnya tinggal install dengan menggunakan perintah
        ```sh
        archinstall
        ```
        > Tunggu sampai masuk ke menu instalasi dan set sesuai kebutuhan saja berikut ini yg saya lakukan
        - Arch language (default English) tidak perlu di ubah
        - Keyboar layout US (skip tidak perlu di ubah)
        - Mirror region bebas mau di atur atau tidak (saran tidak usah)
        - Locale language (skip tidak perlu di ubah en_US default)
        - Locale encoding (skip tidak perlu di ubah utf-8 default)
        - Drive(s)
            > Pemilihan lokasi instalasi pastikan kamu memilih hardrive/ssd jangan pilih flash USB
            - Tab to select
        - Disk layout
            > Note: instalasi full jadi pilih 
            - wipe all selected...
            - pilih ext4 untuk filesystem 
        - Bootloader (skip saja)
        - Swap (skip default ttrue)
        - Hostname (atur sesui ke inginan)
        - Root Passwor (gunakan password yang mudah kamu ingat sendiri dan susah di tebak oleh orang lain)
        - User account (silahkan tambahkan user baru, user dan paddword jangan sampai lupa gan)
        - Profile (atur Pilih xorg: install a minimal ... dan untuk `graphics driver` pilih sesuai kebutuhan atau all saja)
        - Audio (pilih `pipewire`)
        - Kernals (skip saja default `linux`)
        - Addtional package (skip saja biar tidak lama saat proses instalasi)
        - Network configuration (pilih Use network manager....)
        - Timezone (disesuaikan saja, saya menggunakan asia/jakarta)
        - Automatic time sync (skip default true)
        - Optional repsository (skip)
        - **Terakhir pilih install** and pree enter continue
        - Tunggu sampai selesai dan seteleh itu reboot san masuk kemabali login dengan user yang sudah dibuat pas konfigurasi instalasi.
    - **Selamat Arch sudah Terinstall**
- Jangan bingung dulu, lanjut ke **WINDOW MANAGER**

# <h2 style="color:orange">**Window Manager**</h1>
Bebas saja menggunakan window manager apapun tapi disini saya menggunakan DWM dari `suckless`
- **NOTE**
    - Setelah arch kamu di reboot pastikan terlebih dahulu kamu masih konek ke internet jika tidak kamu bisa konek menggunakan perintah ini, `iwctl` sudah tidak bisa di akses gunakan ini untuk setiap kali konek ke wifi misal diluar rumah, di kampus dll.
        ```sh
        # kamu bisa mempelajari dulu iwctl ini apa sih apa aja yg bisa dilakukan dengan ictl sebenarnya bukan cuman ini kamu bisa explore lebih banyak lagi cara konek ke wifi, tools ini bawaan dari arch linux (rekomended pake ini saja).
        
        # ketikkan
        nmcli

        # mendapatkan list wifi 
        nmcli dev wifi list

        # konek ke wifi
        sudo nmcli dev wifi connect NETWORK_SSID password "NETWORK_PASSWORD"
        # Note: jika ssid nya menggunakan spasi gunakan tanda "SSID" atau SSID\ ...

        # uji coba dengan test ping ke google.com -c5
        ping google.com -c5 
        ```

> Penting: Sebelum lanjut keabawah install package ini terlebih dahulu.
```sh
# libxft dan libxinerama
sudo pacman -S libxft libxinerama

# kemudian copy
cp /etc/X11/xinit/xinitrc ~/.xinitrc

# kemudian modifikasi file menjadi seperti ini, bisa menggunakan nano atau vim atau neovim di sini saya menggunakan neovim. kalo tidak tersedia tinggal install menggunakan sudo pacman -S ...

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
> Full menggunakan [suckless.org](https://suckless.org/)
- **Persiapkan Directory**
    - Opsss... pastikan kamu sudah menginstall git jika tidak ikut langkah ini
        ```sh
        # install git dan github-cli
        sudo pacman -S git github-cli

        # kok pake github cli segala? iya sejak update git versi sekarang kamu ga bakal bisa clone repo tanpa login telebih dahulu ikuti langkah ini untuk login ke github menggunakan github cli

        # jika git dan github cli sudah terinstall lanjut eksekusi

        gh auth login
        # pilih github.com, kenapa pake ini iya karna ini lenih simple, kamu ga mungkin mengetik satu-satu ssh keys

        # Note : pastikan kamu sudah membuat token pada github kamu pada pengaturan developer settings, buat personal akses token pilih token(classic), dan tentukan permission saran centang semua saja, nah toke bakal kamu ketik satu persati ke gh atuh login tadi pastikan jangan sampai salah ketik ngab biar ngak bolak balik perlahan aja tapi pasti, tidak ada cara lain selain ketik manual.

        # pastika authentication succes ngab dan bisa clone repo nanti next lanjut di bawah ini
        ```
    - Buatlah directory baru menggunakan perintah ini, pastikan kamu ada di home ~/
        ```sh
        mkdir -p .local/src

        # kemudian change directory
        cd .local/src

        # lanjut ke bawah clone semua dwm, st, dmenu, dwmblock dan surf(tidak rekomended)
        # Note: pastikan clone repo di directory .local/src disini kamu bakal menyimpan semau resource dwm, st, dwmblocks dll.
        ```
- **DWM**
    > Note: Bisa baca-baca [disini](https://dwm.suckless.org/) website resmi dwm untuk memplejari sedikit-demisedikit tantang dynamic window manager ini, config dwm yg saya buat ini menggunakan versi terbaru saat tutorial ini dibuat yaitu v6.4. Kamu dapat mengubah keybinds kamu sendiri bisa di file config.h dan bisa juga di file utama config.def.h. contoh untuk masuk ke terminal st gunakan `super/logo window + shift + enter ` jika file config.h lihat cara instalasi di bawah. makanya pastikan kamu sudah membaca documentasi pada web `suckless.org` biar paham.
    - Patch yang sudah include
        > warning: saran saya sebelum melakukan penambahan patch pastikan kamu sudah mempelajari cara patch yang benar seperti apa dan minimal paham `bahasa pemrograman c.`
        - fullgaps versi 6.4
        - status2d versi 6.3
        - xrdb versi 6.4
        - sudah tersedia directory patch untuk menyimpan patch tambahan kamu. setelah di patch di file patch tidak masalah jika ingin di hapus.
        - tambah sendiri sesuai kebuthan dan hati-hati saran saya pelajari cara patch yang baik dan benar seperti apa bisa lihat tutor di yt dll baru kamu mulai pasang patch baru
    - Clone & install
        > Warning: jangan di modifikasi sembarangan kalo belum paham benar maksudnya pastikan kamu mengerti `bahasa pemrograman C` walaupun dikit dan file jangan ada yang di remove ya biar tidak ada masalah saat instalasi.
        - pastikan kamu ada pada directory ~/.local/src kemudian clone
            ```sh
            # langsung clone saja
            git clone https://github.com/04burhanuddin/dwm.git

            # install
            cd dwm

            # dan eksekusi dengan
            sudo make clean install

            # note jika terjadi error remove config.h dulu
            sudo rm config.h

            # Terus install lagi
            sudo make clean install
            ```
- **ST**<br>
    Clone & install
    > Warning: jangan di modifikasi sembarangan kalo belum paham benar maksudnya pastikan kamu mengerti `bahasa pemrograman C` walaupun dikit dan file jangan ada yang di remove ya biar tidak ada masalah saat instalasi. Kamu dapat mengubah keybinds kamu sendiri bisa di file config.h dan bisa juga di file utama config.def.h
    - pastikan kamu ada pada directory ~/.local/src kemudian clone
        ```sh
        # langsung clone saja
        git clone https://github.com/04burhanuddin/st.git

        # install
        cd st
        
        # dan eksekusi dengan
        sudo make clean install

        # note jika terjadi error remove config.h dulu
        sudo rm config.h

        # Terus install lagi
        sudo make clean install
        ```
- **DMENU**<br>
    Clone & install
    > Warning: jangan di modifikasi sembarangan kalo belum paham benar maksudnya pastikan kamu mengerti `bahasa pemrograman C` walaupun dikit dan file jangan ada yang di remove ya biar tidak ada masalah saat instalasi. Kamu dapat mengubah keybinds kamu sendiri bisa di file config.h dan bisa juga di file utama config.def.h
    - pastikan kamu ada pada directory ~/.local/src kemudian clone
        ```sh
        # langsung clone saja
        git clone https://github.com/04burhanuddin/dmenu.git

        # install
        cd st
        
        # dan eksekusi dengan
        sudo make clean install

        # note jika terjadi error remove config.h dulu
        sudo rm config.h

        # Terus install lagi
        sudo make clean install
        ```
- **DWMBLOCKS**<br>
    Clone & install
    > Warning: jangan di modifikasi sembarangan kalo belum paham benar maksudnya pastikan kamu mengerti.
    - pastikan kamu ada pada directory ~/.local/src kemudian clone
        ```sh
        # langsung clone saja
        git clone https://github.com/04burhanuddin/dwmblocks.git

        # install
        cd st
        
        # dan eksekusi dengan
        sudo make clean install

        # note jika terjadi error remove config.h dulu
        sudo rm config.h

        # Terus install lagi
        sudo make clean install
        ```
    - Biar ini berjalan dengan lancar ada beberapa tambahan berikut ini:
        ```sh
        # buat directory baru di .local/bin
        mkdir .local/bin
        cd .local/bin

        # clone repo
        git clone https://github.com/04burhanuddin/statusbar.git

        # pastikan semua bisa di eksekusi dengan cara melakukan test misal

        ./clock
        # pastikan memiliki output jika tidak kamu dapat mengubahnya dengan cara
        sudo chmod +x clock

        # file ini tidak bisa di eksekusi di luar dari directory caranya bagaimana agar bisa mengaksesnya, kamu bisa menambahkan path pada bash profile di sini saya menggunakan zsh

        # tambahkan pada bagian bawah
        export PATH="$HOME/.local/bin/statusbar/:$PATH"
        # save and exit sekarang ketikkan
        source .zshrc #karna saya menggunakan zsh

        # dan sekarang dimanapun kamu bisa mengetikkan perintah clock untuk mendapatkan waktu begitupun dengan sckrip yang lainya. expore lagi cara yang lainnya
        ```
    - Pengetahuan Dasar
        > Untuk menampikan informasi seperti tanggal itu adalah hasil dari script di atas yang dibuat dan agar bisa di eksekusi gunakan perintah chmod +x namafile kamu dapat melihat contohnya pada statusbar di atas.<br>
        pada file config.h yang ada pada directory `.local/src/dwmblock.config.h` kamu dapat melihat baris ini `{"^c#EBDBB2^", "clock", 5, 0},` yang di eksekusi ada skrip shell semua jadi semisal kamu pengen menambah sebuah script baru pastikan script shell nya bisa di run dan mengeluarkan output.<br>
        <br>
        dwmblocks ini akan terus berjalan karena kita sudah menambahkan pada file `.xinitrc` dwmblock &, kamu dapat menghentikannya dengan cara `pkill dwmblocks` dan menjalankannnya lagi menggunakan `dwmblocks &` begitupun jika kamu melakukan modifikasi kill dan jalankan lagi untuk mendapatkan perubahan, dan buka cuman ini misal kamu menginstall `fehbg` untuk menjalankannya bisa menambakan bari sebelum exec dwm `~/fehbg &` intinya semua program yang ingin kamu jalankan pertama kali saat startx tinggal tambahkan saja pada .xinitrc
- **SURF**
    - Penginstalan sama dan baca isi file readme.md ada beberapa package yang di butuhkan, sebenarnya ini cuman buat browsing aja, namun perlu di ingat di sini tidak akan bisa membuka website yang mengandung play video misal youtube dll. makanya tidak rekomended butuh effort untuk patch dan konfigurasi.

- **Pengecekan Kembali**
    > Pastikan kamu sudah mengikuti semua langkah di atas dengan benar dan cek kembali file .xinitrc kamu sudah benar atau belum seperti ini
    ```sh
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
    > Note: sebelum di start pastikan kamu sudah menginstall beberapa font misal karna default semua dwm, st, dmenu dan dwmblock menggunakan jetbrains mono nerd font kamu bisa menginstall font dengan perintah ```sudo pacman -S ttf-jetbrains-mono``` tidak apa kamu menggunakan font ini setelah di start kamu bisa menambahkan font JetbrainsMono Nerd Font. dan font-font lainnya 
    <br>
    <br>
    **Jika sudah selesai semua kamu dapat masuk tinggal jalankan perintah `startx` pada tty1 arch kamu dan selamat kamu sudah menggunakan dynamic window manager key binds dapat kamu modifikasi pada masing-masing config musal untuk di dwm config nya ada pada directory `.local/src/dwm/config.h` atau modifikasi file utamanya config.def.h**

# <h2 style="color:orange">**Touchpad Error**</h1>
> sebenarnya tidak error cuman tidak berfungsi kayak touchpad pada umumnya tinggal gunakan konfigurasi berikut ini.
```sh
cd /etc/X11/xorg.conf.d/30-touchpad.conf

# cek file apa saja yg ada di sana jika belum ada lanjut buat file di bawah jika ada cikup edit 
ls

# buat file baru dengan menggunakan nano atau vim atau neovim
nvim 30-touchpad.conf

# kemudian copy code ini dan paste pada file 30-touchpad.conf anda.
Section "InputClass"
Identifier "devname"
Driver "libinput"
    Option "Tapping" "on"
    Option "NaturalScrolling" "true"
EndSection

# kemudial kill dwm anda
pkill dwm

# login kembali and finally, jika masih error install driver ini
sudo pacman -S xf86-input-libinput
```
> Cara lain [disini](https://wiki.archlinux.org/title/Touchpad_Synaptics), kode di bawah ini work for me.

# <h2 style="color:orange">**Firewal**</h1>
> Lebih lengkap [disini](https://wiki.archlinux.org/title/Uncomplicated_Firewall)
- Install ufw
    ```sh
    sudo pacman -S ufw
    ```
- Manage firwwall
    ```sh
    systemctl start ufw.service
    ```
## <h2 style="color:orange">**AUR Helper**</h1>
> Saran saya gunakan paru, kenapa pake paru baca lengkap [disini](https://wiki.archlinux.org/title/AUR_helpers) dan lihat sendiri perbandingannya.
```sh
git clone https://aur.archlinux.org/paru.git

cd paru

# kemudian eksekusi
makepkg -si
```

# <h2 style="color:orange">**Apache**</h1>
> Lebih lengkapnya [disini](https://wiki.archlinux.org/title/Apache_HTTP_Server)
- Install apache
    ```sh
    # install
    sudo pacman -S apache

    # manage apache
    sudo systemctl start httpd.service
    sudo systemctl status httpd.service
    ```
# <h2 style="color:orange">**Mysql**</h1>
- Perintah install mysql
    ```sh
    sudo pacman -S mysql
    ```
    > Pilih providers 1 yaitu Mariadb
- Cek versi yan berhasil di install
    ```sh
    mysqld --version
    ```
- Sebelum start jalankan perintah
    ```sh
    sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
    ```
- Manage Mysql/Mariadb
    ```sh
    sudo systemctl start mariadb.service
    sudo systemctl start mysqld
    sudo systemctl status mysqld
    ```
- Mengamankan MYSQL
    > Tinggal ikutin instruksi sampai selesai
    ```sh
    $ sudo mysql_secure_installation

    [sudo] password for dev:
    NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

      In order to log into MariaDB to secure it, we'll need the current
      password for the root user. If you've just installed MariaDB, and
      haven't set the root password yet, you should just press enter here.
      
      Enter current password for root (enter for none):
      OK, successfully used password, moving on...
      
      Setting the root password or using the unix_socket ensures that nobody
      can log into the MariaDB root user without the proper authorisation.
      
      You already have your root account protected, so you can safely answer 'n'
      Switch to unix_socket authentication [Y/n] y
      Enabled successfully!
      Reloading privilege tables..
      ... Success!
    ```
- Membuat User baru
    ```sh
    # login terlebih dahulu
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
- Login dengan user baru
    ```sh
    sudo mysql -u YOUR_USERNAME -P
    ```

# <h2 style="color:orange">**PHP**</h1>
- Perintah install PHP
    > Perlu di ingat perintah ini akan menginstall php paling terbaru yaitu 8.2.. untuk menginstall versi php lainnya anda dapat menginstall melalui aur helper
    ```sh
    sudo pacman -S php
    ```
- Install Module PHP

# <h2 style="color:orange">**Composer**</h1>
> Lebih lengkapnya [disini](https://getcomposer.org/download/)
- Download dan install Composer
    ```sh
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    php composer-setup.php
    php -r "unlink('composer-setup.php');"
    ```
- Pindahkan `composer.phar` kedalan direktori pada PATH untuk menjadikan composer global installer jadi pada direktory manapun anda bisa memanggil composer ngab tadi pas install di root kan seakarang pindahkan menggunakan perintah ini ngab
    ```sh
    sudo mv composer.phar /usr/local/bin/composer
    ```
    Ketik `composer` pada terminal pastikan tidak ada error
# <h2 style="color:orange">**NVM & Node JS**</h1>
> Saran gunakan NVM untuk menginstall node js
- Install NVM (Node Version Manager) mengapa pake nvm, menggunakan nvm kamu bisa menginstall node js versi yang berbeda di laptop kamu
- Install NVM menggunakan perintah
    ```sh
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
    ```
- Jika sudah selesai cek Versi nvm yang terinstall untuk memastikan benar-benar sudah terintall jika mengalami kendala update PATH
- Jika kamu menggunakan zsh sebagai default shell tambahkan PATH pada shell profile dengen menggunakan perintah
    ```sh
    sudo nano ~/.zshrc
    ```
- Pada baris bagian akhir tambahkan
    > otomatis ada pada shell default kamu
    ```sh
    export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
    ```
- Dan cek lagi versi harusnya sudah keluar versi vnm kamu
- Lalu bagaimana menginstall node js menggunakan nvm tenag ngab gampang banget gunakan perintah
    ```sh
    nvm list-remote
    ```
    Tinggal pilih mau install node js versi berapa, mau install lebih dari 1 boleh ngab cara install nya menggunakan
    ```sh
    nvm install Veri_NodeJS
    
    // atau
    nvm install –lts 
    ```
    Lalu bagaiman jika ingin menggunakan node versi yang lain yang sudah kamu install gunakan perintah di ikuti dengan versi node yang ingin kamu gunakan
    ```sh
    nvm use Versi_NodeJS

    // untuk memgatur defailt versi node yang akan kamu gunakan
    nvm alias default Versi_NodeJS 
    ```

## <h2 style="color:orange">**JAVA**</h1>
> [Click ini](https://wiki.archlinux.org/title/java) Untuk menetukan java sesuai kebutuhan kamu
- Install Java Sesuai Kebutuhan, cebagai contoh menggunakan versi 17
    
    ```sh
    sudo pacman -S jre17-openjdk
    ```
- Setting Environment Untuk Java
    > Jika kamu memiliki versi java yang terinstall cuman 1 tidak perlu melakukan konfigurasi environment karna otomatis secara default menggunakan java yg kamu install, environent ini digunakan untu mgganti dfault java kamu.
    ```sh
    sudo archlinux-java status

    # example
    archlinux-java set <JAVA_ENV_NAME>
    archlinux-java set java-8-openjdk/jre
    ```
## <h2 style="color:orange">**Android Studio**</h1>
- Untuk menginstall android studio, bisa menggunakan package dari AUR. Sebenarnya ada beberapa versi android studio pada AUR Package ada beta, stable dll, di sini saya menggunakan channel stable.
    ```sh
    git clone https://aur.archlinux.org/android-studio.git

    # change directory
    cd android-studio

    # instal
    makepkg -si
    ```
- Setelah di install, silahkan di buka dan download SDK yang dibutuhkan

## <h2 style="color:orange">**Flutter**</h1>
> Click [disini](https://docs.flutter.dev/get-started/install/linux) untuk panduan instalasi lengkapnya
- Dwonload flutter
    ```sh
    # sebelumnya buat folder biar rapih
    mkdir development
    cd devekopment
    # extrack file flutter linux tadi dengan perintaj
    tar xf flutter_linux_3.10.5-stable.tar.xz

    # remove file .tar.xz gak guna lagi

    # ketikkan perintah ini kemudian copy path
    export PATH="$PATH:`pwd`/flutter/bin"

    nvim .zshrc
    # Flutter
    export PATH="$PATH:/home/dev/development/flutter/bin"
    ```
- Cek dependensi yang diperlukan dengan cara mengetikkan perintah
    > Sebelum di jalanakn pastikan kamu sudah menginstall android studio dan SDK Android SDK Command Line Tools, bisa di cek SDK Manager aplikasi android studio

    ```sh
    flutter doctor -v
    # pastikan tidak ada error dan perhatikan dependensi yang dibutuhkan apakah sudah terpenuhi jika tidak tinggal mengikuti perintah yang ada pada saat run flutter doctor -v agar semua dependensi yang dibuthkan terinstall, jika kamu hanya fokus pada flutter untuk android dan ios saja maka yang lain dapat di disable dengan perintah
    
    # disable sesuai kebutuhan
    flutter config --no-enable-web
    flutter config --no-enable-macos-desktop
    flutter config --no-enable-windows-desktop
    flutter config --no-enable-linux-desktop
    ```

## <h2 style="color:orange">**Neovim/Visual Studio Code**</h1>
- Install Neovim
    ```sh
    sudo pacman -S neovim
    ```
- Install Visual Studio Code
    - Install mengunakan aur helper
        ```sh
        paru -S code
        ```
    - Install tanpa menggunakan AUR helper
        ```sh
        # clone repo 
        git clone https://aur.archlinux.org/visual-studio-code-bin.git
        
        # pindah ke directory
        cd visual-studio-code-bin

        # Kemudian install
        makepkg -si
        ```
## <h2 style="color:orange">**Laravel Global Installer**</h1>
- Gunakan composer untuk menginstall laravel secara global menggunakan perintah
    ```sh
    composer global require laravel/installer
    ```
- Edit PATH, jika kamu menggunakan zsh atau bash
    ```sh
    export PATH="$HOME/.config/composer/vendor/bin:$PATH"
    ```
- Terakhir cek apakah benar PATH yang dibuat
    ```sh
    laravel --version
    ```
    Harusnya keluar versi laravel global installer ngab.
- untuk membuat project gunakan perintah
    ```sh
    laravel new Nama_Project
    ```
    Jika kamu ingin membuat project laravel dengan versi tertentu silakan gunakan composer ngab
    ```sh
    composer create-project --prefer-dist laravel/laravel blog "6.*"
    ```
    Perintah di atas contoh untuk menginstall laravel versi 6

## <h2 style="color:orange">**Cisco Packet Tracer**</h1>
> Hanya bisa di install melalui aur package, ntah mneggunakan aur helper bebas saja di sini saya install tanpa menggunakan aur helper langsung clone repo dari aur package, kenapa menggunakan cara ini dikarenakan membutuhkan file `CiscoPacketTracer.deb` dapat anda download di akun `netacad` kamu jika belum punya akun bikin terlebih dahulu [disini](https://www.netacad.com/), kemdian download packettracer pada menu resources. setelah itu simpan file yang telah di dwonload kedalam folder packettracer yang di clone tadi kemudian install menggunakan `makepkg -si`.
```sh
git clone https://aur.archlinux.org/packettracer.git

# sebelum di eksekusi pastikan file packettracer.deb ada di dalam directory packettracer kemudian eksekusi dengan perintah ini
makepkg -si

# done
```

## <h2 style="color:orange">**TIPS**</h1> 
> 1. Pasti kamu ngerasa kok aneh banget ya patah-patah saat refresh sebuah halaman page yang isinya gambar yup ini bisa di atasi dengan menginstall `picom` dan kamu dapat menambahakn picom ini pada `.xinitrc` agar berjalan terus saat start tanpa menjalankan secara manual tambahkan pada baris sebelum `exec dwm` ini ```picom &``` dan masalah teratasi dan kamu dapat melihat detailnya di arch wiki
> 2. Masalah kedua lalu bagaimana untuk screenshoot kamu bisa menginstall package `maim` dan untuk mengatur keybinds kamu bisa menginstall tambahan yaitu `sxhkd` silahkan atur config kamu di dirctory .config selengkapnya kamu bisa search di arch wiki
> 3. Lagi lagi jangan malas membaca mau terima beres doang enak bet ya wkwkwkwk.
> 4. Silahkan Explore lebih jau soal package-package yang begitu banyak dan buat rice yang nyaman buat kamu jangan sembarangan patch dwmnya biar aman sering-sering backup aja. dan rada ribet ya untuk patch nya dikarenakan kalo error kamu harus nambah/mengurangi sesuai intruksi riject nya dan manual pada baris ke sekian, tapi kalo udah ngerti cara patch yg benar pasti aman-aman saja.

# <h2 style="color:blue">**I use arch btw**</h1>