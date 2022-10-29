# xx_userhhh
##杂谈

[C语言篇](#1.*__C语言数组的越界访问__*)

[汇编篇](#1.汇编啊额)

[TCP/IP篇待续...](#1.*__汇编啊额__*)

<font color="#00ff00">文章是有关C和汇编的</font>

*emm...*

注意的地方有

#1.*__C语言数组的越界访问__*

```

#include <stdio.h>

int main() {

    int array[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};

    for(int i= 0;i <=10;i++)

    {

        printf("%d ", array[i]);

    }

    printf("\n");

    return 0;

}

}

```

我比较喜欢用printf调试，Linux用那个gdb

`gcc  -g 文件名.c -o 文件名&&gdb 文件名`

gdb也可以反汇编，手机出来的像是我的aarch64架构的指令集

[gdb调试](http://127.0.0.1:8080/gdbtiaoshi.jpg)

ARMv8指令集分为Aarch64和Aarch32指令集，而ARMv7使用的是A32和T16指令集（分别为32位和16位）

![](file:///storage/emulated/0/DCIM/Screenshots/gdbtiaoshi.jpg)

```

#include <stdio.h>

int main()

{

        int array[10];

        for(int i = 1; i < 10; i++)

        {

                scanf("%d\n",&array[i]);

                printf("%d,%d\n",i,array[i]);

        }

        for(int i = 1; i < 10;i++)

        {

                printf("%p->%d \n",i,array[i]);

        }

        printf("%lu\n",sizeof(array));

        printf("\n");

        return 0;

}

```

2.*__野指针__*

```

#include <stdio.h>

int main()

{

        int i = 1;

        int *p;

        printf("%d\n",*p);//Segmentation fault

//      p =&i;

//      printf("%d\n",*p);

/*

        printf("i的地址:%p->\n",i);

        p=&i;

        printf("*p指向i的地址*p:%p->\n",*p);

        printf("*p指向变量地址的数:%d->\n",*p);//*p解引用

        printf("指针p自己的内存地址:%p->\n",p);

*/

}

```

~~数组与指针是一对好（只因油）~~

[数组指针]

```

#include <stdio.h>

int main(){

    int arr[] = { 1,2,3,4,5 };

    int *p = &arr[2];  //也可以写作 int *p = arr + 2;

    printf("%d, %d, %d, %d, %d\n", *(p-2), *(p-1), *p, *(p+1), *(p+2) );

    return 0;

}

```

函数指针

```

#include<stdio.h>

int max(int x,int y){return (x>y? x:y);}

int main()

{

    int (*ptr)(int, int);

    int a, b, c;

    ptr = max;

    scanf("%d%d", &a, &b);

    c = (*ptr)(a,b);

    printf("a=%d, b=%d, max=%d", a, b, c);

    return 0;

}

```

[指向指针]->网上找的~~荔枝~~例子

```

#include <stdio.h>

int main(){

    int a =100;

    int *p1 = &a;

    int **p2 = &p1;

    int ***p3 = &p2;

    printf("%d, %d, %d, %d\n", a, *p1, **p2, ***p3);

    printf("&p2 = %#X, p3 = %#X\n", &p2, p3);

    printf("&p1 = %#X, p2 = %#X, *p3 = %#X\n", &p1, p2, *p3);

    printf(" &a = %#X, p1 = %#X, *p2 = %#X, **p3 = %#X\n", &a, p1, *p2, **p3);

    return 0;

}

```

指针就不讲了，后面再看~~

#1.*__汇编啊额__*

```

存储单元:多个存储元的集合

一般应具有存储数据和读写数据的功能，以8位二进制作为一个存储单元，也就是一个字节。每个单元有一个地址，是一个整数编码，可以表示为二进制整数。程序中的变量和主存储器的存储单元相对应。变量的名字对应着存储单元的地址，变量内容对应着单元所存储的数据。存储地址一般用十六进制数表示，而每一个存储器地址中又存放着一组二进制（或十六进制）表示的数，通常称为该地址的内容

```

[ROM与RAM的区别](https://zhuanlan.zhihu.com/p/357230321)

`动态随机存储器(DRAM),数据要不断刷新，易集成，读写速度慢，价格低，用于制作内存。

静态随机存储器（SRAM），数据不要刷新，不易集成，读写速度快，价格高，用于制作CACHE`

~~GPT与MBR那些纠缠不清的故事~~

[堆栈]->是一种数据结构push，pop

2^16=65536

#浅析Linux 内核空间和用户空间

[000到fff的用户态和内核态](https://www.cnblogs.com/wwchihiro/p/9211975.html)

~~不得不提到漏洞[Meltdown(熔毁)](https://zhuanlan.zhihu.com/p/33621030)和Spectre(幽灵)漏洞~~

windows为每个分配进程的4GB虚拟地址

第一种是：因为在 32 位的操作系统中，一个指针长度是 4 字节，而 4 字节指针的寻址能力是从 0x00000000~0xFFFFFFFF，最大值 0xFFFFFFFF 表示的即为 4GB 大小的容量。

第二种是：32位CPU的地址总线是32根，寻址能力就是2^32也就是4GB

![](file:///storage/emulated/0/DCIM/Screenshots/IMG_20221029_164932.jpg)

8bit 为1个byte

EAX与AX不是独立的，EAX是32位的寄存器，而AX是EAX的低16位。

举例来说

mov eax, 12345678h

那么AX将会是eax的低16位，也就是5678h

[常用寄存器链接](https://www.codenong.com/cs106191861/)

[缓冲区攻击实验链接](https://zhuanlan.zhihu.com/p/138620118)

[Windows永恒之蓝漏洞之kali的msf框架攻击链接](https://www.jianshu.com/p/a3d3857a7511)

[死亡之ping](https://baike.baidu.com/item/死亡之Ping/2046122?fr=aladdin)

而如果此时

mov ax,1234h

mov bx,ax

![](file:///storage/emulated/0/DCIM/Screenshots/IMG_20221029_163657.jpg)

![](file:///storage/emulated/0/DCIM/Screenshots/IMG_20221029_163541.jpg)

)

linux可以用nasm

[汇编语言学习 DOSBox+MASM 安装及使用教程](https://zhuanlan.zhihu.com/p/370786100)

[有关汇编8086d的debug指令的链接](https://blog.51cto.com/u_15127705/3378095?articleABtest=1)

那么eax的值将变为1234h，所以对ax的赋值是会影响eax的,AH是ax的高8位，而AL是ax的低8位，这就是说ah为12h，al为34h

#__*TCP/IP*__

[tcp/ip](https://www.w3school.com.cn/tcpip/index.asp)

[ipv4的头结构](https://blog.csdn.net/kenfan1647/article/details/117765744)

[ipv4的头报文详解](https://blog.csdn.net/weixin_44135544/article/details/103203716)

[ipv6的头结构](https://blog.csdn.net/weixin_44135544/article/details/103203716)

[计算机中的大端存储和小端存储](https://blog.csdn.net/kenfan1647/article/details/117765744)
