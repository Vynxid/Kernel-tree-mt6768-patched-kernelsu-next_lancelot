# SPDX-License-Identifier: GPL-2.0 WITH Linux-syscall-note
#
# (C) COPYRIGHT 2010-2024 ARM Limited. All rights reserved.
#
# This program is free software and is provided to you under the terms of the
# GNU General Public License version 2 as published by the Free Software
# Foundation, and any use by you of this program is subject to the terms
# of such GNU license.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, you can access it online at
# http://www.gnu.org/licenses/gpl-2.0.html.
#
#

KERNEL_SRC ?= /lib/modules/$(shell uname -r)/build
KDIR ?= $(KERNEL_SRC)
M ?= $(shell pwd)

ifeq ($(KDIR),)
    $(error Must specify KDIR to point to the kernel to target))
endif

#
# Default configuration values
#
# Dependency resolution is done through statements as Kconfig
# is not supported for out-of-tree builds.
#
CONFIGS :=
ifeq ($(MALI_KCONFIG_EXT_PREFIX),)
    CONFIG_MALI_MIDGARD ?= m
    ifeq ($(CONFIG_MALI_MIDGARD),m)
        CONFIG_MALI_PLATFORM_NAME ?= "devicetree"
        CONFIG_MALI_TRACE_POWER_GPU_WORK_PERIOD ?= y
        CONFIG_MALI_GATOR_SUPPORT ?= y
        CONFIG_MALI_ARBITRATION ?= n
        CONFIG_MALI_PARTITION_MANAGER ?= n


