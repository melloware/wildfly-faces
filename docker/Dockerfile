# This Dockerfile is used in order to build a container that runs the Wildfly Bootable application in JVM mode
#
# Before building the docker image run:
#
# mvn package
#
# Then, build the image with:
#
# docker build -f docker/Dockerfile -t melloware/wildfly-faces .
#
# Then run the container using:
#
# docker run -i --rm -p 8080:8080 melloware/wildfly-faces
#
FROM registry.access.redhat.com/ubi8/openjdk-11:latest
COPY --chown=jboss:root target/*.jar /deployments/.
EXPOSE 8080
USER 185
ENV AB_JOLOKIA_OFF=""
ENV JAVA_OPTS="-Djboss.bind.address=0.0.0.0 -Djboss.bind.address.management=0.0.0.0"
RUN chmod -R ug+rwX /deployments/.