# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=getdns
PKG_VERSION:=1.4.1
PKG_RELEASE:=2

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=David Mora <iamperson347+public@gmail.com>

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://getdnsapi.net/dist/
PKG_HASH:=245233dc780f615b6ab1472f2b9cdcd957a451a736f3036717d0da466ab1c51e

PKG_FIXUP:=autoreconf

PKG_INSTALL:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_GETDNS_ENABLE_STUB_ONLY \
	CONFIG_GETDNS_ENABLE_IDN_LIBIDN2 
	
include $(INCLUDE_DIR)/package.mk

define Package/getdns/Default
	TITLE:=getdns
	URL:=https://getdnsapi.net/
endef

define Package/getdns
	$(call Package/getdns/Default)
	SECTION:=libs
	CATEGORY:=Libraries
	TITLE+= (library)
	DEPENDS+= +libopenssl +!GETDNS_ENABLE_STUB_ONLY:libunbound +GETDNS_ENABLE_IDN_LIBIDN2:libidn2
	MENU:=1
endef

define Package/getdns/description
	This package contains the getdns library (libgetdns). 
	This package also contains the "getdns_query" command line wrapper for getdns exposing the features of this implementation (both in the official API and the additional API functions).
endef

define Package/getdns/config
	source "$(SOURCE)/Config.in"
endef

CONFIGURE_ARGS += \
		$(if $(CONFIG_GETDNS_ENABLE_STUB_ONLY), --enable-stub-only, ) \
		--without-libidn \
		$(if $(CONFIG_GETDNS_ENABLE_IDN_LIBIDN2), , --without-libidn2 ) \
		--with-ssl="$(STAGING_DIR)/opt" \

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/include/getdns/
	$(CP) $(PKG_INSTALL_DIR)/opt/include/getdns/getdns*.h $(1)/opt/include/getdns/
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libgetdns*.{a,so*} $(1)/opt/lib/
	$(INSTALL_DIR) $(1)/opt/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/pkgconfig/getdns*.pc $(1)/opt/lib/pkgconfig/
endef
	
	
define Package/getdns/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/lib/libgetdns.so.* $(1)/opt/lib/
	$(INSTALL_DIR) $(1)/opt/sbin	
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/getdns_query $(1)/opt/sbin/getdns_query
endef

$(eval $(call BuildPackage,getdns))
