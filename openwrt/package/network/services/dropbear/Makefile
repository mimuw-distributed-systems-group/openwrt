#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=dropbear

# OpenWrt's old dropbear patches are included upstream by now; the
# authorized_keys directory is set with -D in files/dropbear.init.
PKG_VERSION:=2026.94
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:= \
	https://matt.ucc.asn.au/dropbear/releases/ \
	https://dropbear.nl/mirror/releases/
PKG_HASH:=e098034a843699200c8c977a991fff73159735bf795d5f72ef672c41a6b1ae81

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE libtomcrypt/LICENSE libtommath/LICENSE

PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_FIXUP:=autoreconf

PKG_CONFIG_DEPENDS:=CONFIG_DROPBEAR_ECC

include $(INCLUDE_DIR)/package.mk

define Package/dropbear/Default
  URL:=https://matt.ucc.asn.au/dropbear/
endef

define Package/dropbear/config
	source "$(SOURCE)/Config.in"
endef

define Package/dropbear
  $(call Package/dropbear/Default)
  SECTION:=net
  CATEGORY:=Base system
  TITLE:=Small SSH2 client/server
endef

define Package/dropbear/description
 A small SSH2 server/client designed for small memory environments.
endef

define Package/dropbear/conffiles
/etc/dropbear/dropbear_ed25519_host_key
/etc/dropbear/dropbear_rsa_host_key
$(if $(CONFIG_DROPBEAR_ECC),/etc/dropbear/dropbear_ecdsa_host_key)
/etc/config/dropbear
endef

define Package/dropbearconvert
  $(call Package/dropbear/Default)
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Utility for converting SSH keys
endef

CONFIGURE_ARGS += \
	--disable-pam \
	--enable-openpty \
	--enable-syslog \
	$(if $(CONFIG_SHADOW_PASSWORDS),,--disable-shadow) \
	--disable-lastlog \
	--disable-utmp \
	--disable-utmpx \
	--disable-wtmp \
	--disable-wtmpx \
	--disable-loginfunc \
	--disable-pututline \
	--disable-pututxline \
	--disable-zlib \
	--enable-bundled-libtom

# gcc 4.8 defaults to C89, the post-quantum key exchange code needs C99.
TARGET_CFLAGS += -std=gnu99 -DARGTYPE=3 -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -Wl,--gc-sections

# DSS is deprecated upstream and files/dropbear.init doesn't create its key.
define Build/Configure
	: > $(PKG_BUILD_DIR)/localoptions.h
	echo '#define DROPBEAR_DSS 0' >> $(PKG_BUILD_DIR)/localoptions.h
	echo '#define DROPBEAR_ECDSA $(if $(CONFIG_DROPBEAR_ECC),1,0)' >> $(PKG_BUILD_DIR)/localoptions.h
	echo '#define DROPBEAR_ECDH $(if $(CONFIG_DROPBEAR_ECC),1,0)' >> $(PKG_BUILD_DIR)/localoptions.h
	echo '#define DROPBEAR_CURVE25519 $(if $(CONFIG_DROPBEAR_ECC),1,0)' >> $(PKG_BUILD_DIR)/localoptions.h

	$(Build/Configure/Default)
endef

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		PROGRAMS="dropbear dbclient dropbearkey scp" \
		MULTI=1 SCPPROGRESS=1
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		PROGRAMS="dropbearconvert"
endef

define Package/dropbear/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/dropbearmulti $(1)/usr/sbin/dropbear
	$(INSTALL_DIR) $(1)/usr/bin
	$(LN) ../sbin/dropbear $(1)/usr/bin/scp
	$(LN) ../sbin/dropbear $(1)/usr/bin/ssh
	$(LN) ../sbin/dropbear $(1)/usr/bin/dbclient
	$(LN) ../sbin/dropbear $(1)/usr/bin/dropbearkey
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/dropbear.config $(1)/etc/config/dropbear
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/dropbear.init $(1)/etc/init.d/dropbear
	$(INSTALL_DIR) $(1)/usr/lib/opkg/info
	$(INSTALL_DIR) $(1)/etc/dropbear
	touch $(1)/etc/dropbear/dropbear_ed25519_host_key
	touch $(1)/etc/dropbear/dropbear_rsa_host_key
	$(if $(CONFIG_DROPBEAR_ECC),touch $(1)/etc/dropbear/dropbear_ecdsa_host_key)
endef

define Package/dropbearconvert/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/dropbearconvert $(1)/usr/bin/dropbearconvert
endef

$(eval $(call BuildPackage,dropbear))
$(eval $(call BuildPackage,dropbearconvert))
