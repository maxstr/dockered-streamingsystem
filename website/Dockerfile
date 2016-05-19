# DOCKER-VERSION 0.11.1
# streaming-system/website container

FROM    ubuntu:latest
ENV     SOURCEDIR   /home/sources
ENV     BASEDIR     ~/streaming-system
# Installs we need for our image
RUN     apt-get update
RUN     apt-get -q -y install make git-core apache2-utils
RUN     apt-get -q -y install vim htop bash-completion wget
RUN     apt-get -q -y install python2.7 python-software-properties software-properties-common python-virtualenv
RUN     pip install gunicorn

# Anything that requires a configuration file in sources goes below. Source files are located in $SOURECDIR/
ADD     sources $SOURCEDIR
RUN     cp -R $SOURCEDIR/streaming-system $BASEDIR
RUN     apt-get install -y -q nginx
# Set up upstart config
RUN     ln -sf $BASEDIR/website/conf/init.conf /etc/init/website.conf
RUN     ln -sf /lib/init/upstart-job /etc/init.d/website
# Set up nginx config
RUN     ln -sf $SOURCEDIR/website/conf/nginx.conf /etc/nginx/sites-available/website
RUN     ln -sf /etc/nginx/sites-available/website /etc/nginx/sites-enabled/website
RUN     echo '' >> ~/streaming-system/website/private/requirements.txt  
# Setup website
RUN     cd $BASEDIR/website && make prepare-serve
RUN     . $BASEDIR/website/bin/activate && cd $BASEDIR/streaming-system/website && (yes no | python manage.py syncdb)
RUN     initctl reload-configuration
RUN     service website restart
RUN     service nginx restart
CMD     tail -f /var/log/nginx/nginx_error.log
