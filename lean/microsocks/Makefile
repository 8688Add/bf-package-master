#
# Copyright (C) 2021 ImmortalWrt
# <https://immortalwrt.org>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=microsocks
PKG_VERSION=1.0.2
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/rofl0r/microsocks.git
PKG_SOURCE_DATE:=2021-3-13
PKG_SOURCE_VERSION:=c4b927bd2f9f7444f65d64f93c356b79f1c6d3e8
PKG_MIRROR_HASH:=8f0d1f30894d0257a1699f68dc9dd95ad5f828b0befd4e6197c18a08540396d2

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=COPYING
PKG_MAINTAINER:=lean

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/microsocks
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Web Servers/Proxies
	TITLE:=tiny, portable SOCKS5 server with very moderate resource usage
	URL:=https://github.com/rofl0r/microsocks
	DEPENDS:=+libpthread
endef

define Package/microsocks/description
	A SOCKS5 service that you can run on your remote boxes to tunnel connections
	through them, if for some reason SSH doesn't cut it for you.
endef

define Package/microsocks/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/bin/microsocks $(1)/usr/bin/microsocks
endef

$(eval $(call BuildPackage,microsocks))
