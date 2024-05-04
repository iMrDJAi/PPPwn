TARGET = stage2
OBJS = start.o stage2.o proc_utils.o  utils.o

CC = gcc
OBJCOPY = objcopy
CFLAGS = -DSMP -isystem ../freebsd-headers/include -Wl,--build-id=none -Os -fno-stack-protector 
LDFLAGS = -T linker.ld -nostartfiles -nostdlib

ifneq ($(filter $(FW), 850 900 903 904 950 960 1000 1001 1050 1070 1071 1100),)
CFLAGS += -DFIRMWARE=$(FW)
else
$(error "Invalid firmware")
endif

all: $(TARGET).bin

%.bin: %.elf
	$(OBJCOPY) -S -O binary $^ $@

$(TARGET).elf: $(OBJS)
	$(CC) $(CFLAGS) $^ -o $@ $(LDFLAGS)

# Adjusted to ensure correct handling of assembly files
%.o: %.asm
	$(CC) -x assembler-with-cpp -c $< -o $@

clean:
	@rm -f $(TARGET).bin $(TARGET).elf $(OBJS)
