ARG DOCKER_AMAZONLINUX_TAG
FROM amazonlinux:${DOCKER_AMAZONLINUX_TAG}

RUN yum update --assumeyes
RUN yum install --assumeyes httpd httpd-devel httpd-tools
RUN yum install --assumeyes make
RUN yum install --assumeyes tree
RUN yum install --assumeyes bash-completion
RUN yum install --assumeyes php
COPY --chown=apache:apache ./entrypoint.sh /
RUN chmod +x /entrypoint.sh
# USERはrootのままでいいかもしれない(httpd.confにUserがapacheであればOK..?)
#USER apache
CMD ["/entrypoint.sh"]
