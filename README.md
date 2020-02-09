# DRV_0005_MultiTouchPanel

# mtp_input.c

测试:
a、先把原有的drivers\input\touchscreen\ft5x06_ts.c驱动去掉  
   I2C驱动有i2c_driver, i2c_device, ft5x06_ts.c只是i2c_driver  
   修改同目录下的drivers\input\touchscreen\Makefile  
obj-$(CONFIG_TOUCHSCREEN_FT5X0X)                += ft5x06_ts.o  
改为:  
obj-$(CONFIG_TOUCHSCREEN_FT5X0X)                += mtp_input.o  

b、修改arch\arm\mach-exynos\mach-tiny4412.c  
   去掉:  
   i2c_register_board_info(1, i2c_devs1, ARRAY_SIZE(i2c_devs1));  
   不去掉也可以,需要修改mtp_input.c:  
   static const struct i2c_device_id mtp_id_table[] = {  
	{ "100ask_mtp", 0 },  
	{ "ft5x0x_ts", 0}, // 添加这句  
	};  
  
c、make zImage  
  
  
查看是否注册i2c设备:内核启动完成后进入串口终端输入  
dmesg | grep "mtp_probe"  
cd /sys/bus/i2c/devices  查看i2c的总线设备  
  
cat /proc/interrupts   					//查看中断是否注册成功  
cat /proc/interrupts | gerp 100ask_mtp	//查看中断是否注册成功  
