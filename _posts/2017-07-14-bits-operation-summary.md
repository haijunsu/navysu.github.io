---
title: 位操作基础篇之位操作全面总结
author: Haijun (Navy) Su
layout: post
---
Title:       位操作基础篇之位操作全面总结
Author:     MoreWindows
E-mail:      morewindows@126.com
KeyWord:   C/C++ 位操作 位操作技巧 判断奇偶 交换两数 变换符号 求绝对值 位操作压缩空间 筛素数 位操作趣味应用 位操作笔试面试

位操作篇共分为基础篇和提高篇，基础篇主要对位操作进行全面总结，帮助大家梳理知识。提高篇则针对各大IT公司如微软、腾讯、百度、360等公司的笔试面试题作详细的解答，使大家能熟练应对在笔试面试中位操作题目。

下面就先来对位操作作个全面总结，欢迎大家补充。

在计算机中所有数据都是以二进制的形式储存的。位运算其实就是直接对在内存中的二进制数据进行操作，因此处理数据的速度非常快。

在实际编程中，如果能巧妙运用位操作，完全可以达到四两拨千斤的效果，正因为位操作的这些优点，所以位操作在各大IT公司的笔试面试中一直是个热点问题。因此本文将对位操作进行如下方面总结：

 **一． 位操作基础**，用一张表描述位操作符的应用规则并详细解释。
 **二． 常用位操作小技巧**，有判断奇偶、交换两数、变换符号、求绝对值。
 **三． 位操作与空间压缩**，针对筛素数进行空间压缩。
 **四． 位操作的趣味应用**，列举了位操作在高低位交换、二进制逆序、二进制中1的个数以及缺失的数字这4种趣味应用。

希望读者能认真学习和亲自上机输入代码进行实验，相信通过本文及适当的练习可以使你对位操作有更加深入的了解，在笔试面试中遇到位操作相关试题能更加从容。

### 一． 位操作基础
基本的位操作符有与、或、异或、取反、左移、右移这6种，它们的运算规则如下所示：

符号 | 描述 | 运算规则                        by MoreWindows
----|------|-----------------------------------------------
& | 与 | 两个位都为1时，结果才为1
\\|| 或 | 两个位都为0时，结果才为0
^ | 异或 | 两个位相同为0，相异为1
~ | 取反 | 0变1，1变0
<< | 左移 | 各二进位全部左移若干位，高位丢弃，低位补0
>> | 右移 | 各二进位全部右移若干位，对无符号数，高位补0，有符号数，各编译器处理方法不一样，有的补符号位（算术右移），有的补0（逻辑右移）

注意以下几点：

1．  在这6种操作符，只有~取反是单目操作符，其它5种都是双目操作符。

2．  **位操作只能用于整形数据，对float和double类型进行位操作会被编译器报错。**

3．  对于移位操作，在微软的VC6.0和VS2008编译器都是采取算术称位即算术移位操作，算术移位是相对于逻辑移位，它们在左移操作中都一样，低位补0即可，但在右移中逻辑移位的高位补0而算术移位的高位是补符号位。如下面代码会输出-4和3。

~~~
int a = -15, b = 15;
printf("%d %d\n", a >> 2, b >> 2);
~~~
因为15=0000 1111(二进制)，右移二位，最高位由符号位填充将得到0000 0011即3。-15 = 1111 0001(二进制)，右移二位，最高位由符号位填充将得到1111 1100即-4（见注1）。

4．  位操作符的运算优先级比较低，因为尽量使用括号来确保运算顺序，否则很可能会得到莫明其妙的结果。比如要得到像1，3，5，9这些2^i+1的数字。写成int a = 1 << i + 1;是不对的，程序会先执行i + 1，再执行左移操作。应该写成int a = (1 << i) + 1;

5．  另外位操作还有一些复合操作符，如&=、\|=、 ^=、<<=、>>=。

### 二． 常用位操作小技巧
下面对位操作的一些常见应用作个总结，有判断奇偶、交换两数、变换符号及求绝对值。这些小技巧应用易记，应当熟练掌握。

#### 1．判断奇偶
只要根据最未位是0还是1来决定，为0就是偶数，为1就是奇数。因此可以用if ((a & 1) == 0)代替if (a % 2 == 0)来判断a是不是偶数。

下面程序将输出0到100之间的所有奇数。

~~~
for (i = 0; i < 100; ++i)
    if (i & 1)
        printf("%d ", i);
putchar('\n');
~~~

