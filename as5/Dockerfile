FROM tutum/jboss:as5
MAINTAINER info.inspectit@novatec-gmbh.de

#set Workdir
WORKDIR /opt

#set insepctit env
ENV INSPECTIT_VERSION 1.6.2.65
ENV INSPECTIT_AGENT_HOME /opt/agent
ENV INSPECTIT_CONFIG_HOME /opt/agent/config

# get inspectit binary
# set inspectit jvm options
RUN wget ftp://ftp.novatec-gmbh.de/inspectit/releases/RELEASE.${INSPECTIT_VERSION}/inspectit-agent-sun1.5.zip \
&& unzip inspectit-agent-sun1.5.zip \
&& rm -f inspectit-agent-sun1.5.zip \
&& sed -i '9i\'"export INSPECTIT_JAVA_OPTS=\"-javaagent:${INSPECTIT_AGENT_HOME}/inspectit-agent.jar -Dinspectit.config=${INSPECTIT_CONFIG_HOME}\"" /jboss-5.1.0.GA/bin/run.sh \ 
&& sed -i '10i\'"export JAVA_OPTS=\"\${INSPECTIT_JAVA_OPTS} \${JAVA_OPTS}\"" /jboss-5.1.0.GA/bin/run.sh

# define volume
VOLUME /opt/agent/config

# define default command
CMD ["/run.sh"]