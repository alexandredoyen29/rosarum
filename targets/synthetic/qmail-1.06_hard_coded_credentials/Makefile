TARGET = qmail-1.06_hard_coded_credentials
ORIGINAL_REPO = original

SAFE_TARGET = safe/$(TARGET)
BACKDOORED_TARGET = backdoored/$(TARGET)
GROUND_TRUTH_TARGET = ground-truth/$(TARGET)

all: safe backdoored ground-truth

safe: $(SAFE_TARGET)

backdoored: $(BACKDOORED_TARGET)

ground-truth: $(GROUND_TRUTH_TARGET)

$(SAFE_TARGET): $(ORIGINAL_REPO)
	mkdir -p ./safe/build
	cp -r original/* ./safe/build
	cd ./safe/build && patch -p1 << ../../patches/base.patch
	cd ./safe/vsftpd-3.0.5 && patch -p1 < ../../patches/base.patch
	$(MAKE) -C ./safe/vsftpd-3.0.5
	cp ./safe/vsftpd-3.0.5/vsftpd ./safe/
	cp ./vsftpd.conf ./safe/

preamble:
	mkdir -p $(OUTPUT_DIR)
	rm -rf $(OUTPUT_DIR)/netqmail-1.06 $(OUTPUT_DIR)/checkpassword-0.90
	tar xvf ./original/netqmail-1.06.tar.gz -C $(OUTPUT_DIR)
	patch $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.c ./patches/rosa_qmail-popup.c.patch
	cp ./original/*.sh $(OUTPUT_DIR)/netqmail-1.06
	$(OUTPUT_DIR)/netqmail-1.06/ugo.sh
	tar xvf ./original/checkpassword-0.90.tar.gz -C $(OUTPUT_DIR)
	cp ./original/make-non-instrumented-checkpassword.sh $(OUTPUT_DIR)/checkpassword-0.90
	patch $(OUTPUT_DIR)/checkpassword-0.90/Makefile ./original/checkpassword-Makefile.patch
	patch $(OUTPUT_DIR)/checkpassword-0.90/error.h ./original/checkpassword-error-h.patch
	$(OUTPUT_DIR)/checkpassword-0.90/make-non-instrumented-checkpassword.sh
	cp $(OUTPUT_DIR)/checkpassword-0.90/checkpassword $(OUTPUT_DIR)/
	tar xvf ./Mailbox.tar.gz -C $(OUTPUT_DIR)

clean:
	rm -rf $(OUTPUT_DIR) ./backdoored ./ground-truth ./safe

clean-$(OUTPUT_DIR):
	rm -rf $(OUTPUT_DIR)

$(BACKDOORED_VERSIONS): preamble
	patch $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.c ./patches/backdoored/$(subst backdoored-,,$@)/qmail-popup.c.patch
	$(OUTPUT_DIR)/netqmail-1.06/make-non-instrumented.sh
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-pop3d $(OUTPUT_DIR)/qmail-pop3d
	mkdir -p ./backdoored
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.ori ./backdoored/qmail-popup-$@.ori

$(BACKDOORED_VERSIONS_AFL): preamble
	patch $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.c ./patches/backdoored/$(subst backdoored-,,$(subst -afl,,$@))/qmail-popup.c.patch
	$(OUTPUT_DIR)/netqmail-1.06/make-afl.sh
	mkdir -p ./backdoored
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.lto.afl ./backdoored/qmail-popup-$(subst -afl,,$@).lto.afl
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.lto.cmplog ./backdoored/qmail-popup-$(subst -afl,,$@).lto.cmplog
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.laf-intel.afl ./backdoored/qmail-popup-$(subst -afl,,$@).laf-intel.afl

backdoored:
	for VERSION in $(BACKDOORED_VERSIONS); \
	do \
		make clean-$(OUTPUT_DIR); \
		make $$VERSION; \
	done
	cp $(OUTPUT_DIR)/checkpassword ./backdoored/
	cp $(OUTPUT_DIR)/qmail-pop3d ./backdoored/
	cp -r $(OUTPUT_DIR)/Mailbox ./backdoored/

backdoored-afl:
	for VERSION in $(BACKDOORED_VERSIONS_AFL); \
	do \
		make clean-$(OUTPUT_DIR); \
		make $$VERSION; \
	done

$(GROUND_TRUTH_VERSIONS): preamble
	patch $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.c ./patches/ground-truth/$(subst ground-truth-,,$@)/qmail-popup.c.patch
	$(OUTPUT_DIR)/netqmail-1.06/make-non-instrumented.sh
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-pop3d $(OUTPUT_DIR)/qmail-pop3d
	mkdir -p ./ground-truth
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.ori ./ground-truth/qmail-popup-$@.ori

$(GROUND_TRUTH_VERSIONS_AFL): preamble
	patch $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.c ./patches/ground-truth/$(subst ground-truth-,,$(subst -afl,,$@))/qmail-popup.c.patch
	$(OUTPUT_DIR)/netqmail-1.06/make-afl.sh
	mkdir -p ./ground-truth
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.lto.afl ./ground-truth/qmail-popup-$(subst -afl,,$@).lto.afl
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.lto.cmplog ./ground-truth/qmail-popup-$(subst -afl,,$@).lto.cmplog
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.laf-intel.afl ./ground-truth/qmail-popup-$(subst -afl,,$@).laf-intel.afl

ground-truth:
	for VERSION in $(GROUND_TRUTH_VERSIONS); \
	do \
		make clean-$(OUTPUT_DIR); \
		make $$VERSION; \
	done
	cp $(OUTPUT_DIR)/checkpassword ./ground-truth/
	cp $(OUTPUT_DIR)/qmail-pop3d ./ground-truth/
	cp -r $(OUTPUT_DIR)/Mailbox ./ground-truth/

ground-truth-afl:
	for VERSION in $(GROUND_TRUTH_VERSIONS_AFL); \
	do \
		make clean-$(OUTPUT_DIR); \
		make $$VERSION; \
	done

safe: preamble
	$(OUTPUT_DIR)/netqmail-1.06/make-non-instrumented.sh
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-pop3d $(OUTPUT_DIR)/qmail-pop3d
	mkdir -p ./safe
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.ori ./safe/qmail-popup-$@.ori
	cp $(OUTPUT_DIR)/checkpassword ./safe/
	cp $(OUTPUT_DIR)/qmail-pop3d ./safe/
	cp -r $(OUTPUT_DIR)/Mailbox ./safe/

safe-afl: preamble
	$(OUTPUT_DIR)/netqmail-1.06/make-afl.sh
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-pop3d $(OUTPUT_DIR)/qmail-pop3d
	mkdir -p ./safe
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.lto.afl ./safe/qmail-popup-$(subst -afl,,$@).lto.afl
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.lto.cmplog ./safe/qmail-popup-$(subst -afl,,$@).lto.cmplog
	cp $(OUTPUT_DIR)/netqmail-1.06/qmail-popup.laf-intel.afl ./safe/qmail-popup-$(subst -afl,,$@).laf-intel.afl

all: safe backdoored ground-truth

all-afl: safe-afl backdoored-afl ground-truth-afl