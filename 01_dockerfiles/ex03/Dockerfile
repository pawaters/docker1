FROM ubuntu:18.04

MAINTAINER pwaters <pwaters@student.hive.fi>

RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y curl openssh-server ca-certificates tzdata perl postfix

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh && chmod 777 script.deb.sh && ./script.deb.sh && apt-get install -y gitlab-ce

EXPOSE 443 80 22

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null

#build: docker build -t ex03 .
#run: docker run -it --rm -p 8080:80 -p 8022:22 -p 8443:443 --privileged ex03
