# LED_TEST

## process
```console
pi@raspberrypi:~ $ cd Development/drivers/test/
pi@raspberrypi:~/Development/drivers/test $ nano test_driver.c  
```

## test_driver.c
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
