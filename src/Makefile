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
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM = HOST

# Compiler Flags and Defines
OUTPUT = c1m2
GFLAGS = -Wall -Werror -g -O0 -std=c99
ifeq ($(PLATFORM), MSP432)
	# Architectures Specific Flags
	LINKER_FILE = ../msp432p401r.lds
	TARGET = MSP432
	CPU = cortex-m4
	ARCH = armv7e-m
	FLOAT-ABI = soft
	FPU = fpv4-sp-d16
	SPECS = nosys.specs

	# Compile Defines
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	OBJDUMP = arm-none-eabi-objdump
      	CPPFLAGS = $(INCLUDES)
	CFLAGS = -mcpu=$(CPU) -mthumb -march=$(ARCH) -mfloat-abi=$(FLOAT-ABI) --specs=$(SPECS) $(GFLAGS) 
	LDFLAGS = -Wl,-Map=$(OUTPUT).map -T $(LINKER_FILE)

else ifeq ($(PLATFORM), HOST)
	# Architectures Specific Flags
	TARGET = HOST

        # Compile Defines
	CC = gcc
	LD = ld
	OBJDUMP = x86_64-linux-gnu-objdump
	CPPFLAGS = $(INCLUDES)
	CFLAGS =  $(GFLAGS)
	LDFLAGS = -Wl,-Map=$(OUTPUT).map
endif	

#Dependency Files Generation
DEPS = $(SOURCES:.c=.d)

#Object Files Generation
OBJS = $(SOURCES:.c=.o)

%.d : %.c
	$(CC) -D$(TARGET) -MM $< $(CPPFLAGS) -o $@

%.i : %.c
	$(CC) -D$(TARGET) -E $< $(CPPFLAGS) -o $@

%.s : %.c
	$(CC) -D$(TARGET) -S $< $(CPPFLAGS) -o $@

%.o : %.c
	$(CC) -D$(TARGET) -c $< $(CPPFLAGS) $(CFLAGS) -o $@

%.asm : %.o
	$(OBJDUMP) -S $< > $@


.PHONY: compile-all
compile-all:
	make $(OBJS) 	


.PHONY: build
build:$(OUTPUT).out

$(OUTPUT).out:$(OBJS)
	$(CC) $^ $(CFLAGS) $(LDFLAGS) -o $@


.PHONY: clean
clean:
	rm -f *.d *.i *.s *.asm *.o $(OUTPUT).out $(OUTPUT).map