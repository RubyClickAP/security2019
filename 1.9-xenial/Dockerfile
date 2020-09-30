# task `rake generate:ruby-passenger`
# Tori 1.9-xenial

FROM canvastw/ruby-passenger:1.9-xenial
MAINTAINER Click-AP

USER root
#Install SVN client from official repository
RUN echo "deb http://opensource.wandisco.com/ubuntu `lsb_release -cs` svn19" >> /etc/apt/sources.list.d/subversion19.list
RUN curl -sS http://opensource.wandisco.com/wandisco-debian.gpg | apt-key add - \
 && apt-get update \
 && apt-get install -y subversion \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN svn co --non-interactive --no-auth-cache http://192.168.50.166:18080/svn/yakitory/branches/security2019 /opt/yakitory

# Nginx Configuration and entrypoint
COPY main.d/* /opt/nginx/main.d/
COPY nginx.conf.erb /opt/nginx/conf/nginx.conf.erb
COPY entrypoint /usr/src/entrypoint
RUN mkdir -p /opt/yakitory/log \
 && mkdir -p /opt/yakitory/public/data/tmp_uploads \
 && chown -R docker:docker /opt/yakitory \
 && chown docker:docker /usr/src/entrypoint \
 && chmod +x /usr/src/entrypoint

RUN mv /opt/nginx/conf/nginx.conf /opt/nginx/conf/nginx.conf.bk \
 && rm -f /opt/nginx/conf/nginx.conf \
 && chown -R docker /opt/nginx

USER docker

EXPOSE 80
WORKDIR /opt/yakitory
ENTRYPOINT ["/tini", "--", "/usr/src/entrypoint"]
