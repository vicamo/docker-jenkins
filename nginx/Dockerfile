FROM nginx

MAINTAINER You-Sheng Yang <vicamo@gmail.com>

ENV JENKINS_HOSTNAME=localhost

RUN rm /etc/nginx/conf.d/default.conf
ADD jenkins.conf /etc/nginx/conf.d/default.conf

ADD nginx.runit /etc/service/nginx/run
ENTRYPOINT ["/etc/service/nginx/run"]
