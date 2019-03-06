FROM python:2.7-alpine

RUN apk update \ 
    apk add bash

RUN mkdir /app
WORKDIR /app

ENV KEGBOT_VERSION=master
RUN  apk add curl 
RUN  curl -k -L https://github.com/Kegbot/kegbot-server/archive/${KEGBOT_VERSION}.tar.gz -o /tmp/kegbot.tar.gz 
RUN  tar -xf /tmp/kegbot.tar.gz -C /tmp/ 
RUN  ls -la /tmp/ 
RUN  cp -r /tmp/kegbot-server-${KEGBOT_VERSION}/* /app/ 
RUN  apk add gcc musl-dev 
RUN  apk add zlib-dev libjpeg-turbo libjpeg-turbo-dev libjpeg openjpeg 
RUN  apk add mysql-client 
RUN  apk add mariadb-dev 
RUN  ls -la ./ 
RUN  sed '/st_mysql_options options;/a unsigned int reconnect;' /usr/include/mysql/mysql.h -i.bkp
RUN  python ./setup.py install
RUN  apk del musl-dev gcc 
RUN  rm -rf /tmp/kegbot*


RUN mkdir -p /etc/kegbot
COPY local_settings.py /etc/kegbot/

ADD     run.sh /run.sh
RUN     chmod 700 /run.sh

EXPOSE  8000
ENTRYPOINT ["/bin/sh"]
CMD     ["/run.sh"]
