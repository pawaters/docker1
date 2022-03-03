FROM ubuntu

MAINTAINER pwaters <pwaters@student.hive.fi>

RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y ca-certificates
RUN apt-get install -y openssh-server
RUN apt-get install -y wget
RUN apt-get install -y postfix

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
RUN chmod 777 script.deb.sh
RUN ./script.deb.sh
RUN apt-get install -y gitlab-ce

RUN apt-get update 
RUN apt-get install -y tzdata
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 443 80 22

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null

#build: docker build -t ex03 .
#run: docker run -it --rm -p 8080:80 -p 8022:22 -p 8443:443 --privileged ex03
