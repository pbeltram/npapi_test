# Makefile for boot binaries

override SRC_TOP:=..

XILSRC=~/opt/Xilinx/Vitis/2022.2/settings64.sh
PLNXSRC=~/opt/Xilinx/petalnx/2022.2/components/yocto/buildtools/environment-setup-x86_64-petalinux-linux

.PHONY : all help build_lx build_boot clean

all: help

help:
	@echo "usage: make [target]"
	@echo
	@echo "  target:"
	@echo "         help             # This info."
	@echo "         clean            # Delete bin and build directory files."
	@echo "         build_lx         # Builds bin/image.ub using rootfs.its file."
	@echo "         build_boot       # Builds bin/BOOT.BIN using boot.bif file."
	@echo
	@echo "  Examples:"
	@echo "         make build_lx # Builds bin/image.ub."
	@echo "         make build_boot # Builds bin/BOOT.BIN."
	@echo

build_all: build_lx build_boot

build_lx: clean
	@echo "==> Building image.ub"
	-mkdir -p ./build
	-mkdir -p ./bin
	cp ./zImage ./build
	cp ./system.dtb ./build
	source $(PLNXSRC); uboot-mkimage -f ./rootfs.its ./bin/image.ub
	md5sum ./bin/image.ub

build_boot: clean
	@echo "==> Building BOOT.BIN"
	-mkdir -p ./build
	-mkdir -p ./bin
	cp ./zynq_fsbl.elf ./build
	cp ./system.bit ./build
	cp ./u-boot.dtb ./build
	cp ./u-boot.elf ./build
	source $(XILSRC); bootgen -arch zynq -image ./boot.bif -w on -o ./bin/BOOT.BIN -log trace
	md5sum ./bin/BOOT.BIN

clean:
	-rm -rf ./build
	-rm -rf ./bin

