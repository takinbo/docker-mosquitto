# docker-mosquitto #

docker-mosquitto is a dockerized container image for the MQTT-broker called [mosquitto](http://mosquitto.org/).

**Mosquitto Version:** 1.3.5  
**Exposed Ports:** 1883, 8883  
**Volumes:** /etc/mosquitto, /var/data/mosquitto, /var/log/mosquitto  

### Dockerfile ###
    FROM ubuntu:precise
    MAINTAINER Tim Akinbo <takinbo@timbaobjects.com>

    RUN apt-get update
    RUN apt-get install -y build-essential libssl-dev libwrap0-dev libc-ares-dev python-dev

    ADD http://mosquitto.org/files/source/mosquitto-1.3.5.tar.gz /
    RUN tar xzf mosquitto-1.3.5.tar.gz
    WORKDIR mosquitto-1.3.5
    RUN make binary && make install
    WORKDIR /
    RUN rm -R mosquitto-1.3.5.tar.gz mosquitto-1.3.5

    RUN adduser --system --disabled-password --no-create-home --disabled-login mosquitto
    RUN cp /etc/mosquitto/mosquitto.conf.example /etc/mosquitto/mosquitto.conf

    VOLUME ["/etc/mosquitto/", "/var/data/mosquitto", "/var/log/mosquitto"]

    EXPOSE 1883 8883
    CMD /usr/local/sbin/mosquitto -c /etc/mosquitto/mosquitto.conf

    # clean up
    RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
