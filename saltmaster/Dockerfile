FROM ubuntu:18.04
LABEL maintainer="zerocool.jothi@gmail.com"

# Set shell option -o before running shell commands with pipes in them
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Configure TZdata
RUN ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime

# Installation of the latest salt
RUN sed -i 's/archive.ubuntu/nl.archive.ubuntu/' /etc/apt/sources.list
# hadolint ignore=DL3009
RUN apt-get update
# hadolint ignore=DL3009
RUN apt-get install -y --no-install-recommends wget curl software-properties-common vim sudo net-tools iputils-ping netcat gpg-agent python3-distutils
RUN wget -O - https://repo.saltstack.com/py3/ubuntu/18.04/amd64/latest/SALTSTACK-GPG-KEY.pub | apt-key add -
RUN echo 'deb http://repo.saltstack.com/py3/ubuntu/18.04/amd64/latest bionic main' > /etc/apt/sources.list.d/saltstack.list
# hadolint ignore=DL3009
RUN apt-get update

# Install the salt master
RUN apt-get install -y --no-install-recommends salt-master \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Mount /srv/salt and /srv/pillar
VOLUME ["/srv/salt","/srv/pillar"]

# Add run file
COPY run_saltmaster.sh /usr/local/bin/run_saltmaster.sh
RUN chmod +x /usr/local/bin/run_saltmaster.sh

# Ports to expose
EXPOSE 4505 4506

# Last command to run
CMD ["/usr/local/bin/run_saltmaster.sh"]
