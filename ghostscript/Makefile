include $(TOPDIR)/rules.mk

PKG_NAME:=ghostscript
PKG_VERSION:=10.06.0
PKG_VERSION_LIBDIR:=10.06.0
PKG_RELEASE:=1
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/ArtifexSoftware/ghostpdl-downloads/releases/download/gs10060
PKG_HASH:=5bd6da34794928cc7e616f288e32bd0be7f9a5ca2d3c206a0af2c19a4e3a318f
PKG_INSTALL:=1

#no need to build host ghostscript
#PKG_BUILD_DEPENDS:=cups ghostscript/host
PKG_BUILD_DEPENDS:=cups

#HOST_PATCH_DIR := ./patches-host

TARGET_CFLAGS+=-I$(STAGING_DIR)/usr/include
TARGET_LDFLAGS+=-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

#include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/ghostscript
  SECTION:=net
  CATEGORY:=Network
  TITLE:=ghostscript
  DEPENDS:=+cups +libcups +libcupsimage +fontconfig +libtiff \
	+libjpeg-turbo +libpng +libexpat +zlib
  SUBMENU:=Printing
endef

define Package/ghostscript/description
	Ghostscript, an interpreter for the PostScript language and for PDF.
endef

#CONFIG_AUTOREMOVE:=

define Build/Configure
	(cd $(PKG_BUILD_DIR) && $(RM) -r expat jpeg libpng tiff zlib)
	$(call Build/Configure/Default, \
		--disable-gtk	\
		--without-libidn \
		--without-x \
		--with-jbig2dec=yes \
		--enable-cups \
		--with-system-libtiff \
		--with-cups-serverbin="/usr/lib/cups" \
		--with-cups-serverroot="/etc/cups" \
		--with-cups-datadir="/usr/share/cups" \
		CUPSCONFIG="$(STAGING_DIR)/usr/bin/cups-config" \
	)
endef

#define Build/Compile
#	# base/unix-aux.mak is patched, and now we replace these fixed strings by the actual paths.
#	# PS. doing all in sed would be painful because the commands would need too much escaping.
#	$(SED) 's,OPENWRT_BASE_BUILD_PATH,$(HOST_BUILD_DIR),g' $(PKG_BUILD_DIR)/base/unix-aux.mak
#	$(SED) 's,OPENWRT_PGK_BUILD_PATH,$(PKG_BUILD_DIR),g' $(PKG_BUILD_DIR)/base/unix-aux.mak
#	$(call Build/Compile/Default, cross_prepare)
#	$(call Build/Compile/Default)
#endef

#define Host/Configure
#	(cd $(HOST_BUILD_DIR) && $(RM) -r zlib)
#	$(call Host/Configure/Default, \
#	--without-x \
#	--without-jbig2dec \
#	--without-pdftoraster \
#	--without-libpaper \
#	--with-libiconv=no \
#	--disable-gtk \
#	--enable-cups \
#	--without-libidn)
#endef

#define Host/Install
#endef

define Package/ghostscript/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/share/doc/ghostscript
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/ghostscript/* $(1)/usr/share/doc/ghostscript/
	$(INSTALL_DIR) $(1)/usr/share/ghostscript
	$(CP) $(PKG_INSTALL_DIR)/usr/share/ghostscript/* $(1)/usr/share/ghostscript/
endef

#$(eval $(call HostBuild))
$(eval $(call BuildPackage,ghostscript))
