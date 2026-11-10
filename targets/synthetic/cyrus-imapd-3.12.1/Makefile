TARGET = imapd
ORIGINAL_REPO = original

SAFE_TARGET = safe/$(TARGET)
BACKDOORED_TARGET = backdoored/$(TARGET)
GROUND_TRUTH_TARGET = ground-truth/$(TARGET)

all: safe backdoored ground-truth

safe: $(SAFE_TARGET)

backdoored: $(BACKDOORED_TARGET)

ground-truth: $(GROUND_TRUTH_TARGET)

$(SAFE_TARGET): $(ORIGINAL_REPO)
	mkdir -p ./safe/cyrus-imapd-3.12.1
	cp -r ./original/* ./safe/cyrus-imapd-3.12.1
	cd ./safe/cyrus-imapd-3.12.1/ && \
		autoreconf -i && \
		./configure --prefix=$(PWD)/safe/cyrus-imapd-3.12.1/root --enable-static=yes --enable-shared=no && \
		make && \
		make install && \
		cp ./root/libexec/imapd ../imapd

$(BACKDOORED_TARGET): $(ORIGINAL_REPO)
	mkdir -p ./backdoored/cyrus-imapd-3.12.1
	cp -r ./original/* ./backdoored/cyrus-imapd-3.12.1
	cd ./backdoored/cyrus-imapd-3.12.1/ && \
		patch -p1 < ../patches/backdoored.patch && \
		autoreconf -i && \
		./configure --prefix=$(PWD)/backdoored/cyrus-imapd-3.12.1/root --enable-static=yes --enable-shared=no && \
		make && \
		make install && \
		cp ./root/libexec/imapd ../imapd

$(GROUND_TRUTH_TARGET): $(ORIGINAL_REPO)
	mkdir -p ./ground-truth/cyrus-imapd-3.12.1
	cp -r ./original/* ./ground-truth/cyrus-imapd-3.12.1
	cd ./ground-truth/cyrus-imapd-3.12.1/ && \
		patch -p1 < ../patches/ground-truth.patch && \
		autoreconf -i && \
		./configure --prefix=$(PWD)/ground-truth/cyrus-imapd-3.12.1/root --enable-static=yes --enable-shared=no && \
		make && \
		make install && \
		cp ./root/libexec/imapd ../imapd

clean:
	rm -rf ./backdoored ./safe ./ground-truth

setup: $(SAFE_TARGET)
	cp ./imapd.conf /etc/imapd.conf
	cp ./cyrus.conf /etc/cyrus.conf
	mkdir -p /var/lib/cyrus/
	useradd -g mail cyrus
	chown cyrus:mail /var/lib/cyrus/
	./safe/cyrus-imapd-3.12.1/root/sbin/ctl_cyrusdb -r

teardown:
	rm -f /etc/imapd.conf
	rm -f /etc/cyrus.conf
	rm -rf /var/lib/cyrus
	userdel cyrus || true

.PHONY: clean all safe backdoored ground-truth setup teardown