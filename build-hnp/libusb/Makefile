all: download/libusb-1.0.29.tar.bz2
	rm -rf temp build
	mkdir -p temp build
	cd temp && tar xvf ../download/libusb-1.0.29.tar.bz2
	cd temp/libusb-1.0.29 && PKG_CONFIG=/usr/bin/false LIBTOOL=glibtool LIBTOOLIZE=glibtoolize ./configure --prefix=/data/app/base.org/base_1.0 --host aarch64-unknown-linux-musl --enable-shared --disable-static --disable-udev CC=$(OHOS_SDK_HOME)/native/llvm/bin/aarch64-unknown-linux-ohos-clang LD=$(OHOS_SDK_HOME)/native/llvm/bin/ld.lld AR=$(OHOS_SDK_HOME)/native/llvm/bin/llvm-ar RANLIB=$(OHOS_SDK_HOME)/native/llvm/bin/llvm-ranlib STRIP=$(OHOS_SDK_HOME)/native/llvm/bin/llvm-strip
	cd temp/libusb-1.0.29 && make -j $(shell nproc) && make install DESTDIR=$(shell pwd)/build
	mkdir -p ../sysroot
	rm -v build/data/app/base.org/base_1.0/lib/*.la
	$(OHOS_SDK_HOME)/native/llvm/bin/llvm-strip build/data/app/base.org/base_1.0/lib/*.so
	ln -s libusb-1.0 build/data/app/base.org/base_1.0/include/libusb # hdc need this
	cp -rfv build/data/app/base.org/base_1.0/. ../sysroot | tee file.lst

download/libusb-1.0.29.tar.bz2:
	mkdir -p download
	cd download && wget https://github.com/libusb/libusb/releases/download/v1.0.29/libusb-1.0.29.tar.bz2
