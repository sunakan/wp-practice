ARG DOCKER_AMAZONLINUX_TAG
FROM amazonlinux:${DOCKER_AMAZONLINUX_TAG}

RUN yum update --assumeyes
RUN yum install --assumeyes httpd httpd-devel httpd-tools
RUN amazon-linux-extras install php7.2
