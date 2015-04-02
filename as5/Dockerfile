FROM tutum/jboss:as5
MAINTAINER info.inspectit@novatec-gmbh.de

#set Workdir
WORKDIR /opt

#set insepctit env
ENV INSPECTIT_VERSION 1.6.2.65
ENV INSPECTIT_AGENT_HOME /opt/agent
ENV INSPECTIT_CONFIG_HOME /opt/agent/active-config

# get inspectit binary
# set inspectit jvm options
RUN wget ftp://ftp.novatec-gmbh.de/inspectit/releases/RELEASE.${INSPECTIT_VERSION}/inspectit-agent-sun1.5.zip \
&& unzip inspectit-agent-sun1.5.zip \
&& rm -f inspectit-agent-sun1.5.zip \
&& sed -i '$ a\'"export INSPECTIT_JAVA_OPTS=\"-javaagent:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -Dinspectit.config=${INSPECTIT_CONFIG_HOME}\"" /jboss-5.1.0.GA/bin/run.conf \ 
&& sed -i '$ a\'"export JAVA_OPTS=\"\${INSPECTIT_JAVA_OPTS} \${JAVA_OPTS}\"" /jboss-5.1.0.GA/bin/run.conf

#copy start script
COPY run-with-inspectit.sh /run-with-inspectit.sh

#define VOLUME for active agent config
VOLUME ["/opt/agent/active-config"]

# define default command
CMD ["/run-with-inspectit.sh"]
