FROM sensorhome.tk:5000/buildmachine as BUILD

#linux-headers-`uname -r`
RUN apt-get update && apt-get install -y autoconf g++ subversion linux-source  build-essential tofrodos git-core subversion dos2unix make gcc automake cmake checkinstall git-core dpkg-dev fakeroot pbuilder dh-make debhelper devscripts patchutils quilt git-buildpackage pristine-tar git yasm checkinstall cvs mercurial autopoint 
RUN apt-get install -y libexif-dev libjpeg-dev libid3tag* libFLAC*  libvorbis-dev libsqlite3-dev libavformat-dev
RUN mkdir /build
WORKDIR /build

#CHECKOUT master by commit id
RUN git clone git://git.code.sf.net/p/minidlna/git minidlna-git 

WORKDIR ./minidlna-git
RUN git checkout 799e6cf505ec470b2bf0ae4118143380aa16b837
RUN wget https://sourceforge.net/p/minidlna/patches/_discuss/thread/6559c586/9b55/attachment/manual-non-destructive-rescan_v2.patch
RUN patch <  manual-non-destructive-rescan_v2.patch


RUN ./autogen.sh && ./configure
RUN make
RUN checkinstall -D -y --pkgversion="1.2.1" --maintainer="Alex Dubchak <alex.dubchak@gmail.com>" --nodoc --install=no

FROM ubuntu:16.04 as FINAL
RUN apt-get update && apt-get install -y libexif-dev libjpeg-dev libid3tag* libFLAC*  libvorbis-dev libsqlite3-dev libavformat-dev

COPY --from=BUILD /build/minidlna-git/minidlna_1.2.1-1_amd64.deb /
RUN dpkg -i minidlna_1.2.1-1_amd64.deb

RUN mkdir /minidlna

ENTRYPOINT [ "minidlnad", "-f", "/minidlna/minidlna.conf", "-d" ]