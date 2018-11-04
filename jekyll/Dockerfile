FROM ubuntu:18.04
MAINTAINER clu wong clu_wong@163.com

ENV REFRESHED_AT 2018-11-3

COPY sources.list /etc/apt/sources.list
RUN apt-get -yqq update && apt-get install -yqq curl

RUN curl -sSL https://get.rvm.io | bash -s stable
RUN /bin/bash -l -c "rvm requirements"
RUN /bin/bash -l -c "rvm install 2.5"
RUN /bin/bash -l -c "gem install bundler jekyll --no-ri --no-rdoc"


#
VOLUME /data
VOLUME /var/www/html
#
WORKDIR /data
#
ENTRYPOINT [ "/bin/bash", "-l", "-c", "jekyll build --destination=/var/www/html" ]