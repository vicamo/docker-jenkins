FROM vicamo/jenkins-deps-bare

COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt

RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		bzr \
		git \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*_dists_* \
	&& wget -nv -O /usr/bin/repo \
		https://storage.googleapis.com/git-repo-downloads/repo \
	&& chmod +x /usr/bin/repo \
	&& git config --global user.name "Jenkins Bot" \
	&& git config --global user.email "jenkins@localdomain"
