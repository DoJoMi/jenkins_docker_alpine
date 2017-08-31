FROM jenkins:alpine
MAINTAINER dojomi

# change user and install some packages
USER root

RUN apk update
RUN apk --no-cache add \
    gnupg \
    git \
    curl \
    wget \
    bash \
    ruby \
    go \
    sudo \
    fontconfig \
    ttf-dejavu \
    tar \
    docker \
    openrc \
    shadow \
&& rm -rf /var/lib/apt/lists/*

# docker in docker
# check permission on host system ls -lha /var/run/docker.sock 
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers 
RUN rc-update add docker boot  
RUN usermod -aG docker jenkins

# change back to user jenkins
USER jenkins

# Adding plugin
COPY config/plugins.txt /usr/share/jenkins/ref/plugins.txt 
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt
# Adding default jenkins job
COPY jobs/Test-Build.xml /usr/share/jenkins/ref/jobs/Test-Build/config.xml

# Jenkins settings
COPY config/config.xml /usr/share/jenkins/ref/config.xml

# SSH Keys & Credentials which should be added later on to github for auth your project
COPY config/credentials.xml /usr/share/jenkins/ref/credentials.xml
COPY config/ssh_keys/jenkins /usr/share/jenkins/ref/.ssh/id_rsa
COPY config/ssh_keys/jenkins.pub /usr/share/jenkins/ref/.ssh/id_rsa.pub

# tell Jenkins that no banner prompt for pipeline plugins is needed
RUN echo 2.0 > /usr/share/jenkins/ref/jenkins.install.UpgradeWizard.state
