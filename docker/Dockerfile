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
FROM fabric8/java-alpine-openjdk11-jre
COPY target/*-bootable.jar /deployments/app.jar
RUN export JAVA_OPTIONS="$JAVA_OPTIONS -Xmx128m -Xms512m"
ENTRYPOINT [ "/deployments/run-java.sh" ]