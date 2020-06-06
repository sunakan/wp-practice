DOCKER_AMAZONLINUX_TAG := 2
DOCKER_REPOSITORY_NAME := my-wp1

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
.PHONY: build
build:
	docker build . \
		--build-arg DOCKER_AMAZONLINUX_TAG=$(DOCKER_AMAZONLINUX_TAG) \
		--tag $(DOCKER_REPOSITORY_NAME):dev-latest
.PHONY: bash
bash:
	docker run \
		--rm \
		--interactive \
		--tty \
		$(DOCKER_REPOSITORY_NAME):dev-latest bash
