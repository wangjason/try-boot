
ifeq ($(ARCH),i386)
LOAD_ADDR = 0x40000
endif

ifeq ($(ARCH),arm)
LOAD_ADDR = 0xc100000
endif

include $(TOPDIR)/config.mk

ELF	= hello_world
SREC	= hello_world.srec
BIN	= hello_world.bin

ifeq ($(CPU),mpc8xx)
ELF	= test_burst
SREC	= test_burst.srec
BIN	= test_burst.bin
endif


#ifeq ($(ARCH),i386)
#ELF	+= 82559_eeprom
#SREC	+= 82559_eeprom.srec
#BIN	+= 82559_eeprom.bin
#endif

ifeq ($(ARCH),ppc)
ELF	+= sched
SREC	+= sched.srec
BIN	+= sched.bin
endif

ifeq ($(ARCH),blackfin)
ELF	+= smc91111_eeprom
SREC	+= smc91111_eeprom.srec
BIN 	+= smc91111_eeprom.bin
endif

# The following example is pretty 8xx specific...
ifeq ($(CPU),mpc8xx)
ELF	+= timer
SREC	+= timer.srec
BIN	+= timer.bin
endif

# The following example is 8260 specific...
ifeq ($(CPU),mpc8260)
ELF	+= mem_to_mem_idma2intr
SREC	+= mem_to_mem_idma2intr.srec
BIN	+= mem_to_mem_idma2intr.bin
endif

# Demo for 52xx IRQs
ifeq ($(CPU),mpc5xxx)
ELF	+= interrupt
SREC	+= interrupt.srec
BIN	+= interrupt.bin
endif

# Utility for resetting i82559 EEPROM
ifeq ($(BOARD),oxc)
ELF	+= eepro100_eeprom
SREC	+= eepro100_eeprom.srec
BIN	+= eepro100_eeprom.bin
endif

ifeq ($(BIG_ENDIAN),y)
EX_LDFLAGS += -EB
endif

COBJS	:= $(SREC:.srec=.o)

LIB	= $(obj)libstubs.a
LIBAOBJS=
ifeq ($(ARCH),ppc)
LIBAOBJS+= $(ARCH)_longjmp.o $(ARCH)_setjmp.o
endif
ifeq ($(CPU),mpc8xx)
LIBAOBJS+= test_burst_lib.o
endif
LIBCOBJS= stubs.o

LIBOBJS	= $(addprefix $(obj),$(LIBAOBJS) $(LIBCOBJS))

SRCS	:= $(COBJS:.o=.c) $(LIBCOBJS:.o=.c) $(if $(LIBAOBJS),$(LIBAOBJS:.o=.S))
OBJS	:= $(addprefix $(obj),$(COBJS))
ELF	:= $(addprefix $(obj),$(ELF))
BIN	:= $(addprefix $(obj),$(BIN))
SREC	:= $(addprefix $(obj),$(SREC))

gcclibdir := $(shell dirname `$(CC) -print-libgcc-file-name`)
clibdir := $(shell dirname `$(CC) $(CFLAGS) -print-file-name=libc.a`)

CPPFLAGS += -I..

all:	$(obj).depend $(OBJS) $(LIB) $(SREC) $(BIN) $(ELF)

#########################################################################
$(LIB):	$(obj).depend $(LIBOBJS)
		$(AR) $(ARFLAGS) $@ $(LIBOBJS)

$(ELF):
$(obj)%:	$(obj)%.o $(LIB)
		$(LD) -g $(EX_LDFLAGS) -Ttext $(LOAD_ADDR) \
			-o $@ -e $(notdir $(<:.o=)) $< $(LIB) \
			-L$(gcclibdir) -lgcc

$(SREC):
$(obj)%.srec:	$(obj)%
		$(OBJCOPY) -O srec $< $@ 2>/dev/null

$(BIN):
$(obj)%.bin:	$(obj)%
		$(OBJCOPY) -O binary $< $@ 2>/dev/null

#########################################################################

# defines $(obj).depend target
include $(SRCTREE)/rules.mk

sinclude $(obj).depend

#########################################################################
