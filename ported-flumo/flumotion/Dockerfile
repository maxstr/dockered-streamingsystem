FROM    ubuntu:trusty
RUN     apt-get update 
RUN     apt-get install -qq software-properties-common python-software-properties
RUN     add-apt-repository ppa:gstreamer-developers/ppa -y
RUN     apt-get update -qq
RUN     apt-get install -qq subversion autopoint libtool autoconf python-gst-1.0 python-gi python3-gi python-gobject-dev gstreamer1.0* gir1.2-gstreamer-1.0 gir1.2-gst-plugins-base-1.0 libglib2.0-dev gir1.2-glib-2.0 libgirepository1.0-dev libglib2.0-0 gir1.2-gtk-3 libxml-parser-perl python-twisted python-gtk2 pkg-config libglib2.0-dev liborc-0.4-dev bison flex
RUN     apt-get install -qq python-pip python-dev build-essential git  
RUN     pip install --upgrade pip 
RUN     pip install --upgrade virtualenv
RUN     pip install icalendar==2.2 pyparsing python-dateutil
RUN     git clone https://github.com/aps-sids/flumotion-orig.git
RUN     cd flumotion-orig && git checkout porting-to-gst1.0 && ./autogen.sh && make
