# Install and configure arch linux on asus GU603 ROG zephyrus M16 (WIP)
<p>
  <img height="50" src="https://rog.asus.com/dist/img/rog-logo@3x.png">
  <img height="90" src="https://archlinux.org/static/logos/archlinux-logo-dark-scalable.518881f04ca9.svg">
</p>

##
## Installation
#### pacstrap necessary packages
- base: Minimal package set to define a basic Arch Linux installation
- linux: The Linux kernel and modules
- linux-firmware: Firmware files for Linux for common hardware
- nano: any text editor
- intel-ucode:  	Microcode update files for Intel CPUs
- reflector: to retrieve and filter the latest Pacman mirror list  

```
# pacstrap /mnt base linux linux-firmware nano intel-ucode reflector
```  
#### nvidia
all you need is to install [NVIDIA](https://archlinux.org/packages/extra/x86_64/nvidia) package and it will care about switching between intel and nvidia graphic cards.  

nvidia may not boot as it mentioned here: https://wiki.archlinux.org/title/NVIDIA
>nvidia may not boot on Linux 5.18 on systems with Intel CPUs due to FS#74886/FS#74891. Until this is fixed, a workaround is disabling the Indirect Branch Tracking CPU security feature by setting the ibt=off kernel parameter from the bootloader. This security feature is responsible for mitigating a class of exploit techniques, but is deemed safe as a temporary stopgap solution. You can alternatively try nvidia-open and continue using IBT.

edit `/etc/default/grub` and add `ibt=off` to below line 
```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet"
```
then re-configure grub:
```
# grub-mkconfig -o /boot/grub/grub.cfg
```
## Configurations
#### enable microphone mute key of keyboard
Create `/etc/udev/hwdb.d/90-nkey.hwdb`:
```
evdev:input:b0003v0B05p19B6*
KEYBOARD_KEY_ff31007c=f20 # x11 mic-mute
```
Then, update the hwdb:
```
$ sudo systemd-hwdb update
$ sudo udevadm trigger
```
#### asusctl
I installed [asusctl](https://aur.archlinux.org/packages/asusctl) mostly for enabling AURA keyboard backlight and the ability to change performance profiles:

###### performance profiles:
map
```
asusctl change profile: asusctl profile -n
```
into `FN+F5` to make this key functional.


###### AURA:
edit `asusd-ledmodes.toml`:
```
$ sudo nano /etc/asusd/asusd-ledmodes.toml
```
and add ROG Zephyrus M16 `led_data` from [asusd-ledmodes.toml](./asusd-ledmodes.toml)  into it.
remove `aura.conf`:
```
$ sudo rm /etc/asusd/aura.conf
```
restart `asusd.service`
```
$ sudo systemctl restart asusd.service
```
now you can modify keyboard backlight in `rog-control-center`

you can also map
```
$ asusctl led-mode -n
```
into  `FN+F4` to make this key functional.

#### tray icon quality
As mentioned in https://wiki.archlinux.org/title/KDE#Blurry_icons_in_system_tray, to fix blurry tray icons install [libappindicator-gtk3](https://archlinux.org/packages/?name=libappindicator-gtk3)

#### blurry telegram desktop in kde while using scaling
edit `telegramdesktop.desktop`
```
$ sudo nano /usr/share/applications/telegramdesktop.desktop
```
and change `Exec` line into
```
Exec=env QT_SCREEN_SCALE_FACTORS=1 telegram-desktop -- %u
```
to bypass kde scaling feature just for telegram desktop.

#### better font for persian language
Install your desired persian font (IRANSansWeb in this example)  
  
edit `fonts.conf`
```
$ sudo nano /etc/fonts/fonts.conf
```
add
```xml
<!-- Default font for the Persian language (no fc-match pattern) -->
	<match>
		<test compare="contains" name="lang">
			<string>fa</string>
		</test>
		<edit mode="prepend" name="family">
			<string>IRANSansWeb</string>
		</edit>
	</match>
```
and
```xml
<!-- Fallback fonts preference order -->
	<alias>
		<family>sans-serif</family>
		<prefer>
			<family>Noto Sans</family>
			<family>Open Sans</family>
			<family>Droid Sans</family>
			<family>Roboto</family>
			<family>IRANSansWeb</family>
		</prefer>
	</alias>
	<alias>
		<family>serif</family>
		<prefer>
			<family>Noto Serif</family>
			<family>Droid Serif</family>
			<family>Roboto Slab</family>
			<family>IRANSansWeb</family>
		</prefer>
	</alias>
	<alias>
		<family>monospace</family>
		<prefer>
			<family>Noto Sans Mono</family>
			<family>Inconsolata</family>
			<family>Droid Sans Mono</family>
			<family>Roboto Mono</family>
			<family>IRANSansWeb</family>
		</prefer>
	</alias>
```
concept from arabic font configuration https://wiki.archlinux.org/title/Font_configuration/Examples#Arabic

#### Limit battery charge without `asusctl`
to limit battery charge without installing `asusctl` edit `/etc/udev/rules.d/asus-battery-charge-threshold.rules` and add this line into it:
```
ACTION=="add", KERNEL=="asus-nb-wmi", RUN+="/bin/bash -c 'echo 60 > /sys/class/power_supply/BAT?/charge_control_end_threshold'"
```
  
# to be organized
Powertop:
Running sudo powertop --autotune can improve battery life. If you run sudo powertop and go to tunables tab, you would see some options marked as bad. powertop autotune fixes them, but needs to be run every time you reboot. To fix this, create a script in ~/.config/autostart or any other location of your choice, make it executable by running chmod +x /path/to/script and add it to login scripts in kde settings.

https://raphtlw.medium.com/guide-to-installing-arch-on-zephyrus-g14-21b93d2e9f49

