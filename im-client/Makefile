include ../config.mak

SOFILE=libhime-im-client.so
SOFILEVER=libhime-im-client.so.1.2.4

.SUFFIXES:      .c .o .E .cpp
WALL=-Wall
CFLAGS= -DUNIX=1 $(WALL) $(OPTFLAGS) -I. -I.. -I../IMdkit/include $(GTKINC) \
        -DCLIENT_LIB=1 -DHIME_BIN_DIR=\"$(HIME_BIN_DIR)\" -DUNIX=1 \
        -DHIME_TABLE_DIR=\"$(HIME_TABLE_DIR)\" \
        -DFREEBSD=$(FREEBSD) -fPIC
OBJS = hime-im-client.o im-addr.o hime-conf.o util.o hime-crypt-fpic.o

.cpp.E:
	$(CC) $(CFLAGS) -E -o $@ $<

.c.o:
	$(CC) -c $(CFLAGS) $<
.cpp.o:
	$(CC) -x c -c $(CFLAGS) $<

$(SOFILEVER):   $(OBJS)
	$(CC) $(SO_FLAGS) -Wl,-soname,libhime-im-client.so.1 $(OBJS) -lX11 -o $@ -L/usr/X11R6/lib
	ln -sf $(SOFILEVER) $(SOFILE)
	ln -sf $(SOFILEVER) $(SOFILE).1
	if [ $(GTK_IM) = 'Y' ]; then $(MAKE) -C ../gtk-im; fi

LOCALLIB=/usr/local/$(LIB)

install:
	install -d $(himelibdir)
	install -m 755 $(SOFILEVER) $(himelibdir)
	cd $(himelibdir); rm -f $(SOFILE); ln -s $(SOFILEVER) $(SOFILE); \
	ln -sf $(SOFILEVER) $(SOFILE).1

hime-conf.o: ../hime-conf.cpp
	$(CCX) -c $(CFLAGS) -o $@ $<

util.o: ../util.cpp
	$(CCX) -c $(CFLAGS) -o $@ $<

im-addr.o: ../im-addr.cpp
	$(CCX) -c $(CFLAGS) -o $@ $<

hime-crypt-fpic.o: ../hime-crypt.cpp
	$(CCX) -c $(CFLAGS) -o $@ $<

clean:
	rm -f *.o *.so *.so.* *~ *.E *.db config.mak tags core.* .depend

.depend:
	$(CCX) $(CFLAGS) -MM *.cpp > $@

include .depend
