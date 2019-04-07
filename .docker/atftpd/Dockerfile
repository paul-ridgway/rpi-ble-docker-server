FROM ubuntu:18.04

MAINTAINER Paul Ridgway <paul@ridgway.io>

RUN apt-get -y update \
    && apt-get -y install atftpd \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /tmp/* /var/tmp/* /var/lib/apt/lists/*

CMD /usr/sbin/atftpd --user nobody.nogroup --daemon --no-fork --port 69 --bind-address 192.168.0.50 --logfile - /srv/tftp
EXPOSE 69/udp