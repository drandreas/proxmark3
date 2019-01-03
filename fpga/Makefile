include ../common/Makefile.common

PIN_DEF   = ice40up5k.pcf
DEVICE    = up5k
PACKAGE   = sg48
FREQUENCY = 12 # todo change to 24 MHz

all: fpga_lf.bit fpga_hf.bit
clean:
	$(DELETE) *.bin *.json *.asc *.log *.bit

%.json: %.v
	yosys -ql $*.log -p 'synth_ice40 -top $* -json $@' $<

%.asc: $(PIN_DEF) %.json
	nextpnr-ice40 --$(DEVICE) --package $(PACKAGE) --freq $(FREQUENCY) --json $(filter-out $<,$^) --pcf $< --asc $@

%.bit: %.asc
	icepack $< $@

%.rpt: %.asc
	icetime -d $(DEVICE) -mtr $@ $<

.PHONY: all clean help
help:
	@echo Multi-OS Makefile, you are running on $(DETECTED_OS)
	@echo Possible targets:
	@echo +	all   - Make fpga.bit, the FPGA bitstream 
	@echo +	clean - Clean intermediate files, does not clean fpga.bit
	
