# DOCKER-VERSION 0.11.1
# streaming-system/base container. We'd like this container to be easily any part of the streaming-system, so it's got all the parts!

FROM    ubuntu:latest
# Includes any additional files we'd like accessable to our image
ADD     sources /home/
RUN     apt-get update
# Runtime + setup utilities
RUN     apt-get -q -y install make
RUN     apt-get -q -y install git-core
RUN     apt-get -q -y install apache2-utils
# Utilities in case someone connects to the image...
RUN     apt-get -q -y install vim htop bash-completion wget
# Python-specific utilities
RUN     apt-get -q -y install python2.7 python-software-properties
RUN     python2.7 /home/get-pip.py
RUN     pip install virtualenv
RUN     rm /home/get-pip.py
# Add multiverse repositories
RUN     echo "deb http://archive.ubuntu.com/ubuntu precise multiverse" >> /etc/apt/sources.list
RUN     echo "deb http://archive.ubuntu.com/ubuntu precise-updates multiverse" >> /etc/apt/sources.list
RUN     echo "deb http://archive.ubuntu.com/ubuntu precise-backports multiverse" >> /etc/apt/sources.list
RUN     echo "deb http://archive.ubuntu.com/ubuntu precise-security multiverse" >> /etc/apt/sources.list
# GStreamer
RUN     apt-get -y -q install gstreamer0.10.* python-gst0.10 gstreamer-tools
# Flumotion build and runtime dependencies
RUN     apt-get install -y -q build-essential autoconf autopoint libtool libxml-parser-perl python-dev libvorbis-dev libogg-dev libtheora-dev libvpx-dev subversion
RUN     apt-get install -y -q python-kiwi python-twisted.* ssl-cert
# Grabbing streaming system
RUN     git clone https://github.com/maxstr/streaming-system.git -b docker ~/streaming-system
RUN     cd ~/streaming-system && git submodule init && git submodule update
# Website setup
RUN     touch ~/streaming-system/website/private/requirements.txt  
RUN     cd ~/streaming-system/website && make install
RUN     . ~/streaming-system/website/bin/activate && cd ~/streaming-system/website && (yes no | python manage.py syncdb)
# Core flumotion and ugly plugin config
RUN     cd ~/streaming-system/tools/flumotion && git clean -f -x && ./autogen.sh && make -j16 && make install
RUN     cd ~/streaming-system/tools/flumotion-ugly && git clean -f -x && ./autogen.sh && make -j16 && make install
RUN     cp -rf ~/streaming-system/tools/flumotion-config/fromdeb/etc/* /usr/local/etc/
RUN     ln -sf /usr/local/etc/init.d/flumotion /etc/init.d/flumotion
RUN     make-ssl-cert /usr/share/ssl-cert/ssleay.cnf /usr/local/etc/flumotion/default.pem || true
# Generation of flumotion user, making things nice for this user
RUN     sudo adduser --system --quiet --home /usr/local/var/run/flumotion --shell /usr/sbin/nologin --no-create-home --group --disabled-password --disabled-login --gecos "Flumotion Streaming Server" flumotion
RUN     chown flumotion:flumotion /usr/local/etc/flumotion/default.pem
RUN     chown -R flumotion:flumotion ~/streaming-system
RUN     adduser flumotion video
# DVSSwitch below
RUN     apt-get install -y -q libgstreamer-plugins-base0.10-dev
RUN     cd ~/ && git clone https://github.com/timvideos/gst-plugins-dvswitch.git && cd gst-plugins-dvswitch && git clean -f -x && ./autogen.sh && make && make install 
