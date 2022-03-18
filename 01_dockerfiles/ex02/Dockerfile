FROM ruby:3.1.0

MAINTAINER pwaters <pwaters@student.hive.fi>

RUN 	apt update -y && \
	apt install -y nodejs && \
	rm -rf /var/lib/apt/lists/*

WORKDIR /opt/app

ONBUILD COPY . /opt/app
ONBUILD RUN gem install bundler
ONBUILD RUN bundle install
ONBUILD RUN rake db:create
ONBUILD RUN rake db:migrate



# build image from my dockerfile installing ruby and all dependencies for rails: 
#docker build -t ft-rails:on-build .

# create rails app: helloworld gemfile
#docker run -it --rm --mount type=bind,source="$(pwd)",target=/opt/app -w\ 
#/opt/app ruby:3.1.0 /bin/bash -c "gem install rails && rails new helloworld"

#sed -i '' "s|gem 'sqlite3'|&, '~> 1.3.6'|" helloworld/Gemfile

# build image from task dockerfile that calls our own dockerfile to launch the rails server: 
#docker build -t ex02 -f rail-Dockerfile ./helloworld

# run to launch rails server: 
# docker run -it --rm -p 3000:3000 ex02

# test: open Docker app 
