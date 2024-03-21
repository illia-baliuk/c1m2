#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# Make file for Week 2 Application Assigment in the Coursera course The introduce
# to Embedded System Software and Development Environments
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      all: compiles all object files without linking
#      build : builds and links all source files
#      clean : remove all generated files
#      [file].o : build object file
#      [file].asm : build assembly file
#      [file].i : build preprocessed file
#      [file].d : build dependecy file
#
# Platform Overrides:
#      HOST
#      MSP432
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
ifndef PLATFORM
	PLATFORM = HOST
# else
# PLATFORM = $(PLATFORM)
endif 


# Architectures Specific Flags
LINKER_FILE = ../msp432p401r.lds
CPU = cortex-m4
ARCH = armv7e-m
SPECS = nosys.specs


# Compiler Flags and Defines
BASENAME = c1m2
TARGET = $(BASENAME).out
MAP = $(BASENAME).map
OBJS = $(SOURCES:.c=.o)

CC = gcc
AS = as
LDFLAGS = -Wl,-Map=$(MAP)
CFLAGS = -Wall \
	 -Werror \
	 -g \
	 -O0 \
	 -std=c99 \
	 -D$(PLATFORM)

ifeq ($(PLATFORM), MSP432)

CC = arm-none-eabi-gcc
AC = arm-none-eabi-as
CFLAGS += -mcpu=$(CPU) \
	  -march=$(ARCH) \
	  -mthumb\
	  -mfloat-abi=hard \
	  -mfpu=fpv4-sp-d16 \
	  --specs=$(SPECS)

LDFLAGS += -T $(LINKER_FILE)

endif



.PHONY: build
build: $(TARGET)

.PHONY: compile-all
compile-all: $(OBJS)

.PHONY: clean
clean:
	rm -f $(TARGET) $(MAP) *.o *.i *.asm

.PHONY: as
as: $(TARGET)


$(TARGET): $(OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) $(OBJS) $(INCLUDES) -o $(TARGET)

%.i: %.c
	$(CC) -E $^ $(CFLAGS) $(INCLUDES) -o $@

%.o : %.c
	$(CC) -c $^ $(CFLAGS) $(INCLUDES) -o $@

%.asm : %.o
	objdump -S $^ > $@

