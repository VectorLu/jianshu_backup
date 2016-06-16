#函数与递归

###struct & typedef
- struct的使用
```C
struct Point{ double x, y;};
//定义完结构体的大括号后面还要有分号
double dist(struct Point a, struct Point b)
{
    return hypot(a.x - b.x, a.y - b.y);
    //  hypot函数不是标准C的函数，求两个参数平方和的平方根
}
```

- typedef的使用(定义一个新的类型，想使用原生数据类型int等一样使用新定义的类型）
```C
typedef struct{ double x, y; } Point;
double dist (Point a, Point b)
{
    return hypot(a.x - b.x, a.y - b.y);
}
```
