### This makefile is modeled after that found in the avr-libc user manual ###
SILENCE=@

PRG = demo
OBJ = demo.o
MCU_TARGET=attiny26
OPTIMIZE = -O2

DEFS =
LIBS =

CC = avr-gcc

#Override is only needed by avr-lib build system (?)
override CFLAGS  = -g -Wall $(OPTIMIZE) -mmcu=$(MCU_TARGET) $(DEFS)
override LDFLAGS =  -Wl,-Map,$(PRG).map

OBJCOPY = avr-objcopy
OBJDUMP = avr-objdump

all: $(PRG).elf lst text eeprom

$(PRG).elf: $(OBJ)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^ $(LIBS)

#dependency:
demo.o: demo.c

clean:
#*.eps *.pnh *.pdf *.bak
	rm -rf *.o $(PRG).elf
	rm -rf *.lst *.map $(EXTRA_CLEAN_FILES)

lst: $(PRG).lst

%.lst: %.elf
	$(OBJDUMP) -h -S $< > $@

#Rules for buildint the .text rom images
text: hex bin srec

hex: $(PRG).hex
bin: $(PRG).bin
srec: $(PRG).srec

%.hex: %.elf
	$(OBJCOPY) -j .text -j .data -O ihex $< $@

%.srec: %.elf
	$(OBJCOPY) -j .text -j .data -O srec $< $@

%.bin: %.elf
	$(OBJCOPY) -j .text -j .data -O binary $< $@

#Rules for building the .eeprom rom images
eeprom: ehex ebin esrec

ehex: $(PRG)_eeprom.hex
ebin: $(PRG)_eeprom.bin
esrec: $(PRG)_eeprom.srec

%_eeprom.hex: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O ihex $< $@ \
	|| { echo empty $@ not generated; exit 0; }

%_eeprom.srec: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O srec $< $@ \
	|| { echo empty $@ not generated; exit 0; }

%_eeprom.bin: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O binary $< $@ \
	|| { echo empty $@ not generated; exit 0; }


#Everything below here is used by avr-libc's build system
#It can (should?) be ignored by the casual user.
#Haha... oh well.
FIG2DEV = fig2dev
EXTRA_CLEAN_FILES = *.hex *.bin *.srec

dox: eps png pdf

eps: $(PRG).eps
png: $(PRG).png
pdf: $(PRG).pdf

%.eps: %.fig
	$(FIG2DEV) -L eps $< $@

%.pdf: %.fig
	$(FIG2DEV) -L pdf $< $@

%.png: %.fig
	$(FIG2DEV) -L png $< $@


# $@	the name of the target
# $<	the name of the first prerequisite
# $^	the names of all prerequisites separated by a space


#Want automatic dependencies
#WHOA what's with the clean?

#What are srec files?
#--change-section-lma
