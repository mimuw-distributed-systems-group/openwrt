#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

# Newer Debian releases ship only certdata.txt; the .crt files are
# generated from it with certdata2pem.py (host python3).
PKG_NAME:=ca-certificates
PKG_VERSION:=20260601
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)_$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=http://ftp.debian.org/debian/pool/main/c/ca-certificates
PKG_HASH:=7ab6301f7f34eef90a4d278647c260bc0762e0e14561f4649854cf4b0d4bea21

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

define Package/ca-certificates
  SECTION:=base
  CATEGORY:=Base system
  TITLE:=System CA certificates
endef

define Package/ca-bundle
  SECTION:=base
  CATEGORY:=Base system
  TITLE:=System CA certificates as a single bundle
endef

# The build container's python3-cryptography is too old for the
# certdata2pem.py call without a backend argument.
define Build/Compile
	sed -i \
		-e 's/from cryptography import x509/from cryptography import x509\nfrom cryptography.hazmat.backends import default_backend/' \
		-e "s/x509.load_der_x509_certificate(bytes(obj\['CKA_VALUE'\]))/x509.load_der_x509_certificate(bytes(obj['CKA_VALUE']), default_backend())/" \
		$(PKG_BUILD_DIR)/mozilla/certdata2pem.py
	$(MAKE) -C $(PKG_BUILD_DIR)/mozilla all
endef

define Build/Install
	mkdir -p $(PKG_INSTALL_DIR)/usr/share/ca-certificates/mozilla
	$(MAKE) -C $(PKG_BUILD_DIR)/mozilla install CERTSDIR=$(PKG_INSTALL_DIR)/usr/share/ca-certificates/mozilla
endef

define Package/ca-certificates/install
	$(INSTALL_DIR) $(1)/etc/ssl/certs
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/share/ca-certificates/*/*.crt $(1)/etc/ssl/certs/

	for CERTFILE in `ls -1 $(1)/etc/ssl/certs`; do \
		HASH=`openssl x509 -hash -noout -in $(1)/etc/ssl/certs/$$$$CERTFILE` ; \
		SUFFIX=0 ; \
		while [ -h "$(1)/etc/ssl/certs/$$$$HASH.$$$$SUFFIX" ]; do \
			let "SUFFIX += 1" ; \
		done ; \
		$(LN) "$$$$CERTFILE" "$(1)/etc/ssl/certs/$$$$HASH.$$$$SUFFIX" ; \
	done
endef

define Package/ca-bundle/install
	$(INSTALL_DIR) $(1)/etc/ssl/certs
	cat $(PKG_INSTALL_DIR)/usr/share/ca-certificates/*/*.crt >$(1)/etc/ssl/certs/ca-certificates.crt
	$(LN) /etc/ssl/certs/ca-certificates.crt $(1)/etc/ssl/cert.pem
endef

$(eval $(call BuildPackage,ca-certificates))
$(eval $(call BuildPackage,ca-bundle))
