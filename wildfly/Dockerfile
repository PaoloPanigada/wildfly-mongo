FROM centos:latest
USER root

# Set the WILDFLY_VERSION env variable
ENV WILDFLY_VERSION 10.0.0.Final
ENV WILDFLY_SHA1 c0dd7552c5207b0d116a9c25eb94d10b4f375549
ENV JBOSS_HOME /wildfly

# Add the WildFly distribution to /opt, and make wildfly the owner of the extracted tar content
# Make sure the distribution is available from a well-known place

RUN cd $HOME && \
    yum install tar java jdk zip unzip wget curl -y && \
    curl -O https://download.jboss.org/wildfly/$WILDFLY_VERSION/wildfly-$WILDFLY_VERSION.tar.gz && \
    tar zxvf wildfly-$WILDFLY_VERSION.tar.gz && \
    mkdir -p $JBOSS_HOME && \
    mv $HOME/wildfly-$WILDFLY_VERSION/* $JBOSS_HOME && \
    rm wildfly-$WILDFLY_VERSION.tar.gz && \
    wget http://govrepolx01.siav.net/artifactory/arch-snapshot-local/it/siav/arch.showcase.fe.web/0.0.1-SNAPSHOT/arch.showcase.fe.web-0.0.1-20160509.143519-1.war && \
    mv arch.showcase.fe.web-0.0.1-20160509.143519-1.war $JBOSS_HOME/standalone/deployments && \
    wget http://www.java2s.com/Code/JarDownload/ojdbc6/ojdbc6.jar.zip && \
    unzip ojdbc6.jar.zip && \
    mv ojdbc6.jar $JBOSS_HOME/standalone/deployments && \
    cd $HOME
RUN $JBOSS_HOME/bin/add-user.sh admin P@ssw0rd10 --silent
ADD postgresql-9.4.1208.jar $JBOSS_HOME/standalone/deployments

# Ensure signals are forwarded to the JVM process correctly for graceful shutdown
ENV LAUNCH_JBOSS_IN_BACKGROUND true

# Expose the ports we're interested in
EXPOSE 8080 9990

#: For systemd usage this changes to /usr/sbin/init
# Keeping it as /bin/bash for compatability with previous
CMD ["/opt/jboss/wildfly/10.0.0/bin/standalone.sh", "-b", "0.0.0.0"]

