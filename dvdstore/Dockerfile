FROM inspectit/jboss:5
MAINTAINER info.inspectit@novatec-gmbh.de

RUN mkdir -p /database/database

COPY h2.jar /database/
RUN ln -s /database/h2.jar /jboss-5.1.0.GA/server/default/lib/.
COPY dvdstore22.h2.db /database/database/
COPY dvdstore22.trace.db /database/database/
COPY dvdstore.ear /jboss-5.1.0.GA/server/default/deploy/
COPY dvdstore-ds.xml /jboss-5.1.0.GA/server/default/deploy/

# define default command
CMD ["/run.sh"]
