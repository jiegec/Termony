include ../utils/Makefrag

all: download/qemu-10.0.2.tar.xz
	rm -rf temp build
	mkdir -p temp build/bin
	cd temp && tar xvf ../download/qemu-10.0.2.tar.xz
	cd temp/qemu-10.0.2 && sed -i 220,220d linux-user/signal.c
	cd temp/qemu-10.0.2 && sed -i 1326,1326d linux-user/signal.c
	cd temp/qemu-10.0.2 && sed -i 7604,7604d linux-user/syscall.c
	cd temp/qemu-10.0.2 && sed -i 11033,11034d linux-user/syscall.c
	cd temp/qemu-10.0.2 && sed -i 1122,1122d linux-user/strace.c
	cd temp/qemu-10.0.2 && sed -i 221,228d linux-user/aarch64/syscall_64.tbl
	cd temp/qemu-10.0.2 && \
	PKG_CONFIG=$(shell which pkg-config) \
	PKG_CONFIG_PATH= \
	PKG_CONFIG_LIBDIR=$(shell pwd)/../sysroot/lib/pkgconfig:$(shell pwd)/../sysroot/share/pkgconfig \
	PKG_CONFIG_SYSROOT_DIR=$(shell pwd)/../sysroot \
	CFLAGS="-D_UAPI_LINUX_VIRTIO_VSOCK_H -D_UAPI_LINUX_VIRTIO_TYPES_H -D_UAPI_LINUX_VIRTIO_RING_H -D_UAPI_LINUX_VIRTIO_PMEM_H -D_UAPI_LINUX_VIRTIO_NET_H -D_UAPI_LINUX_VIRTIO_IOMMU_H -D_UAPI_LINUX_VIRTIO_FS_H -D_UAPI_LINUX_VIRTIO_CONSOLE_H -D_UAPI_LINUX_VIRTIO_CONFIG_H -D_LINUX_SYSINFO_H -UHAVE_OPENAT2_H -UTARGET_NR_mq_open -D__user= -D__force= ${CFLAGS}" \
	 ./configure --target-list=aarch64-linux-user --cross-prefix= --host-cc=cc --disable-kvm --disable-xen --disable-rust --disable-docs --disable-system
	# cd temp/qemu-10.0.2 && rm -r include/standard-headers/drm include/standard-headers/misc include/standard-headers/linux 
	# cd temp/qemu-10.0.2 && ln -s $(OHOS_SDK_HOME)/native/sysroot/usr/include/drm include/standard-headers/drm
	# cd temp/qemu-10.0.2 && ln -s $(OHOS_SDK_HOME)/native/sysroot/usr/include/misc include/standard-headers/misc
	# cd temp/qemu-10.0.2 && ln -s $(OHOS_SDK_HOME)/native/sysroot/usr/include/linux include/standard-headers/linux
	cd temp/qemu-10.0.2 && make -j $(shell nproc) && make install DESTDIR=$(shell pwd)/build
	mkdir -p ../sysroot
	cp -rv ./build/usr/local/. ../sysroot | tee file.lst

download/qemu-10.0.2.tar.xz:
	mkdir -p download
	cd download && wget https://download.qemu.org/qemu-10.0.2.tar.xz
