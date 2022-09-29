install: pacstrap /mnt base linux linux-firmware nano intel-ucode reflector
graphic: https://wiki.archlinux.org/title/NVIDIA
kernel param: edit /etc/default/grub add `ibt=off` to GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet"
telegram scaling kde: Exec=env QT_SCREEN_SCALE_FACTORS=1 telegram-desktop -- %u
telegram tray icon: https://wiki.archlinux.org/title/KDE#Blurry_icons_in_system_tray
