# JBoss with inspectIT
JBoss Dockerfile including inspectIT

This docker image is based on the tutum/JBoss docker image including the inspectIT agent of the open source APM solution [www.inspectit.eu](http://www.inspectit.eu).
This image can be used easily as a replacement for the JBoss image, meaning you only have to change your existing Dockerfile ```FROM xyz/jboss:5``` to ```FROM inspectit/jboss:5```.

## Quickstart
First you need a running inspectIT CMR. You can use our docker image:

```bash
$ docker run -d --name inspectIT-CMR -p 8182:8182 -p 9070:9070 inspectit/cmr
```

Now you can start a container with the following command:

```bash
$ docker run -d --link inspectIT-CMR:cmr -p 8080:8080 -p 9990:9990 -v $(pwd)/config:/opt/agent/active-config inspectit/jboss:5
```

You can now adjust the instrumentation configuration in the folder *config* for your needs. Please refer to our [documentation](https://documentation.novatec-gmbh.de/display/INSPECTIT/Agent+Configuration) or just leave a comment.

##Usage
To get the admin password of your new container, check the logs of the container by running:

    docker logs <CONTAINER_ID>

You should see something like the following: 

    => Configuring admin user with a random password in JBoss
    => Done!
    ========================================================================
    You can now configure to this JBoss server using:
    
        admin:yDY4ZnRXw1Gy
    
    ========================================================================

## Configuration
### Agent name
By default, the inspectIT agent uses the hostname (docker-ID) as agent name. You can set a different name setting ```AGENT_NAME``` or hostname:

```bash
$ docker run -d --link inspectIT-CMR:cmr -p 8080:8080 -p 9990:9990 -v $(pwd)/config:/opt/agent/active-config -e AGENT_NAME=<agent-name> inspectit/jboss:5
```

```bash
$ docker run -d --link inspectIT-CMR:cmr -h <agent-name> -p 8080:8080 -p 9990:9990 -v $(pwd)/config:/opt/agent/active-config inspectit/jboss:5
```

### JBoss password
If you want to use your own password for the JBoss application server, then you can set a specific password setting ```JBOSS_PASS```
```bash
$ docker run -d --link inspectIT-CMR:cmr -h <agent-name> -p 8080:8080 -p 9990:9990 -v $(pwd)/config:/opt/agent/active-config -e JBOSS_PASS="<jboss-password>" inspectit/jboss:5
```

### Using a custom inspectIT CMR
If you don't want to use the inspectIT CMR docker image or cannot link to it, you can set the IP address and port manually:

```bash
$ docker run -d -e INSPECTIT_CMR_ADDR=<cmr-ip-address> -e INSPECTIT_CMR_PORT=<cmr-port> -p 8080:8080 -p 9990:9990 inspectit/jboss:5
```

## Specifying the JBoss version
Currently, this image is based on the JBoss 5 image. Support for different version 6 and 7 is also available.

## Specifying the inspectIT version
Currently, this image is based on the latest beta inspectIT release. Later, support for other versions is added.

## Build the docker image
If you want to build the Tomcat inspectIT image yourself, checkout this repository and run 

```bash
$ docker build -t inspectit/jboss:5 .
```
