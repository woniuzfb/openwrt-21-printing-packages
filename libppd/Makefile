include $(TOPDIR)/rules.mk

PKG_NAME:=libppd
PKG_VERSION:=2.1.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/OpenPrinting/libppd/releases/download/$(PKG_VERSION)/
PKG_HASH:=b356aeed1335ef0ca7f799741782a2544e7acee63fb4b047b94e4e0395a9cb62
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

TARGET_LDFLAGS+=-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

include $(INCLUDE_DIR)/package.mk

define Package/libppd
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Printing
	DEPENDS:=+libcups +libcupsfilters
	URL:=http://www.cups.org/
	TITLE:=Common UNIX Printing System - libppd
endef

define Package/libppd/description
	Common UNIX Printing System - libppd
endef

define Package/libppd/install
	$(INSTALL_DIR) $(1)/usr/include/ppd
	$(CP) $(PKG_INSTALL_DIR)/usr/include/ppd/* $(1)/usr/include/ppd/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/share/doc/libppd
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/libppd/* $(1)/usr/share/doc/libppd/
	$(INSTALL_DIR) $(1)/usr/share/ppdc
	$(CP) $(PKG_INSTALL_DIR)/usr/share/ppdc/* $(1)/usr/share/ppdc/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/ppd
	$(CP) $(PKG_INSTALL_DIR)/usr/include/ppd/* $(1)/usr/include/ppd/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libppd))
