FROM ubuntu:18.04
MAINTAINER clu wong clu_wong@163.com

ENV REFRESHED_AT 2018-11-3

COPY sources.list /etc/apt/sources.list
RUN apt-get -yqq update && apt-get install -yqq apache2 vim

# RUN curl -sSL https://get.rvm.io | bash -s stable
# RUN /bin/bash -l -c "rvm requirements"
# RUN /bin/bash -l -c "rvm install 2.5"
# RUN /bin/bash -l -c "gem install bundler --no-ri --no-rdoc"


#
# VOLUME /data
VOLUME [ "/var/www/html" ]
#
WORKDIR /var/www/html

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

RUN mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR
EXPOSE 80

RUN /bin/bash -l -c "echo 'ServerName localhost' >> /etc/apache2/apache2.conf"
ENTRYPOINT [ "/usr/sbin/apache2" ]
CMD [ "-D", "FOREGROUND" ]