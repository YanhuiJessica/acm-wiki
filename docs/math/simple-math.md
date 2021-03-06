!!! note "Copyright"
    本页面贡献者：[LyuLumos](https://github.com/LyuLumos)，[YanhuiJessica](https://github.com/YanhuiJessica)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 素数与合数
素数(质数)：除了1和自身外，不能被其他数整除的正整数。素数只有两个因子。

合数：拥有两个以上因子的正整数。

1既不是素数也不是合数。


### 试除法判断素数
$O(\sqrt n)$
```cpp
int isPrime(int n)
{
    int i;
    for(i=2; i<=sqrt(n); i++)    
    {
        if(n%i == 0)    // 如果不为素数返回0 
    　　{
           return 0;
        }
    }
    return 1;    // 反之则返回1 
}
```

### 素数普通筛-埃拉托斯特尼(Eratosthenes)筛法

基本思想：初始将所有大于等于2的数放在一个集合中，每次筛选后集合中剩余最小的数是质数，将它的倍数去掉。

算法结束时，没有被筛去的数就是质数。每个数要被自己所有的因子标记一遍，所以普通筛的时间复杂度为$O(nloglogn)$
```cpp
const int maxn = 1000;
bool isPrime[maxn + 5];
void getPrime()
{
    memset(isPrime, true, sizeof(isPrime));
    isPrime[1] = false; // 如果用不到1也可以不用写
    for(int i = 2; i <= maxn; i++)
        if(isPrime[i])
            for(int j = i; i * j <= maxn; j++)
                isPrime[i * j] = false;
}
```


### ~~素数线性筛~~(数论部分会涉及)


## 斐波那契数列

斐波那契数列（The Fibonacci sequence）的递推式如下：

$$ F_0 = 0, F_1 = 1, F_n = F_{n-1} + F_{n-2} $$

该数列的前几项如下：

$$ 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ... $$


### 简单性质
- 斐波那契数列增长速度趋近于$2^n$
- 斐波那契数列相邻两个数之间的比值趋近于黄金分割率
- 前$n$项之和加一等于第$n+2$项
- 以斐波那契数列相邻两项作为输入会使欧几里德算法达到最坏复杂度
### 拓展
[CUC ACM-Wiki - 数列](https://cuccs.github.io/acm-wiki/math/num-sequence/)


## 卡特兰数
Catalan数定义如下：

$$f_0=1,\,\,f_n = \sum_{i=0}^{n-1}f(i)\cdot f(n-i-1), n \in N_+$$

该数列的前几项如下：

$$1, 1, 2, 5, 14, 42, 132, 429, 1430, 4862, 16796, 58786, 208012 ......$$



### 递推公式
1. $f(n) = \frac{1}{n+1}\tbinom{2n}{n}$

2. $f(n) = \tbinom{2n}{n}-\tbinom{2n}{n-1}$  // 推荐

3. $f(n) = \sum_{i=0}^{n-1}f(i)×f(n-i-1)$
   
4. $f(n) = \frac{f(n-1)×(4n-2)}{n+1}$




#### 应用
* 二叉树的计数问题：已知二叉树有 $n$ 个结点，求能构成多少种不同的二叉树。  
* 括号化问题：一个合法的表达式由()包围，()可以嵌套和连接，如：(())()也是合法表达式，现给出 $n$ 对括号，求可以组成的合法表达式的个数。  
* 划分问题：将一个凸 $n+2$ 多边形区域分成三角形区域的方法数。
* 出栈问题：一个栈的进栈序列为 $1,2,3,..n$，求不同的出栈序列有多少种。    
* 路径问题：在 $n\cdot n$ 的方格地图中，从一个角到另外一个角，求不跨越对角线的路径数有多少种。    
* 握手问题：$2n$ 个人均匀坐在一个圆桌边上，某个时刻所有人同时与另一个人握手，要求手之间不能交叉，求共有多少种握手方法。




## 简单数学实战



### 找规律

- 1 1 2 3 5 8 13 ... 
- 1 1 2 5 14 ...
- 1 5 10 10 5 1
- 1 4 10 20 35 56 ...


### 结论

- 有限项数列都可以用公式表示，比如Lagrange插值公式就可以求出通项。
- 公式显然不止一个。

所以猜测是有风险的。



### 举个栗子

1，2，3，4，5，？

显然答案是126，因为 $a_n = (n-1)(n-2)(n-3)(n-4)(n-5)+n$。

但其实它有很多种答案，比如 $a_n =C \cdot (n-1)(n-2)(n-3)(n-4)(n-5)+n, C为常数$。



### 反例

!!! question "[Codeforces 656A - Da Vinci Powers](https://vjudge.net/contest/372154#problem/D)"

    找规律，输入3输出8，输入10输出1024，输入13输出8092。


需要用下面介绍的OEIS解决。
### OEIS

[The On-Line Encyclopedia of Integer Sequences® (OEIS®)](https://oeis.org/)

包含了绝大部分数列，只能在网络比赛中使用。

**例题**

!!! question "题目来源：2020牛客寒假算法小白训练营"

    一个游戏，人物攻击力为0-10，怪物的生命值为10，每次攻击对怪物以均等的1/11的概率，随机造成0，1，2……10中之一点数的伤害，怪物的体力达到0或更低时视为击败，问击败怪物的平均攻击次数？




设当怪物血剩余 $n$ 时攻击次数的期望为 $a_n$。则显然 $a_0＝0$ ，考虑 $n < 1$，此时肯定要先打一次。打完后有 $\frac{11-n}{11}$的概率怪兽死亡，另外有$\frac{1}{11}$的概率回到 $a_1-a_n$中的一种情况。故有$a_n=1+\frac{1}{11}(a_1+...+a_n)$。令$Sn=a_1+...a_n$，则有$a_{n}=1+\frac{1}{11}S_{n}$。解得 $a_n=(\frac{11}{10})^n$。本题答案为 $1.1^{10}$。



!!! note "具体计算过程"

    $$a_{n}=1+\frac{1}{11}S_{n}$$

    $$a_{n-1}=1+\frac{1}{11}S_{n-1}$$

    相减 $a_n-a_{n-1}=\frac{1}{11}*a_n$

    $$a_n=\frac{11}{10}a_{n-1}(n >0)$$

    当怪物血剩余 $n$ 时攻击次数的期望为 $a_n$。则显然 $a_0＝0$

    $n = 1$代入解得

    $a_1=\frac{11}{10}$ 则 $a_n=(\frac{11}{10})^n$