FROM ubuntu

MAINTAINER khanhnt

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update

RUN apt-get install apache2 -y

RUN apt-get install apache2-utils -y

RUN apt-get clean

COPY index.html /var/www/html

CMD ["apache2ctl", "-D", "FOREGROUND"]

EXPOSE 80

