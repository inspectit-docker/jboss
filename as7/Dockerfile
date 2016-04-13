FROM tutum/jboss:latest
MAINTAINER info.inspectit@novatec-gmbh.de

#set Workdir
WORKDIR /opt

#set insepctit env
ENV INSPECTIT_VERSION 1.6.7.79
ENV INSPECTIT_AGENT_HOME /opt/agent
ENV INSPECTIT_CONFIG_HOME /opt/agent/active-config

# update, install unzip, get inspectit binary
# set inspectit jvm options
RUN apt-get update \
&& apt-get install -y unzip \
&& wget --no-check-certificate https://github.com/inspectIT/inspectIT/releases/download/${INSPECTIT_VERSION}/inspectit-agent-sun1.5.zip \
&& unzip inspectit-agent-sun1.5.zip \
&& apt-get clean \
&& rm -f inspectit-agent-sun1.5.zip \
&& sed -i '6i\'"JBOSS_MODULES_SYSTEM_PKGS=\"org.jboss.logmanager\"" /jboss-as-7.1.1.Final/bin/standalone.conf \
&& sed -i '$ a\'"JBOSS_HOME=\"/jboss-as-7.1.1.Final\"" /jboss-as-7.1.1.Final/bin/standalone.conf \
&& sed -i '$ a\'"JAVA_OPTS=\"\$JAVA_OPTS -Djava.util.logging.manager=org.jboss.logmanager.LogManager -Xbootclasspath/p:\$JBOSS_HOME/modules/org/jboss/logmanager/main/jboss-logmanager-1.2.2.GA.jar -Xbootclasspath/p:\$JBOSS_HOME/modules/org/jboss/logmanager/log4j/main/jboss-logmanager-log4j-1.0.0.GA.jar -Xbootclasspath/p:\$JBOSS_HOME/modules/org/apache/log4j/main/log4j-1.2.16.jar\"" /jboss-as-7.1.1.Final/bin/standalone.conf \
&& sed -i '$ a\'"INSPECTIT_JAVA_OPTS=\"-Xbootclasspath/p:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -javaagent:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -Dinspectit.config=${INSPECTIT_CONFIG_HOME}\"" /jboss-as-7.1.1.Final/bin/standalone.conf \
&& sed -i '$ a\'"JAVA_OPTS=\"\${INSPECTIT_JAVA_OPTS} \${JAVA_OPTS}\"" /jboss-as-7.1.1.Final/bin/standalone.conf

#copy start script
COPY run-with-inspectit.sh /run-with-inspectit.sh

# define volume
VOLUME /opt/agent/active-config

# define default command
CMD ["/run-with-inspectit.sh"]
