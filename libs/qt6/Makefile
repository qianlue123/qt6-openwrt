#
# Copyright (C) 2013 Riccardo Ferrazzo <f.riccardo87@gmail.com>
# Copyright (C) 2017 Paweł Knioła <pawel.kn@gmail.com>
# Copyright (C) 2019 Yaroslav Petrov <info@lank.me>
# Copyright (C) 2024 Wang Peng <wkissni@gmail.com>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=qt6
PKG_VERSION:=6.6
PKG_RELEASE:=1
PKG_MD5SUM:=91aad0b55cf01be5a08ca9bece490f39

PKG_SOURCE:=qt-everywhere-src-$(PKG_VERSION).$(PKG_RELEASE).tar.xz
PKG_SOURCE_URL:=https://download.qt.io/archive/qt/$(PKG_VERSION)/$(PKG_VERSION).$(PKG_RELEASE)/single
PKG_BUILD_DIR=$(BUILD_DIR)/qt-everywhere-src-$(PKG_VERSION).$(PKG_RELEASE)
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_DEPENDS:=libiconv

PKG_MAINTAINER:=Yaroslav Petrov <info@lank.me>

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

define Package/qt6/Default
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=Qt6
  TITLE:=qt6
  URL:=http://qt-project.org
  DEPENDS:=+librt +zlib +libstdcpp +libpcre16 +glib2 +libpthread +libudev +libatomic +libmysqlclient @!LINUX_2_6
  DEPENDS+=+libpcre2-16
endef

define Package/qt6-core
  $(call Package/qt6/Default)
  TITLE+=core
endef

define Package/qt6-concurrent
  $(call Package/qt6/Default)
  TITLE+=concurrent
  DEPENDS+=+qt6-core
endef

define Package/qt6-network
  $(call Package/qt6/Default)
  TITLE+=network
  DEPENDS+=+qt6-core
endef

define Package/qt6-widgets
  $(call Package/qt6/Default)
  TITLE+=widgets
  DEPENDS+=+qt6-core +qt6-network
endef

define Package/qt6-sql
  $(call Package/qt6/Default)
  TITLE+=sql
  DEPENDS+=+qt6-core
endef

define Package/qt6-xml
  $(call Package/qt6/Default)
  TITLE+=xml
  DEPENDS+=+qt6-core
endef

define Package/qt6-serialport
  $(call Package/qt6/Default)
  TITLE+=serialport
  DEPENDS+=+qt6-core
endef

define Package/qt6-serialbus
  $(call Package/qt6/Default)
  TITLE+=serialbus
  DEPENDS+=+qt6-core +qt6-network +qt6-serialport
endef

define Package/qt6-xmlpatterns
  $(call Package/qt6/Default)
  TITLE+=xmlpatterns
  DEPENDS+=+qt6-core +qt6-network
endef

define Package/qt6-test
  $(call Package/qt6/Default)
  TITLE+=test
  DEPENDS+=+qt6-core
endef

define Package/qt6-location
  $(call Package/qt6/Default)
  TITLE+=location
  DEPENDS+=+qt6-core
endef

