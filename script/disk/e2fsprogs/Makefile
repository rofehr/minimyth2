GARNAME = e2fsprogs
GARVERSION = 1.44.4
CATEGORIES = utils
MASTER_SITES = https://www.kernel.org/pub/linux/kernel/people/tytso/$(GARNAME)/v$(GARVERSION)/
DISTFILES = $(DISTNAME).tar.xz
LICENSE = GPL3

DESCRIPTION = 
define BLURB
endef

DEPENDS = lang/c

CONFIGURE_SCRIPTS = $(WORKSRC)/configure
BUILD_SCRIPTS     = $(WORKSRC)/Makefile
INSTALL_SCRIPTS   = $(WORKSRC)/Makefile

CONFIGURE_ARGS = $(DIRPATHS) --build=$(GARBUILD) --host=$(GARHOST) \
	--disable-silent-rules \
	--disable-rpath \
	--with-gnu-ld \

include ../../gar.mk
