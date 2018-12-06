FROM tutum/jboss:as7
MAINTAINER info.inspectit@novatec-gmbh.de

# set Workdir
WORKDIR /opt

# set insepctit env
ENV INSPECTIT_VERSION 1.9.3.107
ENV INSPECTIT_AGENT_HOME /opt/agent

# update, install unzip, get inspectit binary
# set inspectit jvm options
RUN apt-get update \
 && apt-get install -y unzip \
 && apt-get clean \
 && wget --no-check-certificate https://github.com/inspectIT/inspectIT/releases/download/${INSPECTIT_VERSION}/inspectit-agent-java6-${INSPECTIT_VERSION}.zip \
 && unzip inspectit-agent-java6-${INSPECTIT_VERSION}.zip \
 && rm -f inspectit-agent-java6-${INSPECTIT_VERSION}.zip \
 && sed -i '6i\'"JBOSS_MODULES_SYSTEM_PKGS=\"org.jboss.logmanager\"" /jboss-as-7.1.1.Final/bin/standalone.conf \
 && sed -i '$ a\'"JBOSS_HOME=\"/jboss-as-7.1.1.Final\"" /jboss-as-7.1.1.Final/bin/standalone.conf \
 && sed -i '$ a\'"JAVA_OPTS=\"\$JAVA_OPTS -Djava.util.logging.manager=org.jboss.logmanager.LogManager -Xbootclasspath/p:\$JBOSS_HOME/modules/org/jboss/logmanager/main/jboss-logmanager-1.2.2.GA.jar -Xbootclasspath/p:\$JBOSS_HOME/modules/org/jboss/logmanager/log4j/main/jboss-logmanager-log4j-1.0.0.GA.jar -Xbootclasspath/p:\$JBOSS_HOME/modules/org/apache/log4j/main/log4j-1.2.16.jar\"" /jboss-as-7.1.1.Final/bin/standalone.conf \
 && sed -i '$ a\'"INSPECTIT_JAVA_OPTS=\"-javaagent:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -Dinspectit.repository=_CMR_ADDRESS_:_CMR_PORT_ -Dinspectit.agent.name=_AGENT_NAME_\"" /jboss-as-7.1.1.Final/bin/standalone.conf \
 && sed -i '$ a\'"JAVA_OPTS=\"\${INSPECTIT_JAVA_OPTS} \${JAVA_OPTS}\"" /jboss-as-7.1.1.Final/bin/standalone.conf

#copy start script
COPY run-with-inspectit.sh /run-with-inspectit.sh

# define default command
CMD ["/run-with-inspectit.sh"]
