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
ARG gitcommand
ARG knoxversion

RUN $gitcommand

# build
# RUN cd knox && ant clean release

# Skipping tests here for faster turnaround.
RUN cd knox && mvn clean -Ppackage,release install -DskipTests && tar -xvzf target/$knoxversion/*.tar.gz

# move runtime to new location
RUN mkdir /knox-runtime
RUN mv /knox/knox-$knoxversion/* /knox-runtime

# delete build artifacts
RUN rm -rf /knox

ADD master /knox-runtime/data/security/master
# Enable websockets
ADD gateway-site.xml /knox-runtime/conf/gateway-site.xml

RUN chown -R gateway /knox-runtime/

# Cleanup 
RUN rm -rf /home/gateway/.m2/repository

ADD ldap.sh /ldap.sh
ADD gateway.sh /gateway.sh

RUN chmod +x /ldap.sh
RUN chmod +x /gateway.sh

