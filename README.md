# Arch-Notes

## REISUB
```
$ echo kernel.sysrq=1 | sudo tee /etc/sysctl.d/99-sysctl.conf
```

## Dual-Booting
If it takes another OS a while to [update GRUB](https://wiki.archlinux.org/index.php/GRUB#Arch_not_found_from_other_OS), install lsb-release 
```
$sudo pacman -S lsb-release
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
$ systemctl enable org.cups.cupsd.service
$ systemctl start org.cups.cupsd.service
$ hp-setup -u

```
