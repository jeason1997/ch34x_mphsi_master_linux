# 设置交叉编译的工具链前缀
CROSS_COMPILE = /home/jeason/orangepi-build/toolchains/gcc-arm-9.2-2019.12-x86_64-arm-none-linux-gnueabihf/bin/arm-none-linux-gnueabihf-
# 设置交叉编译的目标平台
ARCH = arm

CONFIG_MODULE_SIG=n

DRIVERNAME := ch34x_mphsi_master
all : modules

ifneq ($(KERNELRELEASE),)
obj-m += $(DRIVERNAME).o
$(DRIVERNAME)-y := ch34x_mphsi_master_usb.o ch34x_mphsi_master_spi.o ch34x_mphsi_master_i2c.o ch34x_mphsi_master_gpio.o
else
# KERNELDIR := /lib/modules/$(shell uname -r)/build
# 交叉编译的目标平台内核源码的路径
KERNELDIR := /home/jeason/orangepi-build/kernel/orange-pi-5.4
PWD	:= $(shell pwd)

modules:
	$(MAKE) -C $(KERNELDIR) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) M=$(PWD) modules

clean:
	rm -rf *.mk .tmp_versions Module.symvers *.mod.c *.o *.ko .*.cmd Module.markers modules.order *.a *.mod
load:
	insmod $(DRIVERNAME).ko
unload:
	rmmod $(DRIVERNAME)
install : modules
	mkdir -p /lib/modules/$(shell uname -r)/kernel/drivers/usb/misc
	cp -f ./$(DRIVERNAME).ko /lib/modules/$(shell uname -r)/kernel/drivers/usb/misc/
	depmod -a
uninstall:
	rm -f /lib/modules/$(shell uname -r)/kernel/drivers/usb/misc/$(DRIVERNAME).ko
	depmod -a
endif
