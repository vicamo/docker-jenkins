FROM java:jre

MAINTAINER You-Sheng Yang <vicamo@gmail.com>

ENV DEBIAN_FRONTEND=noninteractive \
    JENKINS_ROOT=/usr/share/jenkins \
    JENKINS_HOME=/var/lib/jenkins \
    JENKINS_VERSION=latest \
    JENKINS_PORT_8080_TCP_PORT=8080 \
    JENKINS_PORT_50000_TCP_PORT=50000

# We need Debian specific bits for better system management while
# also latest official war package to ensure correct dependencies
# between all the plugins. Overwrite jenkins.war directly. Update
# with cautions!
RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		jenkins \
		runit \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*_dists_* \
	&& wget -nv -O $JENKINS_ROOT/jenkins.war \
		http://mirrors.jenkins-ci.org/war/$JENKINS_VERSION/jenkins.war \
	&& mkdir -p $JENKINS_ROOT/plugins \
	&& mkdir -p $JENKINS_HOME/plugins \
	&& chown jenkins.jenkins $JENKINS_HOME/plugins

ADD jenkins.runit /etc/service/jenkins/run
ADD init.groovy ${JENKINS_HOME}/init.groovy.d/tcp-slave-angent-port.groovy

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

ENTRYPOINT ["/usr/sbin/runsvdir-start"]