#### 2．交换两数
一般的写法是：
~~~
void Swap(int &a, int &b)
{
    if (a != b)
    {
        int c = a;
        a = b;
        b = c;
    }
}
~~~
可以用位操作来实现交换两数而不用第三方变量：
~~~
void Swap(int &a, int &b)
{
    if (a != b)
    {
        a ^= b;
        b ^= a;
        a ^= b;
    }
}
~~~
可以这样理解：

第一步  a^=b 即a=(a^b);

第二步  b^=a 即b=b^(a^b)，由于^运算满足交换律，b^(a^b)=b^b^a。由于一个数和自己异或的结果为0并且任何数与0异或都会不变的，所以此时b被赋上了a的值。

第三步 a^=b 就是a=a^b，由于前面二步可知a=(a^b)，b=a，所以a=a^b即a=(a^b)^a。故a会被赋上b的值。
再来个实例说明下以加深印象。int a = 13, b = 6;

a的二进制为 13=8+4+1=1101(二进制)

b的二进制为 6=4+2=110(二进制)

第一步 a^=b  a = 1101 ^ 110 = 1011;

第二步 b^=a  b = 110 ^ 1011 = 1101;即b=13

第三步 a^=b  a = 1011 ^ 1101 = 110;即a=6

#### 3．变换符号
变换符号就是正数变成负数，负数变成正数。

如对于-11和11，可以通过下面的变换方法将-11变成11

  *1111 0101(二进制) –取反-> 0000 1010(二进制) –加1-> 0000 1011(二进制)*

同样可以这样的将11变成-11

  *0000 1011(二进制) –取反-> 0000 0100(二进制) –加1-> 1111 0101(二进制)*

因此变换符号只需要取反后加1即可。完整代码如下：
~~~
//by MoreWindows( http://blog.csdn.net/MoreWindows )
#include <stdio.h>
int SignReversal(int a)
{
    return ~a + 1;
}
int main()
{
    printf("对整数变换符号 --- by MoreWindows( http://blog.csdn.net/MoreWindows )  ---\n\n");
    int a = 7, b = -12345;
    printf("%d  %d\n", SignReversal(a), SignReversal(b));
    return 0;
}
~~~
#### 4．求绝对值
位操作也可以用来求绝对值，对于负数可以通过对其取反后加1来得到正数。对-6可以这样：

  *1111 1010(二进制) –取反->0000 0101(二进制) -加1-> 0000 0110(二进制)*

来得到6。

因此先移位来取符号位，int i = a >> 31;要注意如果a为正数，i等于0，为负数，i等于-1。然后对i进行判断——如果i等于0，直接返回。否之，返回~a+1。完整代码如下：

~~~
//by MoreWindows( http://blog.csdn.net/MoreWindows )
int my_abs(int a)
{
    int i = a >> 31;
    return i == 0 ? a : (~a + 1);
}
~~~
现在再分析下。对于任何数，与0异或都会保持不变，与-1即0xFFFFFFFF异或就相当于取反。因此，a与i异或后再减i（因为i为0或-1，所以减i即是要么加0要么加1）也可以得到绝对值。所以可以对上面代码优化下：
~~~
//by MoreWindows( http://blog.csdn.net/MoreWindows )
int my_abs(int a)
{
    int i = a >> 31;
    return ((a ^ i) - i);
}
~~~
注意这种方法没用任何判断表达式，而且有些笔面试题就要求这样做，因此建议读者记住该方法（^_^讲解过后应该是比较好记了）。

#### 三． 位操作与空间压缩
筛素数法在这里不就详细介绍了，本文着重对筛素数法所使用的素数表进行优化来减小其空间占用。要压缩素数表的空间占用，可以使用位操作。下面是用筛素数法计算100以内的素数示例代码（注2）：

~~~
//by MoreWindows( http://blog.csdn.net/MoreWindows )
#include <stdio.h>
#include <memory.h>
const int MAXN = 100;
bool flag[MAXN];
int primes[MAXN / 3 + 1], pi;
//对每个素数，它的倍数必定不是素数。
//有很多重复如flag[10]会在访问flag[2]和flag[5]时各访问一次
void GetPrime_1()
{
    int i, j;
    pi = 0;
    memset(flag, false, sizeof(flag));
    for (i = 2; i < MAXN; i++)
        if (!flag[i])
        {
            primes[pi++] = i;
            for (j = i; j < MAXN; j += i)
                flag[j] = true;
        }
}
void PrintfArray()
{
    for (int i = 0; i < pi; i++)
        printf("%d ", primes[i]);
    putchar('\n');
}
int main()
{
    printf("用筛素数法求100以内的素数\n-- by MoreWindows( http://blog.csdn.net/MoreWindows ) --\n\n");
    GetPrime_1();
    PrintfArray();
    return 0;
}
~~~
运行结果如下：

