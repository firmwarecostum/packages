#
# Copyright (C) 2017 Lucian Cristian <lucian.cristian@gmail.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# updated to work with latest source from abrasive
#

include $(TOPDIR)/rules.mk

PKG_NAME:=imap
PKG_VERSION:=2007f
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://ftp.ntua.gr/pub/net/mail/imap
PKG_HASH:=53e15a2b5c1bc80161d42e9f69792a3fa18332b7b771910131004eb520004a28

PKG_MAINTAINER:=Lucian Cristian <lucian.cristian@gmail.com>

PKG_LICENSE:=GPL-V2
PKG_LICENSE_FILES:=LICENSE.txt

PKG_FIXUP:=autoreconf
PKG_BUILD_DEPENDS:= +libpthread +libopenssl
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/uw-imap
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=University of Washington IMAP toolkit
  URL:=https://www.washington.edu/imap/
endef

define Package/uw-imap/description
    The UW IMAP tookit includes the following:

	c-lient library: an API (application programming interface) used to build email clients and servers,
	including support for IMAP,POP3, SMTP and NNTP protocols and for local mailbox file access on Unix and Windows
endef

define Build/Compile
    $(MAKE) -C $(PKG_BUILD_DIR)/ slx \
		EXTRACFLAGS="$(TARGET_CFLAGS) -fPIC -DREENTRANT -DDISABLE_POP_PROXY" \
		$(MAKE_FLAGS) CC=$(TARGET_CC) EXTRALDFLAGS="$(TARGET_LDFLAGS)" \
		SSLINCLUDE=$(STAGING_DIR)/usr/include/openssl/ SSLCERTS=/etc/ssl/certs
endef

define Build/InstallDev
	$(INSTALL_DIR)	$(1)/usr/lib \
			$(1)/usr/include
	$(CP) $(PKG_BUILD_DIR)/c-client/*.a $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/c-client/*.h $(1)/usr/include/
endef

$(eval $(call BuildPackage,uw-imap))
