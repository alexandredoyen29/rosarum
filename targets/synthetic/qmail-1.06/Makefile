TARGET = qmail-popup
ORIGINAL_REPO = original

SAFE_TARGET = safe/$(TARGET)
BACKDOORED_TARGET = backdoored/$(TARGET)
GROUND_TRUTH_TARGET = ground-truth/$(TARGET)

USER := $(shell id | tr " " "\n" | grep uid | cut -d"(" -f2 | cut -d")" -f1)
GROUP := $(shell id | tr " " "\n" | grep gid | cut -d"(" -f2 | cut -d")" -f1)

CC = gcc

all: safe backdoored ground-truth

safe: $(SAFE_TARGET)

backdoored: $(BACKDOORED_TARGET)

ground-truth: $(GROUND_TRUTH_TARGET)

$(SAFE_TARGET): $(ORIGINAL_REPO)
#   No-op.

$(BACKDOORED_TARGET): $(ORIGINAL_REPO)
#   No-op.

$(GROUND_TRUTH_TARGET): $(ORIGINAL_REPO)
#   No-op.

clean:
	rm -rf ./backdoored ./safe ./ground-truth

setup:
#   No-op.

teardown:
#   No-op.

.PHONY: clean all safe backdoored ground-truth setup teardown