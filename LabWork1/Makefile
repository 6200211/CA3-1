SDK_PREFIX ?= arm-none-eabi

CC = $(SDK_PREFIX)-gcc
LD = $(SDK_PREFIX)-ld
SIZE = $(SDK_PREFIX)-size
OBJCOPY = $(SDK_PREFIX)-objcopy
QEMU = /home/presniakov_io-21/opt/xPacks/qemu-arm/xpack-qemu-arm-7.2.0-1/bin/qemu-system-gnuarmeclipse #Way to Qemu on my OS
# Configuration table
BOARD ?= STM32F4-Discovery
MCU = STM32F407VG
TARGET = firmware
CPU_CC = cortex-m4

deps = \
    start.S \
    lscript.ld

all: target

target:
	#In this section we are creating bin file for our target platform
	$(CC) -x assembler-with-cpp -c -O0 -g3 -mcpu=$(CPU_CC) -Wall start.S -o start.o
	$(CC) start.o -mcpu=$(CPU_CC) -Wall --specs=nosys.specs -nostdlib -lgcc -T lscript.ld -o $(TARGET).elf
	$(OBJCOPY) -O binary $(TARGET).elf $(TARGET).bin

qemu:
	 #This section was changed due to the deprecation of the g flag. The -s -S flags were added to communicate with GDB.
	$(QEMU) --verbose --verbose --board $(BOARD) --mcu $(MCU) -d unimp,guest_errors -s -S -kernel $(TARGET).elf

clean:
	#This files will be deleted in case of "make clean"
	-rm -f *.o
	-rm -f *.elf
	-rm -f *.bin
