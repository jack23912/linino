# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

ARCH:=mips
BOARD:=atheros
BOARDNAME:=Atheros
FEATURES:=squashfs jffs2

LINUX_VERSION:=2.6.21.5

define Target/Description
	Build firmware images for Atheros SoC boards
endef

include $(INCLUDE_DIR)/kernel-build.mk
DEFAULT_PACKAGES += kmod-madwifi

# include the profiles
-include profiles/*.mk

$(eval $(call BuildKernel))
