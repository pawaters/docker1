FROM ubuntu:focal
RUN apt-get update -y && \
    apt-get upgrade -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    curl \
    openssh-server \
    ca-certificates \
    postfix && \
    mkdir -p /etc/gitlab/trusted-certs && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -subj '/CN=localhost' -keyout /etc/gitlab/trusted-certs/localhost.key -out /etc/gitlab/trusted-certs/localhost.crt && \
    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash && \
    apt-get install -y gitlab-ce && \
    rm -rf /var/lib/apt/lists/* && \
    echo "external_url \"https://localhost\"" >> /etc/gitlab/gitlab.rb && \
    echo "nginx['ssl_certificate'] = \"/etc/gitlab/trusted-certs/localhost.crt\"" >> /etc/gitlab/gitlab.rb && \
    echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/trusted-certs/localhost.key\"" >> /etc/gitlab/gitlab.rb && \
    echo "gitlab_rails['gitlab_shell_ssh_port'] = 4242" >> /etc/gitlab/gitlab.rb
EXPOSE 22 443 80
CMD (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && service ssh restart && gitlab-ctl tail

# Build:
# docker build -t pwaters/gitlab .
# Run:
# docker run -it --rm -p 80:80 -p 4242:22 -p 443:443 --privileged -e GITLAB_ROOT_EMAIL="pwaters@gmail.com" -e GITLAB_ROOT_PASSWORD="12345678" pwaters/gitlab

# Test:
# - open safari, go to  https://localhost (patience!) :)
# - create empty repo called "test", and add ssh key in user preferences.
# - Disable ssl authentication to avoid error message about self signed certificate:
#        git config --global http.sslVerify false
# - clone by https: git clone https://localhost/root/test.git https_test
# - clone by ssh: git clone ssh://git@localhost:4242/root/test.git ssh_test
# if issue with host non recognised, remove known hosts: rm ~/.ssh/known_hosts 	
# as for pushing: create a new dir, git init, add, commit. then git remote add origin ssh://git@localhost:4242/root/test.git then git push -uf origin master.
