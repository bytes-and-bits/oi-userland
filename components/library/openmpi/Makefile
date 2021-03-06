#
# This file and its contents are supplied under the terms of the
# Common Development and Distribution License ("CDDL"), version 1.0.
# You may only use this file in accordance with the terms of version
# 1.0 of the CDDL.
#
# A full copy of the text of the CDDL should have accompanied this
# source.  A copy of the CDDL is also available via the Internet at
# http://www.illumos.org/license/CDDL.
#

#
# Copyright 2013-2018 Aurelien Larcher.  All rights reserved.
#

include ../../../make-rules/shared-macros.mk
MPI_IMPLEMENTATION  = openmpi
include $(WS_MAKE_RULES)/mpi-macros.mk

COMPONENT_NAME =            openmpi/$(COMPILER)
OPENMPI_VERSION_MAJOR =     4.0
COMPONENT_VERSION =         $(OPENMPI_VERSION_MAJOR).1
COMPONENT_REVISION =        3
COMPONENT_FMRI =            library/openmpi/$(COMPILER)
COMPONENT_CLASSIFICATION =  Development/High Performance Computing
COMPONENT_PROJECT_URL =     http://www.open-mpi.org
COMPONENT_SRC_NAME =        openmpi
COMPONENT_SRC =             $(COMPONENT_SRC_NAME)-$(COMPONENT_VERSION)
COMPONENT_ARCHIVE =         $(COMPONENT_SRC).tar.gz
COMPONENT_ARCHIVE_HASH = \
  sha256:e55e213fe09a214ab9f2c722acfd8bf7b39bbc1800e4b7a464d38df15e707f59
COMPONENT_ARCHIVE_URL = \
  http://www.open-mpi.org/software/ompi/v$(OPENMPI_VERSION_MAJOR)/downloads/$(COMPONENT_ARCHIVE)

COMPONENT_SUMMARY =         OpenMPI - High Performance Message Passing Library
COMPONENT_LICENSE_FILE =    $(COMPONENT_SRC_NAME).license
COMPONENT_LICENSE      =    BSD 2-Clause

include $(WS_MAKE_RULES)/prep.mk
include $(WS_MAKE_RULES)/configure.mk
include $(WS_MAKE_RULES)/ips.mk  

PATH=$(PATH.gnu)

unexport SHELLOPTS

CONFIGURE_PREFIX   =    $(MPI_PREFIX)

CONFIGURE_OPTIONS+= --bindir=$(MPI_BINDIR)
CONFIGURE_OPTIONS+= --datadir=$(MPI_DATADIR)
CONFIGURE_OPTIONS+= --docdir=$(MPI_DOCDIR)
CONFIGURE_OPTIONS+= --htmldir=$(MPI_HTMLDIR)
CONFIGURE_OPTIONS+= --includedir=$(MPI_INCDIR)
CONFIGURE_OPTIONS+= --libdir=$(MPI_LIBDIR)
CONFIGURE_OPTIONS+= --mandir=$(MPI_MANDIR)
CONFIGURE_OPTIONS+= --sbindir=$(MPI_SBINDIR)
CONFIGURE_OPTIONS+= --sysconfdir=$(MPI_ETCDIR)
CONFIGURE_OPTIONS+= --enable-ipv6
CONFIGURE_OPTIONS+= --enable-mpi-fortran=usempi
CONFIGURE_OPTIONS+= --enable-mpi-cxx
CONFIGURE_OPTIONS+= --enable-mpi-cxx-seek
CONFIGURE_OPTIONS+= --disable-event-epoll
CONFIGURE_OPTIONS+= --with-wrapper-cflags="$(CC_BITS) -L$(MPI_LIBDIR) -R$(MPI_LIBDIR)"
CONFIGURE_OPTIONS+= --with-wrapper-cxxflags="$(CC_BITS) -L$(MPI_LIBDIR) -R$(MPI_LIBDIR)"
CONFIGURE_OPTIONS+= --with-wrapper-fcflags="$(CC_BITS) -L$(MPI_LIBDIR) -R$(MPI_LIBDIR)"
CONFIGURE_OPTIONS+= --with-wrapper-libs="$(CC_BITS) -L$(MPI_LIBDIR) -R$(MPI_LIBDIR)"
CONFIGURE_OPTIONS+= --without-slurm
CONFIGURE_OPTIONS+= ompi_cv_fortran_ignore_tkr_data=0

build:		$(BUILD_64)

$(BUILD_DIR)/modulefile.%:		files/modulefile
	/bin/sed -e "s#%%BITS%%#$*#g" \
	-e "s#%%COMPONENT_FMRI%%#$(COMPONENT_FMRI)#g" \
	-e "s#%%COMPONENT_VERSION%%#$(COMPONENT_VERSION)#g" \
	-e "s#%%MODULE_CONFLICTS%%#$(MPI_IMPLEMENTATIONS_LIST)#g" \
	-e "s#%%MODULE_INCDIR%%#$(MPI_INCDIR)#g" \
	-e "s#%%MODULE_PREFIX%%#$(MPI_PREFIX.$*)#g" < $< > $@ ;

PROTO_MODULE_PATH = $(PROTO_DIR)$(MODULE_PATH)/$(COMPONENT_NAME)

install:	$(INSTALL_64) $(BUILD_DIR)/modulefile.64
	$(MKDIR) $(PROTO_MODULE_PATH)/64
	$(CP) $(BUILD_DIR)/modulefile.64 $(PROTO_MODULE_PATH)/64/$(COMPONENT_VERSION)

test:	$(TEST_64)


# Build dependencies
REQUIRED_PACKAGES += system/header/header-picl
REQUIRED_PACKAGES += diagnostic/cpu-counters
REQUIRED_PACKAGES += service/picl

# Auto-generated dependencies
REQUIRED_PACKAGES += $(GCC_RUNTIME_PKG)
REQUIRED_PACKAGES += $(GXX_RUNTIME_PKG)
REQUIRED_PACKAGES += developer/hwloc
REQUIRED_PACKAGES += diagnostic/scanpci
REQUIRED_PACKAGES += library/zlib
REQUIRED_PACKAGES += runtime/perl-522
REQUIRED_PACKAGES += system/library
REQUIRED_PACKAGES += system/library/math
