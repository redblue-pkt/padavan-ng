SRC_NAME = mc-4.8.20
SRC_URL = http://ftp.midnight-commander.org/$(SRC_NAME).tar.xz

MC_CFLAGS  += -ffunction-sections -fdata-sections
MC_CFLAGS  += -Os -Wall -g -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64
MC_CFLAGS  += -I$(STAGEDIR)/include

MC_LDFLAGS += -Wl,--gc-sections
MC_LDFLAGS += -L$(STAGEDIR)/lib

MC_LIBS += -ldl -lm -lpthread -lz -lglib-2.0

THISDIR = $(shell pwd)

all: download_test extract_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

download_test:
	( if [ ! -f $(SRC_NAME).tar.xz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).tar.xz $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar -xf $(SRC_NAME).tar.xz; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	autoreconf -fi ; \
	CFLAGS="$(MC_CFLAGS)" LDFLAGS="$(MC_LDFLAGS)" LIBS="$(MC_LIBS)" \
	./configure \
		--prefix=/usr \
		--sysconfdir=/etc_ro \
		--with-homedir=/usr/share/mc \
		--without-gpm-mouse \
		--disable-doxygen-doc \
		--disable-doxygen-dot \
		--disable-doxygen-html \
		--enable-charset \
		--disable-nls \
		--disable-maintainer-mode \
		--disable-dependency-tracking \
		AWK=awk \
		--disable-rpath \
		--disable-static \
		--disable-silent-rules \
		--with-screen=ncurses \
		--without-x \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

romfs:
	mkdir -p $(ROMFSDIR)/usr/share/mc
	mkdir -p $(ROMFSDIR)/usr/share/mc/skins
	mkdir -p $(ROMFSDIR)/usr/share/mc/syntax
	mkdir -p $(ROMFSDIR)/etc_ro/mc
	mkdir -p $(ROMFSDIR)/usr/libexec/mc/extfs.d
	$(ROMFSINST) -p +x $(THISDIR)/$(SRC_NAME)/src/mc /usr/bin/mc
	cp -f $(THISDIR)/$(SRC_NAME)/misc/skins/*.ini $(ROMFSDIR)/usr/share/mc/skins
	cp -f $(THISDIR)/$(SRC_NAME)/misc/syntax/*.syntax $(ROMFSDIR)/usr/share/mc/syntax
	cp -f $(THISDIR)/$(SRC_NAME)/misc/mc.charsets $(ROMFSDIR)/usr/share/mc/mc.charsets
	cp -f $(THISDIR)/$(SRC_NAME)/misc/mc.lib $(ROMFSDIR)/usr/share/mc/mc.lib
	cp -f $(THISDIR)/$(SRC_NAME)/misc/edit.indent.rc $(ROMFSDIR)/etc_ro/mc/edit.indent.rc
	cp -f $(THISDIR)/$(SRC_NAME)/misc/filehighlight.ini $(ROMFSDIR)/etc_ro/mc/filehighlight.ini
	cp -f $(THISDIR)/$(SRC_NAME)/misc/mc.default.keymap $(ROMFSDIR)/etc_ro/mc/mc.keymap
	cp -f $(THISDIR)/$(SRC_NAME)/misc/mc.emacs.keymap $(ROMFSDIR)/etc_ro/mc/mc.emacs.keymap
	cp -f $(THISDIR)/$(SRC_NAME)/misc/mc.ext $(ROMFSDIR)/etc_ro/mc/mc.ext
	cp -f $(THISDIR)/$(SRC_NAME)/misc/mc.menu $(ROMFSDIR)/etc_ro/mc/mc.menu
	cp -f $(THISDIR)/$(SRC_NAME)/misc/mcedit.menu $(ROMFSDIR)/etc_ro/mc/mcedit.menu
	cp -f $(THISDIR)/$(SRC_NAME)/src/vfs/extfs/helpers/sfs.ini $(ROMFSDIR)/etc_ro/mc/sfs.ini
