include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/nls.mk

PKG_NAME:=poppler
PKG_VERSION:=23.12.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://poppler.freedesktop.org/
PKG_HASH:=beba398c9d37a9b6d02486496635e08f1df3d437cfe61dab2593f47c4d14cdbb
PKG_INSTALL:=1

PKG_BUILD_DEPENDS:=libiconv-stub

TARGET_CFLAGS+=-I$(STAGING_DIR)/usr/include
TARGET_LDFLAGS+=-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/poppler
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Poppler PDF rendering library
  DEPENDS:=+libpng +libtiff +libjpeg-turbo +zlib +lcms2 +fontconfig +libpthread +libfreetype +libstdcpp +libcairo +glib2 +libintl-full +curl +libnss +nspr +boost
  URL:=https://poppler.freedesktop.org/
  SUBMENU:=Printing
endef

define Package/poppler/description
	Poppler is a PDF rendering library based on the xpdf-3.0 code base.
endef

CMAKE_OPTIONS += \
	-DCMAKE_CXX_STANDARD=17 \
	-DCMAKE_CXX_STANDARD_REQUIRED=ON \
	-DENABLE_GLIB=OFF \
	-DENABLE_GTK_DOC=OFF \
	-DBUILD_GTK_TESTS=OFF \
	-DENABLE_QT5=OFF \
	-DENABLE_QT6=OFF \
	-DBUILD_QT5_TESTS=OFF \
	-DBUILD_QT6_TESTS=OFF \
	-DENABLE_CPP=ON \
	-DENABLE_ZLIB_UNCOMPRESS=ON \
	-DENABLE_LIBOPENJPEG=none \
	-DENABLE_DCTDECODER=libjpeg \
	-DENABLE_GPGME=OFF \
	-DUSE_CMS=lcms2 \
	-DFONT_CONFIGURATION=fontconfig \
	-DIconv_LIBRARY=$(STAGING_DIR)/usr/lib/libiconv-stub/lib/libiconv.a

define Package/poppler/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/poppler/cpp $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/cpp/* $(1)/usr/include/poppler/cpp/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib
	$(SED) 's,^includedir=.*,includedir=\$$$$\{prefix\}/include,g' $(1)/usr/lib/pkgconfig/poppler.pc
	$(SED) 's,^libdir=.*,libdir=\$$$$\{prefix\}/lib,g' $(1)/usr/lib/pkgconfig/poppler.pc
	$(SED) 's,^includedir=.*,includedir=\$$$$\{prefix\}/include,g' $(1)/usr/lib/pkgconfig/poppler-cpp.pc
	$(SED) 's,^libdir=.*,libdir=\$$$$\{prefix\}/lib,g' $(1)/usr/lib/pkgconfig/poppler-cpp.pc
endef

$(eval $(call BuildPackage,poppler))
