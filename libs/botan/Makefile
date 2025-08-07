#
# Copyright (C) 2025
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=botan2
PKG_VERSION:=2.19
PKG_RELEASE:=5

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/coolshou/botan.git
# which git commit should we use
PKG_SOURCE_VERSION:=935055e839794a076d209c9e9a1e9cd2255aae01
PKG_BUILD_DIR=$(BUILD_DIR)/botan-$(PKG_VERSION)
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_DEPENDS:=

PKG_MAINTAINER:=jimmy

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk
#include $(TARGET_LDFLAGS)/nls.mk

#-include $(if $(DUMP),,./files/qmake.mk)

# not using sstrip here as this fucks up the .so's somehow
STRIP:=/bin/true
RSTRIP:= \
  NM="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)nm" \
  STRIP="$(STRIP)" \
  STRIP_KMOD="$(STRIP)" \
  $(SCRIPT_DIR)/rstrip.sh

define Package/botan2/Default
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=botan2
  TITLE:=Botan2
  URL:=https://github.com/coolshou/botan.git
  DEPENDS:=+librt +zlib +libstdcpp +libgcc +glib2 +libpthread +libatomic +libopenssl3
endef

define Package/botan2
  $(call Package/botan2/Default)
  TITLE+=botan
  DEPENDS:= +openssl
endef

define Package/botan2/description
	Botan (Japanese for peony flower) is a cryptography library released under the permissive Simplified BSD license.
endef

define Build/InstallDev
	$(MAKE) -C $(PKG_BUILD_DIR) install
	$(CP) $(PKG_BUILD_DIR)/libbotan-2.a $(TOOLCHAIN_DIR)/lib/
endef

define Package/botan2/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libbotan.so* $(1)/usr/lib/
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
		--libdir=$(TARGET_LIBDIRS) \
		--without-sphinx \
		--without-rst2man \
	)
endef

define Build/Compile
	TARGET_CC="$(TARGET_CROSS)gcc" \
	TARGET_CXX="$(TARGET_CROSS)g++" \
	TARGET_AR="$(TARGET_CROSS)ar cqs" \
	TARGET_OBJCOPY="$(TARGET_CROSS)objcopy" \
	TARGET_RANLIB="$(TARGET_CROSS)ranlib" \
	TARGET_CFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS)" \
	TARGET_CXXFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS)" \
	TARGET_LDFLAGS="$(TARGET_LDFLAGS) $(EXTRA_LDFLAGS) -ldl -lpthread -lrt" \
	TARGET_INCDIRS="$(TARGET_INCDIRS)" \
	TARGET_LIBDIRS="$(TARGET_LIBDIRS) $(STAGING_DIR)/usr/lib/" \
	STAGING_DIR="$(STAGING_DIR)" \
	STAGING_DIR_HOST="$(STAGING_DIR)/../host" \
	PKG_CONFIG_SYSROOT="$(STAGING_DIR)" \
	TARGET_CPPFLAGS+="-I$(STAGING_DIR)/usr/include/mysql" \
	TARGET_LDFLAGS+="-L$(STAGING_DIR)/usr/lib/mysql -lmysqlclient" \
	CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
	$(MAKE) -j4 -C $(PKG_BUILD_DIR)
endef

$(eval $(call BuildPackage,botan2))
