FROM debian:latest

USER root
WORKDIR /root

COPY ENTRYPOINT.sh /

RUN apt-get update && apt-get install -y --install-recommends \
    curl \
    iproute2 \
    iputils-ping \
    mininet \
    net-tools \
    openvswitch-switch \
    openvswitch-testcontroller \
    tcpdump \
    vim \
    x11-xserver-utils \
    xterm \
    nano \

 && rm -rf /var/lib/apt/lists/* \
 && chmod +x /ENTRYPOINT.sh

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

RUN tar xzf Python-3.4.4.tgz
RUN chmod o+x /usr/src/Python-3.4.4
WORKDIR /usr/src/Python-3.4.4
RUN ./configure
RUN make altinstall

RUN apt-get update
RUN apt-get install dirmngr -y 
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F88F6D313016330404F710FC9A2FD067A2E3EF7B
RUN echo "deb http://ppa.launchpad.net/gns3/ppa/ubuntu xenial main" >> /etc/apt/sources.list
RUN echo "deb-src http://ppa.launchpad.net/gns3/ppa/ubuntu xenial main" >> /etc/apt/sources.list
RUN apt-get update 
RUN DEBIAN_FRONTEND=noninteractive apt-get install gns3-gui -y
RUN apt-get install locales -y
RUN sed -i 's/^# *\(en_US.UTF-8\)/\1/' /etc/locale.gen
RUN locale-gen
RUN echo "export LC_ALL=en_US.UTF-8" >> ~/.bashrc
RUN echo "export LANG=en_US.UTF-8" >> ~/.bashrc
RUN echo "export LANGUAGE=en_US.UTF-8" >> ~/.bashrc
RUN service openvswitch-switch start
RUN apt-get install git ssh -y
RUN apt-get install python-dev  \
     build-essential libssl-dev libffi-dev \
     libxml2-dev libxslt1-dev zlib1g-dev \
     python-pip -y
WORKDIR /root
RUN git clone https://github.com/dlinknctu/OpenNet.git
WORKDIR /root/OpenNet
RUN ./configure.sh
RUN echo "172.17.0.1 master" >> /etc/hosts
RUN echo "[opennet-master]" >> /root/OpenNet/ansible/hosts
RUN echo "master ansible_ssh_user=root" >> /root/OpenNet/ansible/hosts
RUN ./install.sh master
WORKDIR /root

RUN apt-get install openjdk-8-jdk -y
RUN apt-get install ant maven python-dev eclipse -y
RUN mkdir /root/floodlight
RUN mkdir /root/floodlight-webui
COPY floodlight /root/floodlight
COPY floodlight-webui /root/floodlight-webui
WORKDIR /root/floodlight
RUN git submodule init
RUN git submodule update
RUN mkdir /var/lib/floodlight
RUN chmod 777 /var/lib/floodlight
RUN ant
EXPOSE 6633 6653 6640
