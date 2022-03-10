FROM debian:buster

MAINTAINER pwaters <pwaters@student.hive.fi>

RUN apt-get update && DEBIAN_FRONTEND=noninteractive
RUN 	apt-get upgrade -y
RUN 	apt-get install -y ca-certificates
RUN 	apt-get install -y openssh-server
RUN 	apt-get install -y curl
RUN 	apt-get install -y perl

RUN 	curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN 	apt-get install -y gitlab-ce

#RUN 	mkdir -p /etc/gitlab/ssl && chmod 755 /etc/gitlab/ssl
#RUN	openssl req -x509 -nodes -days 365 -newkey rsa:4096 -subj "/CN=waters.fi" -keyout /etc/gitlab/ssl/waters.fi.key -out /etc/gitlab/ssl/waters.fi.crt
#RUN	echo "external_URL \"https://waters.fi\"" >> /etc/gitlab/gitlab.rb
#RUN	echo "nginx['ssl_certificate'] = \"/etc/gitlab/waters.fi.crt\"" >> /etc/gitlab/gitlab.rb
#RUN	echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/waters.fi.key\"" >> /etc/gitlab/gitlab.rb
#RUN	echo "gitlab_rails['gitlab_shell_ssh_port'] = 8022" >> /etc/gitlab/gitlab.rb

EXPOSE	443 80 22

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && \ 
	gitlab-ctl reconfigure && service ssh restart && gitlab-ctl tail

#build: docker build -t pwaters/watersfi .
#run: docker run -it --rm -p 8080:80 -p 8022:22 -p 8443:443 --memory="8g" --privileged -e GITLAB_ROOT_PASSWORD="root" pwaters/watersfi bash
