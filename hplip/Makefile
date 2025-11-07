#	if polkit
#	$(INSTALL_DIR) $(1)/usr/share/dbus-1/system-services
#	$(CP) $(PKG_INSTALL_DIR)/usr/share/dbus-1/system-services/com.hp.hplip.service $(1)/usr/share/dbus-1/system-services
#	$(INSTALL_DIR) $(1)/etc/dbus-1/system.d
#	$(CP) $(PKG_INSTALL_DIR)/etc/dbus-1/system.d/com.hp.hplip.conf $(1)/etc/dbus-1/system.d

include $(TOPDIR)/rules.mk

PKG_NAME:=hplip
PKG_VERSION:=3.25.6
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/$(PKG_NAME)
PKG_HASH:=a6af314a7af0572f2ab6967b2fe68760e64d74628ef0e6237f8504d81047edbe

PKG_BUILD_DEPENDS:=python3 cups cups-filters
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-version.mk

define Package/hplip
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Printing
  TITLE:=HP Linux Imaging and Printing
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS:=+libjpeg-turbo +libpng +libtiff +libusb-1.0 +cups +libcups +libcupsimage +libstdcpp +zlib +libnetsnmp +libopenssl +python3 +python3-dbus +python3-distro +wget-ssl
endef

define Package/hplip/description
	HPLIP is an HP developed solution for printing, scanning, and faxing with HP inkjet and laser based printers in Linux.
endef

define Package/hplip/conffiles
/etc/hp/hplip.conf
/etc/hotplug.d/usb/10-hp-printer
/etc/init.d/hplip-plugin
endef

define Package/hplip-scan
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Printing
  TITLE:=HP Linux Imaging and Printing - Scanning libraries
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS:=+hplip +libsane
endef

define Package/hplip-scan/description
	Libraries to use HP Printers/Scanners with Sane.
endef

define Package/hplip-ppds
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Printing
  TITLE:=HP Linux Imaging and Printing - PPD files
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS:=+hplip
endef

define Package/hplip-ppds/description
	PPD files for CUPS.
endef

define Package/hplip-tools
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Printing
  TITLE:=HP Linux Imaging and Printing - Command line tools
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS:=+hplip +hplip-scan
endef

define Package/hplip-tools/description
	Command line tools for managing your printer.
endef

CONFIGURE_ARGS += \
	--disable-fax-build \
	--disable-qt4 \
	--disable-qt5 \
	--disable-doc-build \
	--disable-gui-build \
	--disable-pp-build

define Build/Configure
	$(call Build/Configure/Default, \
		PYTHON=$(STAGING_DIR_HOSTPKG)/bin/python3 \
	)
	$(INSTALL_DIR) $(PKG_INSTALL_DIR)/usr/share/sane
	sed -n -e '/key="usb.product_id"/{s/.*int_outof="0x//;s/;0x/\n/g;s/".*//;p}' \
		$(PKG_BUILD_DIR)/data/rules/20-hplip-devices.fdi | sort -u > \
		$(PKG_INSTALL_DIR)/usr/share/sane/03f0-hplip.usbid
endef

define Package/hplip/install
	$(INSTALL_DIR) $(1)/etc/hp
	$(CP) $(PKG_INSTALL_DIR)/etc/hp/hplip.conf $(1)/etc/hp/hplip.conf

	$(INSTALL_DIR) $(1)/etc/hotplug.d/usb
	$(CP) ./files/etc/hotplug.d/usb/10-hp-printer $(1)/etc/hotplug.d/usb

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/hplip-plugin $(1)/etc/init.d/hplip-plugin

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/ldl
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/data/ldl/* $(1)/usr/share/hplip/data/ldl

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/localization
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/data/localization/* $(1)/usr/share/hplip/data/localization

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/models
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/data/models/models.dat $(1)/usr/share/hplip/data/models

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/pcl
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/data/pcl/* $(1)/usr/share/hplip/data/pcl

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/ps
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/data/ps/* $(1)/usr/share/hplip/data/ps

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.la $(1)/usr/lib

	$(INSTALL_DIR) $(1)/usr/lib/cups/backend
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cups/backend/hp $(1)/usr/lib/cups/backend

	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cups/filter/* $(1)/usr/lib/cups/filter

	$(INSTALL_DIR) $(1)/usr/share/cups/drv/hp
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/drv/hp/hpcups.drv $(1)/usr/share/cups/drv/hp

	$(INSTALL_DIR) $(1)/usr/share/doc/$(PKG_NAME)-$(PKG_VERSION)
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/$(PKG_NAME)-$(PKG_VERSION)/* $(1)/usr/share/doc/$(PKG_NAME)-$(PKG_VERSION)

	$(INSTALL_DIR) $(1)/usr/share/hal/fdi/preprobe/10osvendor
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hal/fdi/preprobe/10osvendor/* $(1)/usr/share/hal/fdi/preprobe/10osvendor
endef

define Package/hplip-scan/install
	$(INSTALL_DIR) $(1)/usr/lib/sane
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sane/*.so* $(1)/usr/lib/sane
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sane/*.la $(1)/usr/lib/sane
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/x86_64-linux-gnu $(1)/usr/lib

	$(INSTALL_DIR) $(1)/etc/sane.d
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/etc/sane.d/dll.conf $(1)/etc/sane.d

	$(INSTALL_DIR) $(1)/usr/share/sane
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/share/sane/03f0-hplip.usbid \
		$(1)/usr/share/sane/03f0-hplip.usbid

	$(INSTALL_DIR) $(1)/usr/share/hplip
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/scan $(1)/usr/share/hplip
endef

define Package/hplip-ppds/install
	$(INSTALL_DIR) $(1)/usr/share/ppd/HP
	$(CP) $(PKG_INSTALL_DIR)/usr/share/ppd/HP/* $(1)/usr/share/ppd/HP
endef

define Package/hplip-tools/install
	$(INSTALL_DIR) $(1)/usr/lib/python$(PYTHON3_VERSION)/site-packages
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/python$(PYTHON3_VERSION)/site-packages/* $(1)/usr/lib/python$(PYTHON3_VERSION)/site-packages/

	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin

	$(INSTALL_DIR) $(1)/usr/share/hplip
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/hplip/*.py $(1)/usr/share/hplip
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/{base,copier,pcard,prnt,dat2drv,hplip_clean.sh,installer,locatedriver} $(1)/usr/share/hplip
endef

$(eval $(call BuildPackage,hplip))
$(eval $(call BuildPackage,hplip-scan))
$(eval $(call BuildPackage,hplip-ppds))
$(eval $(call BuildPackage,hplip-tools))
