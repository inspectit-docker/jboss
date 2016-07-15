FROM tutum/jboss:as6
MAINTAINER info.inspectit@novatec-gmbh.de

#set Workdir
WORKDIR /opt

#set insepctit env
ENV INSPECTIT_VERSION 1.6.9.83
ENV INSPECTIT_AGENT_HOME /opt/agent
ENV INSPECTIT_CONFIG_HOME /opt/agent/active-config

# get inspectit binary
# set inspectit jvm options
RUN wget --no-check-certificate https://github.com/inspectIT/inspectIT/releases/download/${INSPECTIT_VERSION}/inspectit-agent-sun1.5.zip \
&& unzip inspectit-agent-sun1.5.zip \
&& rm -f inspectit-agent-sun1.5.zip \
&& sed -i '9i\'"JBOSS_MODULES_SYSTEM_PKGS=\"org.jboss.logmanager\"" /jboss-6.1.0.Final/bin/run.sh \
&& sed -i '10i\'"JBOSS_HOME=\"/jboss-6.1.0.Final\"" /jboss-6.1.0.Final/bin/run.sh \
&& sed -i '$ a\'"JAVA_OPTS=\"\$JAVA_OPTS -Djava.util.logging.manager=org.jboss.logmanager.LogManager -Xbootclasspath/p:\${JBOSS_HOME}/lib/jboss-logmanager.jar -Xbootclasspath/p:\${JBOSS_HOME}/common/lib/jboss-logmanager-log4j.jar -Xbootclasspath/p:\${JBOSS_HOME}/common/lib/log4j.jar\"" /jboss-6.1.0.Final/bin/run.conf \
&& sed -i '$ a\'"export INSPECTIT_JAVA_OPTS=\"-javaagent:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -Dinspectit.config=${INSPECTIT_CONFIG_HOME}\"" /jboss-6.1.0.Final/bin/run.conf \
&& sed -i '$ a\'"export JAVA_OPTS=\"\${INSPECTIT_JAVA_OPTS} \${JAVA_OPTS}\"" /jboss-6.1.0.Final/bin/run.conf

#copy start script
COPY run-with-inspectit.sh /run-with-inspectit.sh

# define volume
VOLUME /opt/agent/active-config

# define default command
CMD ["/run-with-inspectit.sh"]