CONFIG_MALI_MIDGARD ?= m
ifeq ($(CONFIG_MALI_MIDGARD),m)
    CONFIG_MALI_PLATFORM_NAME ?= $(CONFIG_MTK_PLATFORM)
    CONFIG_MALI_GATOR_SUPPORT ?= y
    CONFIG_MALI_ARBITRATION ?= n
    CONFIG_MALI_PARTITION_MANAGER ?= n

    ifneq ($(CONFIG_MALI_NO_MALI),y)
        # Prevent misuse when CONFIG_MALI_NO_MALI!=y
        CONFIG_MALI_REAL_HW ?= y
    else
        CONFIG_MALI_CORESIGHT = n
    endif

    ifeq ($(CONFIG_MALI_MIDGARD_DVFS),y)
        # Prevent misuse when CONFIG_MALI_MIDGARD_DVFS=y
        CONFIG_MALI_DEVFREQ ?= n
    else
        CONFIG_MALI_DEVFREQ ?= y
    endif

    ifeq ($(CONFIG_MALI_DMA_BUF_MAP_ON_DEMAND), y)
        # Prevent misuse when CONFIG_MALI_DMA_BUF_MAP_ON_DEMAND=y
        CONFIG_MALI_DMA_BUF_LEGACY_COMPAT = n
    endif

    ifeq ($(CONFIG_MALI_CSF_SUPPORT), y)
        CONFIG_MALI_CORESIGHT ?= n
    endif

    #
    # Expert/Debug/Test released configurations
    #
    ifeq ($(CONFIG_MALI_EXPERT), y)
        ifeq ($(CONFIG_MALI_NO_MALI), y)
            CONFIG_MALI_REAL_HW = n
            CONFIG_MALI_NO_MALI_DEFAULT_GPU ?= "tMIx"

        ifeq ($(CONFIG_MALI_MIDGARD_DVFS),y)
            # Prevent misuse when CONFIG_MALI_MIDGARD_DVFS=y
            CONFIG_MALI_DEVFREQ ?= n
        else
            CONFIG_MALI_DEVFREQ ?= y
        endif

        ifeq ($(CONFIG_MALI_DMA_BUF_MAP_ON_DEMAND), y)
            # Prevent misuse when CONFIG_MALI_DMA_BUF_MAP_ON_DEMAND=y
            CONFIG_MALI_DMA_BUF_LEGACY_COMPAT = n
        endif

        ifeq ($(CONFIG_MALI_CSF_SUPPORT), y)
            CONFIG_MALI_CORESIGHT ?= n
        endif

        #
        # Expert/Debug/Test released configurations
        #
        ifeq ($(CONFIG_MALI_EXPERT), y)
            ifeq ($(CONFIG_MALI_NO_MALI), y)
                CONFIG_MALI_REAL_HW = n
                CONFIG_MALI_NO_MALI_DEFAULT_GPU ?= "tMIx"

            else
                # Prevent misuse when CONFIG_MALI_NO_MALI=n
                CONFIG_MALI_REAL_HW = y
            endif

            ifneq ($(CONFIG_ANDROID), y)
                CONFIG_PAGE_MIGRATION_SUPPORT ?= y
            endif


            ifeq ($(CONFIG_MALI_HW_ERRATA_1485982_NOT_AFFECTED), y)
                # Prevent misuse when CONFIG_MALI_HW_ERRATA_1485982_NOT_AFFECTED=y
                CONFIG_MALI_HW_ERRATA_1485982_USE_CLOCK_ALTERNATIVE = n
            endif

            ifeq ($(CONFIG_MALI_DEBUG), y)
                CONFIG_MALI_MIDGARD_ENABLE_TRACE ?= y
                CONFIG_MALI_SYSTEM_TRACE ?= y

                ifeq ($(CONFIG_SYNC_FILE), y)
                    CONFIG_MALI_FENCE_DEBUG ?= y
                else
                    CONFIG_MALI_FENCE_DEBUG = n
                endif
            else
                # Prevent misuse when CONFIG_MALI_DEBUG=n
                CONFIG_MALI_MIDGARD_ENABLE_TRACE = n
                CONFIG_MALI_SYSTEM_TRACE = n
                CONFIG_MALI_FENCE_DEBUG = n
            endif
        else
            # Prevent misuse when CONFIG_MALI_EXPERT=n
            CONFIG_MALI_CORESTACK = n
            CONFIG_LARGE_PAGE_SUPPORT = y
            CONFIG_MALI_PWRSOFT_765 = n
            CONFIG_MALI_JOB_DUMP = n
            CONFIG_MALI_NO_MALI = n
            CONFIG_MALI_REAL_HW = y
            CONFIG_MALI_HW_ERRATA_1485982_NOT_AFFECTED = n
            CONFIG_MALI_HW_ERRATA_1485982_USE_CLOCK_ALTERNATIVE = n
            CONFIG_MALI_PRFCNT_SET_SELECT_VIA_DEBUG_FS = n
            CONFIG_MALI_DEBUG = n
            CONFIG_MALI_MIDGARD_ENABLE_TRACE = n
            CONFIG_MALI_SYSTEM_TRACE = n
            CONFIG_MALI_FENCE_DEBUG = n
        endif

        ifeq ($(CONFIG_MALI_DEBUG), y)
            CONFIG_MALI_KUTF ?= y
            ifeq ($(CONFIG_MALI_KUTF), y)
                CONFIG_MALI_KUTF_IRQ_TEST ?= y
                CONFIG_MALI_KUTF_CLK_RATE_TRACE ?= y
                CONFIG_MALI_KUTF_MGM_INTEGRATION_TEST ?= y
                ifeq ($(CONFIG_MALI_DEVFREQ), y)
                    ifeq ($(CONFIG_MALI_NO_MALI), y)
                        CONFIG_MALI_KUTF_IPA_UNIT_TEST ?= y
                    endif
                endif

            else
                # Prevent misuse when CONFIG_MALI_KUTF=n
                CONFIG_MALI_KUTF_IRQ_TEST = n
                CONFIG_MALI_KUTF_CLK_RATE_TRACE = n
                CONFIG_MALI_KUTF_MGM_INTEGRATION_TEST = n
            endif
        else
            # Prevent misuse when CONFIG_MALI_DEBUG=n
            CONFIG_MALI_KUTF = n
            CONFIG_MALI_KUTF_IRQ_TEST = n
            CONFIG_MALI_KUTF_CLK_RATE_TRACE = n
            CONFIG_MALI_KUTF_MGM_INTEGRATION_TEST = n
        endif
    else
        # Prevent misuse when CONFIG_MALI_MIDGARD=n
        CONFIG_MALI_ARBITRATION = n
        CONFIG_MALI_KUTF = n
        CONFIG_MALI_KUTF_IRQ_TEST = n
        CONFIG_MALI_KUTF_CLK_RATE_TRACE = n
        CONFIG_MALI_KUTF_MGM_INTEGRATION_TEST = n
    endif

    # All Mali CONFIG should be listed here
    CONFIGS += \
        CONFIG_MALI_MIDGARD \
        CONFIG_MALI_CSF_SUPPORT \
        CONFIG_MALI_GATOR_SUPPORT \
        CONFIG_MALI_ARBITRATION \
        CONFIG_MALI_PARTITION_MANAGER \
        CONFIG_MALI_REAL_HW \
        CONFIG_MALI_DEVFREQ \
        CONFIG_MALI_MIDGARD_DVFS \
        CONFIG_MALI_DMA_BUF_MAP_ON_DEMAND \
        CONFIG_MALI_DMA_BUF_LEGACY_COMPAT \
        CONFIG_MALI_EXPERT \
        CONFIG_MALI_CORESTACK \
        CONFIG_PAGE_MIGRATION_SUPPORT \
        CONFIG_LARGE_PAGE_SUPPORT \
        CONFIG_MALI_PWRSOFT_765 \
        CONFIG_MALI_JOB_DUMP \
        CONFIG_MALI_NO_MALI \
        CONFIG_MALI_IS_FPGA \
        CONFIG_MALI_HW_ERRATA_1485982_NOT_AFFECTED \
        CONFIG_MALI_HW_ERRATA_1485982_USE_CLOCK_ALTERNATIVE \
        CONFIG_MALI_PRFCNT_SET_PRIMARY \
        CONFIG_MALI_PRFCNT_SET_SECONDARY \
        CONFIG_MALI_PRFCNT_SET_TERTIARY \
        CONFIG_MALI_PRFCNT_SET_SELECT_VIA_DEBUG_FS \
        CONFIG_MALI_DEBUG \
        CONFIG_MALI_MIDGARD_ENABLE_TRACE \
        CONFIG_MALI_SYSTEM_TRACE \
        CONFIG_MALI_FENCE_DEBUG \
        CONFIG_MALI_KUTF \
        CONFIG_MALI_KUTF_IRQ_TEST \
        CONFIG_MALI_KUTF_CLK_RATE_TRACE \
        CONFIG_MALI_KUTF_MGM_INTEGRATION_TEST \
        CONFIG_MALI_XEN \
        CONFIG_MALI_CORESIGHT \
        CONFIG_MALI_TRACE_POWER_GPU_WORK_PERIOD



