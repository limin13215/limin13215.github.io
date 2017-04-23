---
layout: post
title:  "Android4.1.1_r1系统移植------TP移植篇"
page.date:   2013-10-19 15:24 +0800
categories: jekyll update
---

学有所得，得有共享，这才是进步之道。
      最近天天加班，很多博文写了一半觉得不完善，只好忍痛丢进了草稿箱。
      不管其他的，今天得讲讲移植TP的东西：
         
         注：此处TP移植讲解以移植适应“思立微”TP IC为实例，其他IC可依葫芦画瓢。
         
1.添加触摸屏驱动
        把触摸IC供应商提供的驱动文件（rockchip_gslX680.c ；rockchip_gslX680.h）复制到kernel/drivers/input/touchscreen/下
        并进行touch命令，以修改文件的时间戳。

2.修改kernel/drivers/input/touchscreen/下的Kconfig文件,在该文件定义config宏
    config TOUCHSCREEN_GSLX680
            tristate "gslX680 touchscreen panel support"
            depends on I2C2_RK29 || I2C2_RK30

3.修改kernel/drivers/input/touchscreen/下的Makefile文件,在该文件最后添加
       obj-$(CONFIG_TOUCHSCREEN_GSLX680)       += rockchip_gslX680.o

4. 修改kernel/arch/arm/mach-rk2928/borad-rk2926-sdk.c文件
   添加如下代码：
#if defined(CONFIG_TOUCHSCREEN_GSLX680)
#define TOUCH_RESET_PIN  INVALID_GPIO //RK2928_PIN0_PD3//RK2928_PIN1_PA3
#define TOUCH_INT_PIN    RK2928_PIN1_PB0
int gslx680_init_platform_hw(void)
{

        //printk("ft5306_init_platform_hw\n");
        if(gpio_request(TOUCH_RESET_PIN,NULL) != 0){
                gpio_free(TOUCH_RESET_PIN);
                printk("gslx680_init_platform_hw gpio_request error\n");
                return -EIO;
        }

        if(gpio_request(TOUCH_INT_PIN,NULL) != 0){
                gpio_free(TOUCH_INT_PIN);
                printk("gslx680_init_platform_hw gpio_request error\n");
                return -EIO;
        }
        gpio_direction_output(TOUCH_RESET_PIN, GPIO_HIGH);
        mdelay(10);
        gpio_set_value(TOUCH_RESET_PIN,GPIO_LOW);
        mdelay(10);
        gpio_set_value(TOUCH_RESET_PIN,GPIO_HIGH);
        msleep(300);
        return 0;

}

struct goodix_platform_data gslx680_ts_hw_info = {
        .model = 8107,
 //       .irq_pin = RK2928_PIN1_PB0,
        .rest_pin = TOUCH_RESET_PIN,
        .init_platform_hw = gslx680_init_platform_hw,
};
#endif


-------------------------------------------------------------------------------------------------------------------
#if defined (CONFIG_TOUCHSCREEN_GSLX680)
        {
            .type           = "gslx680_ts",                                      //名字为gslx680_ts
            .addr           = 0x40,                                                  // i2c地址为0x40
            .flags          = 0,
            .irq            = TOUCH_INT_PIN,                              //指定中断脚
            .platform_data = &gslx680_ts_hw_info,
        },
#endif

5.在menuconfig中选中“gslX680 touchscreen panel support”选项，编译即可。

补充：Kconfig文件的作用
         内核源码树的目录下都有两个文件Kconfig（2.4版本是Config.in）和Makefile。分布到各目录的Kconfig构成了一个分布式的内核配置数据库，每个Kconfig分别描述了所属目录源文件相关的内核配置菜单。在内核配置make menuconfig(或xconfig等)时，从Kconfig中读出菜单，用户选择后保存到.config的内核配置文件中。在内核编译时，主Makefile调用这个.config，就知道了用户的选择。