FROM debian

MAINTAINER pwaters <pwaters@student.hive.fi>

ENV TS3SERVER_LICENSE=accept

WORKDIR /home/teamspeak

EXPOSE 9987/udp 10011 30033

RUN apt-get update && \
	apt-get upgrade -y && \
	apt-get install -y wget bzip2 && \
	wget https://files.teamspeak-services.com/releases/server/3.13.6/teamspeak3-server_linux_amd64-3.13.6.tar.bz2 && \
	tar -xvf teamspeak3-server_linux_amd64-3.13.6.tar.bz2

WORKDIR teamspeak3-server_linux_amd64

ENTRYPOINT sh ts3server_minimal_runscript.sh

#steps to install a TeamSpeak 3 Server on Linux, translated in dockerfile
#https://hub.docker.com/_/teamspeak for ports, license.
#https://www.bennetrichter.de/en/tutorials/teamspeak3-server-linux/ for rest of the build and run: unzipping file, wget download, unzip, run script.

#to build: docker build -t ex01 .
#to run: docker run --rm -p 9987:9987/udp -p 10011:10011 -p 30033:30033 ex01
