FROM tutum/jboss:as6
MAINTAINER info.inspectit@novatec-gmbh.de

# set Workdir
WORKDIR /opt

# set insepctit env
ENV INSPECTIT_VERSION 1.8.2.96
ENV INSPECTIT_AGENT_HOME /opt/agent

# get inspectit binary
# set inspectit jvm options
RUN wget --no-check-certificate https://github.com/inspectIT/inspectIT/releases/download/${INSPECTIT_VERSION}/inspectit-agent-java6-${INSPECTIT_VERSION}.zip \
 && unzip inspectit-agent-java6-${INSPECTIT_VERSION}.zip \
 && rm -f inspectit-agent-java6-${INSPECTIT_VERSION}.zip \
 && sed -i '9i\'"JBOSS_MODULES_SYSTEM_PKGS=\"org.jboss.logmanager\"" /jboss-6.1.0.Final/bin/run.sh \
 && sed -i '10i\'"JBOSS_HOME=\"/jboss-6.1.0.Final\"" /jboss-6.1.0.Final/bin/run.sh \
 && sed -i '$ a\'"JAVA_OPTS=\"\$JAVA_OPTS -Djava.util.logging.manager=org.jboss.logmanager.LogManager -Xbootclasspath/p:\${JBOSS_HOME}/lib/jboss-logmanager.jar -Xbootclasspath/p:\${JBOSS_HOME}/common/lib jboss-logmanager-log4j.jar -Xbootclasspath/p:\${JBOSS_HOME}/common/lib/log4j.jar\"" /jboss-6.1.0.Final/bin/run.conf \
 && sed -i '$ a\'"export INSPECTIT_JAVA_OPTS=\"-javaagent:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -Dinspectit.repository=_CMR_ADDRESS_:_CMR_PORT_ -Dinspectit.agent.name=_AGENT_NAME_\"" /jboss-6.1.0.Final/bin/run.conf \
 && sed -i '$ a\'"export JAVA_OPTS=\"\${INSPECTIT_JAVA_OPTS} \${JAVA_OPTS}\"" /jboss-6.1.0.Final/bin/run.conf

# copy start script
COPY run-with-inspectit.sh /run-with-inspectit.sh

# define default command
CMD ["/run-with-inspectit.sh"]
