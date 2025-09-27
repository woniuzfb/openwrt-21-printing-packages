include $(TOPDIR)/rules.mk

PKG_NAME:=cups
PKG_VERSION:=2.4.14
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-source.tar.gz
PKG_SOURCE_URL:=https://github.com/OpenPrinting/cups/releases/download/v$(PKG_VERSION)/
PKG_HASH:=660288020dd6f79caf799811c4c1a3207a48689899ac2093959d70a3bdcb7699

PKG_BUILD_DEPENDS:=LIBCUPS_OPENSSL:openssl LIBCUPS_GNUTLS:gnutls

TARGET_LDFLAGS+=-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

include $(INCLUDE_DIR)/package.mk

define Package/cups/Default
  URL:=http://www.cups.org/
  SUBMENU:=Printing
endef

define Package/cups
$(call Package/cups/Default)
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+zlib +bzip2 +libcups +libcupsimage +libusb-1.0 +libstdcpp
  TITLE:=Common UNIX Printing System (daemon)
endef

define Package/cups/description
	Common UNIX Printing System (daemon)
endef

define Package/cups/conffiles
/etc/cups/cupsd.conf
/etc/cups/printers.conf
endef

define Package/cups-bsd
$(call Package/cups/Default)
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libcups
  TITLE:=Common UNIX Printing System - BSD commands (old)
endef

define Package/cups-bsd/description
	Common UNIX Printing System - BSD commands (old)
endef

define Package/cups-client
$(call Package/cups/Default)
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libcups +libcupsimage
  TITLE:=Common UNIX Printing System - Client commands
endef

define Package/cups-client/conffiles
/etc/cups/client.conf
endef

define Package/cups-client/description
	Common UNIX Printing System - Client commands
endef

define Package/cups-ppdc
$(call Package/cups/Default)
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libcups +libjpeg-turbo +libpng +libstdcpp +zlib
  TITLE:=Common UNIX Printing System - PPDC utils
endef

define Package/cups-ppdc/description
	Common UNIX Printing System - PPDC utils
endef

define Package/libcups
$(call Package/cups/Default)
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+zlib +libpthread +libpng +libjpeg-turbo $(if $(CONFIG_LIBCUPS_GNUTLS),+libgnutls) $(if $(CONFIG_LIBCUPS_OPENSSL),+libopenssl) +libavahi-client
  TITLE:=Common UNIX Printing System - Core library
endef

define Package/libcups/config
	if PACKAGE_libcups
		config LIBCUPS_OPENSSL
			bool "Compile with OpenSSL support"
			default y
			help
				Use OpenSSL for SSL

		config LIBCUPS_GNUTLS
			bool "Compile with GnuTLS support"
			default n
			help
				Use GnuTLS for SSL
	endif
endef

define Package/libcups/description
	Common UNIX Printing System - Core library
endef

define Package/libcupsimage
$(call Package/cups/Default)
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libcups
  TITLE:=Common UNIX Printing System - Image library
endef

define Package/libcupsimage/description
	Common UNIX Printing System - Image library
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--with-cups-user="nobody" \
		--with-cups-group="nogroup" \
		--with-system-groups="root" \
		--enable-default-shared \
		--enable-shared \
		--enable-libusb \
		--with-dnssd=avahi \
		--disable-systemd \
		--disable-acl \
		--disable-pam \
		--with-tls=$(if $(CONFIG_LIBCUPS_OPENSSL),openssl,gnutls) \
		--disable-gssapi \
		UNAME="Linux" \
		LIBS="$(TARGET_LDFLAGS) -lz -lpng -ljpeg" \
	)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		DSTROOT="$(PKG_INSTALL_DIR)" \
		all install
	$(INSTALL_DIR) $(PKG_INSTALL_DIR)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cups-config $(PKG_INSTALL_DIR)/usr/bin
	$(SED) '1i\DefaultPaperSize A4' $(PKG_INSTALL_DIR)/etc/cups/cupsd.conf
	$(SED) '/Order allow,deny/a\  Allow from all' $(PKG_INSTALL_DIR)/etc/cups/cupsd.conf
	$(SED) 's,localhost:631,631,g' $(PKG_INSTALL_DIR)/etc/cups/cupsd.conf
	$(SED) 's,^ErrorPolicy.*,ErrorPolicy abort-job,g' $(PKG_INSTALL_DIR)/etc/cups/cupsd.conf
endef

