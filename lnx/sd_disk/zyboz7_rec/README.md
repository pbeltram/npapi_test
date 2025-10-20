**NOTE: Old content of SD disk will be overwritten so save its content before proceeding.**

## 1. Create SD disk from pre-created dd image file (preferred)

Change directory to `./sd_disk/zyboz7_rec`.

Compressed file `./sdcard.7z` in `./sd_disk/zyboz7_rec/` directory contains complete dd image of SD disk for ZyboZ7.

Un-compress `./sdcard.7z`. Uncompressed dd image file is 1.6Gbyte so minimum 4GByte SD card is needed.

```
7z x ./sdcard.7z
```

Unmount any partitions mounted on SD disk that will be overwritten.

**NOTE: Doble check which device is your SD disk to not damage your other disks.**

```
sudo umount /dev/sdX1
sudo umount /dev/sdX2
```

Write extracted image `./sdcard.img` to SD disk device.

```
sudo dd if=./sdcard.img of=/dev/sdX bs=4M status=progress
sync
```

Extend partition 2 on SD disk device to its maximum size.

```
sudo parted /dev/sdX resizepart 2 100%
sudo resize2fs /dev/sdX2
sudo fdisk -l /dev/sdX
sync
```

## 2. Manually create SD partitions and copy files to SD disk

You must create SD disk with 2 partitions (e.g. with `fdisk` tool).

**NOTE: Doble check which device is your SD disk to not damage your other disks.**

After create of partitions the SD disk layout should look like this. Example is for 32GByte SD card detected as `/dev/sdf` device with 2 partitions.

```
$ sudo fdisk -l /dev/sdf
Disk /dev/sdf: 29,74 GiB, 31914983424 bytes, 62333952 sectors
Disk model: USB3.0 CRW-SD   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000

Device     Boot   Start      End  Sectors  Size Id Type
/dev/sdf1  *       2048  1050623  1048576  512M  c W95 FAT32 (LBA)
/dev/sdf2       1050624 62333951 61283328 29,2G 83 Linux
```

Partition `/dev/sdf1` mounted as `/media/$USER/boot` type vfat
Format as FAT32.
Contains boot.scr, BOOT.bin and image.ub.
Min size: 512MByte.

Partition `/dev/sdf2` mounted as `/media/$USER/rootfs` type ext4
Format as EXT4.
Contains Linux rootfs.
Min size: 3GByte.

Change directory to `./sd_disk/zyboz7_rec`.

Format `boot` and copy binaries to `boot` partition of SD disk.

```
sudo mount
sudo umount /dev/sdX1
sudo mkfs.vfat /dev/sdX1
sudo fatlabel /dev/sdX1 boot
udisksctl mount -b /dev/sdX1
cp ./boot.scr ./bin/BOOT.BIN ./bin/image.ub /media/$USER/boot/
sync
```

Format `rootfs` and copy binaries to `rootfs` partition of SD disk.

```
sudo mount
sudo umount /dev/sdX2
sudo mkfs.ext4 /dev/sdX2
sudo e2label /dev/sdX2 rootfs
udisksctl mount -b /dev/sdX2
sudo su -c 'tar xvf ./rootfs.tar.gz -C /media/$SUDO_USER/rootfs/'
sync
```

**Create Boot files (BOOT.BIN and image.ub)**

```
make build_boot build_lx
```

```
md5sum ./bin/BOOT.BIN
cp ./bin/BOOT.BIN /media/$USER/boot/

md5sum ./bin/image.ub
cp ./bin/image.ub /media/$USER/boot/

md5sum ./boot.scr
cp ./boot.scr /media/$USER/boot/

md5sum /media/$USER/boot/BOOT.BIN
md5sum /media/$USER/boot/image.ub
md5sum /media/$USER/boot/boot.scr
```
