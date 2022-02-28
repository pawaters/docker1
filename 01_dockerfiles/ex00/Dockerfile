FROM alpine

MAINTAINER pwaters <pwaters@hive.42.fr>

RUN apk update && apk upgrade && apk add vim

ENTRYPOINT vim

## To build: docker build -t ex00 .
## To run: docker run --rm -ti ex00