endif

THIS_DIR := $(dir $(lastword $(MAKEFILE_LIST)))
-include $(THIS_DIR)/../arbitration/Makefile

# MAKE_ARGS to pass the custom CONFIGs on out-of-tree build
#
# Generate the list of CONFIGs and values.
# $(value config) is the name of the CONFIG option.
# $(value $(value config)) is its value (y, m).
# When the CONFIG is not set to y or m, it defaults to n.
MAKE_ARGS := $(foreach config,$(CONFIGS), \
                    $(if $(filter y m,$(value $(value config))), \
                        $(value config)=$(value $(value config)), \
                        $(value config)=n))

ifeq ($(MALI_KCONFIG_EXT_PREFIX),)
    MAKE_ARGS += CONFIG_MALI_PLATFORM_NAME=$(CONFIG_MALI_PLATFORM_NAME)
endif

#
# EXTRA_CFLAGS to define the custom CONFIGs on out-of-tree build
#
# Generate the list of CONFIGs defines with values from CONFIGS.
# $(value config) is the name of the CONFIG option.
# When set to y or m, the CONFIG gets defined to 1.
EXTRA_CFLAGS := $(foreach config,$(CONFIGS), \
                    $(if $(filter y m,$(value $(value config))), \
                        -D$(value config)=1))

