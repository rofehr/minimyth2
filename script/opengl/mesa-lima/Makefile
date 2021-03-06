
# GIT URL: https://gitlab.freedesktop.org/lima/mesa

# Uncomment for downloading current master from 18.2-rc5 branch
# GARNAME      = mesa-lima
# GARVERSION   = 18.2-rc5
# MASTER_SITES = https://gitlab.freedesktop.org/lima/mesa/-/archive/lima-18.2-rc5/
# DISTFILES    = $(DISTNAME).tar.bz2
# WORKSRC      = $(WORKDIR)/mesa-lima-$(GARVERSION)

# Uncomment for downloading at given GIT commit
GARNAME      = mesa-lima
GARVERSION   = 00d67e27d9548eefb9d5e89b896d4b719adeed3b
MASTER_SITES = https://gitlab.freedesktop.org/lima/mesa/-/archive/$(GARVERSION)/
DISTFILES    = mesa-$(GARVERSION).tar.bz2
WORKSRC      = $(WORKDIR)/mesa-$(GARVERSION)

CATEGORIES = $(CATEGORY)
LICENSE = LGPL2/MIT

DESCRIPTION = 
define BLURB
endef

DEPENDS =                      \
	lang/c                 \
	lang/cxx               \
	lib/expat              \
	X11/libvdpau           \
	devel/llvm             \
	devel/libelf           \
	python/python-mako     \
	xorg/xorgproto         \
	xorg/libdrm            \
	xorg/libX11            \
	xorg/libxcb            \
	xorg/libXdamage        \
	xorg/libXext           \
	xorg/libXfixes         \
	xorg/libXxf86vm        \
	xorg/libxshmfence      \
	xorg/wayland-protocols \
	xorg/wayland           \

ifneq ($(DESTIMG),build)
DEPENDS   += system/eudev
endif
BUILDDEPS  = \
	lib/libxml2        \
	python/python      \
	python/python-mako \
	opengl/makedepend  \
	xorg/xorgproto     \
	xorg/libxshmfence  \
	xorg/wayland       \

PWD := $(shell pwd)
CATEGORY := $(shell basename $(shell dirname $(shell pwd)))

CONFIGURE_SCRIPTS  = $(WORKSRC)/configure
BUILD_SCRIPTS      = $(WORKSRC)/Makefile
INSTALL_SCRIPTS    = $(WORKSRC)/Makefile source
ifeq ($(DESTIMG),build)
INSTALL_SCRIPTS   += cross
endif

include ../../gar.mk

CONFIGURE_ARGS = $(DIRPATHS) --build=$(GARBUILD) --host=$(GARHOST) \
	--disable-silent-rules \
	--disable-static \
	--enable-shared \
	--with-dri-driverdir="$(libdir)/dri" \
	--with-vdpau-libdir="$(libdir)" \
	--with-platforms=x11,wayland,drm \
	--enable-llvm \
	--enable-llvm-shared-libs \
	--enable-gbm \
	--enable-shared-glapi \
	--enable-glx \
	--enable-glx-tls \
	--enable-egl \
	--enable-gles1 \
	--enable-gles2 \
	--enable-dri \
	--enable-dri3 \
	LIBUDEV_CFLAGS="$(DESTDIR)$(includedir)" \
	LIBUDEV_LIBS="$(DESTDIR)$(libdir)/libudev.so" \
	--with-wayland-scanner-path="$(build_DESTDIR)$(build_bindir)/wayland-scanner" \

ifeq ($(GARCH),armv7)
CONFIGURE_ARGS += \
	--with-gallium-drivers=exynos,sun4i,meson,rockchip,lima,vc4 \
	--with-dri-drivers=swrast \
	--with-platforms=x11,drm \
	--disable-opencl \
	--disable-opencl-icd \
	--disable-libglvnd \
	--disable-xvmc \
	--disable-vdpau \
	--disable-xa
else
ifeq ($(GARCH),armv8)
CONFIGURE_ARGS += \
	--with-gallium-drivers=exynos,sun4i,meson,rockchip,lima,vc4 \
	--with-dri-drivers= \
	--with-platforms=x11,drm \
	--disable-opencl \
	--disable-opencl-icd \
	--disable-libglvnd \
	--disable-xvmc \
	--disable-vdpau \
	--disable-xa