define Build/Configure
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/qtbase/lib/fonts
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/qtbase/mkspecs/linux-openwrt-g++
	$(CP) ./files/fonts/* $(PKG_BUILD_DIR)/qtbase/lib/fonts/
	$(CP) ./files/qplatformdefs.h $(PKG_BUILD_DIR)/qtbase/mkspecs/linux-openwrt-g++/qplatformdefs.h
	$(CP) ./files/qmake.conf $(PKG_BUILD_DIR)/qtbase/mkspecs/linux-openwrt-g++/qmake.conf
	$(SED) 's@$$$$(TARGET_CROSS)@$(TARGET_CROSS)@g' $(PKG_BUILD_DIR)/qtbase/mkspecs/linux-openwrt-g++/qmake.conf
	( cd $(PKG_BUILD_DIR) ; \
		TARGET_CC="$(TARGET_CROSS)gcc" \
		TARGET_CXX="$(TARGET_CROSS)g++" \
		TARGET_AR="$(TARGET_CROSS)ar cqs" \
		TARGET_OBJCOPY="$(TARGET_CROSS)objcopy" \
		TARGET_RANLIB="$(TARGET_CROSS)ranlib" \
		TARGET_CFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS)" \
		TARGET_CXXFLAGS="$(TARGET_CFLAGS) $(EXTRA_CFLAGS)" \
		TARGET_LDFLAGS="$(TARGET_LDFLAGS) $(EXTRA_LDFLAGS) -lpthread -lrt" \
		TARGET_INCDIRS="$(TARGET_INCDIRS)" \
		TARGET_LIBDIRS="$(TARGET_LIBDIRS) $(STAGING_DIR)/usr/lib/" \
		CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
		CXXFLAGS= \
		LDFLAGS= \
		./configure \
			-prefix /usr \
			-extprefix $(TOOLCHAIN_DIR) \
			-sysroot $(TOOLCHAIN_DIR) \
			-plugindir /usr/lib/Qt/plugins \
			-xplatform linux-openwrt-g++ \
			-opensource \
			-confirm-license \
			-force-asserts \
			-no-iconv \
			-no-pch \
			-no-rpath \
			-no-strip \
			-no-cups \
			-no-dbus \
			-no-eglfs \
			-no-kms \
			-no-opengl \
			-no-directfb \
			-no-xcb \
			-no-ssl \
			-qt-zlib \
			-qt-freetype \
			-make libs \
			-nomake examples \
			-nomake tests \
			-skip qtdeclarative \
			-skip qtmultimedia \
			-skip activeqt \
			-skip qtdoc \
			-skip qtconnectivity \
			-skip wayland \
			-skip qtscript \
			-sql-mysql \
			-I$(STAGING_DIR)/usr/include/mysql \
			-L$(STAGING_DIR)/usr/lib \
			-v \
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

define Build/InstallDev
	$(MAKE) -C $(PKG_BUILD_DIR) install
	$(CP) $(PKG_BUILD_DIR)/qtbase/bin/qmake $(TOOLCHAIN_DIR)/bin/
	$(CP) $(PKG_BUILD_DIR)/qtbase/bin/qt.conf $(TOOLCHAIN_DIR)/bin/
endef

define Package/qt6-core/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Core.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Core.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Core.la $(1)/usr/lib/
endef

define Package/qt6-concurrent/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Concurrent.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Concurrent.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Concurrent.la $(1)/usr/lib/
endef

define Package/qt6-network/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Network.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Network.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Network.la $(1)/usr/lib/
endef

define Package/qt6-widgets/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/generic/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/platforms/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/imageformats/
	$(INSTALL_DIR) $(1)/usr/lib/fonts/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Gui.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Gui.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Gui.la 	$(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtsvg/lib/libQt6Svg.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtsvg/lib/libQt6Svg.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtsvg/lib/libQt6Svg.la $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Widgets.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Widgets.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Widgets.la $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/plugins/generic/*.so $(1)/usr/lib/Qt/plugins/generic/
	$(CP) $(PKG_BUILD_DIR)/qtbase/plugins/platforms/*.so $(1)/usr/lib/Qt/plugins/platforms/
	$(CP) $(PKG_BUILD_DIR)/qtbase/plugins/imageformats/*.so $(1)/usr/lib/Qt/plugins/imageformats/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/fonts/* $(1)/usr/lib/fonts/
endef

define Package/qt6-sql/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/sqldrivers/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Sql.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Sql.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Sql.la 	$(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/plugins/sqldrivers/*.so $(1)/usr/lib/Qt/plugins/sqldrivers/
endef

define Package/qt6-xml/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Xml.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Xml.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Xml.la 	$(1)/usr/lib/
endef

define Package/qt6-serialport/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialport/lib/libQt6SerialPort.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialport/lib/libQt6SerialPort.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialport/lib/libQt6SerialPort.la $(1)/usr/lib/
endef

define Package/qt6-serialbus/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialbus/lib/libQt6SerialBus.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialbus/lib/libQt6SerialBus.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialbus/lib/libQt6SerialBus.la $(1)/usr/lib/
endef

define Package/qt6-xmlpatterns/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtxmlpatterns/lib/libQt6XmlPatterns.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtxmlpatterns/lib/libQt6XmlPatterns.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtxmlpatterns/lib/libQt6XmlPatterns.la $(1)/usr/lib/
endef

define Package/qt6-test/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Test.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Test.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt6Test.la $(1)/usr/lib/
endef

define Package/qt6-location/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtlocation/lib/libQt6Positioning.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtlocation/lib/libQt6Positioning.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtlocation/lib/libQt6Positioning.la $(1)/usr/lib/
endef

$(eval $(call BuildPackage,qt6-core))
$(eval $(call BuildPackage,qt6-concurrent))
$(eval $(call BuildPackage,qt6-network))
$(eval $(call BuildPackage,qt6-widgets))
$(eval $(call BuildPackage,qt6-sql))
$(eval $(call BuildPackage,qt6-xml))
$(eval $(call BuildPackage,qt6-serialport))
$(eval $(call BuildPackage,qt6-serialbus))
$(eval $(call BuildPackage,qt6-xmlpatterns))
$(eval $(call BuildPackage,qt6-test))
$(eval $(call BuildPackage,qt6-location))
