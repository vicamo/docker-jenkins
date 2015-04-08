FROM java:jre

MAINTAINER You-Sheng Yang <vicamo@gmail.com>

ENV DEBIAN_FRONTEND=noninteractive \
    JENKINS_HOME=/var/lib/jenkins \
    JENKINS_HTTP_PORT=8080 \
    JENKINS_SLAVE_AGENT_PORT=50000

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
	&& (cd /usr/share/jenkins; wget http://mirrors.jenkins-ci.org/war/latest/jenkins.war -O jenkins.war) \
	&& mkdir -p /usr/share/jenkins/plugins \
	&& mkdir -p /var/lib/jenkins/plugins \
	&& chown jenkins.jenkins /var/lib/jenkins/plugins

# https://github.com/docker/docker/issues/2174
# Docker seems to map IPv6 ports only. So when connected with IPv6
# host address, jenkins will rejest the connection because in Debian
# it listens to 127.0.0.1 by default.
RUN sed -e 's/^HTTP_HOST=.*/HTTP_HOST=0.0.0.0/' \
		-e 's/^AJP_HOST=.*/AJP_HOST=0.0.0.0/' \
		-i /etc/default/jenkins

ADD jenkins.runit /etc/service/jenkins/run
ADD init.groovy ${JENKINS_HOME}/init.groovy.d/tcp-slave-angent-port.groovy

# Jenkins home directoy is a volume, so configuration and build history
# can be persisted and survive image upgrades
VOLUME ${JENKINS_HOME}

EXPOSE ${JENKINS_HTTP_PORT} ${JENKINS_SLAVE_AGENT_PORT}

ENTRYPOINT ["/usr/sbin/runsvdir-start"]
