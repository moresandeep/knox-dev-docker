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
ARG knoxurl
ARG branch

RUN git clone -b $branch $knoxurl knox

RUN mkdir /knox/knox-temp-artifacts
RUN mkdir /knox/knoxshell-temp-artifacts
# Skipping tests here for faster turnaround.
RUN cd knox && mvn clean -Ppackage,release install -DskipTests && tar -xvzf target/*/knox-*.tar.gz -C knox-temp-artifacts && tar -xvzf /knox/target/*/knoxshell-*.tar.gz -C knoxshell-temp-artifacts

# move runtime to new location
# See https://github.com/docker/compose/issues/4581#issuecomment-321386605
# for KnoxShell dance
RUN mkdir /knox-runtime
RUN mkdir /knoxshell
RUN mkdir /knox-runtime/knoxshell
RUN mv /knox/knox-temp-artifacts/*/* /knox-runtime
RUN mv /knox/knoxshell-temp-artifacts/*/* /knox-runtime/knoxshell

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

