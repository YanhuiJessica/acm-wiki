!!! note "Copyright"
    本页面贡献者：[HBlade](https://github.com/HBlade)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 学习目标

PS:一节课想搞清楚DP是不可能的,回去如果不多看看什么是DP，什么叫状态、决策和转移，那就会一直都搞不懂什么是DP

* 动态规划的概念(状态、决策和转移)

* 动态规划的原理

* 记忆化搜索

* 动态规划解题的一般思路

* 动态规划样例分析

## 动态规划基本思想
* 抽象来看，动态规划就是将一个庞大的问题分解成小问题，通过小问题之间的关联，逐步解决每一个小问题，从而解决大问题。

 
!!! question "组合木块"

    你有两种木块，一种长度为1，一种长度为2，现在让你组成一个长度为n的长条木块，有多少种排列组合方案？$n<=10^5$


拆分方案：用F[i]表示长度为i的长条有多少种情况
关联：F(n)=F(n-1)+F(n-2)
从而往后推导到F(n)，时间复杂度为O(n)。

 


## 例题引入

!!! question 
	zls想成为世界无敌的dp大师，他的训练一共有n天，第i天有a[i]种训练计划,选择第j种训练能提高f[i][j]分。zls每天只能选一种训练，请问n天之后zls的分值最高可以到达多少? 

	1≤n,a[i]≤1000



​	做法：贪心即可


```c++
int sum=0;
for(int i=0;i<n;i++){
	sum+=f[i][max_element(f[i],f[i]+a[i])-f[i]];
    //max_element为STL的函数，用于找到数组中的最大值，返回数组最大值所在的元素的指针（地址），具体用法大家自行上网查询
}
printf("%d\n",sum);//时间复杂度:O(n*a[i]) 1e6
```

 
!!! question
	zls想成为世界无敌的dp大师，他的训练一共有n天，第i天有i种训练方式，如果今天zls用了第j种训练方式，明天他只能用第j种或者第j+1种训练方式了，第i天的第j种训练方式增加的Rating是f[i][j]，这样的话zls最终能达到最大的Rating值会是多少呢？
	1≤n,m,a[i]≤1000


 


数字三角形(dp始祖)：不能通过贪心来选择当前层的最大值，因为下一层的选择会被上一层的选择所影响（可能下一层有一个很大的值却因为上一层没选好就选不到它了）。因此使用DP来处理。<br>

## 什么是DP？？？


先知道一下什么是记忆化搜索：<br>

!!! question
	用递归实现求斐波那契数列对1e9+7取模的前100000项。
	
（单纯的递归100项左右就卡死了，不信你可以试试），所以要怎么做呢？斐波那契数列：1,1,2,3,5,8……f(n)=f(n-1)+f(n-2)，(n≥3)

 

单纯的看递归求fib,假设我们要递归求fib(10)，那么fib(10)=fib(9)+fib(8)，求fib(9)又需要fib(8)+fib(7)，求fib(8)又需要fib(7)+fib(6)……

我们可以看出，其实上面的搜索是有重复的部分的，比如在fib(10)=fib(9)+fib(8)这一步，对于fib(9)，求出它的时候就已经知道了fib(8)、fib(7)是多少，但是再求fib(8)的时候又是当作我们不知道fib(8)是多少来往下递归去找fib(7)和fib(6)，其实这些部分在搜索fib(9)的时候已经搜索过了，这样会导致很多重复搜索的部分。当数量级很大的时候，重复搜索的部分会被放得很大，从而导致时间复杂度很高。

那咋办呢？？？

存下来刚刚搜过的数不就好了嘛！下次再搜的时候，发现这个地方是存在一个值的，那就说明这个地方已经被搜索过了，返回那个值就好了。

 

```c++
const int maxn=1e6+6;
const int mod=1e9+7;
int fib[maxn];

int f(int a){
    if(a==1||a==2) return 1;
    if(fib[a]!=0) return fib[a];
    return fib[a]=(f(a-1)+f(a-2))%mod;
}

```

这就是记忆化搜索了，其实就是把搜过的东西都记下来，以后再搜到一样的部分就不用计算直接输出了。

 

那什么是DP？DP就是通过记忆化搜索，先记录下第一步是什么状态，之后的每一步根据上一步的状态来决定并存储，直到最后一步。最后利用我们通过DP得到的数组处理输出想要的答案。



对于数字三角形，我们只需要记录下每个状态的和的最大值即可，我们选择的状态是当到了第i行第j列的时候，此时能得到的和最大值是多少。

```c++

int dp[1010][1010];int a[1010][1010];
memset(dp,0,sizeof(dp));
for(int i=n;i>=1;i--){
	for(int j=1;j<=i;j++){
		dp[i][j]=max(dp[i+1][j],dp[i+1][j+1])+a[i][j];
    }
}
printf("%d",dp[1][1]);

```




!!! question
	zls又想成为世界无敌的dp大师，他的训练一共有m天，一共有n种训练可供选择，第i种训练需要花费a[i]天，结束以后会增长b[i]点Rating,一种训练只能进行一次，请问n天之后zls的Rating值最高可以到达多少?      1≤n,m,a[i]≤1000

  

  难点：贪心去贪b[i]最大值无效，因为有a[i]的限制;a[i]的限制使得它跟贪心的问题完全不一样，因此需要用DP来实现编程。（背包DP）





```c++
int dp[1010][1010];
//第一维下标：前i个训练得到的状态；
//第二维下标:当前状态已花费多少时间;数组存放当前状态的最大价值是多少。
memset(dp,0,sizeof(dp));//初始化为0
for(int i=1;i<=n;i++){//前i个物品的状态
	for(int j=0;j<=m;j++){
    	dp[i][j]=dp[i-1][j];
    }
	for(int j=a[i];j<=m;--j){
//枚举放入当前物品的背包容量会是多少
		dp[i][j]=max(dp[i][j],dp[i-1][j-a[i]]+b[i]);
//当前状态是从上一个i-1的状态的背包容量j-a[i]选择放不放入这个东西的容量转移过来
	}
}
printf("%d",dp[n][m]);//时间复杂度O(n*m) 1e6
```

 

背包问题其实可以用一维数组去解决，这样就会更加节省空间复杂度。但是时间复杂度并没有变化。


```c++
int dp[1010];//下标是背包的容量
memset(dp,0,sizeof(dp));
for(int i=0;i<n;i++){
	for(int j=m;j>=a[i];--j){
		dp[j]=max(dp[j],dp[j-a[i]]+b[i]);
//背包容量为j的状态从背包容量为j-a[i]的状态转移（选择放入这个东西）
    }
}
printf("%d",dp[m]);
```


大家可以看到，转移过程中背包容量转移是从大到小转移的，为什么要从大到小转移而不是从小到大转移呢？（什么是状态转移（更新状态）?）

 

当状态未进行过更新时，我们可以认为未更新的状态是不存在当前的状态转移的，也就是说，在这个问题中，未更新的状态是还没有做当前的训练的。

举例：如果从小到大更新dp值，假设dp[j]从dp[j-a[i]]处经过了更新，那么之后的dp[j+a[i]]也就会从dp[j]处	更新。假设dp[j]的决策是取第i件物品，那么dp[j]的状态是做了第i次训练了；之后我们的dp[j+a[i]]会从dp[j]这个状态转移，因为之前已经做了第i次训练，按理来说是不能再做这个训练了，但是如果继续做第i个训练，就会导致决策与题意不符。


那么，如果从大到小更新dp值，假设dp[j]从dp[j-a[i]]处更新dp值，由于dp[j-a[i]]处还未进行过更新，所以这个状态是不会去做第i次训练的，所以转移有效。而dp[j-a[i]]是从dp[j-a[i]-a[i]]状态转移过来，同理因为dp[j-a[i]-a[i]]也是未经过状态转移的，所以那个状态也不会做第i个训练，因此转移同样有效，而转移后的dp[j-a[i]]也不会影响之前转移的dp[j]，因为dp[j]是从未进行更新的dp[j-a[i]]转移过来。

 

!!! question
	zls更想成为世界无敌的dp大师，他的训练一共有n天，一共有m种训练可供选择，第i种训练需要花费a[i]天，结束以后会增长到b[i]点Rating,一种训练可以进行无数次，请问n天之后zls的Rating值最高可以到达多少?       1≤n,m,a[i]≤1000

   

  难点：与刚刚的题很类似，但是每种训练可以无数次了，所以只需要变一下刚刚的代码，用训练数量为无数次的方法来转移就好。（完全背包DP）

 

```c++
int dp[1010];//下标是背包的容量
memset(dp,0,sizeof(dp));
for(int i=0;i<n;i++){
	for(int j=a[i];j<=m;++j){
		dp[j]=max(dp[j],dp[j-a[i]]+b[i]);//背包容量为j的状态从背包容量为j-a[i]的状态转移（选择放入这个东西）
    }
}
printf("%d",dp[m]);//时间复杂度：O(n*m) 1e6
```
也就是刚刚的背包dp中背包容量从小到大转移，因为从小到大转移是一个物品可以取多次的。

 

**思考：如果训练时间（weight）很大(1e9)，但是训练成果（value）很小（1e3），这样的话应该怎么进行dp？**


如果有兴趣，还可以想想，如果每种训练是有次数的但不是无限次，那又要怎么转移？（多重背包dp）

 

除了上面的基本的DP，还有状压DP(通过数字来存储状态)，插头DP，树形DP（需要知道什么是树）等等。<br>
还有两个比较经典的DP是最长上升子序列（LCS），最长公共子序列（LIS），由于讲是很难讲懂的，如果你们能真正了解了什么是DP，上面的两个题就会迎刃而解，也能在脑中留下深刻的记忆。

## 关于无后效性：
因为之前往后面转移的时候，前面记录下的都是前面每种状态的最优值，有可能会遇到这样一种情况：之前的某个状态的最优值可能因为后面某个值而导致之前取的最优值并不是适合当前值的最优值（例如：A、B两个公司都需要招8个人，候选者都有a,b能力值，如何取人才能使得A公司招到员工的a能力值总和和B公司员工的b能力值总和加起来最大。这个就不能用动态规划去做，因为会遇到一种情况是，假设前面A公司取到一个a,b能力值同样优秀的人，但是后面如果有一个a能力值很优秀但是没有前面那个人优秀，同时这个人b能力值很差劲，所以需要让前面的人去B公司，让这个人去A公司来保持总和最大这个条件，这就违反了无后效性这个要求。

## 总结

* 状态：这个变量的数组下标代表了什么情况(i不一定是前i个东西，下标代表的情况要根据题意和数据范围来决定)  

* 决策：应该怎么从当前状态变化到下一个状态  

* 转移：如何在代码上进行状态变化  
* 无后效性：未来与过去无关，以后做的决定不会影响之前做的决定（只有符合这一点才可以使用动态规划来做）  


