FROM %%BASE_IMAGE%%
#FROM homeassistant/armhf-base:latest
MAINTAINER Franck Nijhof <frenck@geekchimp.com>

# Add env
ENV LANG="C.UTF-8"
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# Copy run file
COPY files/run.sh /

# Setup base
RUN apk add --no-cache jq git python make g++ libffi-dev openssl-dev dbus \
      avahi-compat-libdns_sd avahi-dev avahi-tools avahi-ui-tools \
      nodejs-current yarn && \
    mkdir -p /var/run/dbus && \
    mkdir -p /var/run/avahi-daemon && \
    chown messagebus:messagebus /var/run/dbus && \
    chown avahi:avahi /var/run/avahi-daemon && \
    yarn global add npm && \
    yarn cache clean && \
    apk del yarn && \
    npm set unsafe-perm true && \
    npm -g install \
      node-gyp \
      homebridge@0.4.27 \
      homebridge-homeassistant@2.3.1 \
      homebridge-config-ui \
      homebridge-broadlink-rm \
      homebridge-mi-air-purifier miio \
      homebridge-xiaomi-mi-robot-vacuum \
      homebridge-mi-aqara && \
    chmod a+x /run.sh

# Copy configuration files
COPY files/avahi-daemon.conf /etc/avahi/
COPY files/homebridge-config.json /root/

CMD [ "/run.sh" ]
