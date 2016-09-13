# MrGeo Docker Image that contains the dependencies installed in the maven repo needed to build and run tests.
# This allows the maven repo to be stored and then used as a volume in other containers to reduce the need
# to download dependencies each time the parent container is built. In other words, it allows changes to
# the parent repo without requiring a time consuming download of all maven dependencies. This image only
# needs to be rebuilt when changes are made to the MrGeo pom files.

# Needs to be built from the parent of the MrGeo source and this file's directory
# using the -f option to identify the Dockerfile path in order to correctly install pom files
FROM anapsix/alpine-java:jdk7

WORKDIR /home/mrgeo/mrgeo

# Need Maven so we can get dependencies
ENV MAVEN_VERSION="3.3.9" \
    M2_HOME=/usr/lib/mvn \
    MAVEN_CONFIG=/root/.m2

RUN apk add --update wget && \
    cd /tmp && \
    wget "http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz" && \
    tar -zxvf "apache-maven-$MAVEN_VERSION-bin.tar.gz" && \
    mv "apache-maven-$MAVEN_VERSION" "$M2_HOME" && \
    ln -s "$M2_HOME/bin/mvn" /usr/bin/mvn && \
    apk del wget && \
    rm /tmp/* /var/cache/apk/*

# Have Maven download all of it‘s dependencies in build to minimize the number
# of dependencies downloaded when the image is run
# see https://keyholesoftware.com/2015/01/05/caching-for-maven-docker-builds/
# Selectively add the POM
COPY mrgeo/pom.xml /home/mrgeo/mrgeo/
RUN cd /home/mrgeo/mrgeo
RUN mvn verify clean --fail-never

# Add the source
COPY mrgeo /home/mrgeo/mrgeo/

# Run a build to download most of the remaining (goal-dependent) dependencies
RUN ./clean && ./build cdh570 -Dskip-all-tests=true -Dfindbugs.skip=true