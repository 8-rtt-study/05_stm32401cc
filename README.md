# rtt和cubemx的架构分工

## cubemx主要的功能是什么？

cubemx只是配置了io的设置，生成了mspinit的函数

这个函数放在cube里面的stm32f4xx_hal_msp.c这个文件中；

而board.h里面说明，需要复制这个函数到board.c文件中，其实是不需要的；

## msp_init函数是在哪里调用的？

实际在board.h里面打开bsp_using_xx只是将drv_xxx的配置打开；

所以程序启动的时候，就会自动去调用hal库的初始化，比如hal_adc_init()

而这个函数就会去调用msp_init()

## hal_xx_h/c的库是由谁控制加入的

通过hal_conf.h这个头文件里面的宏定义

# 三个步骤来通过rtt和cubemx测试adc功能

1. cubemx里面只需要打开相应的gpio引脚，屏蔽编译cubemx里面main文件，gitignore掉cubemx假如的hal库文件，只保留hal_msp.c文件
2. 在board.h里面解开adc1的注释，打开hal_config里面adc的注释；通过rt_config的按钮，打开adc;此时系统已经加入了adc设备
3. 通过rt_config，加入adc的demo,按照help提示，使用cmd来运行demo


# 关于adc的io初始化和通道配置

在rtt的drv_adc中，实际在读取adc指定通道的时候，才去配置一次adc，而不是提前配置的；

所以需要提前配置的只有adc的io配置
