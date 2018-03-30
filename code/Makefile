# Calendar Example Makefile
#
# Copyright (c) 2009-2017 Arm Limited (or its affiliates). All rights reserved.
# Use, modification and redistribution of this file is subject to your
# possession of a valid DS-5 end user licence agreement and your compliance
# with all applicable terms and conditions of such licence agreement.
#
# This makefile is intended for use with GNU make
# This example is intended to be built with Arm Compiler 6

TARGET=armtest.axf


CC=armclang
LD=armlink

# Select build rules based on Windows or Unix
ifdef WINDIR
DONE=@if exist $(1) echo Build completed.
RM=if exist $(1) del /q $(1)
SHELL=$(WINDIR)\system32\cmd.exe
else
ifdef windir
DONE=@if exist $(1) echo Build completed.
RM=if exist $(1) del /q $(1)
SHELL=$(windir)\system32\cmd.exe
else
DONE=@if [ -f $(1) ]; then echo Build completed.; fi
RM=rm -f $(1)
endif
endif

all: $(TARGET)
	$(call DONE,$(TARGET))
	
rebuild: clean all

clean:
	$(call RM,*.o)
	$(call RM,$(TARGET))
	
armasm.o: armasm.c
	$(CC) -c --target=arm-arm-none-eabi -mcpu=Cortex-A9 -mfpu=none -g -O0 $^ -o $@

armtest_gen.o: armtest_gen.c
	$(CC) -c --target=arm-arm-none-eabi -mcpu=Cortex-A9 -mfpu=none -g -O0 $^ -o $@

armtest.o: armtest.c
# Compile for best debug view (lowest optimization)
	$(CC) -c --target=arm-arm-none-eabi -mcpu=Cortex-A9 -mfpu=none -g -O0 $^ -o $@

$(TARGET): armasm.o armtest_gen.o armtest.o
# Link with specific base address to suit VE model memory layout
	$(LD) --ro-base=0x80000000 $^ -o $@
