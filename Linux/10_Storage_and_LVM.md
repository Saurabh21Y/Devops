# Linux Notes — Storage, Mounting, EBS, and LVM

> Commands for discovering disks, creating LVM layers, formatting filesystems, mounting storage, and extending volumes.

## 1. Storage model

```text
Disk/partition → Physical Volume (PV) → Volume Group (VG)
                                      → Logical Volume (LV)
                                      → Filesystem → Mount point
```

In cloud environments such as AWS, an EBS volume is attached as a block device. The device must be partitioned or used directly, formatted, and mounted before applications can use it.

> **Warning:** `mkfs`, `pvcreate`, and similar commands can destroy existing data. Confirm the device with `lsblk`, backups, and the change plan before proceeding.

## 2. `lsblk` — discover block devices

```bash
lsblk
lsblk -f
lsblk -o NAME,SIZE,FSTYPE,UUID,MOUNTPOINTS
lsblk -p
lsblk -S
```

`-f` shows filesystem information, `-o` selects columns, `-p` shows full device paths, `-a` includes empty devices, and `-S` lists SCSI devices.

## 3. `pvcreate` — create a physical volume

```bash
sudo pvcreate /dev/xvdf
sudo pvcreate /dev/xvdf /dev/xvdg
sudo pvs
sudo pvdisplay
```

Useful options include `-f` force, `-y` answer yes, `-u` set UUID, `-Z` zero initial sectors, and `--setphysicalvolumesize` set the physical volume size. Use `pvdisplay -m` to inspect physical-to-logical extent mappings.

## 4. `vgcreate` — create a volume group

```bash
sudo vgcreate my_vg /dev/xvdf /dev/xvdg
sudo vgs
sudo vgdisplay
```

A VG pools free space from one or more PVs. `-s` sets physical extent size, `-f` forces, and `-p`/`-l` can limit PV/LV counts. Use `vgdisplay -v` for detailed information.

## 5. `lvcreate` — create a logical volume

```bash
sudo lvcreate -L 10G -n my_lv my_vg
sudo lvcreate -l 80%FREE -n data_lv my_vg
sudo lvs
sudo lvdisplay
```

`-L` specifies a size, `-n` a name, `-l` extents or percentages, `-s` creates a snapshot, and `-v` is verbose. LVs provide flexible virtual block devices inside a VG.

## 6. Inspect LVM layers

```bash
sudo pvdisplay
sudo pvdisplay -m
sudo vgdisplay
sudo vgdisplay -v
sudo lvdisplay
sudo lvdisplay -m
```

`pvdisplay` reports physical volumes and `-m` shows extent mappings. `vgdisplay` reports volume-group size/free space and `-v` adds detail. `lvdisplay` reports logical volumes and `-m` shows their mapping. The shorter `pvs`, `vgs`, and `lvs` commands are useful summary views.

## 7. Create a filesystem

```bash
sudo mkfs.ext4 /dev/my_vg/my_lv
sudo mkfs.ext4 -L app-data /dev/my_vg/my_lv
sudo mkfs.xfs -L app-data /dev/my_vg/my_lv
```

`mkfs.ext4` options include `-F` force, `-L` label, `-m` reserved-block percentage, and `-n` dry-run-like inspection. `mkfs.xfs` supports `-f` force and `-L` label.

Formatting destroys the existing filesystem contents. Never run `mkfs` on a device until the target has been independently verified.

## 8. `mkdir`, `mount`, and `umount`

```bash
sudo mkdir -p /mnt/myvolume
sudo mount /dev/my_vg/my_lv /mnt/myvolume
mount | grep myvolume
df -hT /mnt/myvolume
sudo umount /mnt/myvolume
```

`mount -t` sets filesystem type, `-o` sets mount options, `-a` mounts entries from `/etc/fstab`, `-r` mounts read-only, and `-v` is verbose. `umount -a`, `-f`, `-l`, `-v`, and `-R` unmount all, force, lazy-unmount, verbose, and recursively respectively.

Do not unmount a filesystem that is in use. Find users with `fuser -m /mnt/myvolume` or `lsof` if installed.

## 9. Persistent mounts with `/etc/fstab`

Use a filesystem UUID rather than relying on a device name that may change:

```bash
blkid /dev/my_vg/my_lv
sudo vim /etc/fstab
sudo mount -a
```

Test `mount -a` before rebooting. A malformed `/etc/fstab` can prevent normal boot, so keep a recovery plan.

## 10. `lvextend` — grow an LV

```bash
sudo lvextend -L +5G /dev/my_vg/my_lv
sudo lvextend -r -L +5G /dev/my_vg/my_lv
sudo lvs
sudo df -h /mnt/myvolume
```

`-L` specifies a new/additional size, `-l` uses extents or percentages, `-r` resizes the filesystem together with the LV, `-f` forces, and `-v` is verbose. Confirm free VG space first. Filesystem growth rules depend on filesystem type; shrinking is a different and riskier operation.

## Complete example

```bash
lsblk -f
sudo pvcreate /dev/xvdf
sudo vgcreate data_vg /dev/xvdf
sudo lvcreate -L 10G -n data_lv data_vg
sudo mkfs.ext4 /dev/data_vg/data_lv
sudo mkdir -p /mnt/data
sudo mount /dev/data_vg/data_lv /mnt/data
df -hT /mnt/data
```

## Quick revision

- `lsblk` → discover disks and filesystems
- `pvcreate` → disk/partition to PV
- `vgcreate` → PVs to VG
- `lvcreate` → VG space to LV
- `mkfs.*` → create filesystem
- `mount` → attach filesystem to a directory
- `umount` → detach filesystem
- `lvextend` → grow an LV
