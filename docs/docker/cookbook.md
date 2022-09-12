## Examples

* Launch /bin/bash with interactive mode and mount ~proj to /tmp/xxx  
> docker run --rm -it -v c:\users\bolli\proj:/tmp/xxx superq:latest /bin/bash

## Basic commands

### Create a user(instead of running as root)

* Create a new user(superq) and a home directory
> useradd -m superq

* Set a password
> passwd superq

* Install sudo command and add this account to the sudoers file.
> apt install sudo -y

### Build a image

* Read Dockerfile from current directory and build new image named `superq`
> docker build -t superq . --no-cache

### Commit changes

* Get the *CONTAINER ID* from `docker ps`
> docker ps  

* Store the changes 
  * -a: Author
  * -m: Commit message
  * REPOSITORY[:TAG]
> docker commit -a "SuperQ" -m "Install dotnet6, dos2unix" b9a425aa0080 superq:v1

### Export / Load image from container

* Export to *file* from *container*
> docker save -o superq.tar superq

* Load from *file*
> docker load -i superq.tar

