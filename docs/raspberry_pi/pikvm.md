### Environment
* Raspberry Pi 4B
* 16GB sd card (8GB is acceptable)
* [Video capture (HDMI to USB)](https://24h.pchome.com.tw/prod/DCAX3W-A900APS0S?gclid=Cj0KCQiAn4SeBhCwARIsANeF9DKO3O17qq2p3rkIOh_wnS459DC4nF87vLvm5Dbz8flAZLUZmiTiaBcaAvYOEALw_wcB)
* Signal split(Type C => Power + OTG)
* [PiKVM OS](https://pikvm.org/download/)
  * If you are using USB HDMI capture, please download **HDMI-to-USB dongle** image

### Setup flow
1. Flash PiKVM image to SD card 
2. Connect Pi 4 with a target machine
3. Boot your Pi 4
4. Open the browser and visit the URL assigned to PiKVM
	* Default user name and password to login WEB is admin/admin

#### Mount virtual drive to the target machine
1. Create a img file
2. Format the img file as vfat 
3. Mount img to /mnt
4. Copy files to /mnt
5. Umount

```
$ dd if=/dev/zero of=disk.img bs=1M count=64
$ mkfs.vfat -F 32 ./disk.img
$ mount ./disk.img /mnt
$ cp -fr <your data> /mnt
$ umount /mnt
```