else
CONFIGURE_ARGS += \
	--with-gallium-drivers=r300,r600,radeonsi,svga,swrast \
	--with-dri-drivers=i915,i965,r200,radeon,swrast \
	--with-platforms=x11,wayland,drm \
	--disable-opencl \
	--disable-opencl-icd \
	--disable-libglvnd \
	--disable-xvmc \
	--enable-vdpau \
	--disable-xa
endif
endif

ifneq ($(DESTIMG),build)
CONFIGURE_ARGS += LDFLAGS="-L$(build_DESTDIR)$(build_libdir)/glibc-nptl/"
endif

# Mesa 11+ has bug with honouring vdpau dir durring configure
#	--with-vdpau-libdir="$(libdir)/vdpau" \

BUILD_ARGS  = \
	HOST_CC="$(build_CC)" \
	HOST_CFLAGS="$(build_CFLAGS)"

CPPFLAGS += -ffat-lto-objects
CXXFLAGS += -ffat-lto-objects
CFLAGS   += -ffat-lto-objects


git//%$(DISTNAME).tar.xz:
	@$(call FETCH_GIT, $*, $(word 2,$(subst -, ,$(GARVERSION))), $(DISTNAME))
	@$(MAKECOOKIE)

pre-configure:
	@$(call RUN_AUTOTOOLS)
	@#xxxx - fixme
	@rm -f $(DESTDIR)$(libdir)/libcrypt.so.1
	@rm -f $(DESTDIR)$(libdir)/libdl.so.2
	@rm -f $(DESTDIR)$(libdir)/libpthread.so.0
	@rm -f $(DESTDIR)$(libdir)/libm.so.6
	@rm -f $(DESTDIR)$(libdir)/libresolv.so.2
	@rm -f $(DESTDIR)$(libdir)/librt.so.1
	@$(MAKECOOKIE)

post-configure:
	@#xxxx - fixme
	@if [ -e $(DESTDIR)$(elibdir64)/libcrypt.so.1   ] ; then rm -f $(DESTDIR)$(libdir)/libcrypt.so.1   ; ln -sr $(DESTDIR)$(elibdir64)/libcrypt.so.1   $(DESTDIR)$(libdir)/libcrypt.so.1   ; fi
	@if [ -e $(DESTDIR)$(elibdir64)/libdl.so.2      ] ; then rm -f $(DESTDIR)$(libdir)/libdl.so.2      ; ln -sr $(DESTDIR)$(elibdir64)/libdl.so.2      $(DESTDIR)$(libdir)/libdl.so.2      ; fi
	@if [ -e $(DESTDIR)$(elibdir64)/libpthread.so.0 ] ; then rm -f $(DESTDIR)$(libdir)/libpthread.so.0 ; ln -sr $(DESTDIR)$(elibdir64)/libpthread.so.0 $(DESTDIR)$(libdir)/libpthread.so.0 ; fi
	@if [ -e $(DESTDIR)$(elibdir64)/libm.so.6       ] ; then rm -f $(DESTDIR)$(libdir)/libm.so.6       ; ln -sr $(DESTDIR)$(elibdir64)/libm.so.6       $(DESTDIR)$(libdir)/libm.so.6       ; fi
	@if [ -e $(DESTDIR)$(elibdir64)/libresolv.so.2  ] ; then rm -f $(DESTDIR)$(libdir)/libresolv.so.2  ; ln -sr $(DESTDIR)$(elibdir64)/libresolv.so.2  $(DESTDIR)$(libdir)/libresolv.so.2  ; fi
	@if [ -e $(DESTDIR)$(elibdir64)/librt.so.1      ] ; then rm -f $(DESTDIR)$(libdir)/librt.so.1      ; ln -sr $(DESTDIR)$(elibdir64)/librt.so.1      $(DESTDIR)$(libdir)/librt.so.1      ; fi
	@$(MAKECOOKIE)

install-source:
	@mkdir -p $(DESTDIR)$(sourcedir)
	@rm -f $(DESTDIR)$(sourcedir)/mesa
	@ln -sf $(PWD)/$(WORKSRC) $(DESTDIR)$(sourcedir)/mesa
	@$(MAKECOOKIE)

