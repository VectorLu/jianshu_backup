[洛叶的完整代码](https://github.com/VectorLu/algorithmCompetition)
####中国剩余定理，题目要求：
每组数据包含3个非负整数a,b,c，表示队尾人数（a<3, b<5, c<7），输出总人数的最小值（或报告无解）。已知总人数不小于10，不超过100。输入到文件结束为止。
我的这个解法是考虑到测试输入的a,b,c都满足条件，未对其进行合法判定。其实还应该加入对a, b , c对于3，5，7的大小判定才更加严密。

```C
//韩信点兵问题，又称中国剩余定理
//5和7的公倍数除以3余1的最小数是70
//3和7的公倍数除以5余1的最小数是21
//3和5的公倍数除以7余1的最小数是15
//总数除以3、5、7的余数分别设为a,b,c
//则sum = a*70 + b*21 + c*15
//再按范围减去105(3, 5, 7的最小公倍数)
#include <stdio.h>
int main()
{
  int a, b, c;
  int kase = 0;
  while(scanf("%d%d%d", &a, &b, &c)==3)
  {
    int sum = 0;
    sum = a*70 + b*21 + c*15;
    while(sum > 100)
    {
      sum -= 105;
    }
    if(sum < 10)
    {
      printf("Case%d: No answer\n", kase);
    }
    else
    {
      printf("Case%d: %d\n", kase,sum);
    }
    kase++;
  }
  return 0;
}
```

####子序列的和，题目要求：
输入两个正整数 n<m<10^6, 输入1/n^2+1/(n+1)^2+...+1/m^2，保留5位小数。输入包含多组数据，结束标记为n=m=0。
样例输入：
2 4
65536 655360
0 0 
样例输出：
Case1: 0.42361
Case2: 0.00001
```C
//主要在于m<10^6，算法竞赛中的int类型范围大约是+-2*10^10，
//所以i^2可能会超过int类型表示的范围，要用更大的类型来存储
#include <stdio.h>
int main()
{
  long n,m;
  int kase = 1;
  while(scanf("%ld%ld", &n, &m)==2 && n && m)
  {
    double sum = 0;
    for(long i = n; i <= m; i++)
    {
      sum += (double)1/(i*i);
    }
    printf("Case%d: %.5f\n", kase, sum);
    kase++;
  }
  return 0;
}
```
####分数化小数
输入正整数a，b，c，输出a/b的小数形式，精确到小数点后c位。a，b<=10^6，c<=100。输入包含多组数据，结束标记为 a = b = c = 0。
样例输入：
1 6 4 
0 0 0
样例输出：
Case1： 0.1667
```C
//关键在于小数部分的除法究竟是怎样除的
//已经太习惯去除但是忘记了究竟是怎样的原理
//就是把小数当做整数一样除，不过得到的商放在小数点之后
void decimal(int a, int b, int c)
{
  printf("%d.", a/b);
  int rem = a%b;
  for(int i = 1; i <= c; i++)
  {
    rem *= 10;
    printf("%d", rem/b);
    rem = rem%b;
  }
}
```


####题目：
用1，2，3，...，9组成3个三位数abc,def和ghi，每个数字恰好使用一次，要求abc:def:ghi = 1:2:3。按照“abc def ghi”的格式输出所有解，每行一个解。提示：不必太动脑筋（既然题目说了不必太懂脑筋我就用了非常暴力的方法，见method2）
[暴力法求解的完整代码](https://github.com/VectorLu/algorithmCompetition/blob/master/ch2/permutation_force.c)

```C
//由1~9的和为45，9！= 362880判定是否互异
//其实在不知道结果的前提下不够严谨
//要学会熟练运用指针
//method 1
#include <stdio.h>
void result(int num, int *result_add, int *result_multi)
{
  int a = num/100;
  int b = num/10%10;
  int c = num%10;
  
  *result_add += (a+b+c);
  *result_multi *= (a*b*c);
}

int main()
{
  int result_add , result_multi;
  for(int abc = 100; abc < 333; abc++)
  {
    int def = 2*abc;
    int ghi = 3*abc;

    result_add = 0;
    result_multi = 1;

    result(abc, &result_add, &result_multi);
    result(def, &result_add, &result_multi);
    result(ghi, &result_add, &result_multi);
    
    //判断是否互异
    if(result_add == 45 && result_multi == 362880)
    {
      printf("%d %d %d\n", abc, def, ghi);
    }
  }
  return 0;
}

```

```C
//method 2  片段             
for(i = 1; i < 10; i++)
                  {
                    if(i == a){continue;}
                    if(i == b){continue;}
                    if(i == c){continue;}
                    if(i == d){continue;}
                    if(i == e){continue;}
                    if(i == f){continue;}
                    if(i == g){continue;}
                    if(i == h){continue;}
                    int abc = 100*a + 10*b + c;
                    int def = 100*d + 10*e + f;
                    int ghi = 100*g + 10*h + i;
                    if(def == 2*abc && ghi == 3*abc)
                    {
                      printf("%d %d %d\n", abc, def, ghi);
                    }      
int main()
{
    permutation();
    return 0;
}
```

####实验

![实验代码](http://upload-images.jianshu.io/upload_images/1213502-9949a96f4f69261b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Xcode给出的警告](http://upload-images.jianshu.io/upload_images/1213502-71c0cf8b1c5720fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![输出结果](http://upload-images.jianshu.io/upload_images/1213502-e2a2c70face3b28e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
