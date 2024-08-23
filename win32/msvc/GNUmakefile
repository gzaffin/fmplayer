vpath %.c ../
vpath %.c ../../fmdriver
vpath %.c ../../libopna
vpath %.c ../../fmdsp
vpath %.c ../../tonedata
vpath %.c ../../common
vpath %.c ../../fft
vpath %.c ../../pacc
vpath %.rc ..

WINDOWS_OS_MSVC = 1
ICOTOOL_PATH=C:/Users/gzaff/Devs/png2ico/build/Release
ICOTOOL=png2ico.exe

VERSION=$(shell TYPE ..\..\versionprint.txt)
SSE2_SUPPORTED=$(shell .\..\..\InstructionSet.exe | sed -n "/^SSE2 supported/p")

include ../fmplayer.mak

OBJS=$(addsuffix .obj,$(OBJBASE))
OBJS+=$(addsuffix .res,$(RESBASE))
OBJS+=$(addsuffix .sse.obj,$(SSEOBJBASE))
CC=cl /nologo
RC=rc /nologo
CFLAGS=/W2 /Os /Oi /MT /I.. /I..\.. \
       $(addprefix /D,$(DEFINES))
LIBS=$(addsuffix .lib,$(LIBBASE)) \
     /link /SUBSYSTEM:WINDOWS

ifneq ($(SSE2_SUPPORTED),)
CFLAGS+=/DSSE2_SUPPORTED
endif

$(TARGET):	$(OBJS)
	@echo   VERSION  $(VERSION)
	@echo   $(SSE2_SUPPORTED)
	@echo   CCLD     $@
	@$(CC) $(LDFLAGS) /Fe$@ $(OBJS) $(LIBS)

%.obj:	%.c
	@echo   CC       $@
	@$(CC) $(CFLAGS) /c $< /Fo$@

%.sse.obj:	%.c
	@echo   CC       $@
	@$(CC) $(CFLAGS) $(SSECFLAGS) /c $< /Fo$@

%.res:	%.rc $(ICON) $(MANIFEST)
	@echo   RC       $@
	@$(RC) /fo$@ $<

$(MANIFEST):    $(MANIFEST).in
	@echo   VERSION  $(VERSION)
	sed -e "s/@VER@/$(VERSION)/g" $< > $@

$(ICON):	$(ICONFILES)
	$(ICOTOOL_PATH)/$(ICOTOOL) $@ $^

clean:
	@echo   VERSION  $(VERSION)
	@echo   $(SSE2_SUPPORTED)
	del $(TARGET) $(OBJS) $(MANIFEST)