install-cross:
	@$(MAKECOOKIE)

post-install:
	@rm -f $(DESTDIR)$(libdir)/libGL.la
	@rm -f $(DESTDIR)$(libdir)/libEGL.la
	@rm -f $(DESTDIR)$(libdir)/libglapi.la
	@rm -f $(DESTDIR)$(libdir)/libgbm.la
	@rm -f $(DESTDIR)$(libdir)/libGLESv2.la
	@rm -f $(DESTDIR)$(libdir)/libGLESv1_CM.la
	@rm -f $(DESTDIR)$(libdir)/libwayland-egl.la
	@rm -f $(DESTDIR)$(libdir)/libxatracker.la
	@rm -f $(DESTDIR)$(libdir)/dri/gallium_drv_video.la
	@rm -f $(DESTDIR)$(libdir)/dri/i915_dri.la
	@rm -f $(DESTDIR)$(libdir)/dri/i965_dri.la
	@rm -f $(DESTDIR)$(libdir)/dri/r200_dri.la
	@rm -f $(DESTDIR)$(libdir)/dri/r300_dri.la
	@rm -f $(DESTDIR)$(libdir)/dri/r600_dri.la
	@rm -f $(DESTDIR)$(libdir)/dri/radeon_dri.la
	@rm -f $(DESTDIR)$(libdir)/dri/radeonsi_dri.la
	@rm -f $(DESTDIR)$(libdir)/dri/swrast_dri.la
	@rm -f $(DESTDIR)$(libdir)/dri/vmwgfx_dri.la
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_r600.la
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_r300.la
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_i915.la
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_swrast.la
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_radeonsi.la
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_vmwgfx.la
	@rm -f $(DESTDIR)$(libdir)/vdpau/libvdpau_r300.la
	@rm -f $(DESTDIR)$(libdir)/vdpau/libvdpau_r600.la
	@rm -f $(DESTDIR)$(libdir)/vdpau/libvdpau_radeonsi.la
	@rm -f $(DESTDIR)$(libdir)/vdpau/libvdpau_softpipe.la
	@rm -f $(DESTDIR)$(libdir)/libvdpau_r300.la
	@rm -f $(DESTDIR)$(libdir)/libvdpau_r600.la
	@rm -f $(DESTDIR)$(libdir)/libvdpau_radeonsi.la
	@rm -f $(DESTDIR)$(libdir)/libvdpau_softpipe.la
ifneq ($(DESTIMG),build)
ifneq (,$(findstring vc4,$(mm_GRAPHICS)))
	@echo "--------------------------------------------------------------------------------"     > $(DESTDIR)/usr/share/supported-vc4-gfx-hardware.txt
	@echo "Broadcom VC4 by $(GARNAME)-$(GARVERSION)"                                            >> $(DESTDIR)/usr/share/supported-vc4-gfx-hardware.txt
	@echo "--------------------------------------------------------------------------------"    >> $(DESTDIR)/usr/share/supported-vc4-gfx-hardware.txt
	@echo "Raspberry PI 2/3"                                                                    >> $(DESTDIR)/usr/share/supported-vc4-gfx-hardware.txt
	@echo " "                                                                                   >> $(DESTDIR)/usr/share/supported-vc4-gfx-hardware.txt
endif
ifneq (,$(findstring meson,$(mm_GRAPHICS)))
	@echo "--------------------------------------------------------------------------------"     > $(DESTDIR)/usr/share/supported-meson-gfx-hardware.txt
	@echo "ARM Mali450 by $(GARNAME)-$(GARVERSION)"                                             >> $(DESTDIR)/usr/share/supported-meson-gfx-hardware.txt
	@echo "--------------------------------------------------------------------------------"    >> $(DESTDIR)/usr/share/supported-meson-gfx-hardware.txt
	@echo "ARM Mali 450"                                                                        >> $(DESTDIR)/usr/share/supported-meson-gfx-hardware.txt
	@echo " "                                                                                   >> $(DESTDIR)/usr/share/supported-meson-gfx-hardware.txt
endif
endif
	@$(MAKECOOKIE)

