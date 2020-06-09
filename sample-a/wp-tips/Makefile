################################################################################
# Apache: バージョン確認
# $ make apache-version
################################################################################
.PHONY: apache-version
apache-version:
	httpd -v

################################################################################
# Apache: 起動（でも使わない）
# $ make start
################################################################################
.PHONY: start
start:
	apachectl start

################################################################################
# Apache: DocumentRoot Apache起動後、この下のやつを見る
# $ make document-root
################################################################################
.PHONY: document-root
document-root:
	grep --recursive --line-number --extended-regexp '^DocumentRoot' /etc/httpd/conf/httpd.conf

################################################################################
# Apache: Log周り
# $ make log-path
################################################################################
.PHONY: log-path
log-path:
	grep --recursive --line-number --extended-regexp '^ErrorLog' /etc/httpd/
	grep --recursive --line-number --extended-regexp '  CustomLog' /etc/httpd/

################################################################################
# WordPress: バージョン確認
# $ make wp-version
################################################################################
.PHONY: wp-version
wp-version:
	grep --recursive --line-number --with-filename 'wp_version = ' /var/www/html/**/*/version.php

################################################################################
# WordPress: LogのON/OFFの確認、true/false
# $ make wp-logging
################################################################################
.PHONY: wp-logging
wp-logging:
	grep --recursive --line-number --with-filename --extended-regexp "'WP_DEBUG_LOG'" /var/www/html/ | grep define\(

################################################################################
# WordPress: Logのpath
# $ make wp-log-path
################################################################################
.PHONY: wp-log-path
wp-log-path:
	grep --recursive --line-number --with-filename "log_path = " /var/www/html/
	grep --recursive --line-number --with-filename "'WP_CONTENT_DIR'" /var/www/html/ | grep define\(
