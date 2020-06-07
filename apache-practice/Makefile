################################################################################
# Docker
################################################################################
DOCKER_AMAZONLINUX_TAG := 2
DOCKER_REPOSITORY_NAME := my-apache
.PHONY: build
build:
	docker build . \
		--build-arg DOCKER_AMAZONLINUX_TAG=$(DOCKER_AMAZONLINUX_TAG) \
		--tag $(DOCKER_REPOSITORY_NAME):latest
.PHONY: up
up:
	docker run \
		--rm \
		--interactive \
		--init \
		--hostname my-apache \
		--name my-apache \
		--tty \
		--publish 80:80 \
		--mount type=bind,source=$(PWD)/,target=/usr/local/app/ \
		--mount type=bind,source=$(PWD)/httpd.conf,target=/etc/httpd/conf/httpd.conf,readonly \
		--mount type=bind,source=$(PWD)/html/,target=/var/www/html/,readonly \
		--mount type=bind,source=$(PWD)/cgi-bin/,target=/var/www/cgi-bin/ \
		--mount type=bind,source=$(PWD)/php.conf,target=/etc/httpd/conf.d/php.conf,readonly \
		--mount type=bind,source=$(PWD)/php.ini,target=/etc/php.ini,readonly \
		--workdir /usr/local/app/ \
		$(DOCKER_REPOSITORY_NAME):latest

.PHONY: clean
clean:
	docker container ls --quiet --filter 'Name=my-apache' | xargs -I {container-id} docker container stop {container-id}
.PHONY: curl-sample
curl-sample:
	curl --include http://localhost/sample.html
curl-cgi:
	curl --include http://localhost/cgi-bin/test.cgi
curl-php:
	curl --include http://localhost/test.php

################################################################################
# Apache (ApacheのTips的な奴)
################################################################################
.PHONY: v
v:
	httpd -v
.PHONY: start
start:
	apachectl start
.PHONY: stop
stop:
	apachectl stop
.PHONY: status
status:
	apachectl status
# 設定ファイルのシンタックスチェック
.PHONY: check
check:
	apachectl configtest
lookup-httpd:
	tree -F -L 1 /etc/httpd/
.PHONY: lookup-conf
lookup-conf:
	tree -F -L 1 /etc/httpd/conf/
.PHONY: lookup-conf.d
lookup-conf.d:
	tree -F -L 1 /etc/httpd/conf.d/
.PHONY: confirm-include-conf
confirm-include-conf:
	grep --recursive --line-number 'IncludeOptional' /etc/httpd/
# Web管理者のメールアドレス
.PHONY: server-admin
server-admin:
	grep --recursive --line-number 'ServerAdmin' /etc/httpd/
.PHONY: document-root
document-root:
	grep --recursive --line-number DocumentRoot /etc/httpd/
.PHONY: log-path
log-path:
	grep --recursive --line-number --extended-regexp '(ErrorLog|CustomLog)' /etc/httpd/
.PHONY: cgi-bin
cgi-bin:
	grep --recursive --line-number 'ScriptAlias /cgi-bin/' /etc/httpd/
php-module:
	grep --recursive --line-number 'LoadModule php' /etc/httpd/
################################################################################
# PHP (PHPのTips的な奴)
################################################################################
php-memory-limit:
	grep --recursive --line-number 'memory_limit' /etc/php.ini
