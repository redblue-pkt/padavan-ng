SRC_NAME=glib-2.38.2
SRC_URL=https://download.gnome.org/sources/glib/2.38/$(SRC_NAME).tar.xz

CFLAGS += -O2

all: download_test extract_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

download_test:
	( if [ ! -f $(SRC_NAME).tar.xz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).tar.xz $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar zxf $(SRC_NAME).tar.xz; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	echo "glib_cv_va_copy=no" > config.cache; \
	echo "glib_cv___va_copy=yes" >> config.cache; \
	echo "glib_cv_va_val_copy=yes" >> config.cache; \
	echo "ac_cv_func_posix_getpwuid_r=yes" >> config.cache; \
	echo "ac_cv_func_posix_getgrgid_r=yes" >> config.cache; \
	echo "glib_cv_stack_grows=no" >> config.cache; \
	echo "glib_cv_uscore=no" >> config.cache; \
	echo "ac_cv_path_GLIB_GENMARSHAL=/usr/bin/glib-genmarshal" >> config.cache; \
	echo "" >> README; \
	autoreconf -fi ; \
	./configure \
		--prefix= \
		--enable-static \
		--disable-shared \
		--disable-fam \
		--disable-libmount \
		--disable-gtk-doc \
		--disable-gtk-doc-html \
		--with-threads="posix" \
		--with-pcre=internal \
		--cache-file=config.cache \
		--disable-compile-warnings \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

install:
	$(MAKE) -C $(SRC_NAME) install DESTDIR=$(STAGEDIR) ;

romfs:
