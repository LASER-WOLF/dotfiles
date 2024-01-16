# dotfiles

![screenshot](screenshots/screenshot_00.png?raw=true "screenshot")

------

![screenshot](screenshots/screenshot_01.png?raw=true "screenshot")

------

![screenshot](screenshots/screenshot_02.png?raw=true "screenshot")

------

![screenshot](screenshots/screenshot_03.png?raw=true "screenshot")

\*
Fonts from:
The Ultimate Oldschool PC Font Pack
https://int10h.org/oldschool-pc-fonts/

Lock-screen from:
https://www.reddit.com/r/unixporn/comments/3358vu/i3lock_unixpornworthy_lock_screen/

Vim settings from:
https://github.com/gustavopezzi/vimfiles

// SYSTEM:
* OS: ARCH LINUX
* DISPLAY MANAGER: Ly
* WINDOW MANAGER: i3
* LAUNCHER: rofi 

// THEME:
* MAIN THEME: Apprentice (https://romainl.github.io/Apprentice/) 
* VIM THEME: Benokai
* ROFI THEME: Custom (inversed Apprentice)
* MAIN FONT: IBM VGA 8x16

// USAGE/KEY COMMANDS:
* [win] + ENTER = terminal
* [win] + D = rofi launcher
* [win] + 0-9 = workspace 1-10
* [win] + [shift] + Q = kill (quit)
* [win] + [shift] + E = lock
* [win] + F = fullscreen
* [win] + R = resize
* [win] + H = split h
* [win] + V = split v
* [win] + LEFT = focus left
* [win] + RIGHT = focus right
* [win] + UP = focus up
* [win] + DOWN = focus down
* [win] + [shift] + LEFT = move left
* [win] + [shift] + RIGHT = move right
* [win] + [shift] + UP = move up
* [win] + [shift] + DOWN = move down

// USAGE/TERMINAL COMMANDS:
* SPECS: $ neofetch
* FILE MANAGER: $ ranger
* MUSIC PLAYER: $ cmus
* PROCESS MANAGER: $ htop
* NETWORK MONITOR: $ slurm
* AUDIO MIXER: $ alsamixer
* TEXT EDITOR: $ vim

// USAGE/APPS (THROUGH ROFI):
* WEB BROWSER: firefox
* MUSIC PLAYER: spotify-launcher
* CHAT CLIENT: discord
* FILE MANAGER: thunar
* TORRENT CLIENT: qbittorrent
* BLUETOOTH MANAGER: blueman-manager
* AUDIO MIXER: pavucontrol
* 3D EDITOR: blender
* GAME EDITOR: godot

// TODO:
* Issue: unresponsive keyboard/mouse after inactivity
* Issue: urxvt-unicode adds empty lines when moving window
* Find a console based wifi manager (similar to wicd curses)

// ARCH INSTALLATION:
$ loadkeys no
$ cat /sys/firmware/efi/fw_platform_size
$ iwctl
$ adapter phy0 set-property Powered on
$ device wlan0 set-property Powered on
$ station wlan0 scan
$ station wlan0 connect <wifi name>
$ ping archlinux.org
$ timedatectl
$ fdisk -l
$ cfdisk /dev/<disk name>
* Make EFI partion (if not dual boot)
* Make swap partion (8GB?)
* Make main partion
* Format EFI partion (if not dual boot)
$ mkswap /dev/<swap partion>
$ swapon /dev/<swap partion>
$ free -m
$ mkfs.ext4 /dev/<main partion>
$ mount /dev/<main partion> /mnt
$ mount /dev/<efi partion> --mkdir /mnt/boot/efi
$ lsblk
$ pacstrap -K base base-devel intel-ucode linux linux-firmware linux-headers grub efibootmgr os-prober vim nano networkmanager dhcpcd
$ genfstab -U /mnt >> /mnt/etc/fstab
$ arch-chroot /mnt
$ ln -sf /usr/share/zoneinfo/Europe/Oslo /etc/localtime
$ hwclock --systohc
$ vim /etc/locale.gen
* Uncomment line: en_US.UTF-8 UTF-8
* Uncomment line: nb_NO.UTF-8 UTF-8
$ locale-gen
$ vim /etc/locale.conf
* LANG=en_US.UTF-8
$ vim /etc/vconsole.conf
* KEYMAP=no
$ vim /etc/hostname
# <hostname>
$ vim /etc/hosts
* Add these lines:
* 127.0.0.1     localhost
* ::1           localhost
* 127.0.1.1     <hostname>.localdomain  localhost
$ (usually not required) mkinitcpio -P linux
$ passwd
$ useradd -m <username>
$ passwd <username>
$ visudo
* <username> ALL=(ALL:ALL) NOPASSWD: ALL
$ vim /etc/default/grub
* Uncomment line: GRUB_DISABLE_OS_PROBER=false
$ grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck
$ grub-mkconfig -o /boot/grub/grub.cfg
$ vim /etc/pacman.conf/
* Uncomment these lines:
* [multilib]
* Include = /etc/pacman.d/mirrorlist
$ pacman -Syu
$ pacman -S nvidia libglvnd nvidia-utils opencl-nvidia lib32-libglvnd lib32-nvidia-utils lib32-opencl-nvidia nvidia-settings
$ pacman -S ly xorg i3 rxvt-unicode rofi
$ pacman -S git neofetch alsa-utils feh scrot numlockx unclutter playerctl redshift imagemagick acpi unzip ranger cmus htop xf86-input-synaptics
$ systemctl enable dhcpcd.service
$ systemctl enable NetworkManager.service
$ systemctl enable ly.service
$ exit
$ umount -lR /mnt
$ reboot
* login with <username>
$ localectl --no-convert set-x11-keymap no pc104
$ (if dual boot:) sudo grub-mkconfig -o /boot/grub/grub.cfg
$ vim /etc/modprobe.d/nobeep.conf
* blacklist pcspkr
* blacklist snd_pcsp
$ nmcli device wifi list
$ nmcli device wifi connect <ssid> password <password>

// TOUCHPAD GESTURES:
$ sudo vim /etc/X11/xorg.conf.d/70-synaptics.conf
* Add these lines:
Section "InputClass"
    Identifier "touchpad"
    Driver "synaptics"
    MatchIsTouchpad "on"
        Option "TapButton1" "1"
        Option "TapButton2" "3"
        Option "TapButton3" "2"
        Option "VertEdgeScroll" "on"
        Option "VertTwoFingerScroll" "on"
        Option "HorizEdgeScroll" "on"
        Option "HorizTwoFingerScroll" "on"
        Option "EmulateTwoFingerMinZ" "40"
        Option "EmulateTwoFingerMinW" "8"
        Option "CoastingSpeed" "0"
        Option "FingerLow" "30"
        Option "FingerHigh" "50"
        Option "MaxTapTime" "125"
EndSection

// DISABLE SUSPEND
$ sudo mkdir /etc/systemd/sleep.conf.d/
$ sudo vim /etc/systemd/sleep.conf.d/disable-suspend.conf
* Add these lines:
[Sleep]
AllowSuspend=no
AllowHibernation=no
AllowSuspendThenHibernate=no
AllowHybridSleep=no

// PACKAGES:
$ pacman -S firefox discord spotify-launcher okular qbittorrent thunar mplayer
$ pacman -S blender cuda godot
$ pacman -S github-cli python python-pip sdl2 python-pygame cmake
$ pacman -S bluez bluez-utils blueman-manager pulseaudio-bluetooth pavucontrol

// DOTFILES:
$ git clone https://github.com/LASER-WOLF/dotfiles.git
* Copy contents of dotfiles to appropriate folders

// YAY:
$ git clone https://aur.archlinux.org/yay.git
$ cd yay
$ makepkg -si
$ cd
$ rm -rf ~/yay/

// YAY PACKAGES:
$ yay -S sublime-text-4 slurm

// VIM:
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
$ vim
* :PluginInstall
$ ./.vim/bundles/YouCompleteMe/install.py
\*
