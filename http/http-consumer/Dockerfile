FROM registry.access.redhat.com/ubi8/ubi-minimal:latest

USER root

RUN microdnf update \
    && microdnf install java-11-openjdk-headless shadow-utils \
    && microdnf clean all

# Set JAVA_HOME env var
ENV JAVA_HOME /usr/lib/jvm/jre-11

# Add strimzi user with UID 1001
# The user is in the group 0 to have access to the mounted volumes and storage
RUN useradd -r -m -u 1001 -g 0 strimzi

ARG version=latest
ENV VERSION ${version}

COPY ./scripts/ /bin
COPY ./src/resources/log4j2.properties /bin/log4j2.properties

ADD target/test-client-http-consumer.jar /

USER 1001

CMD ["/bin/run.sh", "/test-client-http-consumer.jar"]