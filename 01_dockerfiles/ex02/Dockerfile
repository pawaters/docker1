FROM ruby

MAINTAINER pwaters <pwaters@student.hive.fi>

RUN apt-get update -y
RUN apt-get install -y build-essential libpq-dev nodejs sqlite3
RUN gem update && gem install rails && gem install bundler

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app
ONBUILD EXPOSE 3000
ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# 1) build image from my dockerfile installing ruby and all dependencies for rails: docker build -t ft-rails:on-build .
# 2) build image from task dockerfile that calls our own dockerfile to launch the rails server: docker build -t ex02 .
# 3) run to launch rails server: docker run -it --rm -p 3000:3000 ex02 
