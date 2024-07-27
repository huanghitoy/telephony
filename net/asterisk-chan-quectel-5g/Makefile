#
# Copyright (C) 2017 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk-chan-quectel-5g
PKG_VERSION:=3.6
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/huanghitoy/asterisk-chan-quectel-5g.git
PKG_SOURCE_VERSION:=b3534eaedce52819ee3f61db6d191ed35177b955
PKG_SOURCE_DATE=2024-07-27
PKG_MIRROR_HASH:=50a69010c61383cbedd102a290f39abc6d74b760fb0d4d1bd997ad38a7cf07d9

PKG_FIXUP:=autoreconf

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt
PKG_MAINTAINER:=Jiri Slachta <jiri@slachta.eu>

MODULES_DIR:=/usr/lib/asterisk/modules

include $(INCLUDE_DIR)/package.mk
# asterisk-chan-quectel needs iconv
include $(INCLUDE_DIR)/nls.mk

define Package/asterisk-chan-quectel-5g
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Utilities
  URL:=https://github.com/huanghitoy/asterisk-chan-quectel-5g
  DEPENDS:=asterisk +libiconv-full +kmod-usb-acm +kmod-usb-serial +kmod-usb-serial-option +libusb-1.0 +usb-modeswitch +alsa-lib
  TITLE:=Quectel RM500U-CN RM500Q modem support
endef

define Package/asterisk-chan-quectel-5g/description
 Asterisk channel driver for Quectel RM500U-CN RM500Q modem.
endef

CONFIGURE_ARGS+= \
	--with-asterisk=$(STAGING_DIR)/usr/include \
	--with-astversion=20 \
	--with-iconv=$(ICONV_PREFIX)/include

TARGET_CFLAGS+= \
	-I$(CHAN_quectel_AST_HEADERS)

MAKE_FLAGS+=LD="$(TARGET_CC)"

CONFIGURE_VARS += \
	DESTDIR="$(MODULES_DIR)" \
	ac_cv_type_size_t=yes \
	ac_cv_type_ssize_t=yes

define Package/asterisk-chan-quectel-5g/conffiles
	/etc/asterisk/quectel5g.conf
endef

define Package/asterisk-chan-quectel-5g/install
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/etc/quectel.conf $(1)/etc/asterisk
	$(INSTALL_DIR) $(1)$(MODULES_DIR)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/chan_quectel.so $(1)$(MODULES_DIR)
endef

$(eval $(call BuildPackage,asterisk-chan-quectel-5g))
