The default account will be root, however, this is not convinent when we use a container as a building environment,  
all the user and group id will be set to *root:root* in this case.  

Here are some useful commands to create a new user, it's also apply to traditional installation Ubuntu.  

* Create a new user(superq) and a home directory
> useradd -m superq

* Set a password
> passwd superq

* Or set a empty password instead
> usermod -p U6aMy0wojraho superq

* Install sudo command and add this account to the sudoers file.
> apt install sudo -y

* Rather than modify */etc/sudoers* directly, grant a user with sudo privilege with command below: 
> usermod -G sudo -a superq
