#
# Copyright (C) 2017 Lucian Cristian
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=nss
PKG_VERSION:=3.33
PKG_RELEASE:=1
PKG_MAINTAINER:=Lucian Cristian <lucian.cristian@gmail.com>
PKG_LICENCE:=GPL-2.0+

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-with-nspr-4.17.tar.gz
PKG_SOURCE_URL:=https://ftp.mozilla.org/pub/security/nss/releases/NSS_3_33_RTM/src/
PKG_HASH:=878d505ec0be577c45990c57eb5d2e5c8696bfa3412bd0fae193b275297bf5c4

PKG_BUILD_PARALLEL:=1

PKG_FIXUP:=libtool

include $(INCLUDE_DIR)/package.mk

x64=--enable-64bit

ifeq ($(ARCH),aarch64)
	conf=$(x64)
else ifeq ($(ARCH),ppc64)
	conf=$(x64)
else ifeq ($(ARCH),x86_64)
	conf=$(x64)
endif

define Build/Compile
	(cd $(PKG_BUILD_DIR)/nss; \
		NSPR_CONFIGURE_OPTS="--build=$(GNU_HOST_NAME) \
				     --host=$(GNU_HOST_NAME) \
				     --target=$(REAL_GNU_TARGET_NAME) \
				     --disable-debug \
				     --enable-optimize \
				     --enable-strip \
				     $(if $(CONFIG_IPV6),--enable-ipv6,) \
				     $(conf)" \
		make nss_build_all NSS_ENABLE_WERROR=0 BUILD_OPT=1 CROSS_COMPILE=1 \
		OS_TARGET=OpenWRT OBJDIR_NAME=build_dir NSS_USE_SYSTEM_SQLITE=1 NSS_DISABLE_GTESTS=1 SEED_ONLY_DEV_URANDOM=1 \
		TARGET=$(TARGET_CC) TARGETCXX=$(TARGET_CXX) CPU_ARCH=$(ARCH) MUSL=$(if $(CONFIG_LIBC_USE_GLIBC),0,1))
endef

define Package/libnss
  SECTION:=libs
  SUBMENU:=SSL
  CATEGORY:=Libraries
  TITLE:=Library Network Security Services (NSS)
  URL:=http://www.mozilla.org/projects/security/pki/nss/
  DEPENDS:=+libpthread +librt +zlib +libstdcpp +libsqlite3
endef

define Package/libnss/description
  Library Network Security Services (NSS)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/libnss
	$(FIND) $(PKG_BUILD_DIR)/nss $(PKG_BUILD_DIR)/nspr -type f -name *.h -exec $(CP) -a {} $(1)/usr/include/libnss/ \;

	$(CP) $(PKG_BUILD_DIR)/nspr/pr/include/md/_linux.cfg $(1)/usr/include/prcpucfg.h

	$(INSTALL_DIR) $(1)/usr/lib
	$(FIND) $(PKG_BUILD_DIR)/nss $(PKG_BUILD_DIR)/nspr -type f -name *.a -exec $(CP) -a {} $(1)/usr/lib/ \;
	$(FIND) $(PKG_BUILD_DIR)/nss $(PKG_BUILD_DIR)/nspr -type f -name *.so -exec $(CP) -a {} $(1)/usr/lib/ \;

	$(INSTALL_DIR) $(1)/usr/include/obsolete
	$(CP) $(PKG_BUILD_DIR)/nspr/pr/include/obsolete/*.h $(1)/usr/include/obsolete/

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) ./files/nss.pc.in $(1)/usr/lib/pkgconfig/nss.pc
	$(SED) 's#EXEC_PREFIX#/usr#g' $(1)/usr/lib/pkgconfig/nss.pc
	$(SED) 's#PREFIX#/usr#g' $(1)/usr/lib/pkgconfig/nss.pc
	$(SED) 's#VERSION#${PKG_VERSION}#g' $(1)/usr/lib/pkgconfig/nss.pc

	$(CP) ./files/nspr.pc.in $(1)/usr/lib/pkgconfig/nspr.pc
	$(SED) 's#EXEC_PREFIX#/usr#g' $(1)/usr/lib/pkgconfig/nspr.pc
	$(SED) 's#PREFIX#/usr#g' $(1)/usr/lib/pkgconfig/nspr.pc
	$(SED) 's#VERSION#${PKG_VERSION}#g' $(1)/usr/lib/pkgconfig/nspr.pc
endef

define Package/libnss/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/nss/lib/nss/build_dir/libnss3.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/nss/lib/smime/build_dir/libsmime3.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/nss/lib/ssl/build_dir/libssl3.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/nss/lib/util/build_dir/libnssutil3.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/nss/lib/softoken/build_dir/libsoftokn3.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/nss/lib/freebl/build_dir/OpenWRT_SINGLE_SHLIB/libfreeblpriv3.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/nspr/build_dir/lib/libc/src/build_dir/libplc4.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/nspr/build_dir/lib/ds/build_dir/libplds4.so $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/nspr/build_dir/pr/src/build_dir/libnspr4.so $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/nss/cmd/certutil/build_dir/certutil $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/nss/cmd/pk12util/build_dir/pk12util $(1)/usr/bin
endef

$(eval $(call BuildPackage,libnss))