ifeq ($(MALI_KCONFIG_EXT_PREFIX),)
    EXTRA_CFLAGS += -DCONFIG_MALI_PLATFORM_NAME='\"$(CONFIG_MALI_PLATFORM_NAME)\"'
    EXTRA_CFLAGS += -DCONFIG_MALI_NO_MALI_DEFAULT_GPU='\"$(CONFIG_MALI_NO_MALI_DEFAULT_GPU)\"'
endif

#
# KBUILD_EXTRA_SYMBOLS to prevent warnings about unknown functions
#
BASE_SYMBOLS =


EXTRA_SYMBOLS += \
    $(BASE_SYMBOLS)

KBUILD_CFLAGS += -Wall -Werror

# The following were added to align with W=1 in scripts/Makefile.extrawarn
# from the Linux source tree (v5.18.14)
KBUILD_CFLAGS += -Wextra -Wunused -Wno-unused-parameter
KBUILD_CFLAGS += -Wmissing-declarations
KBUILD_CFLAGS += -Wmissing-format-attribute
KBUILD_CFLAGS += -Wmissing-prototypes
KBUILD_CFLAGS += -Wold-style-definition
# The -Wmissing-include-dirs cannot be enabled as the path to some of the
# included directories change depending on whether it is an in-tree or
# out-of-tree build.
KBUILD_CFLAGS += $(call cc-option, -Wunused-but-set-variable)
KBUILD_CFLAGS += $(call cc-option, -Wunused-const-variable)
KBUILD_CFLAGS += $(call cc-option, -Wpacked-not-aligned)
KBUILD_CFLAGS += $(call cc-option, -Wstringop-truncation)
# The following turn off the warnings enabled by -Wextra
KBUILD_CFLAGS += -Wno-sign-compare
KBUILD_CFLAGS += -Wno-shift-negative-value
# This flag is needed to avoid build errors on older kernels
KBUILD_CFLAGS += $(call cc-option, -Wno-cast-function-type)

KBUILD_CPPFLAGS += -DKBUILD_EXTRA_WARN1

# The following were added to align with W=2 in scripts/Makefile.extrawarn
# from the Linux source tree (v5.18.14)
KBUILD_CFLAGS += -Wdisabled-optimization
# The -Wshadow flag cannot be enabled unless upstream kernels are
# patched to fix redefinitions of certain built-in functions and
# global variables.
KBUILD_CFLAGS += $(call cc-option, -Wlogical-op)
KBUILD_CFLAGS += -Wmissing-field-initializers
# -Wtype-limits must be disabled due to build failures on kernel 5.x
KBUILD_CFLAGS += -Wno-type-limits
KBUILD_CFLAGS += $(call cc-option, -Wmaybe-uninitialized)
KBUILD_CFLAGS += $(call cc-option, -Wunused-macros)
# The following ensures the stack frame does not get larger than a page
KBUILD_CFLAGS += -Wframe-larger-than=4096

KBUILD_CPPFLAGS += -DKBUILD_EXTRA_WARN2

# This warning is disabled to avoid build failures in some kernel versions
KBUILD_CFLAGS += -Wno-ignored-qualifiers

ifeq ($(CONFIG_GCOV_KERNEL),y)
    KBUILD_CFLAGS += $(call cc-option, -ftest-coverage)
    KBUILD_CFLAGS += $(call cc-option, -fprofile-arcs)
    EXTRA_CFLAGS += -DGCOV_PROFILE=1
endif

ifeq ($(CONFIG_MALI_KCOV),y)
    KBUILD_CFLAGS += $(call cc-option, -fsanitize-coverage=trace-cmp)
    EXTRA_CFLAGS += -DKCOV=1
    EXTRA_CFLAGS += -DKCOV_ENABLE_COMPARISONS=1
endif

all:
	$(MAKE) -C $(KDIR) M=$(M) $(MAKE_ARGS) EXTRA_CFLAGS="$(EXTRA_CFLAGS)" KBUILD_EXTRA_SYMBOLS="$(EXTRA_SYMBOLS)" modules

modules_install:
	$(MAKE) -C $(KDIR) M=$(M) $(MAKE_ARGS) modules_install

clean:
	$(MAKE) -C $(KDIR) M=$(M) $(MAKE_ARGS) clean
