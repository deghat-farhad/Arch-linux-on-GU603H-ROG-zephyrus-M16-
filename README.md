install: pacstrap /mnt base linux linux-firmware nano intel-ucode reflector
graphic: https://wiki.archlinux.org/title/NVIDIA
kernel param: edit /etc/default/grub add `ibt=off` to GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet"
telegram scaling kde: Exec=env QT_SCREEN_SCALE_FACTORS=1 telegram-desktop -- %u
telegram tray icon: https://wiki.archlinux.org/title/KDE#Blurry_icons_in_system_tray
mic key:     Create a file at /etc/udev/hwdb.d/90-nkey.hwdb with:

`evdev:input:b0003v0B05p19B6*
  KEYBOARD_KEY_ff31007c=f20 # x11 mic-mute`

Then, update the hwdb with:

`sudo systemd-hwdb update
sudo udevadm trigger`

asusctl change profile: asusctl profile -n

AURA: /etc/asusd/asusd-ledmodes.toml  /etc/asusd/aura.conf
