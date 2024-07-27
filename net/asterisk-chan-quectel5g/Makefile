#
# Copyright (C) 2017 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-quectel5g

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/huanghitoy/asterisk-chan-quectel-5g.git
PKG_SOURCE_VERSION:=a5915af36f22dfc6f0eeec20b743e910bb423e1b
PKG_SOURCE_DATE=2024-07-27
PKG_RELEASE:=1
PKG_MIRROR_HASH:=9b478f30865f0c408bf372aea6ae0e6fe2aa831a152ad79d864965d8de076884

PKG_FIXUP:=autoreconf

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt
PKG_MAINTAINER:=Jiri Slachta <jiri@slachta.eu>

MODULES_DIR:=/usr/lib/asterisk/modules

include $(INCLUDE_DIR)/package.mk
# asterisk-chan-quectel needs iconv
include $(INCLUDE_DIR)/nls.mk

define Package/asterisk-chan-quectel5g
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  URL:=https://github.com/IchthysMaranatha/asterisk-chan-quectel
  DEPENDS:=asterisk $(ICONV_DEPENDS) +kmod-usb-acm +kmod-usb-serial +kmod-usb-serial-option +libusb-1.0 +usb-modeswitch +alsa-lib
  TITLE:= Asterisk Mobile Telephony Module for quectel chips
endef

define Package/asterisk-chan-quectel5g/description
 Asterisk channel driver for quectel chips.
endef

CONFIGURE_ARGS+= \
	--with-asterisk=$(STAGING_DIR)/usr/include \
	--with-astversion=18 \
	--enable-debug
#	DESTDIR=/usr/lib/asterisk/modules

ifeq ($(CONFIG_BUILD_NLS),y)
CONFIGURE_ARGS+=--with-iconv=$(ICONV_PREFIX)/include
else
CONFIGURE_ARGS+=--with-iconv=$(TOOLCHAIN_DIR)/include
endif

TARGET_CFLAGS+= \
	-I$(CHAN_quectel_AST_HEADERS)

MAKE_FLAGS+=LD="$(TARGET_CC)"

CONFIGURE_VARS += \
	DESTDIR="$(MODULES_DIR)" \
	ac_cv_type_size_t=yes \
	ac_cv_type_ssize_t=yes

define Package/asterisk-chan-quectel5g/conffiles
/etc/asterisk/quectel5g.conf
endef

define Package/asterisk-chan-quectel5g/install
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/etc/quectel5g.conf $(1)/etc/asterisk
	$(INSTALL_DIR) $(1)$(MODULES_DIR)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/chan_quectel.so $(1)$(MODULES_DIR)
endef

define Package/asterisk-chan-quectel5g/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
  echo
  echo "o-------------------------------------------------------------------o"
  echo "| asterisk-chan-quectel5g note                                         |"
  echo "o-------------------------------------------------------------------o"
  echo "| Adding the \"asterisk\" user to the \"dialout\" group might be        |"
  echo "| required for asterisk to be able to access the quectel.            |"
  echo "o-------------------------------------------------------------=^_^=-o"
  echo
fi
exit 0
endef

$(eval $(call BuildPackage,asterisk-chan-quectel5g))
