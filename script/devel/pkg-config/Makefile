GARNAME = pkg-config
GARVERSION = 0.28
CATEGORIES = devel
MASTER_SITES = http://pkgconfig.freedesktop.org/releases/
DISTFILES = $(DISTNAME).tar.gz 
PATCHFILES = \
	$(DISTNAME)-automake_1.13.patch
LICENSE = GPL2

DESCRIPTION =
define BLURB
endef

DEPENDS = lang/c

CONFIGURE_SCRIPTS = $(WORKSRC)/configure
BUILD_SCRIPTS     = $(WORKSRC)/Makefile
INSTALL_SCRIPTS   = $(WORKSRC)/Makefile

CONFIGURE_ARGS = $(DIRPATHS) --build=$(GARBUILD) --host=$(GARHOST) \
	--enable-shared \
	--disable-static \
	--disable-silent-rules \
	--with-gnu-ld \
	--with-internal-glib

include ../../gar.mk

pre-configure:
	@$(call RUN_AUTOTOOLS)
	@$(call FIX_LIBTOOL,$(WORKSRC),configure)
	@$(MAKECOOKIE)
