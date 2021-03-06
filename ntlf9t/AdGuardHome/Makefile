#
# Copyright (C) 2015-2016 OpenWrt.org
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE.txt for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=AdGuardHome
PKG_VERSION:=0.106.0-b.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/AdguardTeam/AdGuardHome
PKG_MIRROR_HASH:=e9e760bccfd549c6eed129896608066681c47c7cd44fabfb5be2b76b1e878712
PKG_SOURCE_VERSION:=2a43560176b55924d24f0026c55ba5d2b2b50800

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE.txt
PKG_MAINTAINER:=Dobroslaw Kijowski <dobo90@gmail.com>

PKG_BUILD_DEPENDS:=golang/host node/host node-yarn/host packr/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

PKG_CONFIG_DEPENDS:= \
	CONFIG_ADGUARDHOME_COMPRESS_GOPROXY \
	CONFIG_ADGUARDHOME_COMPRESS_UPX

GO_PKG:=github.com/AdguardTeam/AdGuardHome
# GO_PKG_EXCLUDES:=internal/dhcpd/standalone
GO_PKG_BUILD_PKG:=github.com/AdguardTeam/AdGuardHome

AGH_VERSION_PKG:=github.com/AdguardTeam/AdGuardHome/internal/version
GO_PKG_LDFLAGS:=-s -w
GO_PKG_LDFLAGS_X:= \
	$(AGH_VERSION_PKG).channel=release \
	$(AGH_VERSION_PKG).version=v$(PKG_VERSION) \
	$(AGH_VERSION_PKG).goarm=$(GO_ARM) \
	$(AGH_VERSION_PKG).gomips=$(GO_MIPS)

ifneq ($(findstring $(ARCH),mips mipsel),)
	ADG_CONF_NAME:=small_flash
else
	ADG_CONF_NAME:=big_space
endif

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/AdGuardHome
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Network-wide ads and trackers blocking DNS server
  URL:=https://github.com/AdguardTeam/AdGuardHome
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
endef

define Package/AdGuardHome/description
Free and open source, powerful network-wide ads and trackers blocking DNS server.
endef

define Package/$(PKG_NAME)/config
config ADGUARDHOME_COMPRESS_GOPROXY
	bool "Compiling with GOPROXY proxy"
	default n

config ADGUARDHOME_COMPRESS_UPX
	bool "Compress executable files with UPX"
	default y
endef

ifeq ($(CONFIG_ADGUARDHOME_COMPRESS_GOPROXY),y)
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
endif

define Build/Compile
( \
  pushd $(PKG_BUILD_DIR) ; \
  make js-deps js-build ; \
  packr -z -v -i internal ; \
  popd ; \
  $(call GoPackage/Build/Compile) ; \
)
ifeq ($(CONFIG_ADGUARDHOME_COMPRESS_UPX),y)
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/AdGuardHome || true
endif
endef

define Package/AdGuardHome/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/AdGuardHome
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/AdGuardHome $(1)/usr/bin/AdGuardHome
	$(INSTALL_CONF) ./conf/$(ADG_CONF_NAME).yaml $(1)/etc/AdGuardHome/AdGuardHome.yaml
endef

$(eval $(call GoBinPackage,AdGuardHome))
$(eval $(call BuildPackage,AdGuardHome))
