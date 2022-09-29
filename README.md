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
asusctl change keyboard backlight mode: asusctl led-mode -n

AURA: /etc/asusd/asusd-ledmodes.toml  /etc/asusd/aura.conf

Powertop:
Running sudo powertop --autotune can improve battery life. If you run sudo powertop and go to tunables tab, you would see some options marked as bad. powertop autotune fixes them, but needs to be run every time you reboot. To fix this, create a script in ~/.config/autostart or any other location of your choice, make it executable by running chmod +x /path/to/script and add it to login scripts in kde settings.

https://raphtlw.medium.com/guide-to-installing-arch-on-zephyrus-g14-21b93d2e9f49
