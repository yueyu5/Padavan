THISDIR=$(shell pwd)

SRC_VER = 2.87
SRC_NAME = dnsmasq-$(SRC_VER)
SRC_FILE = $(SRC_NAME).tar.gz
SRC_URL := https://thekelleys.org.uk/dnsmasq/$(SRC_FILE)

COPTS := -DHAVE_BROKEN_RTC -DNO_INOTIFY -DNO_AUTH -DNO_LOOP
ifndef CONFIG_IPV6
COPTS += -DNO_IPV6
endif
ifndef CONFIG_USB_SUPPORT
COPTS += -DNO_TFTP
endif
ifneq ($(CONFIG_FIRMWARE_INCLUDE_IPSET),y)
COPTS += -DNO_IPSET
endif

all: download_test extra_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME) COPTS="$(COPTS)"

download_test:
	( if [ ! -f $(SRC_NAME).tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_FILE) $(SRC_URL); \
	fi )
extra_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar xf $(SRC_NAME).tar.gz ; \
		tree ./ ; \
	fi )
config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		touch config_done; \
	fi )
clean:
	$(MAKE) -C $(SRC_NAME) clean
	rm -f dnsmasq

romfs:
	cp $(SRC_NAME)/src/dnsmasq .
	$(STRIP) dnsmasq
	$(ROMFSINST) /usr/sbin/dnsmasq
	$(ROMFSINST) -p +x $(THISDIR)/dnsmasq.sh /usr/bin/dnsmasq.sh
