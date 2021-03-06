# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id$

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=broadcom-wl
PKG_VERSION:=4.80.53.0
PKG_RELEASE:=1
WLC_VERSION:=0.1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://downloads.openwrt.org/sources
PKG_MD5SUM:=a7d8dde3ce474c361143b83e1d9890b1

include $(INCLUDE_DIR)/package.mk

define Package/broadcom-wl/Default
  SECTION:=kernel
  CATEGORY:=Kernel modules
  DEPENDS:=@PACKAGE_kmod-brcm-wl||PACKAGE_kmod-brcm-wl-mimo
  SUBMENU:=Proprietary BCM43xx WiFi driver
  SUBMENUDEP:=@LINUX_2_4_BRCM
endef

define KernelPackage/brcm-wl/Default
  $(call Package/broadcom-wl/Default)
  SECTION:=kernel
  DEPENDS:=@LINUX_2_4_BRCM
  TITLE:=Kernel driver for BCM43xx chipsets
  DESCRIPTION:=\
	This package contains the proprietary wireless driver for the Broadcom \\\
	BCM43xx chipset
  VERSION:=$(LINUX_VERSION)+$(PKG_VERSION)-$(PKG_RELEASE)
  FILES:=$(PKG_BUILD_DIR)/kmod/wl$(1).o
  AUTOLOAD:=$(call AutoLoad,30,wl$(1))
endef

define KernelPackage/brcm-wl
  $(call KernelPackage/brcm-wl/Default,)
  TITLE+= (normal version)
  DESCRIPTION+= (normal version).
endef

define KernelPackage/brcm-wl-mimo
  $(call KernelPackage/brcm-wl/Default,_mimo)
  TITLE+= (MIMO version)
  DESCRIPTION+= (MIMO version).
endef

define Package/wlc
  $(call Package/broadcom-wl/Default)
  TITLE:=wl driver setup utility
  DESCRIPTION:=\
	This package contains an utility for initializing the proprietary Broadcom \\\
	wl driver.
endef

define Package/wl
  $(call Package/broadcom-wl/Default)
  TITLE:=Proprietary Broadcom wl driver config utility
  DESCRIPTION:=\
	This package contains the proprietary utility (wl) for configuring the \\\
	proprietary Broadcom wl driver.
endef

define Package/nas
  $(call Package/broadcom-wl/Default)
  DEPENDS+=+nvram
  TITLE:=Proprietary Broadcom WPA/WPA2 authenticator
  DESCRIPTION:=\
	This package contains the proprietary WPA/WPA2 authenticator (nas) for the \\\
	proprietary Broadcom wl driver.
endef


define install_template
	$(INSTALL_DIR) $(1)/lib/modules/$(LINUX_VERSION)
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/kmod/wl$(2).o.patch $(1)/lib/modules/$(LINUX_VERSION)/
endef

define KernelPackage/brcm-wl/install
	$(call install_template,$(1),)
endef

define KernelPackage/brcm-wl_mimo/install
	$(call install_template,$(1),_mimo)
endef

MAKEFLAGS_KMOD:= -C "$(LINUX_DIR)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(LINUX_KARCH)" \
		PATH="$(TARGET_PATH)" \
		SUBDIRS="$(PKG_BUILD_DIR)/kmod"

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) src/* $(PKG_BUILD_DIR)/
endef

define Build/Compile
	# Compile the kernel part
	$(MAKE) $(MAKEFLAGS_KMOD) \
		modules
	$(MAKE) $(MAKEFLAGS_KMOD) MOD_NAME="_mimo" \
		modules

	# Compile wlc
	$(MAKE) -C $(PKG_BUILD_DIR)/wlc \
		$(TARGET_CONFIGURE_OPTS) \
		CFLAGS="-I$(PKG_BUILD_DIR)/wlc/include $(TARGET_CFLAGS)" \
		all
endef

define Package/wlc/install
	$(CP) ./files/* $(1)/
	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/wlc/wlc $(1)/sbin/
endef

define Package/wl/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/wl $(1)/usr/sbin/
endef

define Package/nas/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libbcmcrypto.so $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/nas $(1)/usr/sbin/
	ln -sf nas $(1)/usr/sbin/nas4not
	ln -sf nas $(1)/usr/sbin/nas4wds
endef

$(eval $(call KernelPackage,brcm-wl))
$(eval $(call KernelPackage,brcm-wl-mimo))
$(eval $(call BuildPackage,wlc))
$(eval $(call BuildPackage,wl))
$(eval $(call BuildPackage,nas))
