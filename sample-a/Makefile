
################################################################################
# $ make setup-wordpress
# WordPressのDLとchecksumと解凍
################################################################################
TARGET_WORDPRESS := wordpress-5.2.6-ja.tar.gz
TARGET_WORDPRESS_SHA1 := 4dc204904d672b7d506694b36659b697fcf9f665
TARGET_WORDPRESS_MD5  := 63cf10bccb5f54de3cefdbabb4646718
.PHONY: download-wordpress
download-wordpress:
	ls $(TARGET_WORDPRESS) || curl -o $(TARGET_WORDPRESS) https://ja.wordpress.org/$(TARGET_WORDPRESS)
.PHONY: checksum-wordpress
checksum-wordpress:
	echo "$(TARGET_WORDPRESS_SHA1)  $(TARGET_WORDPRESS)" | sha1sum --check -
	echo "$(TARGET_WORDPRESS_MD5)  $(TARGET_WORDPRESS)" | md5sum --check -
.PHONY: unarchive-wordpress
unarchive-wordpress:
	ls ./wordpress || tar --gzip --extract --file $(TARGET_WORDPRESS)
.PHONY: setup-wordpress
setup-wordpress:
	make download-wordpress
	make checksum-wordpress
	make unarchive-wordpress

################################################################################
# Docker
################################################################################
DOCKER_AMAZONLINUX_TAG := 2
DOCKER_REPOSITORY_NAME := my-wp1
DOCKER_MYWP_HOST       := my-wp1
DOCKER_NETWORK_NAME    := my-network
DOCKER_MYSQL_TAG       := 5.7
DOCKER_MYSQL_HOST      := mysql
DOCKER_DBVOLUME_NAME   := mysql-volume
MYSQL_DATABASE         := hoge-db
MYSQL_USER             := hoge-user
MYSQL_PASSWORD         := hoge-pass
.PHONY: docker-network
docker-network:
	( docker network ls | grep $(DOCKER_NETWORK_NAME) ) \
		|| docker network create $(DOCKER_NETWORK_NAME)

################################################################################
# Docker Apache + PHP + WordPress
################################################################################
.PHONY: build
build:
	docker build . \
		--build-arg DOCKER_AMAZONLINUX_TAG=$(DOCKER_AMAZONLINUX_TAG) \
		--tag $(DOCKER_REPOSITORY_NAME):latest
.PHONY: bash
bash: docker-network
	docker run \
		--rm \
		--interactive \
		--tty \
		--network $(DOCKER_NETWORK_NAME) \
		--hostname $(DOCKER_MYWP_HOST) \
		--name $(DOCKER_MYWP_HOST) \
		--publish 80:80 \
		$(DOCKER_REPOSITORY_NAME):latest bash

################################################################################
# Docker MySQL
################################################################################
.PHONY: db-volume
db-volume:
	docker volume ls | grep $(DOCKER_VOLUME_NAME) \
		|| docker volume create $(DOCKER_VOLUME_NAME)
.PHONY: up-db
up-db: db-volume docker-network
	( docker container ls | grep $(DOCKER_MYSQL_HOST) ) \
		|| docker run \
			--detach \
			--rm \
			--interactive \
			--tty \
			--network $(DOCKER_NETWORK_NAME) \
			--hostname $(DOCKER_MYSQL_HOST) \
			--name $(DOCKER_MYSQL_HOST) \
			--publish 3306:3306 \
			--mount type=volume,source=$(DOCKER_VOLUME_NAME),target=/var/lib/mysql/ \
			--env MYSQL_ROOT_PASSWORD=root \
			--env MYSQL_DATABASE=$(MYSQL_DATABASE) \
			--env MYSQL_USER=$(MYSQL_USER) \
			--env MYSQL_PASSWORD=$(MYSQL_PASSWORD) \
			--env LANG=C.UTF-8 \
			--env TZ=Asia/Tokyo \
			mysql:$(DOCKER_MYSQL_TAG) \
				--character-set-server=utf8mb4 \
				--collation-server=utf8mb4_unicode_ci

.PHONY: clean
clean:
	docker container ls --quiet --filter 'Name=$(DOCKER_MYSQL_HOST)' | xargs -I {container-name} docker stop {container-name}
	docker container ls --quiet --filter 'Name=$(DOCKER_MYWP_HOST)' | xargs -I {container-name} docker stop {container-name}
	docker network rm $(DOCKER_NETWORK_NAME) || echo ok
