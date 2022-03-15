FROM ubuntu:focal

RUN 	apt-get update -y
RUN 	apt-get upgrade -y
RUN 	DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    	curl \
    	openssh-server \
    	ca-certificates \
    	postfix

RUN	mkdir -p /etc/gitlab/trusted-certs

RUN	openssl req -x509 -nodes -days 365 -newkey rsa:2048 -subj '/CN=localhost' \
	-keyout /etc/gitlab/trusted-certs/localhost.key -out /etc/gitlab/trusted-certs/localhost.crt 
RUN 	curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN 	apt-get install -y gitlab-ce
RUN 	rm -rf /var/lib/apt/lists/*

RUN	echo "external_URL \"https://localhost\"" >> /etc/gitlab/gitlab.rb
RUN	echo "nginx['ssl_certificate'] = \"/etc/gitlab/trusted-certs/localhost.crt\"" >> /etc/gitlab/gitlab.rb
RUN	echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/trusted-certs/localhost.key\"" >> /etc/gitlab/gitlab.rb
RUN	echo "gitlab_rails['gitlab_shell_ssh_port'] = 4242" >> /etc/gitlab/gitlab.rb

EXPOSE	22 80 443

CMD (/opt/gitlab/embedded/bin/runsvdir-start &) && \ 
	gitlab-ctl reconfigure && service ssh restart && gitlab-ctl tail

#build: docker build -t pwaters/gitlab .
#run: docker run -it --rm -p 80:80 -p 4242:22 -p 443:443 --privileged -e GITLAB_ROOT_EMAIL="pierre.alban.waters@gmail.com" -e GITLAB_ROOT_PASSWORD="12345678" pwaters/gitlab
