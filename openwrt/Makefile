
include $(TOPDIR)/rules.mk

PKG_NAME:=evmongoose
PKG_VERSION:=2017-7-9
PKG_RELEASE:=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/zhaojh329/evmongoose.git
PKG_SOURCE_VERSION:=20da6d22d2242c2510ac98cf5e28d564cbc0e046
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz

PKG_CONFIG_DEPENDS:= CONFIG_EVMONGOOSE_SSL_NONE CONFIG_EVMONGOOSE_SSL_OPENSSL CONFIG_EVMONGOOSE_SSL_MBEDTLS CONFIG_EVMONGOOSE_GZIP

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/evmongoose
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=Networking
  TITLE:=An asynchronous, event(libev) based multi-protocol embedded networking library
  URL:=https://github.com/zhaojh329/evmongoose
  DEPENDS:=+libev +lua-ev +EVMONGOOSE_SSL_OPENSSL:libopenssl +EVMONGOOSE_SSL_MBEDTLS:libmbedtls +EVMONGOOSE_GZIP:lua-lzlib
endef

define Package/evmongoose/description
	Evmongoose is an asynchronous, event(libev) based multi-protocol embedded networking library.
	It is based on mongoose and libev implementation and it's support Lua API.
endef

define Package/evmongoose/config
menu "Configuration"
	depends on PACKAGE_evmongoose

	config EVMONGOOSE_GZIP
		bool
		default n
		prompt "Enable HTTP gzip module"
	
	choice
		prompt "Selected SSL library"
		default EVMONGOOSE_SSL_NONE
		
		config EVMONGOOSE_SSL_NONE
			bool "No SSL support"
			
		config EVMONGOOSE_SSL_OPENSSL
			bool "OpenSSL"
			
		config EVMONGOOSE_SSL_MBEDTLS
			bool "mbed TLS"
		
	endchoice
endmenu
endef

CMAKE_OPTIONS = -DHTTPS_SUPPORT=OFF
CMAKE_SOURCE_DIR = $(PKG_BUILD_DIR)/src

ifeq ($(CONFIG_EVMONGOOSE_SSL_OPENSSL),y)
  CMAKE_OPTIONS = -DHTTPS_SUPPORT=ON
endif

ifeq ($(CONFIG_EVMONGOOSE_SSL_MBEDTLS),y)
  CMAKE_OPTIONS = -DHTTPS_SUPPORT=ON -DUSE_MBEDTLS=ON
endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib $(1)/usr/include
	$(CP) $(PKG_BUILD_DIR)/libevmongoose.so $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/src/mongoose.h $(1)/usr/include
endef

define Package/evmongoose/install
	$(INSTALL_DIR) $(1)/usr/lib $(1)/usr/lib/lua
	$(CP) $(PKG_BUILD_DIR)/libevmongoose.so $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/lua/evmongoose.so $(1)/usr/lib/lua
endef

$(eval $(call BuildPackage,evmongoose))
