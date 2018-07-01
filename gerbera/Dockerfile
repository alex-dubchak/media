FROM ubuntu:16.04

RUN apt-get update && apt-get install -y software-properties-common
RUN add-apt-repository ppa:stephenczetty/gerbera 
RUN apt-get update && apt install -y gerbera

COPY config.xml /etc/gerbera/config.xml

ENTRYPOINT [ "/usr/bin/gerbera", "-c", "/etc/gerbera/config.xml"]
