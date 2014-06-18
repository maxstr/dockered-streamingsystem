# DOCKER-VERSION 0.11.1
FROM    streamingsystem/base
RUN apt-get update

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

ADD     sources /home/    
RUN     mkdir /root/.ssh
RUN     cat /home/public_keys >> /root/.ssh/authorized_keys
RUN     chmod 700 /root/.ssh/
RUN     chmod 600 /root/.ssh/authorized_keys


EXPOSE 22
CMD    ["/usr/sbin/sshd", "-D"]
