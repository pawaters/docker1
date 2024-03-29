# NOTES FOR EVAL
In general:
- at the end of each dockerfile: instructions to build, run & test. Made to respond to the requirements of the eval form. 
- in this readme, comments and extra tests and commands on top of what eval forms requests.

Basically, we should be able to go through evaluation with the minimum comments in the dockerfiles. If not, this file has extra information if needed.

_I recommend to start with the last exercise, as it takes 10-20 minutes to build and run. I will do it on my laptop on the backgroun and we can go on with the eval on my desktop._

## 00) HOW TO DOCKER
Docker version: docker --version only show client version.

### A) Basic containers commands:
1) docker pull
3) if port blocked, lsof -i -P -n | grep 5000; kill -9 [PID]; 
 docker inspect -f {{.HostConfig.RestartPolicy}} overlord
6) to launch a shell of a debian container: docker run -it --rm debian /bin/bash
- apt-get install sudo
- sudo apt-get install git -y
- sudo apt-get install vim -y 
- sudo apt-get install gcc -y
- sudo apt-get install make -y
Git repo to use: https://github.com/pawaters/fillit-docker

### B) Wordpress:
9-10) SQL: SHOW DATABASES; or SHOW SCHEMA;

11) Set up wordpress with 2 containers: WP and SQL for DB.
both containers can be run in the same Docker Host, so no need to install Docker in a virtual machine for now. http://localhost:8080.
Other option than --link: 
a) create a network: docker network create wp_mysql_net
b) connect both containers to the network: 
docker network connect wp_mysql_net spawning-pool
docker network connect wp_mysql_net lair

12) Set up a phpmyadmin to manage the SQL database created before.
we reuse the --link option, or can create and connect to network.
To test: http://localhost:8081, then use the username (root) and password (Kerrigan) defined previously as environment variables (q10).

13) -f option for following the logs in real time

15) docker ps shows how long the container has been running.

### C) Flask Website

16) http://localhost:3000/
You can check directly in root that .py file is there.

### D) The Swarm

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

19)For this, I have used the VM as the leader, root user, with the local machine as slave.
Reason: local machine firewall (Mac Mini). 
check leader status: docker node ls
docker swarm join-token worker
docker node ls -  only works on master
do it twice, before and after joining to see the difference.:w

20) if needed: docker network rm

21 - 22) docker service inspect -f "{{.Spec.TaskTemplate.ContainerSpec.Env}}" orbital-command

23 - 24) docker service rm engineering-bay (if needed to reset)
each node will access independently.
docker service ps engineering-bay
Docker service inspect -f "{{.Spec.TaskTemplate.ContainerSpec.Env}}" engineering-bay
It is not command 27 to run (docker service scale -d marines=20) the mariens service is not running.
It is command ex 24 to see the logs: docker service logs -f $(docker service ps engineering-bay -f "name=engineering-bay.1" -q)

25 - 26 - 27) docker service ps marines
docker service inspect -f "{{.Spec.TaskTemplate.ContainerSpec.Env}}" marines
docker service logs marines

### E) CLEANUP
28) docker swarm leave -f is other option.

## 01) DOCKERFILES
Every time, look for build and run instructions as comments of the dockerfile.

_00)_ VIM: to launch Vim's explorer - ":Explore"
To prove it is the container's env, go to Home, compare with local home.

_01_) DEBIAN / TEAM SPEAK: 

*SETUP*
installing the necesarry packages, removig compressed bz2 file after extraction
 curl	-k (checking is the server secured, having proper SSL certificate)
	-L (location)
	-J (make sure if there is already a same filename not be overwritten)
	-O (only the file path will be used for naming, the path(html name) will be cut off)

tar	-x (extract)
		-v (will extract with the same name what is the zip file)
		-f (file specifier)

steps to install a TeamSpeak 3 Server on Linux, translated in dockerfile
https://www.bennetrichter.de/en/tutorials/teamspeak3-server-linux/ for rest of the build and run: unzipping file, wget download, unzip, run script.

*USER TESTING*
- connect to TS server in TS client: Bookmarks > TeamSpeak server; then add token.i

01-ex01 Team speak client crashes when you connect to any server in its latest version (3.5.6) with latest Mac OS (12.2 Monterrey).
--> solution (as of 18/3/2022): Download the beta (3.5.7).

_ex 02)_ Dockerfile to containerize a Rails app

The goal is to create a dockerfile that will install all that is needed to launch the rails server: dependencies, gems, copy app to right place, launch migrations.
Then the dockerfile given by the subject will laucnh the rails server.
All the ONBUILD actions will be done when the child image is built.
Details in parent Dockerfile.

In the vogsphere repo, we have turned in the Dockerfile that was to be created, as well as the subject "rail-Dockerfile", for ease of testing.

_ex 03) Containerize dev version of Gitlab_
we followed the instructions indicated in the link of instructions, the updated version to install gitlab. the hardest was to set up https, and the time taken by the installation to be able to test different options. Because of the extensive build and run time, I switched to the best machine I have: MacBook Air (2020) with M1 chip. 

