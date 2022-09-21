## Examples

* Launch /bin/bash with interactive mode and mount ~proj to /tmp/xxx  
> docker run --rm -it -v c:\users\bolli\proj:/tmp/xxx superq:latest /bin/bash

## Basic commands

* Get the *CONTAINER ID* from `docker ps`
> docker ps  

* Launch a bash in the specified container
> docker exec -it *container id* /bin/bash

### Build a image

* Read Dockerfile from current directory and build new image named `superq`
> docker build -t superq . --no-cache

### Commit changes

* Store the changes 
  * -a: Author
  * -m: Commit message
  * REPOSITORY[:TAG]
> docker commit -a "SuperQ" -m "Install dotnet6, dos2unix" b9a425aa0080 superq:v1

* Simply overwrite current tag
> docker commit b9a425aa0080 superq:v1

### Export / Load image from container

* Export to *file* from *container*
> docker save -o superq.tar superq

* Load from *file*
> docker load -i superq.tar
