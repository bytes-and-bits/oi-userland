#
# This file and its contents are supplied under the terms of the
# Common Development and Distribution License ("CDDL"). You may
# only use this file in accordance with the terms of the CDDL.
#
# A full copy of the text of the CDDL should have accompanied this
# source. A copy of the CDDL is also available via the Internet at
# http://www.illumos.org/license/CDDL.
#

#
# Copyright 2020, 2021 David Stes
#


#
# Package Makefile for the "OpenSmalltalk Cog Spur" Squeak Smalltalk V5 system
# See http://squeak.org and http://opensmalltalk.org
#

# opensmalltalk-vm can be built both in 32 and 64bit
# but generally SqueakV5 images are 64bit
BUILD_BITS=64

include ../../../make-rules/shared-macros.mk

# there is no official triplet version for opensmalltalk-vm
# we use 5.0.<n> for VMMaker.oscog-eem.<n>
# sometimes the Stack VM is generated from a different VMMaker as the Cog VM

COMPONENT_NAME=		squeak-5c
COMPONENT_VERSION=	5.0.2934
GIT_TAG=		sun-v5.0.25
PLUGIN_REV=		5.0-202101150901-cog
COMPONENT_SUMMARY=	The Cog Implementation of the Squeak V5 Smalltalk Virtual Machine
COMPONENT_PROJECT_URL=	http://www.squeak.org
COMPONENT_FMRI=		runtime/squeak-5c
COMPONENT_CLASSIFICATION=	Development/Other Languages

# See http://wiki.squeak.org/squeak/933
# See http://wiki.squeak.org/squeak/159
# there's 2 license lines in the manifest, the MIT squeak.license and this one
COMPONENT_LICENSE=	Squeak5
COMPONENT_LICENSE_FILE=	squeak5.license

COMPONENT_SRC=		opensmalltalk-vm-$(GIT_TAG)
COMPONENT_ARCHIVE=	$(COMPONENT_SRC).tar.gz
COMPONENT_ARCHIVE_HASH=	sha256:f1961dafc1d3b45444053d2633408280be2cc37e5ece33f2d5c2e8641c825df1
COMPONENT_ARCHIVE_URL=	https://codeload.github.com/cstes/opensmalltalk-vm/tar.gz/$(GIT_TAG)

TEST_TARGET= $(NO_TESTS)

# some builds seem to be broken with -Bdirect
LD_B_DIRECT=

include $(WS_MAKE_RULES)/common.mk

PKG_OPTIONS += -D PLUGIN_REV="$(PLUGIN_REV)"
PKG_OPTIONS += -DBRANCHID="$(BRANCHID)"

PATH=$(PATH.gnu)

# opensmalltalk configure script checks for plugins files in builddir
COMPONENT_PRE_CONFIGURE_ACTION = ( cp plugins.ext plugins.int $(BUILD_DIR_64) )

# the default CFLAGS.gcc will be -m64 -O3 but this does not work for us
# opensmalltalk code is not compatible with the gcc optimizer -O2 or higher
# also the -g (debug) flag is intentional, perhaps omit-frame-pointer issues
gcc_OPT=		-g -DAIO_DEBUG -DNDEBUG -DDEBUGVM=0 -DCOGMTVM=0

CONFIGURE_SCRIPT= $(SOURCE_DIR)/platforms/unix/config/configure
CONFIGURE_OPTIONS +=	--without-npsqueak
CONFIGURE_OPTIONS +=	--with-vmversion=5.0
CONFIGURE_OPTIONS +=	--disable-dynamicopenssl
CONFIGURE_OPTIONS +=	--without-libtls
CONFIGURE_OPTIONS +=	--with-src=spur64src

CONFIGURE_ENV +=	TARGET_ARCH="-m64"

# the opensmalltalk Makefile does not use DESTDIR
COMPONENT_INSTALL_ARGS=  	ROOT=$(PROTO_DIR)
COMPONENT_BUILD_TARGETS= 	getversion squeak plugins ckformat
COMPONENT_INSTALL_TARGETS= 	install-squeak install-doc install-plugins \
	install-ckformat

# Makefile has no strip target
COMPONENT_POST_INSTALL_ACTION = \
( $(STRIP) $(PROTOUSRLIBDIR64)/squeak/$(PLUGIN_REV)/squeak )

REQUIRED_PACKAGES += x11/library/mesa
REQUIRED_PACKAGES += system/library/dbus
REQUIRED_PACKAGES += library/audio/gstreamer
REQUIRED_PACKAGES += library/libffi
REQUIRED_PACKAGES += system/library/freetype-2
REQUIRED_PACKAGES += x11/library/libxext
REQUIRED_PACKAGES += x11/library/libxevie

# Auto-generated dependencies
REQUIRED_PACKAGES += SUNWcs
REQUIRED_PACKAGES += library/audio/pulseaudio
REQUIRED_PACKAGES += library/desktop/cairo
REQUIRED_PACKAGES += library/desktop/pango
REQUIRED_PACKAGES += library/glib2
REQUIRED_PACKAGES += library/security/openssl
REQUIRED_PACKAGES += system/library
REQUIRED_PACKAGES += system/library/math
REQUIRED_PACKAGES += x11/library/libx11
REQUIRED_PACKAGES += x11/library/libxrender
