vpath %.c ../
vpath %.c ../../fmdriver
vpath %.c ../../libopna
vpath %.c ../../fmdsp
vpath %.c ../../tonedata
vpath %.c ../../common
vpath %.c ../../fft
vpath %.c ../../pacc
vpath %.rc ..

include ../fmplayer.mak

OBJS=$(addsuffix .o,$(OBJBASE) $(RESBASE))
OBJS+=$(addsuffix .sse.o,$(SSEOBJBASE))
ARCH=i686
PREFIX=$(ARCH)-w64-mingw32-
CC=$(PREFIX)gcc
WINDRES=windres
STRIP=strip
CFLAGS=-std=c99 -O2 -Wall -Wextra -Werror -pedantic -I../.. \
	$(addprefix -D,$(DEFINES)) \
	-march=i586
SSECFLAGS=-mssse3 -O3
LIBS=-s -mwindows -municode \
	$(addprefix -l,$(LIBBASE))
VERSION=`dd bs=6 status=none if=../../versionprint.txt`

$(TARGET):	$(OBJS)
	@echo "  CCLD     $@"
	@$(CC) -o $@ $(OBJS) $(LIBS)
	@$(STRIP) $@

%.o:	%.c
	@echo "  CC       $@"
	@$(CC) $(CFLAGS) -c $< -o $@

%.sse.o:	%.c
	@echo "  CC       $@"
	@$(CC) $(CFLAGS) $(SSECFLAGS) -c $< -o $@

%.o:	%.rc $(ICON) $(MANIFEST)
	@echo "  WINDRES  $@"
	@$(WINDRES) -o $@ -i $<

$(MANIFEST):	$(MANIFEST).in ../../versionprint.txt
	@echo "  VERSION  $(VERSION)"
	sed -e "s/@VER@/$(VERSION)/g" $< > $@

$(ICON):	$(ICONFILES)
	icotool -o $@ -c $^

clean:
	rm -f $(TARGET) $(OBJS)
