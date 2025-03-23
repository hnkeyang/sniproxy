include $(TOPDIR)/rules.mk

PKG_NAME:=sniproxy
PKG_VERSION:=1.0.1
PKG_RELEASE:=1

#PKG_SOURCE_URL:=https://github.com/dlundquist/sniproxy/archive/
PKG_SOURCE_URL:=https://github.com/hnkeyang/sniproxy/archive/
PKG_SOURCE:=$(PKG_VERSION).tar.gz
PKG_MD5SUM:=c242487137ecde9fa37aba3ab34e9d2b

PKG_MAINTAINER:=Philipp Kern <pkern@debian.org>

include $(INCLUDE_DIR)/package.mk

define Package/sniproxy
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Transparent TLS and HTTP layer 4 proxy with SNI support
  URL:=https://github.com/dlundquist/sniproxy
  DEPENDS:=+libev +libpcre +libudns
endef

define Package/sniproxy/description
 Transparent TLS and HTTP layer 4 proxy with SNI support
 Proxies incoming HTTP and TLS connections based on the hostname contained in
 the initial request of the TCP session. This enables HTTPS name-based virtual
 hosting to separate backend servers without installing the private key on the
 proxy machine.
endef

define Build/Configure
	( cd $(PKG_BUILD_DIR) && ./autogen.sh )
	$(call Build/Configure/Default)
endef

TARGET_CFLAGS+=-std=c99

PKG_INSTALL:=1
define Package/sniproxy/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/$(PKG_NAME) $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/sniproxy.init $(1)/etc/init.d/sniproxy
endef

define Package/sniproxy/postinst
#!/bin/sh
# check if we are on real system
if [ -z "$${IPKG_INSTROOT}" ]; then
	echo "Enabling rc.d symlink for $(PKG_NAME)"
	/etc/init.d/$(PKG_NAME) enable
fi
exit 0
endef

define Package/sniproxy/prerm
#!/bin/sh
# check if we are on real system
if [ -z "$${IPKG_INSTROOT}" ]; then
	echo "Removing rc.d symlink for $(PKG_NAME)"
	/etc/init.d/$(PKG_NAME) disable
fi
exit 0
endef

$(eval $(call BuildPackage,sniproxy))

