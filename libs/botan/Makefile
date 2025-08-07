#
# Copyright (C) 2025
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=botan
PKG_VERSION:=2.19
PKG_RELEASE:=5

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/coolshou/botan.git
# which git commit should we use
PKG_SOURCE_VERSION:=935055e839794a076d209c9e9a1e9cd2255aae01
PKG_BUILD_DIR=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_BUILD_PARALLEL:=0
PKG_USE_MIPS16:=0
PKG_BUILD_DEPENDS:=openssl
PKG_MAINTAINER:=Jimmy

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/version.mk

define Package/botan/Default
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=botan
  TITLE:=botan
  URL:=https://github.com/coolshou/botan.git
  DEPENDS:=+librt +zlib +libstdcpp +libgcc +glib2 +libpthread +libatomic +libopenssl3
endef

define Package/botan
  $(call Package/botan/Default)
  TITLE+=botan
endef

define Package/botan/description
	Botan (Japanese for peony flower) is a cryptography library released under the permissive Simplified BSD license.
endef

define Package/botan/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libbotan.so* $(1)/usr/lib/
endef

define Build/InstallDev
	#$(MAKE) -C $(PKG_BUILD_DIR) install
	$(CP) $(PKG_BUILD_DIR)/libbotan-2.a $(TOOLCHAIN_DIR)/lib/
endef

define Build/Configure
	( cd $(PKG_BUILD_DIR) ; \
		python3 ./configure.py \
		--cpu=$(ARCH) \
		--cc-bin=$(TARGET_CROSS)g++ \
		--ar-command="$(TARGET_CROSS)ar" \
		--ar-options="cqs" \
		--cxxflags="$(TARGET_CFLAGS) $(EXTRA_CFLAGS)" \
		--ldflags="$(TARGET_LDFLAGS) $(EXTRA_LDFLAGS)" \
		--without-sphinx \
		--without-rst2man \
	)
endef

define Build/Compile
	$(MAKE) -j4 -C $(PKG_BUILD_DIR)
endef

$(eval $(call BuildPackage,botan))
