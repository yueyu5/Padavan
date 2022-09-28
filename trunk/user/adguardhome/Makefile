THISDIR=$(shell pwd)

#SRC_VER = 1.20.2
SRC_NAME = AdGuardHome
#SRC_FILE = $(SRC_NAME).tar.gz
#SRC_URL := http://nginx.org/download/$(SRC_FILE)

all: extra_test config_test
	$(MAKE) -C $(SRC_NAME) ; \
	tree $(SRC_NAME)

extra_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		git clone https://github.com/AdguardTeam/AdGuardHome;
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		touch config_done; \
	fi )

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/adguardhome.sh /usr/bin/adguardhome.sh
