#
# Copyright (C) 2016 Intel Corporation
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=directconnect-dp
PKG_VERSION:=1.2.1
PKG_SOURCE_PROTO:=git
PKG_RELEASE:=1

PKG_SOURCE_VERSION:=ugw_8.5.2.10
PKG_SOURCE_URL:=https://gitlab.com/prpl-foundation/intel/directconnect_dp.git
PKG_SOURCE_PROTO:=git
#PKG_SOURCE_MIRROR:=1
PKG_MIRROR:=0
PKG_MIRROR_HASH:=15d7fd8a2b0a4076e2994c0a2b9046bf55e25281354b2efa13e429916112d2df

include $(INCLUDE_DIR)/package.mk

define KernelPackage/$(PKG_NAME)
  SUBMENU:=Intel
  TITLE:=DirectConnect datapath driver
  KCONFIG:= \
	CONFIG_DIRECTCONNECT_DP_API=y
  FILES:= \
	$(PKG_BUILD_DIR)/common/directconnect_datapath.ko \
	$(PKG_BUILD_DIR)/dc_mode/dc_mode-hw.ko
  AUTOLOAD:=$(call AutoProbe, \
		directconnect_datapath \
		dc_mode-hw)
  MENU:=1
endef

define KernelPackage/$(PKG_NAME)/description
  DirectConnect datapath driver
endef

EXTRA_CFLAGS += -DCONFIG_DIRECTCONNECT_DP_DBG
EXTRA_CFLAGS += -I$(PKG_BUILD_DIR)/include

ifeq ($(CONFIG_PACKAGE_kmod-ppa-drv),y)
	EXTRA_CFLAGS += -DCONFIG_DIRECTCONNECT_DP_LITEPATH
	PKG_EXTRA_KCONFIG += CONFIG_DIRECTCONNECT_DP_LITEPATH=y
endif

ifeq ($(CONFIG_TARGET_intel_mips_xrx500),y)
	EXTRA_CFLAGS += -DHAVE_DATAPATH_API
	PKG_EXTRA_KCONFIG += CONFIG_DIRECTCONNECT_DP_XRX500=y
endif

ifeq ($(CONFIG_TARGET_intel_x86_lgm),y)
	EXTRA_CFLAGS += -DHAVE_DATAPATH_EXT_API
	PKG_EXTRA_KCONFIG += CONFIG_DIRECTCONNECT_DP_LGM=y
endif

ifeq ($(CONFIG_TARGET_intel_mips_prx300),y)
	EXTRA_CFLAGS += -DHAVE_DATAPATH_EXT_API
	PKG_EXTRA_KCONFIG += CONFIG_DIRECTCONNECT_DP_PRX300=y
endif

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR)/include/net/
	$(CP) $(PKG_BUILD_DIR)/include/*.h $(STAGING_DIR)/include/net/
endef

KERNEL_MAKEOPTS += \
	$(PKG_EXTRA_KCONFIG) \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)"

define Build/Compile
	$(KERNEL_MAKE) SUBDIRS="$(PKG_BUILD_DIR)" modules
endef

$(eval $(call KernelPackage,$(PKG_NAME)))
