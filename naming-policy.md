# External drive naming policy

How this Ventoy drive is labeled so it is easy to spot in Ubuntu, Disks, and a new PC firmware boot menu.

## Standard label

**`INSTALL`**

```bash
sudo ./Ventoy2Disk.sh -i -g -L INSTALL /dev/sdX
```

Ubuntu should mount the data partition at `/media/$USER/INSTALL`.

## Rules

1. One purpose, one name. This stick is for OS install ISOs only.
2. Keep the label to **15 characters or fewer**.
3. Letters and numbers only. No spaces or punctuation.
4. Prefer ALL CAPS.
5. Do not put a person's name, a shop name, or a long sentence in the volume label.
6. Firmware may still show the USB brand. That is normal.
7. A second stick needs a different label. Do not have two drives named `INSTALL`.

## Allowed labels

| Label | Use |
|---|---|
| `INSTALL` | Default. Multi-ISO installer for new hardware |
| `VENTOY` | Acceptable default from Ventoy |
| `OSINSTALL` | Same job, slightly more specific |
| `MULTIBOOT` | Same job, emphasizes several ISOs |
| `RESCUE` | Spare stick with GParted / Clonezilla only |
| `WINPE` | Spare stick that is Windows-repair only |

## Names to avoid

| Bad example | Why |
|---|---|
| `JOHNDOE Multi Boot Installer` | Spaces, too long, truncated in BIOS |
| `Untitled` / `USB Drive` | Easy to wipe the wrong disk |
| `New Volume` | Same problem |
| `COMPANY` | Business name; this drive is not store media |
| `BACKUP` | Wrong job; mixes ISOs with backups |

## Check or rename later

```bash
lsblk -o NAME,SIZE,LABEL,FSTYPE,MOUNTPOINT
sudo apt install exfatprogs
sudo umount /dev/sdX1
sudo exfatlabel /dev/sdX1 INSTALL
```

Confirm the **data** partition first. Do not rename the small EFI/boot partition.

Write `INSTALL` on the enclosure so you can find it in a drawer.
