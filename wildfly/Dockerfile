FROM jboss/wildfly:8.1.0.Final
MAINTAINER info.inspectit@novatec-gmbh.de


# User root user to install software
USER root

# Install necessary packages
RUN yum -y install wget \
 && yum clean all

# User jboss to install/configure agent
USER jboss

#set Workdir
WORKDIR /opt/jboss

#set insepctit env
ENV INSPECTIT_VERSION 1.6.9.83
ENV INSPECTIT_AGENT_HOME /opt/jboss/agent
ENV INSPECTIT_CONFIG_HOME /opt/jboss/agent/active-config
ENV JBOSS_LOGMANAGER_VERSION 1.5.2.Final

# get inspectit binary
# set inspectit jvm options
RUN mkdir -p ${INSPECTIT_AGENT_HOME} ${INSPECTIT_CONFIG_HOME} \
 && wget --no-check-certificate https://github.com/inspectIT/inspectIT/releases/download/${INSPECTIT_VERSION}/inspectit-agent-sun1.5.zip \
 && unzip inspectit-agent-sun1.5.zip \
 && rm -f inspectit-agent-sun1.5.zip \
 && sed -i '6i\'"JBOSS_MODULES_SYSTEM_PKGS=\"org.jboss.logmanager\"" /opt/jboss/wildfly/bin/standalone.conf \
 && sed -i '$ a\'"JBOSS_HOME=\"/opt/jboss/wildfly\"" /opt/jboss/wildfly/bin/standalone.conf \
 && sed -i '$ a\'"JAVA_OPTS=\"\${JAVA_OPTS} -Djava.util.logging.manager=org.jboss.logmanager.LogManager -Xbootclasspath/p:\${JBOSS_HOME}/modules/system/layers/base/org/jboss/logmanager/main/jboss-logmanager-\${JBOSS_LOGMANAGER_VERSION}.jar\"" /opt/jboss/wildfly/bin/standalone.conf \
 && sed -i '$ a\'"INSPECTIT_JAVA_OPTS=\"-Xbootclasspath/p:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -javaagent:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -Dinspectit.config=${INSPECTIT_CONFIG_HOME}\"" /opt/jboss/wildfly/bin/standalone.conf \
 && sed -i '$ a\'"JAVA_OPTS=\"\${INSPECTIT_JAVA_OPTS} \${JAVA_OPTS}\"" /opt/jboss/wildfly/bin/standalone.conf

#copy start script
COPY run-with-inspectit.sh /run-with-inspectit.sh

#define VOLUME for active agent config
VOLUME ["/opt/jboss/agent/active-config"]

# define default command
CMD ["/run-with-inspectit.sh"]
