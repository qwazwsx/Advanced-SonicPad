
By default, KlipperScreen does not have special support for USB disks in the way that the Creality modified version does. To remedy this, we can create some fstab rules and force our USB device to be mounted inside the gcodes folder.

1) Rename your flash drive to "USB" (case sensitive)

2) Add the following lines to the end of /etc/fstab ($ `sudo nano /etc/fstab`)

```
/dev/disk/by-label/USB  /home/sonic/printer_data/gcodes/USB    auto    defaults,uid=sonic      0       0
```
3) Create the mountpoint

$ `mkdir ~/printer_data/gcodes/USB`

4) Mount

`sudo mount -a`

Refresh the file listing in KlipperScreen and you should see a new directory called USB containing the contents of your USB device. Note that KlipperScreen will not show directories if there are no .gcode files present.


-----

#### Further Information:


First we can list all block devices. You can see my USB flash drive listed at /dev/sdb1

$ `lsblk -f`

```

NAME         FSTYPE FSVER LABEL          UUID                                 FSAVAIL FSUSE% MOUNTPOINT

sdb

└─sdb1       vfat   FAT32 My USB Disk    B424-8768                               7.4G     0% /home/sonic/USBDrive

mmcblk0

├─mmcblk0p1  vfat   FAT16 Volumn

├─mmcblk0p2

├─mmcblk0p3

├─mmcblk0p4

├─mmcblk0p5  ext4   1.0                  57f8f4bc-abf4-655f-bf67-946fc0f9f25b    1.5G    79% /

└─mmcblk0p6

mmcblk0boot0

mmcblk0boot1

mmcblk0rpmb

```

Your drive won't always be listed as this lettered block device, so we'll need to either address it using the partitions's UUID, or using the port it's been connected to. You can see all the different ways of addressing your systems disks using `ls /dev/disk/*`

In my case, my USB device can be addressed as:

`/dev/sdb1`

`/dev/disk/by-uuid/B424-8768`

`/dev/disk/by-path/platform-5200000.ehci1-controller-usb-0:1.3:1.0-scsi-0:0:0:0-part1`

`/dev/disk/by-label/My USB Disk`


The easiest one for the user to control is the partition volume name, so we're just going to hardcode the USB disk's name. This means you must rename your USB disk to "USB" (case sensitive) in order for this to work. However you can choose whatever method you want.


Next we need to tell Linux where to mount this device when it's connected. Add the following line to the end of your `/etc/fstab` file


$ `sudo nano /etc/fstab`

```
/dev/disk/by-label/USB  /home/sonic/printer_data/gcodes/USB    auto    defaults,uid=sonic      0       0
```


This mounts the partition located at `/dev/disk/by-label/USB` to the `/home/sonic/printer_data/gcodes/USB` directory, using `auto` filesystem type detection, making user `sonic` the owner.


Next we need to actually create this folder  

`mkdir ~/printer_data/gcodes/USB`

Then mount the device

`sudo mount -a`


Refresh the file listing in KlipperScreen and (ONLY if your flash drive contains .gcode files) you should see a new directory called USB containing the contents of your USB device.
