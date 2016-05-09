FROM centos:latest

# Set the WILDFLY_VERSION env variable
ENV WILDFLY_VERSION 10.0.0.Final
ENV WILDFLY_SHA1 c0dd7552c5207b0d116a9c25eb94d10b4f375549
ENV JBOSS_HOME /opt/jboss/wildfly/10.0.0

# Add the WildFly distribution to /opt, and make wildfly the owner of the extracted tar content
# Make sure the distribution is available from a well-known place

RUN cd $HOME && \
    yum install tar jdk zip unzip wget curl -y && \
    curl -O https://download.jboss.org/wildfly/$WILDFLY_VERSION/wildfly-$WILDFLY_VERSION.tar.gz && \
    sha1sum wildfly-$WILDFLY_VERSION.tar.gz | grep $WILDFLY_SHA1 && \
    tar zxvf wildfly-$WILDFLY_VERSION.tar.gz && \
    mkdir -p $JBOSS_HOME && \
    mv $HOME/wildfly-$WILDFLY_VERSION/* $JBOSS_HOME && \
    rm wildfly-$WILDFLY_VERSION.tar.gz && \
    wget "http://downloads.jboss.org/apiman/1.2.3.Final/apiman-distro-wildfly10-1.2.3.Final-overlay.zip" && \
    mv apiman-distro-wildfly10-1.2.3.Final-overlay.zip apiman-distro-overlay.zip && \
    unzip -o apiman-distro-overlay.zip -d $JBOSS_HOME && \
    wget "http://downloads.jboss.org/keycloak/1.9.4.Final/keycloak-1.9.4.Final.tar.gz" && \
    mv keycloak-1.9.4.Final.tar.gz keycloak-distro-overlay.tar.gz && \
    tar zxvf keycloak-distro-overlay.tar.gz $JBOSS_HOME/.
RUN $JBOSS_HOME/bin/add-user.sh admin P@ssw0rd10 --silent
ADD mongojdbc1.2.jar $JBOSS_HOME/bin/standalone/deployments
    
# Ensure signals are forwarded to the JVM process correctly for graceful shutdown
ENV LAUNCH_JBOSS_IN_BACKGROUND true

# Expose the ports we're interested in
EXPOSE 8080 9990

#: For systemd usage this changes to /usr/sbin/init
# Keeping it as /bin/bash for compatability with previous
CMD ["/opt/jboss/wildfly/10.0.0/bin/standalone.sh", "-b", "0.0.0.0"]
RUN /opt/jboss/wildfly/10.0.0/bin/jboss-cli.sh --connect --controller=localhost:9990 &&\
    data-source add --name=mongoDB --jndi-name=java:/mongoDB --driver-name=mongodb.jdbc.MongoDriver --connectionurl=jdbc:mongo://mongodb.jarvis-sit.svc/jarvisoak --user-name=oak --password=password

