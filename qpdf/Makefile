include $(TOPDIR)/rules.mk

PKG_NAME:=qpdf
PKG_VERSION:=12.2.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/qpdf/qpdf/releases/download/v$(PKG_VERSION)/
PKG_HASH:=b3d1575b2218badc3549d6977524bb0f8c468c6528eebc8967bbe3078cf2cace

CMAKE_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/qpdf
  SECTION:=net
  CATEGORY:=Network
  TITLE:=QPDF is a command-line program for transformations on PDF files.
  DEPENDS:= +zlib +libstdcpp +libatomic +libopenssl +libgnutls +libjpeg-turbo
  URL:=http://qpdf.sourceforge.net/
  SUBMENU:=Printing
endef

define Package/qpdf/description
QPDF is a command-line program that does structural, content-preserving
transformations on PDF files. It could have been called something like pdf-to-pdf.
It also provides  many useful capabilities to developers of PDF-producing software
or for people who just want to look at the innards of a PDF file to learn more
about how they work.
endef

CMAKE_OPTIONS += \
	-DBUILD_DOC:BOOL=OFF

define Package/qpdf/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/bin $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) -r $(PKG_INSTALL_DIR)/usr/include/$(PKG_NAME) $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib
	$(SED) 's,^includedir=.*,includedir=\$$$$\{prefix\}/include,g' $(1)/usr/lib/pkgconfig/libqpdf.pc
	$(SED) 's,^libdir=.*,libdir=\$$$$\{exec_prefix\}/lib,g' $(1)/usr/lib/pkgconfig/libqpdf.pc
endef

$(eval $(call BuildPackage,qpdf))
