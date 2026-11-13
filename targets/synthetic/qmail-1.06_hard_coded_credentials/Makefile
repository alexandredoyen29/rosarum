TARGET = qmail-1.06_hard_coded_credentials
ORIGINAL_REPO = original

SAFE_TARGET = safe/$(TARGET)
BACKDOORED_TARGET = backdoored/$(TARGET)
GROUND_TRUTH_TARGET = ground-truth/$(TARGET)

USER := $(shell id | tr " " "\n" | grep uid | cut -d"(" -f2 | cut -d")" -f1)
GROUP := $(shell id | tr " " "\n" | grep gid | cut -d"(" -f2 | cut -d")" -f1)

all: safe backdoored ground-truth

safe: $(SAFE_TARGET)

backdoored: $(BACKDOORED_TARGET)

ground-truth: $(GROUND_TRUTH_TARGET)

$(SAFE_TARGET): $(ORIGINAL_REPO)
	mkdir -p ./safe/build
	cp -r original/* ./safe/build
	cd ./safe/build && patch -p1 < ../../patches/base.patch
	cd ./safe/build/checkpassword-0.90 && \
		make clean && \
		CC=gcc make && \
		cp checkpassword ../..
	sed -re "s/%USER/$(USER)/g" -i ./safe/build/netqmail-1.06/conf-users
	sed -re "s/%GROUP/$(GROUP)/g" -i ./safe/build/netqmail-1.06/conf-groups
	cd ./safe/build/netqmail-1.06 && \
		make clean && \
		CC=gcc make && \
		cp qmail-popup ../.. && \
		cp qmail-pop3d ../..
