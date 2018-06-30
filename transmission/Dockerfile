FROM buildmachine as BUILD

RUN mkdir /build
WORKDIR /build
RUN curl -L https://github.com/midenok-forks/transmission-patches/archive/master.tar.gz -o master.tar.gz
RUN tar -xf master.tar.gz
WORKDIR ./transmission-patches-master
RUN ./configure --disable-gtk
RUN make
RUN mkdir /transmission
RUN cp ./cli/transmission-cli /transmission/transmission-cli
RUN cp ./utils/transmission-create /transmission/transmission-create
RUN cp ./utils/transmission-edit /transmission/transmission-edit
RUN cp ./utils/transmission-show /transmission/transmission-show
RUN cp ./daemon/transmission-daemon /transmission/transmission-daemon
RUN cp ./daemon/transmission-remote /transmission/transmission-remote
RUN cp -a ./web/ /transmission/


FROM ubuntu:16.04 as FINAL
RUN apt-get update && apt-get install -y libcurl4-openssl-dev libevent-dev 
#libglib2.0  libminiupnpc libappindicator3 libssl
#WORKDIR /usr/local/bin/
COPY --from=BUILD /transmission/* /usr/local/bin/

RUN mkdir /usr/local/share/transmission/
RUN mkdir /usr/local/share/transmission/web/
COPY --from=BUILD /transmission/web/ /usr/local/share/transmission/web/

RUN mkdir /torrents
RUN mkdir /downloads

VOLUME [ "/torrents" ]
VOLUME [ "/downloads" ]

ENTRYPOINT ["transmission-daemon", "-a *.*.*.*", "-c /torrents","-w /downloads", "-f", "-u media", "-v Passw00rd"]