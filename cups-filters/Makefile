include $(TOPDIR)/rules.mk

PKG_NAME:=cups-filters
PKG_VERSION:=2.0.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/OpenPrinting/cups-filters/releases/download/$(PKG_VERSION)/
PKG_HASH:=3de1cbb889d06e5a6a945dcb921292544477ab89da95ca89f1eec2de29937afb
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

TARGET_LDFLAGS+=-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

include $(INCLUDE_DIR)/package.mk

define Package/cups-filters
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Printing
  DEPENDS:=+cups +libcupsfilters +libppd
  URL:=http://www.cups.org/
  TITLE:=Common UNIX Printing System - filters
endef

define Package/cups-filters/description
	Common UNIX Printing System - filters
endef

define Package/cups-filters/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/lib/cups/backend
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/backend/* $(1)/usr/lib/cups/backend/
	$(INSTALL_DIR) $(1)/usr/lib/cups/driver
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/driver/* $(1)/usr/lib/cups/driver/
	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/filter/* $(1)/usr/lib/cups/filter/
	$(INSTALL_DIR) $(1)/usr/share/cups/drv
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/drv/* $(1)/usr/share/cups/drv/
	$(INSTALL_DIR) $(1)/usr/share/cups/mime
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/mime/* $(1)/usr/share/cups/mime/
	$(INSTALL_DIR) $(1)/usr/share/doc/cups-filters
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups-filters/* $(1)/usr/share/doc/cups-filters/
	$(INSTALL_DIR) $(1)/usr/share/ppd/cupsfilters
	$(CP) $(PKG_INSTALL_DIR)/usr/share/ppd/cupsfilters/* $(1)/usr/share/ppd/cupsfilters/
	$(INSTALL_DIR) $(1)/usr/share/ppdc
	$(CP) $(PKG_INSTALL_DIR)/usr/share/ppdc/* $(1)/usr/share/ppdc/
endef

$(eval $(call BuildPackage,cups-filters))
