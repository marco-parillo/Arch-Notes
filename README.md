# Arch-Notes

## Missed Installation Steps

1. [Make Partition Bootable](https://wiki.archlinux.org/index.php/Fdisk#Make_a_partition_bootable.)
1. Add [dhcpcd](https://wiki.archlinux.org/index.php/Dhcpcd)
2. Add [grub](https://wiki.archlinux.org/index.php/GRUB)
3. Add [efibootmgr](https://wiki.archlinux.org/index.php/GRUB#Installation_2)
4. Add [XDG user directories](https://archlinux.org/packages/?name=xdg-user-dirs)

```
pacstrap /mnt base linux linux-firmware intel-ucode vim sudo grub plasma-meta kde-applications-meta plasma-wayland-session
```

### Do I need to set the local hostname and localhost by configuring the hosts(5) file?

```
vim /etc/hosts
127.0.0.1        localhost
::1              localhost
127.0.1.1        myhostname
```

### Grub

```
grub-mkconfig -o /boot/grub/grub.cfg
```
### After reboot, enable wired networking and SDDM

```
systemctl enable NetworkManager.service
systemctl enable sddm.service
```

### Wireless (WPA/WPA2 Personal):


T410:
```
Device-2: Intel Centrino Advanced-N 6200 driver: iwlwifi 
IF: wlp3s0 state: up mac: 
```
EliteBook 8440p (Garuda and Anarchy)
```
Device-2: Intel Centrino Ultimate-N 6300 driver: iwlwifi
IF: wlo1 state: up mac: 
```
EliteBook 8460p (Manjaro)
```
Device-2: Intel Centrino Ultimate-N 6300 driver: iwlwifi 
IF: wlo1 state: up mac: 
```

### [Add User](https://wiki.archlinux.org/index.php/Users_and_groups), [uncomment wheel, then enable sudo](https://wiki.archlinux.org/index.php/Sudo).

```
useradd -m mparillo
pacman -S sudo
EDITOR=vim visudo
usermod –aG wheel mparillo
```
## If after updating your ArchLinux your Plasma/X11 looks YUGE add

```
sudo vim /etc/sddm.conf
[X11]  
ServerArguments=-nolisten tcp -dpi 96
```

## If youtube videos do not play
```
sudo pacman -S pipewire-pulse
```
and let it replace PulseAudio.
[Hat Tip](https://www.reddit.com/r/archlinux/comments/unfa71/youtube_videos_wont_load_when_using_any_browser/).

## Sync System Clock

System Settings > Regional Settings > Date and Time > Set date and time automatically

## REISUB
```
$ echo kernel.sysrq=1 | sudo tee /etc/sysctl.d/99-sysctl.conf
```

## Spell-checking
```
$ sudo pacman -S hunspell-en_us
```
Note hunspell was already installed as a dependency.

## Building AUR Packages
```
$ sudo pacman -S git
$ sudo pacman -S base-devel --needed
```
## Fonts
[atkinson-hyperlegible-font](https://archlinux.org/packages/community/any/ttf-atkinson-hyperlegible/)

## Virtualization
KVM (the built-in Linux hypervisor) with [libvirt](https://wiki.archlinux.org/title/Libvirt) via virt-manager. 

## Dual-Booting
If it takes another OS a while to [update GRUB](https://wiki.archlinux.org/index.php/GRUB#Arch_not_found_from_other_OS), install lsb-release 
```
$ sudo pacman -S lsb-release
```
Edit /etc/default/grub by manually adding the line:
```
GRUB_DISABLE_OS_PROBER=false
```

## ThinkPad Touch Pad

```
$ sudo pacman -S xorg-xinput 
```

## ThinkPad Fan
[Install](https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_T420#Fans) from the AUR [thinkfan](https://aur.archlinux.org/packages/thinkfan/).
```
$ sudo pacman -S acpi_call-dkms
$ sudo pacman -S tp_smapi
```
Maybe [tp_smapi-dkms](https://aur.archlinux.org/packages/tp_smapi-dkms/) but tp_smapi-dkms and tp_smapi are in conflict

## CPU Overheating
[Install and enable Thermald](https://wiki.archlinux.org/title/CPU_frequency_scaling#thermald)?


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

## Pacman Bad GPG Keys

If updating the archlinux-keyring does not work, delete any .sig or .sig.part files for the official repos you have in /var/lib/pacman/sync/

## Plasma 6

with sddm-qt6 and alpha packages from kde-unstable everything is good on arch

## Printing

### Try this first

```
$ sudo pacman -S python-pyqt5 cups hplip
$ systemctl enable cups.service
Created symlink /etc/systemd/system/printer.target.wants/cups.service → /usr/lib/systemd/system/cups.service.
Created symlink /etc/systemd/system/sockets.target.wants/cups.socket → /usr/lib/systemd/system/cups.socket.
Created symlink /etc/systemd/system/multi-user.target.wants/cups.path → /usr/lib/systemd/system/cups.path.
$ systemctl start cups.service
$ hp-setup -u
```

### Full List

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