clean-all:
	@rm -f $(DESTDIR)$(libdir)/libGL*
	@rm -f $(DESTDIR)$(libdir)/libEGL*
	@rm -f $(DESTDIR)$(libdir)/libglapi*
	@rm -f $(DESTDIR)$(libdir)/libgbm*
	@rm -f $(DESTDIR)$(libdir)/libGLESv2*
	@rm -f $(DESTDIR)$(libdir)/libGLESv1_CM*
	@rm -f $(DESTDIR)$(libdir)/libwayland-egl*
	@rm -f $(DESTDIR)$(libdir)/libxatracker*
	@rm -f $(DESTDIR)$(libdir)/dri/gallium_drv_video*
	@rm -f $(DESTDIR)$(libdir)/dri/i915_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/i965_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/r200_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/r300_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/r600_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/radeon_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/radeonsi_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/swrast_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/vmwgfx_dri*
	@rm -f $(DESTDIR)$(libdir)/dri/exynos_dri.so
	@rm -f $(DESTDIR)$(libdir)/dri/lima_dri.so
	@rm -f $(DESTDIR)$(libdir)/dri/meson_dri.so
	@rm -f $(DESTDIR)$(libdir)/dri/rockchip_dri.so
	@rm -f $(DESTDIR)$(libdir)/dri/sun4i-drm_dri.so
	@rm -f $(DESTDIR)$(libdir)/dri/vc4_dri.so
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_r600*
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_r300*
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_i915*
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_swrast*
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_radeonsi*
	@rm -f $(DESTDIR)$(libdir)/gallium-pipe/pipe_vmwgfx*
	@rm -f $(DESTDIR)$(libdir)/vdpau/libvdpau_r300*
	@rm -f $(DESTDIR)$(libdir)/vdpau/libvdpau_r600*
	@rm -f $(DESTDIR)$(libdir)/vdpau/libvdpau_radeonsi*
	@rm -f $(DESTDIR)$(libdir)/vdpau/libvdpau_softpipe*
	@rm -f $(DESTDIR)$(libdir)/libvdpau_r300*
	@rm -f $(DESTDIR)$(libdir)/libvdpau_r600*
	@rm -f $(DESTDIR)$(libdir)/libvdpau_radeonsi*
	@rm -f $(DESTDIR)$(libdir)/libvdpau_softpipe*
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/d3d.pc
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/dri.pc
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/egl.pc
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/gbm.pc
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/gl.pc
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/glesv1_cm.pc
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/glesv2.pc
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/wayland-egl.pc
	@rm -f $(DESTDIR)$(libdir)/pkgconfig/xatracker.pc
	@rm -f $(DESTDIR)$(includedir)/GL/glcorearb.h
	@rm -f $(DESTDIR)$(includedir)/GL/glext.h
	@rm -f $(DESTDIR)$(includedir)/GL/gl.h
	@rm -f $(DESTDIR)$(includedir)/GL/gl_mangle.h
	@rm -f $(DESTDIR)$(includedir)/GL/glxext.h
	@rm -f $(DESTDIR)$(includedir)/GL/glx.h
	@rm -f $(DESTDIR)$(includedir)/GL/glx_mangle.h
	@rm -f $(DESTDIR)$(includedir)/GL/mesa_glinterop.h
	@rm -f $(DESTDIR)$(includedir)/GL/osmesa.h
	@rm -f $(DESTDIR)$(includedir)/GL/wglext.h
	@rm -f $(DESTDIR)$(includedir)/GL/internal/dri_interface.h
	@rm -rf $(DESTDIR)$(includedir)/EGL*
	@rm -rf $(DESTDIR)$(includedir)/GLES*
	@rm -rf $(DESTDIR)$(includedir)/GLES2*
	@rm -rf $(DESTDIR)$(includedir)/GLES3*
	@rm -rf $(DESTDIR)$(includedir)/KHR*
	@rm -rf $(DESTDIR)$(includedir)/d3dadapter*
	@rm -f $(DESTDIR)$(includedir)/gbm.h
	@rm -f $(DESTDIR)$(includedir)/xa_composite.h
	@rm -f $(DESTDIR)$(includedir)/xa_context.h
	@rm -f $(DESTDIR)$(includedir)/xa_tracker.h
	@rm -f $(DESTDIR)/usr/versions/mesa