define Package/cups/install
	$(INSTALL_DIR) $(1)/etc/cups
	$(CP) $(PKG_INSTALL_DIR)/etc/cups/* $(1)/etc/cups/

	$(INSTALL_DIR) $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/backend $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/cgi-bin $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/command $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/daemon $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/driver $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/monitor $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/notifier $(1)/usr/lib/cups
	$(INSTALL_DIR) $(1)/usr/share/cups/templates 
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/templates/*.tmpl \
		$(1)/usr/share/cups/templates/
	$(INSTALL_DIR) $(1)/usr/share/cups/mime
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/mime/* $(1)/usr/share/cups/mime/
	$(INSTALL_DIR) $(1)/usr/share/cups/usb
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/usb/* $(1)/usr/share/cups/usb/
	$(INSTALL_DIR) $(1)/usr/share/cups/model
	$(INSTALL_DIR) $(1)/usr/share/cups/banners
	$(INSTALL_DIR) $(1)/usr/share/cups/data
	$(INSTALL_DIR) $(1)/usr/share/cups/examples
	$(INSTALL_DIR) $(1)/usr/share/cups/profiles
	$(INSTALL_DIR) $(1)/usr/share/doc/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups/*.*html \
		$(1)/usr/share/doc/cups/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups/*.css \
		$(1)/usr/share/doc/cups/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups/*.txt \
		$(1)/usr/share/doc/cups/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups/images \
		$(1)/usr/share/doc/cups/
	$(CP) $(PKG_INSTALL_DIR)/etc/cups/* $(1)/etc/cups/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/cupsd $(1)/etc/init.d/cupsd
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/sbin/{cupsctl,cupsd} \
		$(1)/usr/sbin/
	chmod 700 $(1)/usr/lib/cups/backend/usb

	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/filter/{commandtops,gziptoany,pstops,rastertoepson,rastertohp,rastertolabel,rastertopwg} \
		$(1)/usr/lib/cups/filter
	cd $(1)/usr/lib/cups/filter && \
		ln -sf rastertolabel rastertodymo
endef

define Package/cups-bsd/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/{lprm,lpq,lpr} $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/lpc $(1)/usr/sbin/
endef

define Package/cups-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/{lp,cancel,cupstestppd,ipptool,lpoptions,lpstat,ippeveprinter,ippfind} \
		$(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/sbin/{cupsaccept,cupsfilter,lpadmin,lpinfo,lpmove} \
		$(1)/usr/sbin/
	(cd $(1)/usr/sbin; ln -sf cupsaccept accept; ln -sf cupsaccept cupsenable; ln -sf cupsaccept cupsdisable; ln -sf cupsaccept reject; ln -sf cupsaccept cupsreject;)
	$(INSTALL_DIR) $(1)/usr/share/cups/ipptool
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/ipptool/* \
		$(1)/usr/share/cups/ipptool
endef

define Package/cups-ppdc/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/{ppdc,ppdhtml,ppdi,ppdmerge,ppdpo} \
		$(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/share/cups/ppdc
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/ppdc/* \
		$(1)/usr/share/cups/ppdc
endef

define Package/libcups/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib*/libcups.so* $(1)/usr/lib/
endef

define Package/libcupsimage/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib*/libcupsimage.so* $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(2)/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cups-config $(2)/bin/
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/cups $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib*/* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_BUILD_DIR)/cups.pc $(1)/usr/lib/pkgconfig/
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cups-config $(1)/usr/bin/

	$(SED) 's,^prefix=.*,prefix=$(STAGING_DIR)/usr,g' $(1)/usr/bin/cups-config
	$(SED) 's,^exec_prefix=.*,exec_prefix=\$$$$\{prefix\},g' $(1)/usr/bin/cups-config
	$(SED) 's,^includedir=.*,includedir=\$$$$\{prefix\}/include/cups,g' $(1)/usr/bin/cups-config
	$(SED) 's,^libdir=.*,libdir=\$$$$\{exec_prefix\}/lib,g' $(1)/usr/bin/cups-config

	$(SED) 's,^includedir=.*,includedir=\$$$$\{prefix\}/include/cups,g' $(1)/usr/lib/pkgconfig/cups.pc
	$(SED) 's,^libdir=.*,libdir=\$$$$\{exec_prefix\}/lib,g' $(1)/usr/lib/pkgconfig/cups.pc
	[ -n "$(TARGET_LDFLAGS)" ] && $(SED) 's#$(TARGET_LDFLAGS)##g' $(1)/usr/lib/pkgconfig/cups.pc || true
endef

$(eval $(call BuildPackage,cups))
$(eval $(call BuildPackage,libcups))
$(eval $(call BuildPackage,libcupsimage))
$(eval $(call BuildPackage,cups-bsd))
$(eval $(call BuildPackage,cups-client))
$(eval $(call BuildPackage,cups-ppdc))
