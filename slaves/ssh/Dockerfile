FROM vicamo/java:jessie-openjdk-7-jre

ENV JENKINS_UID=105 \
	JENKINS_GID=108 \
	JENKINS_HOME=/var/lib/jenkins \
	JENKINS_PORT_22_TCP_PORT=22

RUN apt-get update \
	&& apt-get install --no-install-recommends -y --force-yes \
		openssh-server \
	&& mkdir -p /var/run/sshd \
	&& apt-get clean \
	&& rm -f /var/lib/apt/lists/*_dists_*

RUN addgroup --system --gid ${JENKINS_GID} jenkins \
	&& adduser --home ${JENKINS_HOME} --no-create-home \
		--shell /bin/bash \
		--disabled-password \
		--uid ${JENKINS_UID} \
		--ingroup jenkins \
		--quiet \
		jenkins

# Jenkins home directoy is a volume, so configuration and build history
# can be persisted and survive image upgrades
VOLUME ${JENKINS_HOME}

EXPOSE ${JENKINS_PORT_22_TCP_PORT}
CMD ["/usr/sbin/sshd", "-D"]
