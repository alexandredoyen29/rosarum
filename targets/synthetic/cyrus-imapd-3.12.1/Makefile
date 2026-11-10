.DEFAULT_GOAL := all

.PHONY: safe safe-afl backdoored backdoored-afl ground-truth ground-truth-afl clean all all-afl

safe:
	rm -rf build/
	mkdir -p build/
	mkdir -p safe/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		autoreconf -i && \
		./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		make && \
		make install && \
		cp ./root/libexec/imapd ../safe/imapd.ori
	cp ./imapd.conf /etc/imapd.conf
	cp ./cyrus.conf /etc/cyrus.conf
	mkdir -p /var/lib/cyrus/
	chown cyrus:mail /var/lib/cyrus/
	./build/root/sbin/ctl_cyrusdb -r

safe-afl:
	rm -rf build/
	mkdir -p build/
	mkdir -p safe/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		CC=afl-clang-lto autoreconf -i && \
		CC=afl-clang-lto ./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		CC=afl-clang-lto make ; \
		CC=afl-clang-lto make install ; \
		cp ./imap/imapd ../safe/imapd.lto.afl
	rm -rf build/
	mkdir -p build/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto autoreconf -i && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto ./configure --prefix=$(PWD)/buid/root --enable-static=yes --enable-shared=no && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto make ; \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto make install ; \
		cp ./imap/imapd ../safe/imapd.lto.cmplog
	rm -rf build/
	mkdir -p build/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast autoreconf -i && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast ./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast make ; \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast make install ; \
		cp ./root/libexec/imapd ../safe/imapd.laf-intel.afl
	cp ./imapd.conf /etc/imapd.conf
	cp ./cyrus.conf /etc/cyrus.conf
	mkdir -p /var/lib/cyrus/
	chown cyrus:mail /var/lib/cyrus/
	./build/root/sbin/ctl_cyrusdb -r

backdoored:
	rm -rf build/
	mkdir -p build/
	mkdir -p backdoored/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		patch -p1 < ../patches/backdoored.patch && \
		autoreconf -i && \
		./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		make && \
		make install && \
		cp ./root/libexec/imapd ../backdoored/imapd.ori
	cp ./imapd.conf /etc/imapd.conf
	cp ./cyrus.conf /etc/cyrus.conf
	mkdir -p /var/lib/cyrus/
	chown cyrus:mail /var/lib/cyrus/
	./build/root/sbin/ctl_cyrusdb -r

backdoored-afl:
	rm -rf build/
	mkdir -p build/
	mkdir -p backdoored/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		patch -p1 < ../patches/backdoored.patch && \
		CC=afl-clang-lto autoreconf -i && \
		CC=afl-clang-lto ./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		CC=afl-clang-lto make ; \
		CC=afl-clang-lto make install ; \
		cp ./imap/imapd ../backdoored/imapd.lto.afl
	rm -rf build/
	mkdir -p build/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		patch -p1 < ../patches/backdoored.patch && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto autoreconf -i && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto ./configure --prefix=$(PWD)/buid/root --enable-static=yes --enable-shared=no && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto make ; \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto make install ; \
		cp ./imap/imapd ../backdoored/imapd.lto.cmplog
	rm -rf build/
	mkdir -p build/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		patch -p1 < ../patches/backdoored.patch && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast autoreconf -i && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast ./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast make ; \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast make install ; \
		cp ./imap/imapd ../backdoored/imapd.laf-intel.afl
	cp ./imapd.conf /etc/imapd.conf
	cp ./cyrus.conf /etc/cyrus.conf
	mkdir -p /var/lib/cyrus/
	chown cyrus:mail /var/lib/cyrus/
	./build/root/sbin/ctl_cyrusdb -r

ground-truth:
	rm -rf build/
	mkdir -p build/
	mkdir -p ground-truth/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		patch -p1 < ../patches/ground-truth.patch && \
		autoreconf -i && \
		./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		make && \
		make install && \
		cp ./root/libexec/imapd ../ground-truth/imapd.ori
	cp ./imapd.conf /etc/imapd.conf
	cp ./cyrus.conf /etc/cyrus.conf
	mkdir -p /var/lib/cyrus/
	chown cyrus:mail /var/lib/cyrus/
	./build/root/sbin/ctl_cyrusdb -r

ground-truth-afl:
	rm -rf build/
	mkdir -p build/
	mkdir -p ground-truth/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		patch -p1 < ../patches/ground-truth.patch && \
		CC=afl-clang-lto autoreconf -i && \
		CC=afl-clang-lto ./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		CC=afl-clang-lto make ; \
		CC=afl-clang-lto make install ; \
		cp ./imap/imapd ../ground-truth/imapd.lto.afl
	rm -rf build/
	mkdir -p build/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		patch -p1 < ../patches/ground-truth.patch && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto autoreconf -i && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto ./configure --prefix=$(PWD)/buid/root --enable-static=yes --enable-shared=no && \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto make ; \
		AFL_LLVM_CMPLOG=1 CC=afl-clang-lto make install ; \
		cp ./imap/imapd ../ground-truth/imapd.lto.cmplog
	rm -rf build/
	mkdir -p build/
	cp -r original/* build/
	cd build/ && \
		patch -p1 < ../patches/rosa_imapd.c.patch && \
		patch -p1 < ../patches/ground-truth.patch && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast autoreconf -i && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast ./configure --prefix=$(PWD)/build/root --enable-static=yes --enable-shared=no && \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast make ; \
		AFL_LLVM_LAF_ALL=1 CC=afl-clang-fast make install ; \
		cp ./imap/imapd ../ground-truth/imapd.laf-intel.afl
	cp ./imapd.conf /etc/imapd.conf
	cp ./cyrus.conf /etc/cyrus.conf
	mkdir -p /var/lib/cyrus/
	chown cyrus:mail /var/lib/cyrus/
	./build/root/sbin/ctl_cyrusdb -r

clean:
	rm -rf ./build ./backdoored ./safe ./ground-truth
	rm -f /etc/imapd.conf
	rm -f /etc/cyrus.conf
	rm -rf /var/lib/cyrus/

all: safe backdoored ground-truth

all-afl: safe-afl backdoored-afl ground-truth-afl