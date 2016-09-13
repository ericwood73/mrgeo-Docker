# MrGeo Docker Image that contains all runtime dependencies needed to run tests.
FROM cloudera/quickstart

WORKDIR /home/mrgeo/mrgeo

COPY core-site-local.xml /etc/hadoop/conf.pseudo/core-site.xml

# Need Maven so we can build and run unit and integration tests
ENV MAVEN_VERSION="3.3.9" \
    M2_HOME=/usr/share/maven \
    MAVEN_CONFIG=/root/.m2

RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz \
    | tar -xzC /usr/share/maven --strip-components=1 \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

# Add the EPEL and PostgreSQL repos so we can get GDAL
RUN rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm \
    && rpm -Uvh http://yum.postgresql.org/9.1/redhat/rhel-6.3-x86_64/pgdg-centos91-9.1-4.noarch.rpm \
    && rpm -Uvh http://yum.postgresql.org/9.3/redhat/rhel-6-x86_64/pgdg-centos93-9.3-1.noarch.rpm

# Now install the GDAL framework, libraries and java bindings with yum
RUN sudo yum install -y gdal-1.9.2 gdal-devel-1.9.2 gdal-java-1.9.2

# Add SPARK_HOME environment
ENV SPARK_HOME /usr/lib/spark
# Add SPARK to python path environment.  This puts py4j in the python path as well
ENV PYTHONPATH $SPARK_HOME/python/:$SPARK_HOME/python/lib/py4j-0.9-src.zip:$PYTHONPATH
ENV MRGEO_COMMON_HOME /home/mrgeo/mrgeo
