If we would like to start a service from a container and expose to external,  
the network configuration is essential knowledge.

* Port forward
> docker run -p <host>:<container> <container>

For example, add sub commands `-p 8080:80` to the docker, it would map 80 port from container to 8080 in the host.
