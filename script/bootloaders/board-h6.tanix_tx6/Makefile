
GARNAME      = bootfiles-h6.tanix_tx6
GARVERSION   = 2020-11-07
CATEGORIES   = bootloaders
MASTER_SITES = 
DISTFILES    = \
               h6-boot-debug.scr \
               h6-boot-debug.txt \
               h6-boot.scr       \
               h6-boot.txt       \

export mm_U-BOOT_BOARD_TYPE = tanix_tx6_defconfig

WORKSRC      = $(WORKDIR)

LICENSE = GPL2

DESCRIPTION =
define BLURB
endef

BUILD_SCRIPTS   = custom
INSTALL_SCRIPTS = custom

include ../../gar.mk

build-custom:
	@$(MAKE) clean -C ../u-boot-h6
	@$(MAKE) install -C ../u-boot-h6
	@$(MAKECOOKIE)

install-custom:
	@mkdir -p $(DESTDIR)/boot
	@cp -f    $(WORKSRC)/h6-boot-debug.scr                                 $(DESTDIR)/boot/boot-debug.scr
	@cp -f    $(WORKSRC)/h6-boot.scr                                       $(DESTDIR)/boot/boot.scr
	@mkdir -p $(DESTDIR)/boot/h6loaderfiles
	@cp -f    $(DESTDIR)$(libdir)/u-boot/h6/$(mm_U-BOOT_BOARD_TYPE)/u-boot-sunxi-with-spl.bin \
		  $(DESTDIR)/boot/h6loaderfiles/u-boot-sunxi-with-spl.bin

clean-all:
	@rm -f  $(DESTDIR)/boot/boot-debug.scr
	@rm -f  $(DESTDIR)/boot/boot.scr
	@rm -rf $(DESTDIR)/boot/h6loaderfiles*
