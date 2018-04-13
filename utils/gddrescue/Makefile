#
# Copyright (C) 2018 Lucian Cristian
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gddrescue
PKG_VERSION:=1.22
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)_$(PKG_VERSION).orig.tar.xz
PKG_SOURCE_URL:=http://http.debian.net/debian/pool/main/g/$(PKG_NAME)
PKG_HASH:=a715488f77ae201099ff99360baf7d909bce0fa6c43588e076e830fec93d09f1
PKG_MAINTAINER:=Lucian Cristian <lucian.cristian@gmail.com>

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/gddrescue
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Data recovery tool
  URL:=https://www.gnu.org/software/ddrescue/
  DEPENDS:= +libstdcpp
endef

define Package/gddrescue/description
	GNU ddrescue is a data recovery tool. It copies data from one file
	or block device (hard disc, cdrom, etc) to another, trying to rescue
	the good parts first in case of read errors.
endef

MAKE_FLAGS += \
	prefix="/usr"

define Package/gddrescue/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ddrescue $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ddrescuelog $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,gddrescue))
