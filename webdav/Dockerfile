FROM ubuntu:trusty

RUN apt-get update && apt-get install -y nginx nginx-extras apache2-utils

VOLUME /media
EXPOSE 80
COPY webdav.conf /etc/nginx/conf.d/default.conf
RUN rm /etc/nginx/sites-enabled/*

COPY entrypoint.sh /
RUN chmod +x entrypoint.sh
RUN mkdir -p /media/invoices
RUN chmod a+rwx /media -R
CMD /entrypoint.sh && nginx -g "daemon off;"
