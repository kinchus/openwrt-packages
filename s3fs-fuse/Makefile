#
# Copyright (C) 2020 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#


include $(TOPDIR)/rules.mk

PKG_NAME:=s3fs-fuse
PKG_RELEASE:=1
PKG_VERSION:=1.92
PKG_SOURCE_URL:=https://github.com/s3fs-fuse/s3fs-fuse/archive/refs/tags/
PKG_SOURCE:=v$(PKG_VERSION).tar.gz
PKG_MD5SUM:=66413cd3eef48882b1b90d869b1bfed6

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_BUILD_DEPENDS:= fuse libxml2 mbedtls libgcrypt

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

CONFIGURE_VARS += \
        BUILD_CC="$(TARGET_CC)" \
        HOSTCC="$(HOSTCC)"

CONFIGURE_ARGS += \
        --with-mbedtls

MAKE_FLAGS := CCOPT="$(TARGET_CFLAGS)" INCLS="-I. $(TARGET_CPPFLAGS)"

TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib -lfuse -lxml2 -lmbedtls 

define Package/s3fs-fuse
  	SECTION:=net
  	CATEGORY:=Network
  	SUBMENU:=Filesystem
  	TITLE:=AWS S3 - Fuse filesystem
  	DEPENDS:= \
	  +libstdcpp \
	  +libfuse \
          +libcurl \
	  +libgcrypt \
          +libxml2
endef

define Package/s3fs-fuse/Description
	AWS S3 Fuse filesystem	
endef

define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CC="$(TARGET_CC)" \
		CFLAGS="$(TARGET_CFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/s3fs-fuse/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/s3fs $(1)/usr/bin/s3fs
endef

$(eval $(call BuildPackage,s3fs-fuse))
