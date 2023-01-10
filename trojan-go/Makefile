#
# Copyright (C) 2011-2023 Entware
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# The newer version has different dependency tracking

include $(TOPDIR)/rules.mk

PKG_NAME:=trojan-go
PKG_VERSION:=v0.10.6
PKG_RELEASE:=1

PKG_GOGET:=github.com/p4gefau1t/trojan-go

PKG_SOURCE_VERSION:=2dc60f52e79ff8b910e78e444f1e80678e936450

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://$(PKG_GOGET)
PKG_MIRROR_HASH:=skip

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/golang.mk

define Package/trojan-go/Default
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Web Servers/Proxies
	TITLE:=A Trojan proxy written in Go
	URL:=https://github.com/p4gefau1t/trojan-go
	MAINTAINER:=Entware team, https://entware.net
endef

define Package/trojan-go
	$(call Package/trojan-go/Default)
	VARIANT:=hf
endef

define Package/trojan-go_nohf
	$(call Package/trojan-go/Default)
	VARIANT:=nohf
	DEPENDS:=@(TARGET_armv7_2_6||TARGET_armv7_3_2)
endef

define Package/rclone/description
 An unidentifiable mechanism that helps you bypass GFW
endef

Package/trojan-go/description=$(Package/rclone/description)

define Download/geoip
  URL:=https://github.com/v2fly/geoip/raw/release/
  FILE:=geoip.dat
  HASH:=118e2eff18e8deee76453ce903266fc0ecce86b1fd0f353efe606ba734e70ad8
endef
$(eval $(call Download,geoip))


define Download/geoip-cn
  URL:=https://github.com/v2fly/geoip/raw/release/
  FILE:=geoip-only-cn-private.dat
  HASH:=8b9175e07c7ddeea4220cf9f755cd8f77aaaeb89551ea3d4e909febfd3f83812
endef
$(eval $(call Download,geoip-cn))

define Download/dlc
  URL:=https://github.com/v2fly/domain-list-community/releases/latest/download/
  FILE:=dlc.dat
  HASH:=76509a69a0d1ccd5a9722bc42fbb32893b571cc90d2e8364a798f5cb495f225c
endef
$(eval $(call Download,dlc))


GO_LDFLAGS += \
	-X '$(PKG_GOGET)/constant.Version=$(PKG_VERSION)' \
	-X '$(PKG_GOGET)/constant.Commit=$(PKG_SOURCE_VERSION)'

GO_TAGS:=-tags="full"
GO_VARS += GO111MODULE=on

define Package/trojan-go/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_DIR) $(1)/opt/etc/trojan-go/example
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/trojan-go $(1)/opt/bin
	$(CP) $(PKG_BUILD_DIR)/src/$(PKG_GOGET)/example/*.json $(1)/opt/etc/trojan-go/example
	$(CP) $(PKG_BUILD_DIR)/src/$(PKG_GOGET)/example/*.yaml $(1)/opt/etc/trojan-go/example
	$(INSTALL_DIR) $(1)/opt/share/trojan-go
	$(CP) $(DL_DIR)/geoip.dat $(1)/opt/share/trojan-go
	$(CP) $(DL_DIR)/geoip-only-cn-private.dat $(1)/opt/share/trojan-go
	$(CP) $(DL_DIR)/dlc.dat $(1)/opt/share/trojan-go/geosite.dat
endef

Package/rclone_nohf/install=$(Package/trojan-go/install)

$(eval $(call BuildPackage,trojan-go))
$(eval $(call BuildPackage,trojan-go_nohf))
