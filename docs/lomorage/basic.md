Lomorage is a free and open source solution to build up a personal private cloud to backup your photos, it supports iOS/Android/Web client,  
and the server can be hosted on Raspberry Pi/Linux/Windows/Mac,etc.

Here is the guide to setup Lomorage on Raspberry Pi:  

**Server Setting**  

1. Download the Lomorage RP prebuilt image from its official site: [Lomorage RP Image](https://github.com/lomorage/pi-gen/releases/download/2021_11_05.23_32_53.0.4a997ab/image_2021-11-05-lomorage-lite.zip)
2. Flash to micro SD card, Lomorage recommend to use [balenaEtcher](https://www.balena.io/etcher/) to write image into SD card.
3. Insert the micro SD card to Raspberry Pi and reboot the device. You need to plugin a **HDMI** cable to RP to configure the service.
4. Once the boot is completed, you'll see a QR code on the screen, but we now need to press `R` and then `ctrl + alt + F2` to switch to another terminal.  
  4-1. The default user/password is lomoware/lomorage.  
5. Set the wifi SSID with command `wifi_switch.sh client "Lomorage's 2.4G" mypassword`
6. Insert a USB disk as a storage, you need to let lomod figure out where is the mount point, if the USB disk is mounted to */media/pi/sd*  
  **Make sure the USB disk is format to the supported file system: vfat exfat ext2 ext3 ext4 hfsplus ntfs fuseblk**  
  6-1. Run command: `echo "LOMOD_MOUNT_DIR=/media/pi/sd" | tee -a /opt/lomorage/etc/environment`  
  6-2. Modify the **ExecStart** in file */lib/systemd/system/lomod.service* to specify the *mount-dir*  
  > ExecStart=/opt/lomorage/bin/lomod -b /opt/lomorage/var --mount-dir /media/pi  --max-upload 1 --max-fetch-preview 3  
7. Restart service by
```
# restart lomo-backend
sudo systemctl restart lomod

# restart lomo-web
sudo systemctl restart lomow

# restart lomo-frame
sudo service supervisor restart
```


**Client Setting**

1. Download the client from App Store or Google Play on your phone.
2. Make sure your phone and Raspberry Pi are in the same network, and open the Lomorage App and scan the QR code created by Lomorage Server, you can also input IP/Port manually.
3. If the App has connected to server, you'll need to create a user in the next step.
4. Finally, you can select photos to update one by one or drag down to upload all photos.
