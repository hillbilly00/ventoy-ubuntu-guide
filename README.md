# Ventoy on Ubuntu — Multi-OS Bootable External Drive

How to install **Ventoy** from Ubuntu and use it to install operating systems on new hardware.

Ventoy is installed **once** on the external drive. After that you copy ISO files onto the drive. At boot you pick which ISO to run.

Official project: https://www.ventoy.net  
Official packages: https://github.com/ventoy/Ventoy/releases

More notes in this repo:

- [External drive naming policy](naming-policy.md) — volume label `INSTALL`
- [Recommended ISOs](recommended-isos.md)

---

## Warning

Installing Ventoy **erases the target drive**. Confirm the device name with `lsblk` before you run the installer. Do not pick your Ubuntu system disk.

---

## What you need

- Ubuntu desktop with `sudo`
- External USB flash drive, USB SSD, or USB HDD (32 GB or larger recommended)
- ISO files to install
- The new PC set to boot from USB (UEFI)

Suggested ISOs for new hardware:

| ISO | Why |
|---|---|
| Ubuntu 26.04 LTS Desktop | Main Linux installer |
| Ubuntu 26.04 LTS Server | Headless / server boxes |
| Windows 11 | If a machine will run Windows |
| Xubuntu 26.04 (optional) | Low-power mini-PCs |

Skip older LTS releases unless a board refuses 26.04.

---

## 1. Identify the external drive

```bash
lsblk -o NAME,SIZE,TYPE,TRAN,MODEL,MOUNTPOINT
```

Find the whole disk (not a partition). If unsure, unplug the drive, run `lsblk` again, plug it back in, and see what appeared.

---

## 2. Download Ventoy for Linux

```bash
cd ~
VER=$(curl -sL https://api.github.com/repos/ventoy/Ventoy/releases/latest | grep tag_name | head -1 | sed 's/.*"v\([^"]*\)".*/\1/')
echo "Latest Ventoy: $VER"

wget "https://github.com/ventoy/Ventoy/releases/download/v${VER}/ventoy-${VER}-linux.tar.gz"
tar -xzf ventoy-${VER}-linux.tar.gz
cd ventoy-${VER}
```

Use the official release tarball, not a git source tree.

---

## 3. Install Ventoy on the drive

Label the data partition `INSTALL` per the [naming policy](naming-policy.md).

```bash
sudo ./Ventoy2Disk.sh -i -g -L INSTALL /dev/sdX
```

Type `y` twice when asked.

---

## 4. Copy ISO files

```bash
cp ~/Downloads/ubuntu-26.04*-desktop-amd64.iso /media/$USER/INSTALL/
cp ~/Downloads/ubuntu-26.04*-live-server-amd64.iso /media/$USER/INSTALL/
cp ~/Downloads/Win11*.iso /media/$USER/INSTALL/
sync
```

If the partition does not mount:

```bash
sudo apt update
sudo apt install exfatprogs
```

---

## 5. Boot a new PC and install

1. Plug the drive into the new machine.
2. Open the firmware boot menu (`F12`, `F10`, `Esc`, `Del`, or `F2`).
3. Select the external drive.
4. Pick the ISO in the Ventoy menu.
5. Install to the **internal** disk, not this drive.
6. Remove the drive and boot the new OS. Apply updates.

---

## 6. Update Ventoy later

```bash
sudo ./Ventoy2Disk.sh -u /dev/sdX
```

`-u` keeps the ISOs.

---

## Command cheat sheet

```bash
lsblk -o NAME,SIZE,TYPE,TRAN,MODEL,LABEL,MOUNTPOINT
sudo ./Ventoy2Disk.sh -i -g -L INSTALL /dev/sdX
sudo ./Ventoy2Disk.sh -u /dev/sdX
```

Ventoy is third-party software. This repo is personal notes, not an official Ventoy document.
