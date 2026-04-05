# MTD子系统与JFFS2文件系统



## 遗留问题:

从spi-nor目录的Makefile可以看出目录下文件的关系, 可以看出spi-nor子系统主要由核心、控制器以及FLASH厂商组成；
```shell
# SPDX-License-Identifier: GPL-2.0

spi-nor-objs			:= core.o sfdp.o swp.o otp.o sysfs.o
spi-nor-objs			&#43;= atmel.o
spi-nor-objs			&#43;= catalyst.o
spi-nor-objs			&#43;= eon.o
spi-nor-objs			&#43;= esmt.o
spi-nor-objs			&#43;= everspin.o
spi-nor-objs			&#43;= fujitsu.o
spi-nor-objs			&#43;= gigadevice.o
spi-nor-objs			&#43;= intel.o
spi-nor-objs			&#43;= issi.o
spi-nor-objs			&#43;= macronix.o
spi-nor-objs			&#43;= micron-st.o
spi-nor-objs			&#43;= spansion.o
spi-nor-objs			&#43;= sst.o
spi-nor-objs			&#43;= winbond.o
spi-nor-objs			&#43;= xilinx.o
spi-nor-objs			&#43;= xmc.o
obj-$(CONFIG_MTD_SPI_NOR)	&#43;= spi-nor.o

obj-$(CONFIG_MTD_SPI_NOR)	&#43;= controllers/

```



-- drivers\mtd\spi-nor\controllers\intel-spi.c
https://lwn.net/Articles/692383/



-- drivers\mtd\spi-nor\controllers\hisi-sfc.c
对应的`Hi3519`芯片, 该芯片的设备树并没有开源，需要对应的SDK开发包才能看到。





初始化顺序 

FILE :: `include\linux\init.h`
```c
/*
 * Early initcalls run before initializing SMP.
 *
 * Only for built-in code, not modules.
 */
#define early_initcall(fn)		__define_initcall(fn, early)

/*
 * A &#34;pure&#34; initcall has no dependencies on anything else, and purely
 * initializes variables that couldn&#39;t be statically initialized.
 *
 * This only exists for built-in code, not for modules.
 * Keep main.c:initcall_level_names[] in sync.
 */
#define pure_initcall(fn)		__define_initcall(fn, 0)

#define core_initcall(fn)		__define_initcall(fn, 1)
#define core_initcall_sync(fn)		__define_initcall(fn, 1s)
#define postcore_initcall(fn)		__define_initcall(fn, 2)
#define postcore_initcall_sync(fn)	__define_initcall(fn, 2s)
#define arch_initcall(fn)		__define_initcall(fn, 3)
#define arch_initcall_sync(fn)		__define_initcall(fn, 3s)
#define subsys_initcall(fn)		__define_initcall(fn, 4)
#define subsys_initcall_sync(fn)	__define_initcall(fn, 4s)
#define fs_initcall(fn)			__define_initcall(fn, 5)
#define fs_initcall_sync(fn)		__define_initcall(fn, 5s)
#define rootfs_initcall(fn)		__define_initcall(fn, rootfs)
#define device_initcall(fn)		__define_initcall(fn, 6)
#define device_initcall_sync(fn)	__define_initcall(fn, 6s)
#define late_initcall(fn)		__define_initcall(fn, 7)
#define late_initcall_sync(fn)		__define_initcall(fn, 7s)

#define __initcall(fn) device_initcall(fn)
```

---

> Author: Kristoffer  
> URL: https://psuvtk.github.io/posts/7.spi_nor/  

