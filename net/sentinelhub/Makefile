#
# Copyright (C) 2019 dvpn Inc
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sentinelhub
PKG_VERSION:=development
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/sentinel-official/hub.git

PKG_MAINTAINER:=Arjun Nemani <nemaniarjun@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

VERSION:=0.5.0-52-g23bd05c
COMMIT:=23bd05c68a069fe076c09f19d39a106f760ccc23
TM_CORE_SEMVER:=v0.34.9
GO_PKG_TAGS:=$(strip netgo,ledger)

GO_PKG:=github.com/sentinel-official/hub
GO_PKG_BUILD_PKG:=github.com/sentinel-official/hub github.com/bufbuild/buf/cmd/buf github.com/golangci/golangci-lint/cmd/golangci-lint github.com/goware/modvendor
GO_PKG_LD_FLAGS := -s -w \
    -X github.com/cosmos/cosmos-sdk/version.Name=sentinel \
    -X github.com/cosmos/cosmos-sdk/version.AppName=sentinelhub \
    -X github.com/cosmos/cosmos-sdk/version.Version=${VERSION} \
    -X github.com/cosmos/cosmos-sdk/version.Commit=${COMMIT} \
    -X github.com/cosmos/cosmos-sdk/version.BuildTags=${BUILD_TAGS} \
    -X github.com/tendermint/tendermint/version.TMCoreSemVer=$(TM_CORE_SEMVER)

include $(INCLUDE_DIR)/package.mk
include ../../lang/golang/golang-package.mk

define Package/sentinelhub
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Sentinel Hub dvpn
  URL:=https://github.com/nemani/hub
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
endef

define Package/sentinelhub/conffiles
/etc/config/dvpn
endef

define Package/sentinelhub/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))

	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/sentinelhub $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/sentinelhub.config $(1)/etc/config/sentinelhub
endef

define Package/sentinelhub/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
  make install
fi
endef

# define Package/sentinelhub/prerm
# #!/bin/sh
# if [ -z "$${IPKG_INSTROOT}" ]; then
#   sentinelhub uninstall
# fi
# endef

define Package/sentinelhub/description
  Official senthub dvpn
endef

define Package/golang-github-nemani-hub-dev
$(call Package/sentinelhub)
$(call GoPackage/GoSubMenu)
  TITLE+= (source files)
  PKGARCH:=all
endef

define Package/golang-github-nemani-hub-dev/description
$(call Package/sentinelhub/description)

  This package provides the source files for the client/bridge program.
endef

$(eval $(call GoBinPackage,sentinelhub))
$(eval $(call BuildPackage,sentinelhub))
$(eval $(call GoSrcPackage,golang-github-nemani-hub-dev))
$(eval $(call BuildPackage,golang-github-nemani-hub-dev))