在上面程序是用bool数组来作标记的，bool型数据占1个字节（8位），因此用位操作来压缩下空间占用将会使空间的占用减少八分之七。

下面考虑下如何在数组中对指定位置置1，先考虑如何对一个整数在指定位置上置1。对于一个整数可以通过将1向左移位后与其相或来达到在指定位上置1的效果，代码如下所示：
~~~
//在一个数指定位上置1
int j = 0;
j |=  1 << 10;
printf("%d\n", j);
~~~
同样，可以1向左移位后与原数相与来判断指定位上是0还是1（也可以将原数右移若干位再与1相与）。
~~~
//判断指定位上是0还是1
int j = 1 << 10;
if ((j & (1 << 10)) != 0)
    printf("指定位上为1");
else
    printf("指定位上为0");
~~~
扩展到数组上，我们可以采用这种方法，因为数组在内存上也是连续分配的一段空间，完全可以“认为”是一个很长的整数。先写一份测试代码，看看如何在数组中使用位操作：

~~~
//by MoreWindows( http://blog.csdn.net/MoreWindows )
#include <stdio.h>
int main()
{
    printf("     对数组中指定位置上置位和判断该位\n");
    printf("--- by MoreWindows( http://blog.csdn.net/MoreWindows )  ---\n\n");
    //在数组中在指定的位置上写1
    int b[5] = {0};
    int i;
    //在第i个位置上写1
    for (i = 0; i < 40; i += 3)
        b[i / 32] |= (1 << (i % 32));
    //输出整个bitset
    for (i = 0; i < 40; i++)
    {
        if ((b[i / 32] >> (i % 32)) & 1)
            putchar('1');
        else
            putchar('0');
    }
    putchar('\n');
    return 0;
}
~~~
运行结果如下：



可以看出该数组每3个就置成了1，证明我们上面对数组进行位操作的方法是正确的。因此可以将上面筛素数方法改成使用位操作压缩后的筛素数方法：

~~~
//使用位操作压缩后的筛素数方法
//by MoreWindows( http://blog.csdn.net/MoreWindows )
#include <stdio.h>
#include <memory.h>
const int MAXN = 100;
int flag[MAXN / 32 + 1];
int primes[MAXN / 3 + 1], pi;
void GetPrime_1()
{
    int i, j;
    pi = 0;
    memset(flag, 0, sizeof(flag));
    for (i = 2; i < MAXN; i++)
        if (!((flag[i / 32] >> (i % 32)) & 1))
        {
            primes[pi++] = i;
            for (j = i; j < MAXN; j += i)
                flag[j / 32] |= (1 << (j % 32));
        }
}
void PrintfArray()
{
    for (int i = 0; i < pi; i++)
        printf("%d ", primes[i]);
    putchar('\n');
}
int main()
{
    printf("用位操作压缩后筛素数法求100以内的素数\n-- by MoreWindows( http://blog.csdn.net/MoreWindows ) --\n\n");
    GetPrime_1();
    PrintfArray();
    return 0;
}
~~~
同样运行结果为：



另外，还可以使用C++ STL中的bitset类来作素数表。筛素数方法在笔试面试出现的几率还是比较大的，能写出用位操作压缩后的筛素数方法无疑将会使你的代码脱颖而出，因此强烈建议读者自己亲自动手实现一遍，平时多努力，考试才不慌。

位操作的压缩空间技巧也被用于strtok函数的实现，请参考《strtok源码剖析 位操作与空间压缩》（http://blog.csdn.net/morewindows/article/details/8740315）

#### 四． 位操作的趣味应用
位操作有很有趣的应用，下面列举出一些，欢迎读者补充。

**1．  高低位交换**
给出一个16位的无符号整数。称这个二进制数的前8位为“高位”，后8位为“低位”。现在写一程序将它的高低位交换。例如，数34520用二进制表示为：

*10000110 11011000*

将它的高低位进行交换，我们得到了一个新的二进制数：

*11011000 10000110*

它即是十进制的55430。

这个问题用$位操作解决起来非常方便，设x=34520=10000110 11011000(二进制) 由于x为无符号数，右移时会执行逻辑右移即高位补0，因此x右移8位将得到00000000 10000110。而x左移8位将得到11011000 00000000。可以发现只要将x&gt;&gt;8与x&lt;&lt;8这两个数相或就可以得到11011000 10000110。用代码实现非常简洁：

