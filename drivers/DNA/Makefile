#
# IMPORTANT IMPORTANT IMPORTANT IMPORTANT IMPORTANT
#
# When you change the versions below also update
#  - ~/PF_RING/PF_RING/package/ubuntu/Makefile
#  - ~/PF_RING/package/rpm/pfring.spec

E1000=e1000-8.0.35
E1000E=e1000e-3.1.0.2
IGB=igb-5.2.18
IXGBE=ixgbe-4.0.3

all: igb ixgbe e1000e e1000

e1000:
#	cd $(E1000)-DNA/src; make

e1000e:
	mkdir /lib/modules/$(shell uname -r)/build/include/linux
	ln -s /lib/modules/$(shell uname -r)/build/include/generated/utsrelease.h /lib/modules/$(shell uname -r)/build/include/linux/utsrelease.h
	ln -s /lib/modules/$(shell uname -r)/build/include/generated/autoconf.h /lib/modules/$(shell uname -r)/build/include/linux/autoconf.h
	cd $(E1000E)-DNA/src; make

igb:
	cd $(IGB)-DNA/src; make

ixgbe:
	cd $(IXGBE)-DNA/src; make

clean:
#	cd $(E1000)-DNA/src; make clean
	sudo rm -rf /lib/modules/$(shell uname -r)/build/include/linux
	cd $(E1000E)-DNA/src; make clean
	cd $(IGB)-DNA/src; make clean
	cd $(IXGBE)-DNA/src; make clean

cleanup:
	sudo rm -rf /var/lib/dkms/*-dna /lib/modules/`uname -r`/extra/*-dna.ko

dkms-deb-virtual:
	/bin/rm -rf tmp
	mkdir tmp
	cp -r pfring-drivers-dna-dkms tmp
	find tmp -name .svn|xargs /bin/rm -rf	
	cd tmp; dpkg-deb -b pfring-drivers-dna-dkms .; mv *.deb ..

dkms-deb: cleanup dkms-deb-virtual
	sudo make -f Makefile.dkms.e1000e deb
	cp /var/lib/dkms/e1000e-dna/*/deb/*.deb .
	sudo make -f Makefile.dkms.igb deb
	cp /var/lib/dkms/igb-dna/*/deb/*.deb .
	sudo make -f Makefile.dkms.ixgbe deb
	cp /var/lib/dkms/ixgbe-dna/*/deb/*.deb .

dkms-rpm: cleanup dkms-rpm-virtual
	sudo make -f Makefile.dkms.e1000e rpm	
	sudo make -f Makefile.dkms.igb rpm
	sudo make -f Makefile.dkms.ixgbe rpm

dkms-rpm-virtual:
	rpmbuild -ba pfring-dna-rpm.spec
	cp ~/rpmbuild/RPMS/noarch/pfring-drivers-dna-dkms-*.noarch.rpm .
