# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=lzma
PKG_VERSION:=432

PKG_SOURCE:=$(PKG_NAME)$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/sevenzip
PKG_MD5SUM:=155c1ebce5bc6710ae7ecc926226d9d7

PKG_BUILD_DIR:=$(TOOL_BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)/lzma

include $(INCLUDE_DIR)/host-build.mk

LIB_DIR=$(PKG_BUILD_DIR)/C/7zip/Compress/LZMA_Lib
ALONE_DIR=$(PKG_BUILD_DIR)/C/7zip/Compress/LZMA_Alone

define Build/Prepare
	bzcat $(DL_DIR)/$(PKG_SOURCE) | $(TAR) -C $(PKG_BUILD_DIR)/ $(TAR_OPTIONS)
	$(PATCH) $(PKG_BUILD_DIR) ./patches
endef

define Build/Compile
	$(MAKE) -C $(LIB_DIR)
	$(MAKE) -f makefile.gcc -C $(ALONE_DIR)
endef

define Build/Install
	mkdir -p $(STAGING_DIR)/host/lib
	$(CP) $(LIB_DIR)/liblzma.a $(STAGING_DIR)/host/lib/
	mkdir -p $(STAGING_DIR)/bin
	$(CP) $(ALONE_DIR)/lzma $(STAGING_DIR)/bin/
endef

define Build/Clean
	rm -f $(STAGING_DIR)/host/lib/liblzma.a $(STAGING_DIR)/bin/lzma
endef

$(eval $(call HostBuild))
