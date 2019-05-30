FROM ubuntu:14.04
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get -y install sudo
RUN apt-get -y install apt-utils
RUN apt-get update && apt-get install -y --no-install-recommends \
    iproute2 \
    iputils-ping \
    net-tools \
    tcpdump \
    nano \
    x11-xserver-utils \
    xterm \
    curl \
    git \
    nmap \
    python-setuptools \
    libssl-dev \
    libcap-dev \
pkg-config gcc make build-essential libexpat-dev libgmp-dev \
             libssl-dev libpcap-dev byacc flex \
             python-dev \
		libelf-dev

RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    build-essential checkinstall -y \
    libreadline-gplv2-dev \
    libncursesw5-dev \
    libssl-dev \
    libsqlite3-dev \
    tk-dev \
    libgdbm-dev \
    libc6-dev \
    libbz2-dev \
    wget
WORKDIR /usr/src
RUN wget https://www.python.org/ftp/python/3.4.4/Python-3.4.4.tgz

RUN tar -xzf Python-3.4.4.tgz
RUN rm -rf Python-3.4.4.tgz
RUN chmod o+x /usr/src/Python-3.4.4
WORKDIR /usr/src/Python-3.4.4
RUN ./configure
RUN make altinstall

RUN mkdir /root/floodlight
RUN mkdir /root/floodlight-webui
COPY floodlight /root/floodlight
COPY floodlight-webui /root/floodlight-webui
WORKDIR /root/floodlight
RUN git submodule init
RUN git submodule update
RUN mkdir /var/lib/floodlight
RUN chmod 777 /var/lib/floodlight
RUN apt-get install ant maven python-dev eclipse -y
RUN ant
COPY floodlight-webui/ /var/www/html/
RUN apt-get install software-properties-common -y
RUN add-apt-repository ppa:openjdk-r/ppa -y
RUN apt-get update
RUN apt-get install openjdk-8-jdk -y
RUN apt-get install openvswitch-switch -y
WORKDIR /root
RUN git clone git://github.com/mininet/mininet
WORKDIR /root/mininet
RUN git checkout -b 2.2.1 2.2.1
WORKDIR /root
RUN mininet/util/install.sh -a
RUN service openvswitch-switch start
RUN apt-get install -y git autoconf screen cmake build-essential sysstat python-matplotlib uuid-runtime python-pip 
COPY OpenNet /root/OpenNet
RUN wget http://glaros.dtc.umn.edu/gkhome/fetch/sw/metis/metis-5.1.0.tar.gz
RUN tar -xzf metis-5.1.0.tar.gz
RUN rm metis-5.1.0.tar.gz
WORKDIR metis-5.1.0
RUN make config
RUN make
RUN make install
RUN pip install Pyro4
WORKDIR /root
RUN git clone git://github.com/MaxiNet/MaxiNet.git
WORKDIR /root/MaxiNet
RUN git checkout v1.2
RUN make install

RUN git clone https://github.com/dlinknctu/OpenNet.git
WORKDIR /root/OpenNet
RUN echo "10.0.0.1 master" >> /etc/hosts
RUN echo "10.0.0.2 slave" >> /etc/hosts
RUN echo "10.0.0.3 slave" >> /etc/hosts

WORKDIR /root/
RUN apt-get install -y libnl-3-dev libnl-genl-3-dev libnl-route-3-dev pkg-config python-tz libpcap-dev libcap2-dev

RUN git clone https://github.com/intrig-unicamp/mininet-wifi
WORKDIR mininet-wifi
RUN util/install.sh -W
