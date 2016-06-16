[洛叶的完整代码](https://github.com/VectorLu/algorithmCompetition)
##数组

####开灯问题
```C
/*
 有n盏灯，编号为1-n。第1个人把所有灯打开，第2个人按下所有编号为2
 的倍数的开关（这些灯将被关掉），第3个人按下所有编号为3的倍数的开关
 （其中关掉的灯打开，打开的灯关闭），依次类推，一共有k个人，问最后有
 哪些灯开着？输入n和k，输出开着的灯的编号。k<=n<=1000。
 */
#include <stdio.h>
#include <string.h>
//将较大的数组定义在main函数外，否则程序可能无法正常运行
#define maxn 1010
int light[maxn];

int main()
{
    int n, k, first = 1;
    //将light数组清零
    memset(light, 0, sizeof(light));
    scanf("%d%d", &n, &k);
    //模拟开关灯，当i = 1的时候，j%i == 0
    //light[j]都变为1，即为开灯
    for(int i = 1; i <= k; i++)
    {
        for(int j = 1; j <= n; j++)
        {
            if(j % i == 0){light[j] = !light[j];}
        }
    }
    for(int i = 1; i <= n; i++)
    {
        if(light[i])
        {
          //当不是第一个输出的时候，在输出数字之前还
          //输出一个空格，即将输出的数字用空格隔开
          if(first) {first = 0;}
          else {printf(" ");}
          printf("%d", i);
        }
    }
    printf("\n");
    return 0;
}
```

####蛇形填数（摘要）
```C
/*
 例如
 10 11 12 1
 9  16 13 2
 8  15 14 3
 7  6  5  4
 其实就是从右上方开始下、左、上、右地循环填数
 */
//初始化
    tot = snake[x = 0][y = n - 1] = 1;
    
    //下左上右，并避开已经填过数的位置
    while(tot < n*n)
    {
        while(x+1<n && !snake[x+1][y])
        {snake[++x][y] = ++tot;}
        while(y-1>=0 && !snake[x][y-1])
        {snake[x][--y] = ++tot;}
        while(x-1>=0 && !snake[x-1][y])
        {snake[--x][y] = ++tot;}
        while(y+1<n && !snake[x][y+1])
        {snake[x][++y] = ++tot;}
    }
```
***
##字符串
```
scanf("%s", s)
//遇到空格等会停下来
```

***
####竖式问题
这道题关键在于判断abc，de，x，y，z的每一位数是否都属于给定的数字集合，感觉这道题只是用来了解一下sprintf()函数和strchr()函数，没有什么实际意义。

```
/*
p41的竖式问题

找出所有的形如abc*de的算式，是的在完整的竖式中，
所有的数字都属于一个特定的数字集合。输入数字集合（相邻数字间没有空格），
输出所有满足题意的竖式。每个竖式前应有编号，之后应有一个空行。最后输出解的总数

样例输入：
2357

样例输出：
<1>
  775
X  33
-----
 2325
2325
-----
25575

*/

#include <stdio.h>
#include <string.h>

int main()
{
  int count = 0;
  char s[20], buf[100];
  scanf("%s", s);
  for(int abc = 111; abc < 1000; abc++)
  {
    for(int de = 11; de < 100; de++)
    {
      int x = abc*(de%10);
      int y = abc*(de/10);
      int z = abc*de;

      //sprintf()将信息输出到字符串
      //应当保证字符串足够大，可以容纳输出信息
      //这里将整数转换成了字符串，放到字符数组里
      sprintf(buf, "%d%d%d%d%d", abc, de, x, y,z);
      int ok = 1;
      //strlen()获取buf的实际长度
      for(int i = 0; i < strlen(buf); i++)
      {
        //strchr()在一个字符串中查找单个字符
        //如果buf中没有这个数
        if(strchr(s, buf[i]) == NULL)
        {ok = 0;}
      }
      if(ok)
      {
        printf("<%d>\n", ++count);
        printf("%5d\nX%4d\n-----\n%5d\n%4d\n-----\n%5d\n\n", abc, de, x, y, z );
      }
    }
  }
  printf("The number of solutions = %d\n", count);
  return 0;
}

```
***
####Tex中的引号
1. quotesTex.c是p45的例题3-1：将双引号转换为Tex中的左引号和右引号。
  1. 解题关键在于设置标志变量来判断一个双引号究竟是左引号还是右引号。
  2. 输入字符串：使用```getchar（）```一个一个地获取字符。文件结束的标志是EOF,定义在```<stdio.h>```里。关键代码只有两行。
```
           printf("%s", q? "``": "''");
           q = !q;
```
***
####WERTYU, UVa10082
用键盘打出的错位字符串，将其纠正，要求是把每个都变成其前一位，输入始终合法，即不考虑输入为`QAZ的情况，输入均为大写。
**注意getchar()的返回值是标准输入的ASCII码，所以读零的时候s[i] = 48，不是0，所以if还是执行。**
```
#include <stdio.h>
char *s = "`1234567890-=QWERTYUIOP[]\\ASDFGHJKL;'ZXCVBNM,./";
int main(void)
{
	int i, c;
	while((c = getchar()) != EOF)
	{
		for(i = 1; s[i] && s[i] != c; i++)
			;
    /*注意getchar()的返回值是标准输入的ASCII码，
    所以读零的时候s[i] = 48，不是0，所以if还是执行
    */
		if(s[i])
			putchar(s[i-1]);
		else
			putchar(c);
	}
	return 0;
}
```
