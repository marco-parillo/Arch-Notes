# Arch-Notes

## Missed Installation Steps

1. [Make Partition Bootable](https://wiki.archlinux.org/index.php/Fdisk#Make_a_partition_bootable.)
1. Add [dhcpcd](https://wiki.archlinux.org/index.php/Dhcpcd)
1. Maybe add [wpa_supplicant](https://archlinux.org/packages/?name=wpa_supplicant)

```
pacstrap /mnt base linux linux-firmware vim dhcpcd wpa_supplicant
```
1. Enable
```
systemctl enable --now systemd-networkd.service
```
1. Grub

```
grub-mkconfig -o /boot/grub/grub.cfg
```
1. After reboot, enable wired networking

```
ip link
dhcpcd ens33
vim /etc/systemd/network/20-wired.network
i
[Match]
Name=ens33

[Network]
DHCP=yes
Escape : wq
```
1. Wireless (WPA/WPA2 Personal):


1. Add User, uncomment wheel, then enable sudo.
```

pacman -S sudo
EDITOR=vim visudo
usermod –aG wheel mparillo
```

## REISUB
```
$ echo kernel.sysrq=1 | sudo tee /etc/sysctl.d/99-sysctl.conf
```

## Spell-checking
```
$ sudo pacman -S hunspell-en_US hunspell
```
## Building Packages
```
$ sudo pacman -S git
$ sudo pacman -S base-devel --needed
```

## Dual-Booting
If it takes another OS a while to [update GRUB](https://wiki.archlinux.org/index.php/GRUB#Arch_not_found_from_other_OS), install lsb-release 
```
$ sudo pacman -S lsb-release
```
Edit /etc/default/grub by manually adding the line:
```
GRUB_DISABLE_OS_PROBER=false
```

## ThinkPad Fan
[Install](https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_T420#Fans) from the AUR [thinkfan](https://aur.archlinux.org/packages/thinkfan/).
```
$ sudo pacman -S acpi_call-dkms
$ sudo pacman -S tp_smapi
```
Maybe [tp_smapi-dkms](https://aur.archlinux.org/packages/tp_smapi-dkms/) but tp_smapi-dkms and tp_smapi are in conflict


## Turn off Hibernation
Use [Polkit](https://wiki.archlinux.org/index.php/Polkit#Disable_suspend_and_hibernate)
```
sudo vim /etc/polkit-1/rules.d/10-disable-suspend.rules
i
polkit.addRule(function(action, subject) {
    if (action.id == "org.freedesktop.login1.suspend" ||
        action.id == "org.freedesktop.login1.suspend-multiple-sessions" ||
        action.id == "org.freedesktop.login1.hibernate" ||
        action.id == "org.freedesktop.login1.hibernate-multiple-sessions")
    {
        return polkit.Result.NO;
    }
});
escape :wq
```

## Printing

```
Optional dependencies for imagemagick
    ghostscript: PS/PDF support [installed]
    libheif: HEIF support
    libraw: DNG support [installed]
    librsvg: SVG support [installed]
    libwebp: WEBP support [installed]
    libwmf: WMF support
    libxml2: Magick Scripting Language [installed]
    ocl-icd: OpenCL support
    openexr: OpenEXR support [installed]
    openjpeg2: JPEG2000 support [installed]
    djvulibre: DJVU support [installed]
    pango: Text rendering [installed]
    imagemagick-doc: manual and API docs


Optional dependencies for cups-filters
    ghostscript: for non-PostScript printers to print with CUPS to convert PostScript to raster images [installed]
    foomatic-db: drivers use Ghostscript to convert PostScript to a printable form directly
    foomatic-db-engine: drivers use Ghostscript to convert PostScript to a printable form directly [pending]
    foomatic-db-nonfree: drivers use Ghostscript to convert PostScript to a printable form directly
    antiword: to convert MS Word documents
    docx2txt: to convert Microsoft OOXML text from DOCX files


Optional dependencies for hplip
    cups: for printing support
    sane: for scanner support [installed]
    xsane: sane scanner frontend
    python-pillow: for commandline scanning support
    python-reportlab: for pdf output in hp-scan
    rpcbind: for network support
    python-pyqt5: for running GUI and hp-toolbox
    libusb: for advanced usb support [installed]
    wget: for network support [installed]
    
    
$ sudo pacman -S python-pyqt5
$ sudo pacman -S cups
$ sudo pacman -S print-manager
$ sudo pacman -S system-config-printer
# was $ systemctl enable org.cups.cupsd.service
# was $ systemctl start org.cups.cupsd.service
$ systemctl enable cups.service
Created symlink /etc/systemd/system/printer.target.wants/cups.service → /usr/lib/systemd/system/cups.service.
Created symlink /etc/systemd/system/sockets.target.wants/cups.socket → /usr/lib/systemd/system/cups.socket.
Created symlink /etc/systemd/system/multi-user.target.wants/cups.path → /usr/lib/systemd/system/cups.path.
$ systemctl start cups.service
$ hp-setup -u

```
