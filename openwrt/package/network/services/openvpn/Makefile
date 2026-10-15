#
# Copyright (C) 2010-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=openvpn

PKG_VERSION:=2.7.6
PKG_RELEASE:=1

PKG_SOURCE_URL:=\
	https://build.openvpn.net/downloads/releases/ \
	https://swupdate.openvpn.net/community/releases/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_HASH:=10e24a9385f23cc38cc5cf448f3ca0769f939bc4cbecc4f4647d7e006e52db74

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf
PKG_BUILD_PARALLEL:=1
PKG_LICENSE:=GPL-2.0

include $(INCLUDE_DIR)/package.mk

# Only the OpenSSL variant; 2.7 has no non-TLS mode and needs mbedTLS >= 3.2.1.
define Package/openvpn/Default
  TITLE:=Open source VPN solution using $(2)
  SECTION:=net
  CATEGORY:=Network
  URL:=http://openvpn.net
  SUBMENU:=VPN
  MENU:=1
  DEPENDS:=+kmod-tun +OPENVPN_$(1)_ENABLE_LZO:liblzo +OPENVPN_$(1)_ENABLE_IPROUTE2:ip $(3)
  VARIANT:=$(1)
  MAINTAINER:=Mirko Vogt <mirko@openwrt.org>
  # config-vpn depends on openvpn-mbedtls
  PROVIDES:=openvpn-mbedtls
endef

Package/openvpn-openssl=$(call Package/openvpn/Default,openssl,OpenSSL,+libopenssl +libcap-ng)

define Package/openvpn/config/Default
	source "$(SOURCE)/Config-$(1).in"
endef

Package/openvpn-openssl/config=$(call Package/openvpn/config/Default,openssl)

CONFIG_OPENVPN_OPENSSL:=y

CONFIGURE_VARS += \
	IFCONFIG=/sbin/ifconfig \
	ROUTE=/sbin/route \
	IPROUTE=/usr/sbin/ip \
	NETSTAT=/sbin/netstat

# gcc 4.8 defaults to gnu89; 2.7.x's source uses C11 static_assert.
TARGET_CFLAGS += -std=gnu11

# Options removed in 2.7 are dropped from the configure line.
define Build/Configure
	$(call Build/Configure/Default, \
		$(if $(CONFIG_OPENVPN_$(BUILD_VARIANT)_ENABLE_SMALL),--enable-small) \
		--disable-selinux \
		--disable-systemd \
		--disable-plugins \
		--disable-debug \
		--disable-pkcs11 \
		$(if $(CONFIG_OPENVPN_$(BUILD_VARIANT)_ENABLE_LZO),--enable,--disable)-lzo \
		--disable-lz4 \
		$(if $(CONFIG_OPENVPN_$(BUILD_VARIANT)_ENABLE_MANAGEMENT),--enable,--disable)-management \
		$(if $(CONFIG_OPENVPN_$(BUILD_VARIANT)_ENABLE_FRAGMENT),--enable,--disable)-fragment \
		$(if $(CONFIG_OPENVPN_$(BUILD_VARIANT)_ENABLE_IPROUTE2),--enable,--disable)-iproute2 \
		$(if $(CONFIG_OPENVPN_$(BUILD_VARIANT)_ENABLE_PORT_SHARE),--enable,--disable)-port-share \
		--with-crypto-library=openssl --with-openssl-engine=no \
	)
endef

define Package/openvpn-$(BUILD_VARIANT)/conffiles
/etc/config/openvpn
endef

define Package/openvpn-$(BUILD_VARIANT)/install
	$(INSTALL_DIR) \
		$(1)/usr/sbin \
		$(1)/etc/init.d \
		$(1)/etc/config \
		$(1)/etc/openvpn \
		$(1)/lib/upgrade/keep.d

	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/sbin/openvpn \
		$(1)/usr/sbin/

	$(INSTALL_BIN) \
		files/openvpn.init \
		$(1)/etc/init.d/openvpn

	$(INSTALL_CONF) files/openvpn.config \
		$(1)/etc/config/openvpn

	$(INSTALL_DATA) \
		files/openvpn.upgrade \
		$(1)/lib/upgrade/keep.d/openvpn
endef

$(eval $(call BuildPackage,openvpn-openssl))
