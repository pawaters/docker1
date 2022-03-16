# NOTES FOR EVAL

## 00) how to docker

# a) Basic containers:
1) docker pull
3) if port blocked, lsof -i -P -n | grep 5000; kill -9 [PID]; 
 docker inspect -f {{.HostConfig.RestartPolicy}} overlord
6) to launch a shell of a debian container: docker run -it --rm debian /bin/bash
install vim, gcc, make.
*ToDo: Find my clean version of fillit. and test it there. Think of easiest I am most proud of.*

# b) Wordpress:
SQL: SHOW DATABASES; or SHOW SCHEMA;

