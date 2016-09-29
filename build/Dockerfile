FROM ubuntu

MAINTAINER moresandeep

# Install dependencies
RUN apt-get update

# install Java 8
RUN apt-get install -y openjdk-8-jdk
RUN apt-get install -y openjdk-8-jre
RUN update-alternatives --config java
RUN update-alternatives --config javac
RUN apt-get install -y git
RUN apt-get install -y maven
RUN apt-get install -y ant

RUN useradd -ms /bin/bash gateway

# Clone our dev branch
RUN git clone -b KNOX-752_Websockets --single-branch https://github.com/moresandeep/knox.git knox

# build
# RUN cd knox && ant clean release

# Skipping tests here for faster turnaround.
RUN cd knox && mvn clean -Ppackage,release install -DskipTests && tar -xvzf target/0.10.0-SNAPSHOT/knox-0.10.0-SNAPSHOT.tar.gz

ADD master /knox/knox-0.10.0-SNAPSHOT/data/security/master
RUN chown -R gateway /knox/knox-0.10.0-SNAPSHOT/

ADD ldap.sh /ldap.sh
ADD gateway.sh /gateway.sh

RUN chmod +x /ldap.sh
RUN chmod +x /gateway.sh

USER gateway
WORKDIR /home/gateway