~~~
//高低位交换 by MoreWindows( http://blog.csdn.net/MoreWindows )
#include <stdio.h>
template <class T>
void PrintfBinary(T a)
{
    int i;
    for (i = sizeof(a) * 8 - 1; i >= 0; --i)
    {
        if ((a >> i) & 1)
            putchar('1');
        else
            putchar('0');
        if (i == 8)
            putchar(' ');
    }
    putchar('\n');
}
int main()
{
    printf("高低位交换 --- by MoreWindows( http://blog.csdn.net/MoreWindows )  ---\n\n");

    printf("交换前:    ");
    unsigned short a = 3344520;
    PrintfBinary(a);

    printf("交换后:    ");
    a = (a >> 8) | (a << 8);
    PrintfBinary(a);
    return 0;
}
~~~
运行结果如下：



**2．  二进制逆序**

我们知道如何对字符串求逆序，现在要求计算二进制的逆序，如数34520用二进制表示为：

*10000110 11011000*

将它逆序，我们得到了一个新的二进制数：

*00011011 01100001*

它即是十进制的7009。

回顾下字符串的逆序，可以从字符串的首尾开始，依次交换两端的数据。在二进制逆序我们也可以用这种方法，但运用位操作的高低位交换来处理二进制逆序将会得到更简洁的方法。类似于归并排序的分组处理，可以通过下面4步得到16位数据的二进制逆序：

第一步：每2位为一组，组内高低位交换
~~~
      10 00 01 10  11 01 10 00

  -->01 00 10 01 11 10 01 00
~~~
第二步：每4位为一组，组内高低位交换
~~~
      0100 1001 1110 0100

  -->0001 0110 1011 0001
~~~
第三步：每8位为一组，组内高低位交换
~~~
      00010110 10110001

  -->01100001 00011011
~~~
第四步：每16位为一组，组内高低位交换
~~~
      01100001 00011011

  -->00011011 01100001
~~~
对第一步，可以依次取出每2位作一组，再组内高低位交换，这样有点麻烦，下面介绍一种非常有技巧的方法。先分别取10000110 11011000的奇数位和偶数位，空位以下划线表示。
~~~
      原 数    10000110 11011000

      奇数位 1_0_0_1_ 1_0_1_0_

      偶数位  _0_0_1_0 _1_1_0_0
~~~
将下划线用0填充，可得
~~~
      原 数    10000110 11011000

      奇数位 10000010 10001000

      偶数位 00000100 01010000
~~~
再将奇数位右移一位，偶数位左移一位，此时将这两个数据相或即可以达到奇偶位上数据交换的效果了。
~~~
      原 数           10000110 11011000

      奇数位右移 01000001 01000100

      偶数位左移 00001000 10100000

      相或得到      01001001 11100100
~~~
可以看出，结果完全达到了奇偶位的数据交换，再来考虑代码的实现——
~~~
      取x的奇数位并将偶数位用0填充用代码实现就是x & 0xAAAA

      取x的偶数位并将奇数位用0填充用代码实现就是x & 0x5555
~~~
因此，第一步就用代码实现就是：
~~~
       x = ((x & 0xAAAA) >> 1) | ((x & 0x5555) << 1);
~~~
类似可以得到后三步的代码。完整程序如下：

~~~
//二进制逆序 by MoreWindows( http://blog.csdn.net/MoreWindows )
#include <stdio.h>
template <class T>
void PrintfBinary(T a)
{
    int i;
    for (i = sizeof(a) * 8 - 1; i >= 0; --i)
    {
        if ((a >> i) & 1)
            putchar('1');
        else
            putchar('0');
        if (i == 8)
            putchar(' ');
    }
    putchar('\n');
}
int main()
{
    printf("二进制逆序 --- by MoreWindows( http://blog.csdn.net/MoreWindows )  ---\n\n");

    printf("逆序前:    ");
    unsigned short a = 34520;
    PrintfBinary(a);

    printf("逆序后:    ");
    a = ((a & 0xAAAA) >> 1) | ((a & 0x5555) << 1);
    a = ((a & 0xCCCC) >> 2) | ((a & 0x3333) << 2);
    a = ((a & 0xF0F0) >> 4) | ((a & 0x0F0F) << 4);
    a = ((a & 0xFF00) >> 8) | ((a & 0x00FF) << 8);
    PrintfBinary(a);
}
~~~
运行结果如下：


**3．  二进制中1的个数**
统计二进制中1的个数可以直接移位再判断，当然像《编程之美》书中用循环移位计数或先打一个表再计算都可以。本文详细讲解一种高效的方法。以34520为例，可以通过下面四步来计算其二进制中1的个数二进制中1的个数。

第一步：每2位为一组，组内高低位相加
~~~
      10 00 01 10  11 01 10 00

  -->01 00 01 01  10 01 01 00
