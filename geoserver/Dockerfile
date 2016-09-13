# A container for running a Geoserver instance with the MrGeoPlugin for reading MrGeo images and Pyramids.  Building this container requires the 
# MrGeo plugin distribution "gt-mrgeo-1.0-1.1.0-cdh5.6.0-SNAPSHOT.tar.gz" to be in the build context.  This distribution can be obtained by cloning and building the
# MrGeo Geoserver plugin project from https://github.com/ngageoint/mrgeo-geoserver-plugin

FROM tomcat:jre8-alpine

# Install Geoserver
RUN apk add --update wget && apk add --update unzip && \
    cd /tmp && \
    wget -q "http://downloads.sourceforge.net/project/geoserver/GeoServer/2.9.1/geoserver-2.9.1-war.zip?r=&ts=1470848131&use_mirror=heanet" -O geoserver-war.zip && \
    unzip -q /tmp/geoserver-war.zip -d /tmp && \
    unzip -q /tmp/geoserver.war -d $CATALINA_HOME/webapps/geoserver && \
    apk del wget && apk del unzip

# Setup MRGEO environment
ENV MRGEO_COMMON_HOME=/usr/local/mrgeo \
    MRGEO_CONF_DIR=/usr/local/mrgeo/conf \
    HADOOP_CONF_DIR=/usr/local/hadoop/conf 

# For now, just force creation of some mrgeo directories locally to see if we can get the plugin working. Later, we'll send this stuff in at runtime via a -v
RUN mkdir -p /mrgeo/images

# Install the plugin
ADD gt-mrgeo-1.0-1.1.0-cdh5.6.0-SNAPSHOT.tar.gz webapps/geoserver/WEB-INF/lib

# Configure Hadoop client
#COPY core-site-local.xml /usr/local/hadoop/conf/

# Clean up
RUN rm -fR /tmp/* /var/cache/apk/*