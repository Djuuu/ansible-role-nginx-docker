ARG LINUXSERVER_NGINX_VERSION=latest

FROM lscr.io/linuxserver/nginx:${LINUXSERVER_NGINX_VERSION}

# Override default nginx conf files
COPY config/nginx/nginx.conf.sample              /defaults/nginx/nginx.conf.sample
COPY config/nginx/site-confs/default.conf.sample /defaults/nginx/site-confs/default.conf.sample

##
# Install Nginx ultimate bad bot blocker
# https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker
##
RUN mkdir -p /usr/local/sbin \
 && wget https://raw.githubusercontent.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker/master/install-ngxblocker \
         -O /usr/local/sbin/install-ngxblocker \
 && chmod +x /usr/local/sbin/install-ngxblocker \
 && /usr/local/sbin/install-ngxblocker -x

# setup blocker update cron
RUN echo -e "#!/bin/sh\n\n/usr/local/sbin/update-ngxblocker -n\n" > /etc/periodic/daily/update-ngxblocker \
 && chmod +x /etc/periodic/daily/update-ngxblocker