~~~
第二步：每4位为一组，组内高低位相加
~~~
      0100 0101 1001 0100

  -->0001 0010 0011 0001
~~~
第三步：每8位为一组，组内高低位相加
~~~
      00010010 00110001

  -->00000011 00000100
~~~
第四步：每16位为一组，组内高低位相加
~~~
      00000011 00000100

  -->00000000 00000111
~~~
这样最后得到的00000000 00000111即7即34520二进制中1的个数。类似上文中对二进制逆序的做法不难实现第一步的代码：
~~~
       x = ((x & 0xAAAA) >> 1) + (x & 0x5555);
~~~
好的，有了第一步，后面几步就请读者完成下吧，先动动笔再看下面的完整代码：

~~~
//二进制中1的个数  by MoreWindows( http://blog.csdn.net/MoreWindows )
#include <stdio.h>
template <class T>
void PrintfBinary(T a)
{
    int i;
    for (i = sizeof(a) * 8 - 1; i >= 0; --i)
    {
        if ((a >> i) & 1)
            putchar('1');
        else
            putchar('0');
        if (i == 8)
            putchar(' ');
    }
    putchar('\n');
}
int main()
{
    printf("二进制中1的个数 --- by MoreWindows( http://blog.csdn.net/MoreWindows )  ---\n\n");

    unsigned short a = 34520;
    printf("原数    %6d的二进制为:  ", a);
    PrintfBinary(a);

    a = ((a & 0xAAAA) >> 1) + (a & 0x5555);
    a = ((a & 0xCCCC) >> 2) + (a & 0x3333);
    a = ((a & 0xF0F0) >> 4) + (a & 0x0F0F);
    a = ((a & 0xFF00) >> 8) + (a & 0x00FF);
    printf("计算结果%6d的二进制为:  ", a);
    PrintfBinary(a);
    return 0;
}
~~~
运行结果如下：



可以发现巧妙运用分组处理确实是解决很多二进制问题的灵丹妙药。

**4．  缺失的数字**

很多成对出现数字保存在磁盘文件中，注意成对的数字不一定是相邻的，如2, 3, 4, 3, 4, 2……，由于意外有一个数字消失了，如何尽快的找到是哪个数字消失了？

由于有一个数字消失了，那必定有一个数只出现一次而且其它数字都出现了偶数次。用搜索来做就没必要了，利用异或运算的两个特性——1.自己与自己异或结果为0，2.异或满足交换律。因此我们将这些数字全异或一遍，结果就一定是那个仅出现一个的那个数。 示例代码如下：

~~~
//缺失的数字  by MoreWindows( http://blog.csdn.net/MoreWindows )
#include <stdio.h>
int main()
{
    printf("缺失的数字 --- by MoreWindows( http://blog.csdn.net/MoreWindows )  ---\n\n");

    const int MAXN = 15;
    int a[MAXN] = {1, 347, 6, 9, 13, 65, 889, 712, 889, 347, 1, 9, 65, 13, 712};
    int lostNum = 0;
    for (int i = 0; i < MAXN; i++)
        lostNum ^= a[i];
    printf("缺失的数字为:  %d\n", lostNum);
    return 0;
}
~~~
在这个题目中有一个数字丢失了，如果有两个数字丢失了应该怎么做了，请看《【白话经典算法系列之十二】数组中只出现1次的两个数字(百度面试题)》
地址：<http://blog.csdn.net/morewindows/article/details/8214003>



位操作是一种高效优美的方法，同时由于其高效的运算性能和掌握难度较大，位操作运算一直是笔试面试时的热门话题之一。本文详细总结了位操作的方法与技巧并列出4种位操作趣味应用，如果读者能亲自上机实现代码，相信必能更好应对笔试和面试时可能遇到的位操作问题。

另外，欢迎各位能提供笔试面试中的位操作相关的题目给我，我将会在提高篇中加入这些。谢谢大家。





注1．int类型一般占4字节，32位。因此15准确表达为

15=00000000 00000000 00000000 00001111(二进制)

-15准确表达为

-15=11111111 11111111 11111111 11110001(二进制)

为了简便起见，文章中使用15=00001111(二进制)，-15=11110001(二进制)。



注2．这种筛素数的方法很朴素，会多次重复访问数据，有什么办法能改进一下吗？请看[《改进的筛素数方法》](http://blog.csdn.net/morewindows/article/details/7347459)一文。



转载请标明出处，原文地址：<http://blog.csdn.net/morewindows/article/details/7354571>

如果觉得本文对您有帮助，请点击‘顶’支持一下，您的支持是我写作最大的动力，谢谢。

