Here is the tutorial to setup Nextcloud on Raspberry Pi.

### Environment
* Raspberry Pi 3b
* 16GB sd card
* Raspbian 11

I recommend to flash Raspbian via *Raspberry Pi Imager* instead of using ubuntu Nextcloud image, because my raspberry pi always run into the maintanence mode in the next boot.

### Setting flow
1. Flash Raspbin 11 to sd card and boot machine  
2. Install docker  
  2-1. Install by [convinent script](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script)  
  2-2. Allow normal user to execute docker  
3. Setup nextcloud  
  3-1. Pull image  
  3-2. Assign volume to container to [persist data](https://hub.docker.com/_/nextcloud)  


#### Allow normal user to execute docker

After installed docker, run the command below to grant a non-prileged user to use docker:  
> sudo apt install -y uidmap
> dockerd-rootless-setuptool.sh install


Start Nextcloud service...  
> docker run --name nc -d -p 8080:80 arm64v8/nextcloud

#### Assign volume to container to persist data

1. Create some folders
  > mkdir -p nc/data && mkdir -p nc/config
2. Bind folders via `-v` command
  > docker run --name nc -d -p 8080:80 -v $(pwd)/nc/data:/var/www/html/data -v $(pwd)/nc/config:/var/www/html/config arm64v8/nextcloud  

Nextcloud will put runtime data including photos, sqlite database and necessary information here, and config folder is used to hold current setting status.

**Troubleshooting - uid/gid doesn't exist**  
However, the uid and gid *data* and *config* folders doesn't exist on your host:  
```
$ ll
drwxr-xr-x 2 100032 100032 4096 Sep 27 10:34 config
drwxr-xr-x 2 100032 100032 4096 Sep 27 12:08 data
```

I tried to add the sub command `-u $(id -u ${USER}):$(id -g ${USER})`, but it doesn't help, and the container will stop automatically with no reason, you might think that we could copy these two folders with `sudo`, but the privilege setting might be lost.  
So I create a new user and group with specified id(here the id is 100032):  
```
$ sudo groupadd -g 100032 nextcloud
$ sudo useradd -u 100032 nextcloud
$ sudo passwd nextcloud
$ ll
drwxr-xr-x 2 nextcloud nextcloud 4096 Sep 27 12:35 config
drwxrwx--- 4 nextcloud nextcloud 4096 Sep 27 12:45 data
```

Then you can switch to user *nextcloud* by `su nextcloud` and check the content inside those folders.  
