# This image provides a base for building and running WildFly applications.
# It builds using maven and runs the resulting artifacts on WildFly 10.1.0 Final

FROM websphere-liberty:webProfile7
USER root
MAINTAINER David Stanke <David.Stanke2@skoda-auto.cz>

LABEL io.openshift.s2i.scripts-url=image:///usr/local/s2i \
      io.s2i.scripts-url=image:///usr/local/s2i \
      io.k8s.description="Platform for building and running JEE applications on IBM WebSphere Liberty Profile" \
      io.k8s.display-name="Liberty 19.0.0.2" \
      io.openshift.expose-services="9080/tcp:http, 9443/tcp:https" \
      io.openshift.tags="runner, builder,liberty" \
      io.openshift.s2i.destination="/tmp"

ENV STI_SCRIPTS_PATH="/usr/local/s2i" \ 
    WORKDIR="/usr/local/workdir" \
    WLP_DEBUG_ADDRESS="7777" \
    JOLOKIA_PORT="8778" \
    ENABLE_DEBUG="true" \ 
    ENABLE_JOLOKIA="true" \
    S2I_DESTINATION="/tmp" \
    JOLOKIA_VERSION="1.3.5"

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH 


RUN mkdir -p $WORKDIR/artifacts && \
    mkdir -p $WORKDIR/config && \
    chown -R 1001:0 $WORKDIR && \
    chmod -R g+rw $WORKDIR && \
    ln $STI_SCRIPTS_PATH/assemble-runtime $STI_SCRIPTS_PATH/assemble    

COPY ./placeholder.txt $WORKDIR/artifacts 
COPY ./placeholder.txt $WORKDIR/config 

WORKDIR $WORKDIR

EXPOSE $WLP_DEBUG_ADDRESS
# EXPOSE $JOLOKIA_PORT

USER 1001
CMD ["$STI_SCRIPTS_PATH/run"]
