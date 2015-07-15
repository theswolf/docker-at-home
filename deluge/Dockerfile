FROM theswolf/basement
MAINTAINER Christian Geymonat <chris.geymo@gmail.com>

# Install components
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
	sudo \
   deluged \
   deluge-web \
   deluge-console \
   runit \
   psmisc \
   nginx \
   php5-fpm \
   unzip \
   wget \
   php5-gd \
   libav-tools \
   zip \
   imagemagick \
   apache2-utils

ENV USER root

VOLUME ["/data","/app"]
WORKDIR /app

#RUN useradd -m -s /bin/nologin deluge
RUN deluge sh -c "deluged; sleep 5; killall deluged"
RUN deluge sh -c "echo \"deluge:deluge:10\" >> /app/.config/deluge/auth"
RUN deluge sh -c "deluged && sleep 10 && deluge-console \"config -s allow_remote True\" && deluge-console \"config allow_remote\" && killall deluged"
RUN mkdir -p /etc/service/deluged; mkdir -p /etc/service/deluge-web; mkdir -p /etc/service/php5-fpm; mkdir -p /etc/service/nginx


# Configure nginx
ADD nginx.conf /etc/nginx/nginx.conf
ADD default.conf /etc/nginx/sites-available/default

# Configure runit
ADD deluged.service /etc/service/deluged/run
ADD deluge-web.service /etc/service/deluge-web/run
ADD php5-fpm.service /etc/service/php5-fpm/run
ADD nginx.service /etc/service/nginx/run

ADD core.conf /tmp/core.conf

# Expose ports
EXPOSE 80
EXPOSE 8112
EXPOSE 58846
EXPOSE 58847
EXPOSE 58847/udp


CMD ["runsvdir", "/etc/service"]