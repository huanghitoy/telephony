#
# Copyright (C) 2017 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-quectel
PKG_VERSION:=1.2-20210918
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/etnperlong/asterisk-chan-quectel.git
PKG_SOURCE_VERSION:=ddcc56cb7d1115edf5d735a9a9b8fd42e6229303
PKG_SOURCE_DATE=2021-09-18
PKG_MIRROR_HASH:=ee0734e9bcf0ec7ae1b4ccb6ee08ecb0924231869c2ff6e306afb4a74343fcdf

PKG_FIXUP:=autoreconf

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt
PKG_MAINTAINER:=spacedream <spacedream@tutamail.com>

MODULES_DIR:=/usr/lib/asterisk/modules


include $(INCLUDE_DIR)/package.mk

define Package/asterisk-chan-quectel/Default
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  URL:=https://github.com/wdoekes/asterisk-chan-quectel
  DEPENDS:=+USE_UCLIBC:libiconv-full +kmod-usb-acm +kmod-usb-serial +kmod-usb-serial-option +libusb-1.0 +usb-modeswitch
  TITLE:=Quectel EC25 modem support
endef

define Package/asterisk13-chan-quectel
$(call Package/asterisk-chan-quectel/Default)
  DEPENDS+=asterisk13
  VARIANT:=asterisk13
endef

define Package/asterisk15-chan-quectel
$(call Package/asterisk-chan-quectel/Default)
  DEPENDS+=asterisk15
  VARIANT:=asterisk15
endef

define Package/description/Default
 Asterisk channel driver for Quectel EC25 modem.
endef

Package/asterisk13-chan-quectel/description = $(Package/description/Default)
Package/asterisk15-chan-quectel/description = $(Package/description/Default)

ifeq ($(BUILD_VARIANT),asterisk13)
  CHAN_DONGLE_AST_HEADERS:=$(STAGING_DIR)/usr/include/asterisk-13/include
  CONFIGURE_ARGS+= \
	  --with-astversion=13
endif

ifeq ($(BUILD_VARIANT),asterisk15)
  CHAN_DONGLE_AST_HEADERS:=$(STAGING_DIR)/usr/include/asterisk-15/include
  CONFIGURE_ARGS+= \
	  --with-astversion=15
endif

CONFIGURE_ARGS+= \
	--with-asterisk=$(CHAN_DONGLE_AST_HEADERS)

TARGET_CFLAGS+= \
	-I$(CHAN_DONGLE_AST_HEADERS)

MAKE_FLAGS+=LD="$(TARGET_CC)"

# musl and glibc include their own iconv, but uclibc does not
TARGET_CPPFLAGS+=$(if $(CONFIG_USE_UCLIBC),-I$(STAGING_DIR)/usr/lib/libiconv-full/include)
TARGET_LDFLAGS+=$(if $(CONFIG_USE_UCLIBC),-L$(STAGING_DIR)/usr/lib/libiconv-full/lib -liconv)

# $CHAN_DONGLE_ICONV_INC used by 200-fix-iconv-detection.patch
CONFIGURE_VARS += \
	CHAN_DONGLE_ICONV_INC="$(TOOLCHAIN_DIR)/include $(STAGING_DIR)/usr/lib/libiconv-full/include" \
	DESTDIR="$(MODULES_DIR)" \
	ac_cv_type_size_t=yes \
	ac_cv_type_ssize_t=yes

define Package/conffiles/Default
/etc/asterisk/quectel.conf
endef

Package/asterisk13-chan-quectel/conffiles = $(Package/conffiles/Default)
Package/asterisk15-chan-quectel/conffiles = $(Package/conffiles/Default)

define Package/Install/Default
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/etc/quectel.conf $(1)/etc/asterisk
	$(INSTALL_DIR) $(1)$(MODULES_DIR)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/chan_quectel.so $(1)$(MODULES_DIR)
endef

Package/asterisk13-chan-quectel/install = $(Package/Install/Default)
Package/asterisk15-chan-quectel/install = $(Package/Install/Default)

$(eval $(call BuildPackage,asterisk13-chan-quectel))
$(eval $(call BuildPackage,asterisk15-chan-quectel))
