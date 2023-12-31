## Process this file with automake to produce Makefile.in
include $(top_srcdir)/glib.mk

# http://people.gnome.org/~walters/docs/build-api.txt
.buildapi-allow-builddir:

ACLOCAL_AMFLAGS = -I m4macros ${ACLOCAL_FLAGS}

SUBDIRS = . m4macros glib gmodule gthread gobject gio
DIST_SUBDIRS = $(SUBDIRS) build

bin_SCRIPTS = glib-gettextize

if OS_LINUX
TESTS += check-abis.sh
endif

AM_CPPFLAGS = 					\
	-DG_LOG_DOMAIN=g_log_domain_glib 	\
	@GLIB_DEBUG_FLAGS@ 			\
	-DG_DISABLE_DEPRECATED 			\
	-DGLIB_COMPILATION

EXTRA_DIST += 			\
	ChangeLog.pre-2-20	\
	ChangeLog.pre-2-18	\
	ChangeLog.pre-2-16	\
	ChangeLog.pre-2-14	\
	ChangeLog.pre-2-12	\
	ChangeLog.pre-2-10	\
	ChangeLog.pre-2-8	\
	ChangeLog.pre-2-6	\
	ChangeLog.pre-2-4	\
	ChangeLog.pre-2-2	\
	ChangeLog.pre-2-0	\
	ChangeLog.pre-1-2	\
	glib-zip.in		\
	NEWS.pre-1-3		\
	acglib.m4		\
	sanity_check		\
	README.commits		\
	README.in		\
	INSTALL.in		\
	README.win32		\
	HACKING			\
	autogen.sh		\
	glib.mk			\
	glib-tap.mk		\
	tap-test		\
	makefile.msc		\
	msvc_recommended_pragmas.h \
	config.h.win32.in	\
        po/po2tbl.sed.in        \
	glib-2.0.pc.in 		\
	gobject-2.0.pc.in 	\
	gmodule-2.0.pc.in	\
	gmodule-export-2.0.pc.in	\
	gmodule-no-export-2.0.pc.in	\
	gthread-2.0.pc.in	\
	gio-2.0.pc.in		\
	gio-unix-2.0.pc.in	\
	gio-windows-2.0.pc.in	\
	check-abis.sh		\
	gtk-doc.make		\
	$(NULL)


# These may be in the builddir too
BUILT_EXTRA_DIST += 		\
	README			\
	INSTALL			\
	ChangeLog		\
	config.h.win32		\
	$(NULL)

CONFIGURE_DEPENDENCIES = acglib.m4

ChangeLog:
	$(AM_V_GEN) if test -d "$(srcdir)/.git"; then \
	  (GIT_DIR=$(top_srcdir)/.git ./missing --run git log GLIB_2_20_0^^.. --stat) | fmt --split-only > $@.tmp \
	  && mv -f $@.tmp $@ \
	  || ($(RM) $@.tmp; \
	      echo Failed to generate ChangeLog, your ChangeLog may be outdated >&2; \
	      (test -f $@ || echo git-log is required to generate this file >> $@)); \
	else \
	  test -f $@ || \
	  (echo A git checkout and git-log is required to generate ChangeLog >&2 && \
	  echo A git checkout and git-log is required to generate this file >> $@); \
	fi

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = glib-2.0.pc gobject-2.0.pc gmodule-2.0.pc gmodule-export-2.0.pc gmodule-no-export-2.0.pc gthread-2.0.pc gio-2.0.pc

if OS_UNIX
pkgconfig_DATA += gio-unix-2.0.pc
endif

if OS_WIN32
pkgconfig_DATA += gio-windows-2.0.pc
endif

$(pkgconfig_DATA): config.status

# build documentation when doing distcheck
DISTCHECK_CONFIGURE_FLAGS = --enable-debug --enable-gtk-doc --enable-man --disable-maintainer-mode

DISTCLEANFILES += config.lt

distclean-local: lcov-clean
	if test $(srcdir) = .; then :; else \
	    rm -f $(BUILT_EXTRA_DIST); \
	fi

.PHONY: files release sanity snapshot ChangeLog

files:
	@files=`ls $(DISTFILES) 2> /dev/null `; for p in $$files; do \
	  echo $$p; \
	done

release:
	rm -rf .deps */.deps
	cd docs && make glib.html
	$(MAKE) distcheck

sanity:
	./sanity_check $(VERSION)

snapshot:
	$(MAKE) dist distdir=$(PACKAGE)-snap`date +"%Y%m%d"`

snapcheck:
	$(MAKE) distcheck distdir=$(PACKAGE)-snap`date +"%Y%m%d"`

dist-hook: $(BUILT_EXTRA_DIST)
	files='$(BUILT_EXTRA_DIST)'; \
	for f in $$files; do \
	  if test -f $$f; then d=.; else d=$(srcdir); fi; \
	  rm -f $(distdir)/$$f && cp $$d/$$f $(distdir) || exit 1; done
