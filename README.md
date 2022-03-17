# NOTES FOR EVAL

## 00) how to docker
Docker version: docker --version only show client version.

# A) Basic containers:
1) docker pull
3) if port blocked, lsof -i -P -n | grep 5000; kill -9 [PID]; 
 docker inspect -f {{.HostConfig.RestartPolicy}} overlord
6) to launch a shell of a debian container: docker run -it --rm debian /bin/bash
install vim, gcc, make.
*ToDo: Find my clean version of fillit. and test it there. Think of easiest I am most proud of.*

# B) Wordpress:
9-10) SQL: SHOW DATABASES; or SHOW SCHEMA;

11) Set up wordpress with 2 containers: WP and SQL for DB.
both containers can be run in the same Docker Host, so no need to install Docker in a virtual machine for now. http://localhost:8080.
Other option than --link: 
a) create a network: docker network create wp_mysql_net
b) connect both containers to the network: 
docker network connect wp_mysql_net spawning-pool
docker network connect wp_mysql_net lair
*Question: how does WP exactly use the SQL db? examples.*

12) Set up a phpmyadmin to manage the SQL database created before.
we reuse the --link option, or can create and connect to network.
To test: http://localhost:8081, then use the username (root) and password (Kerrigan) defined previously as environment variables (q10).

13) -f option for following the logs in real time

15) docker ps shows how long the container has been running.

# C) Flask Website

16) http://localhost:3000/
You can check directly in root that .py file is there.

# D) The Swarm

17) docker swarm leave --force (if already in a swarm)
18) to install docker on debian 11, https://www.linuxtechi.com/install-docker-engine-on-debian/
to check version: sudo docker version
to check status: sudo systemctl status docker
if needed, to start: sudo systemctl start docker
to test: sudo docker run hello-world

The VM was created the following way:
1) I started fresh, I did not use the disk images from roger.
2) I chose debian as it is famous for its stability vs ubuntu, even if less complete.
3) set up a network in Virtualbox host manager, with the address 192.168.56.1, mask 255.255.255.0, DHCP deactivated. 
4) set up 2 adapters: one NAT, one Host-only adapter with the network created.
5) edited /etc/network/interfaces to define a static IP to the enp0s8 adapter, which is the host-only adapter, the adaptor for the VM, and gave that a static IP

Drawing to explain networking: https://bit.ly/3tfHFnx

19) check that the local machine is leader: docker node ls
