FROM java:8-jdk

MAINTAINER You-Sheng Yang <vicamo@gmail.com>

ENV DEBIAN_FRONTEND=noninteractive \
    JENKINS_ROOT=/usr/share/jenkins \
    JENKINS_HOME=/var/lib/jenkins \
    JENKINS_UC=https://updates.jenkins-ci.org \
    JENKINS_PORT_8080_TCP_PORT=8080 \
    JENKINS_PORT_50000_TCP_PORT=50000

RUN (wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -) \
	&& (echo "deb http://pkg.jenkins-ci.org/debian binary/" > /etc/apt/sources.list.d/jenkins.list) \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends \
		jenkins \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*_dists_* \
	&& mkdir -p $JENKINS_ROOT/plugins \
	&& mkdir -p $JENKINS_HOME/plugins \
	&& chown jenkins.jenkins $JENKINS_HOME/plugins

ADD entrypoint.sh /
ADD init.groovy ${JENKINS_HOME}/init.groovy.d/tcp-slave-angent-port.groovy
ADD plugins.sh /usr/local/bin/plugins.sh

# Respect Docker ENV
RUN sed -e "s!^JENKINS_ROOT=.*!JENKINS_ROOT=$JENKINS_ROOT!" \
		-e "s!^JENKINS_HOME=.*!JENKINS_HOME=$JENKINS_HOME!" \
		-e "s!^HTTP_PORT=.*!HTTP_PORT=$JENKINS_PORT_8080_TCP_PORT!" \
		-i /etc/default/jenkins \
	&& sed -e "s!50000!$JENKINS_PORT_50000_TCP_PORT!" \
		-i ${JENKINS_HOME}/init.groovy.d/tcp-slave-angent-port.groovy

# https://github.com/docker/docker/issues/2174
# Docker seems to map IPv6 ports only. So when connected with IPv6
# host address, jenkins will rejest the connection because in Debian
# it listens to 127.0.0.1 by default.
RUN sed -e 's!^HTTP_HOST=.*!HTTP_HOST=0.0.0.0!' \
		-e 's!^AJP_HOST=.*!AJP_HOST=0.0.0.0!' \
		-i /etc/default/jenkins

# Jenkins home directoy is a volume, so configuration and build history
# can be persisted and survive image upgrades
VOLUME ${JENKINS_HOME}

EXPOSE ${JENKINS_PORT_8080_TCP_PORT} ${JENKINS_PORT_50000_TCP_PORT}

ENTRYPOINT ["/entrypoint.sh"]
