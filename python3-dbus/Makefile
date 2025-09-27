include $(TOPDIR)/rules.mk

PKG_NAME:=python3-dbus
PKG_VERSION:=1.3.2
PKG_RELEASE:=2
PKG_SOURCE:=dbus-python-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://dbus.freedesktop.org/releases/dbus-python/
PKG_HASH:=ad67819308618b5069537be237f8e68ca1c7fcc95ee4a121fe6845b1418248f8
PKG_BUILD_DIR:=$(BUILD_DIR)/dbus-python-$(PKG_VERSION)
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

PKG_BUILD_DEPENDS:=python3/host

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-version.mk

TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include/python$(PYTHON3_VERSION)
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib -lpython$(PYTHON3_VERSION)

define Package/python3-dbus
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Printing
	TITLE:=Python3 DBus bindings
	DEPENDS:=+libdbus +glib2 +libpython3
endef

define Package/python3-dbus/description
	Python3 bindings for D-Bus
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/dbus-1.0/dbus
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/include/dbus-1.0/dbus/* \
		$(1)/usr/include/dbus-1.0/dbus/

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* \
		$(1)/usr/lib/pkgconfig
endef

PYTHON_PKG_DIR:=/usr/lib/python$(PYTHON3_VERSION)/site-packages

define Package/python3-dbus/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/*.so \
		$(1)$(PYTHON_PKG_DIR)

	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)/dbus
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/dbus/*.py \
		$(1)$(PYTHON_PKG_DIR)/dbus

	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)/dbus/mainloop
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/dbus/mainloop/*.py \
		$(1)$(PYTHON_PKG_DIR)/dbus/mainloop

endef

$(eval $(call BuildPackage,python3-dbus))
