FROM vicamo/jenkins-bare

# Boundled plugins in version 1.607:
#   $ docker exec vicamo/jenkins:bare ls -1 /var/lib/jenkins/plugins | grep jpi
#   ant.jpi
#   antisamy-markup-formatter.jpi
#   credentials.jpi
#   cvs.jpi
#   external-monitor-job.jpi
#   javadoc.jpi
#   junit.jpi
#   ldap.jpi
#   mailer.jpi
#   matrix-auth.jpi
#   matrix-project.jpi
#   maven-plugin.jpi
#   pam-auth.jpi
#   script-security.jpi
#   ssh-credentials.jpi
#   ssh-slaves.jpi
#   subversion.jpi
#   translation.jpi
#   windows-slaves.jpi
RUN cd /usr/share/jenkins/plugins \
	&& wget \
		http://updates.jenkins-ci.org/latest/analysis-collector.hpi \
		http://updates.jenkins-ci.org/latest/analysis-core.hpi \
		http://updates.jenkins-ci.org/latest/backup.hpi \
		http://updates.jenkins-ci.org/latest/batch-task.hpi \
		http://updates.jenkins-ci.org/latest/bazaar.hpi \
		http://updates.jenkins-ci.org/latest/build-with-parameters.hpi \
		http://updates.jenkins-ci.org/latest/checkstyle.hpi \
		http://updates.jenkins-ci.org/latest/conditional-buildstep.hpi \
		http://updates.jenkins-ci.org/latest/copy-to-slave.hpi \
		http://updates.jenkins-ci.org/latest/dashboard-view.hpi \
		http://updates.jenkins-ci.org/latest/disk-usage.hpi \
		http://updates.jenkins-ci.org/latest/docker-plugin.hpi \
		http://updates.jenkins-ci.org/latest/durable-task.hpi \
		http://updates.jenkins-ci.org/latest/dynamicparameter.hpi \
		http://updates.jenkins-ci.org/latest/findbugs.hpi \
		http://updates.jenkins-ci.org/latest/gerrit.hpi \
		http://updates.jenkins-ci.org/latest/git-client.hpi \
		http://updates.jenkins-ci.org/latest/git-server.hpi \
		http://updates.jenkins-ci.org/latest/git.hpi \
		http://updates.jenkins-ci.org/latest/github-api.hpi \
		http://updates.jenkins-ci.org/latest/github-oauth.hpi \
		http://updates.jenkins-ci.org/latest/github.hpi \
		http://updates.jenkins-ci.org/latest/instant-messaging.hpi \
		http://updates.jenkins-ci.org/latest/ircbot.hpi \
		http://updates.jenkins-ci.org/latest/mapdb-api.hpi \
		http://updates.jenkins-ci.org/latest/mercurial.hpi \
		http://updates.jenkins-ci.org/latest/openid.hpi \
		http://updates.jenkins-ci.org/latest/openid4java.hpi \
		http://updates.jenkins-ci.org/latest/parameterized-trigger.hpi \
		http://updates.jenkins-ci.org/latest/publish-over-ftp.hpi \
		http://updates.jenkins-ci.org/latest/publish-over-ssh.hpi \
		http://updates.jenkins-ci.org/latest/rebuild.hpi \
		http://updates.jenkins-ci.org/latest/repo.hpi \
		http://updates.jenkins-ci.org/latest/run-condition.hpi \
		http://updates.jenkins-ci.org/latest/scm-api.hpi \
		http://updates.jenkins-ci.org/latest/scriptler.hpi \
		http://updates.jenkins-ci.org/latest/startup-trigger-plugin.hpi \
		http://updates.jenkins-ci.org/latest/swarm.hpi \
		http://updates.jenkins-ci.org/latest/throttle-concurrents.hpi \
		http://updates.jenkins-ci.org/latest/token-macro.hpi \
		http://updates.jenkins-ci.org/latest/violations.hpi \
		http://updates.jenkins-ci.org/latest/ws-cleanup.hpi

RUN apt-get update && apt-get install -y --no-install-recommends \
		bzr \
		git \
	&& wget https://storage.googleapis.com/git-repo-downloads/repo -O /usr/bin/repo \
	&& chmod +x /usr/bin/repo \
	&& git config --global user.name "Jenkins Bot" \
	&& git config --global user.email "jenkins@localdomain"
