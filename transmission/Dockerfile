FROM sensorhome.tk:5000/buildmachine as BUILD

RUN mkdir /build
WORKDIR /build
RUN curl -L https://github.com/Mikayex/transmission/archive/2.94-seq.tar.gz -o master.tar.gz
RUN tar -xf master.tar.gz
WORKDIR ./transmission-2.94-seq
RUN mkdir build
WORKDIR ./build
RUN cmake ..
RUN make
RUN mkdir /transmission
#RUN cp ./cli/transmission-cli /transmission/transmission-cli
RUN cp ./utils/transmission-create /transmission/transmission-create
RUN cp ./utils/transmission-edit /transmission/transmission-edit
RUN cp ./utils/transmission-show /transmission/transmission-show
RUN cp ./daemon/transmission-daemon /transmission/transmission-daemon
RUN cp ./daemon/transmission-remote /transmission/transmission-remote
RUN cp -a ../web/ /transmission/


FROM ubuntu:16.04 as FINAL
RUN apt-get update && apt-get install -y libcurl4-openssl-dev libevent-dev libminiupnpc-dev
#libglib2.0  libminiupnpc libappindicator3 libssl
#WORKDIR /usr/local/bin/
COPY --from=BUILD /transmission/* /usr/local/bin/

RUN mkdir /usr/local/share/transmission/
RUN mkdir /usr/local/share/transmission/web/
COPY --from=BUILD /transmission/web/ /usr/local/share/transmission/web/

RUN mkdir /torrents
RUN mkdir /downloads

RUN chmod +xwr /torrents
RUN chmod +xwr /downloads

VOLUME [ "/torrents" ]
VOLUME [ "/downloads" ]

ENTRYPOINT ["transmission-daemon", "-a *.*.*.*", "-c/torrents","-w/downloads/movies", "-f", "-u media", "-v Passw00rd"]
