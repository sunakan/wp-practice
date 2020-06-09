ARG DOCKER_AMAZONLINUX_TAG
FROM amazonlinux:${DOCKER_AMAZONLINUX_TAG}
RUN yum update --assumeyes
RUN yum install --assumeyes httpd httpd-devel httpd-tools
RUN amazon-linux-extras install php7.2
RUN yum install --assumeyes yum-utils
RUN yum install --assumeyes https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
RUN yum-config-manager --disable mysql80-community
RUN yum-config-manager --enable mysql57-community
RUN yum install --assumeyes mysql-community-client
RUN yum install --assumeyes make
COPY --chown=apache:apache ./wordpress /var/www/html/_wp
COPY --chown=apache:apache ./entrypoint.sh /
RUN chmod +x /entrypoint.sh
CMD ["/entrypoint.sh"]
