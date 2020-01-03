# LED_TEST

## process
```console
pi@raspberrypi:~ $ cd Development/drivers/test/
pi@raspberrypi:~/Development/drivers $ mkdir test
pi@raspberrypi:~/Development/drivers $ cd test
pi@raspberrypi:~/Development/drivers/test $ nano test_driver.c  
```

### test_driver.c
```c
#include <linux/module.h>
#include <linux/io.h>
#include <linux/delay.h>

volatile unsigned int* map;

static int __init JongHo_TEST_init(void)
{
    map = (volatile unsigned int*) ioremap(0x3F200000, 180);

    *(map + 0) = 0x08040000;

    *(map + 7) = 0x00000240;
    msleep(5000);

    *(map + 10) = 0x00000240;

    printk(KERN_INFO "map : %p\n", map);
    printk(KERN_INFO "call JongHo_TEST_init\n");

    return 0;
}

static void __exit JongHo_TEST_exit(void)
{
    msleep(5000);

    *(map + 10) = 0x00000240;

    printk(KERN_INFO "map : %p\n", map);
    printk(KERN_INFO "call JongHo_TEST_init\n");

    return 0;
}

static void __exit JongHo_TEST_exit(void)
{
    iounmap(map);                                                                                                           printk(KERN_INFO "call JongHo_TEST_exit\n");
}

module_init(JongHo_TEST_init);
module_exit(JongHo_TEST_exit);

MODULE_LICENSE("GPL");
```

## process
```console
pi@raspberrypi:~/Development/drivers/test $ nano Makefile
```

### Makefile
```c
obj-m+=test_driver.o

all:
        make -C /lib/modules/$(shell uname -r)/build/ M=$(PWD) modules
clean:
        make -C /lib/modules/$(shell uname -r)/build/ M=$(PWD) clean
```

## process
```console
pi@raspberrypi:~/Development/drivers/test $ make
make -C /lib/modules/4.19.75-v7+/build/ M=/home/pi/Development/drivers/test modules
make[1]: Entering directory '/usr/src/linux-headers-4.19.75-v7+'
  Building modules, stage 2.
  MODPOST 1 modules
make[1]: Leaving directory '/usr/src/linux-headers-4.19.75-v7+'
pi@raspberrypi:~/Development/drivers/test $ ls
Makefile       Module.symvers  test_driver.ko     test_driver.mod.o
modules.order  test_driver.c   test_driver.mod.c  test_driver.o
pi@raspberrypi:~/Development/drivers/test $ sudo insmod test_driver.ko
pi@raspberrypi:~/Development/drivers/test $ sudo rmmod test_driver.ko
pi@raspberrypi:~/Development/drivers/test $ tail /var/log/kern.log
Jan  3 02:33:59 raspberrypi kernel: [ 4378.019258] map : 6c13bd03
Jan  3 02:33:59 raspberrypi kernel: [ 4378.019269] call JongHo_TEST_init
Jan  3 02:34:08 raspberrypi kernel: [ 4386.729454] call JongHo_TEST_exit
```
