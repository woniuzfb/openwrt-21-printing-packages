include $(TOPDIR)/rules.mk

PKG_NAME:=libcupsfilters
PKG_VERSION:=2.1.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/OpenPrinting/libcupsfilters/releases/download/$(PKG_VERSION)/
PKG_HASH:=ff31778a438bf335ceed254ccc706d5cd0eee55f608fcf567d88699b15f4fa9e
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

include $(INCLUDE_DIR)/package.mk

define Package/libcupsfilters
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Printing
	DEPENDS:=+cups +ghostscript +poppler +fontconfig +lcms2 +qpdf +libjpeg-turbo +libpng +libtiff +libexif
	URL:=http://www.cups.org/
	TITLE:=Common UNIX Printing System - libcupsfilters
endef

define Package/libcupsfilters/description
	Common UNIX Printing System - libcupsfilters
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--with-cups-config="$(STAGING_DIR)/usr/bin/cups-config" \
	)
endef

define Package/libcupsfilters/install
	$(INSTALL_DIR) $(1)/usr/include/cupsfilters
	$(CP) $(PKG_INSTALL_DIR)/usr/include/cupsfilters/* $(1)/usr/include/cupsfilters/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
	$(SED) 's,^Libs.private.*,Libs.private: -lcups -ltiff -L\$$$$\{libdir\} -lpng16 -lqpdf,g' $(1)/usr/lib/pkgconfig/libcupsfilters.pc || true
	$(INSTALL_DIR) $(1)/usr/share/cups/banners
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/banners/* $(1)/usr/share/cups/banners/
	$(INSTALL_DIR) $(1)/usr/share/cups/charsets
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/charsets/* $(1)/usr/share/cups/charsets/
	$(INSTALL_DIR) $(1)/usr/share/cups/data
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/data/* $(1)/usr/share/cups/data/
	$(INSTALL_DIR) $(1)/usr/share/doc/libcupsfilters
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/libcupsfilters/* $(1)/usr/share/doc/libcupsfilters/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/cupsfilters
	$(CP) $(PKG_INSTALL_DIR)/usr/include/cupsfilters/* $(1)/usr/include/cupsfilters/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libcupsfilters))
