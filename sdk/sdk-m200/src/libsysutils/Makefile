include $(SDK_TOP)/config.mk

#Define compiler
CROSS_COMPILE ?= mips-linux-gnu-

CC = $(CROSS_COMPILE)gcc
CPLUSPLUS = $(CROSS_COMPILE)g++
LD = $(CROSS_COMPILE)ld
AR = $(CROSS_COMPILE)ar cr
STRIP = $(CROSS_COMPILE)strip

INCLUDES = -I$(SDK_TOP)/include \
	-I$(shell pwd)/include

CFLAGS = $(INCLUDES) -O2 -Wall -march=mips32r2

all: libsysutils.a

export CFLAGS LDFLAG CC CPLUSPLUS LD AR STRIP SDK_TOP BUILD_DIR Q MAKE

#Modules to be compiled.
MODULES = sysutils_base sysutils_battery sysutils_misc

M_OBJS = $(foreach m, $(MODULES), $(m)/$(m).o)

$(M_OBJS):
	$(foreach m, $(MODULES), $(MAKE) -C $(m);)

libsysutils.a: LDFLAG = -r #-static
libsysutils.a: $(M_OBJS)
	$(AR) $@ $(M_OBJS)
	@echo "  [AR]	$(subst $(BUILD_DIR)/,,$(shell pwd)/$@)"

libsysutils.so: CFLAGS += -shared -fpic
libsysutils.so: LDFLAG = -shared -fpic
libsysutils.so: $(M_OBJS)
	$(LD) $(LDFLAG) -o $@ $(M_OBJS)

.PHONY: clean distclean $(M_OBJS)
clean:
	@find . -type f \( -name '*~' -o -name '*.o' \) -print | xargs rm -f

distclean: clean
	@find . -type f \( -name '*.a' -o -name '*.so' \) -print | xargs rm -f

install:
	@mkdir -p $(INSTALL_DIR)/lib
	@cp libsysutils.* $(INSTALL_DIR)/lib/
