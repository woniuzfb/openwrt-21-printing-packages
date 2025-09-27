include $(TOPDIR)/rules.mk

PKG_NAME:=libcairo
PKG_VERSION:=1.16.0
PKG_RELEASE:=2

PKG_SOURCE:=cairo-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://cairo.freedesktop.org/releases/
PKG_HASH:=5e7b29b3f113ef870d1e3ecf8adf21f923396401604bda16d44be45e66052331
PKG_BUILD_DIR:=$(BUILD_DIR)/cairo-$(PKG_VERSION)

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/libcairo
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Printing
  TITLE:=cairo
  URL:=https://cairo.freedesktop.org/
  DEPENDS:=+libpng +libfreetype +fontconfig +pixman +glib2 +libjpeg-turbo +zlib +liblzo
endef

define Package/libcairo/description
  Cairo is a 2D graphics library with support for multiple output devices.
endef

CONFIGURE_ARGS += \
	--enable-fc=yes \
	--enable-ft=yes \
	--enable-png=yes \
	--disable-xcb \
	--disable-xlib \
	--disable-xlib-xcb

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/cairo
	$(CP) $(PKG_INSTALL_DIR)/usr/include/cairo/*.h $(1)/usr/include/cairo
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/cairo
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cairo/*.so $(1)/usr/lib/cairo
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc $(1)/usr/lib/pkgconfig
endef

define Package/libcairo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/cairo
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cairo/*.so $(1)/usr/lib/cairo
endef

$(eval $(call BuildPackage,libcairo))
