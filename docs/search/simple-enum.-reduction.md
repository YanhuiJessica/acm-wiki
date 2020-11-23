!!! note "Copyright"
    本页面贡献者：[LyuLumos](https://github.com/LyuLumos)，[YanhuiJessica](https://github.com/YanhuiJessica)。
    本页面内容遵循 MIT 协议，转载请附上原文出处链接和本声明。

## 枚举约简的基本原则
- 提取有效信息
- 减少重复计算
- 将原问题转化为更小的问题
- 根据问题的性质进行搜索剪枝
- 引入其他算法



**例题**

!!! question "题目来源：《啊哈算法》"
    □□□+□□□=□□□，将数字1~9分别填入9个□中，
    每个数字只能使用一次使得等式成立。
    例如173+286=459就是一个合理的组合，
    请问一共有多少种合理的组合呢？

    注意：173+286=459 与 286+173=459 是同一种组合。



枚举每一位上所有可能的数
```cpp
#include <stdio.h>
int main()
{
  int a,b,c,d,e,f,g,h,i,total=0;
  for(a=1;a<=9;a++)//第1个数的百位
   for(b=1;b<=9;b++)//第1个数的十位
    for(c=1;c<=9;c++)//第1个数的个位
     for(d=1;d<=9;d++)//第2个数的百位
      for(e=1;e<=9;e++)//第2个数的十位
       for(f=1;f<=9;f++)//第2个数的个位
        for(g=1;g<=9;g++)//第3个数的百位
         for(h=1;h<=9;h++)//第3个数的十位
          for(i=1;i<=9;i++)//第3个数的个位
          { //接下来要判断每一位上的数互不相等
            if(a!=b && a!=c && a!=d && a!=e && a!=f && a!=g && a!=h && a!=i
                    && b!=c && b!=d && b!=e && b!=f && b!=g && b!=h && b!=i
                            && c!=d && c!=e && c!=f && c!=g && c!=h && c!=i
                                    && d!=e && d!=f && d!=g && d!=h && d!=i
                                                && e!=f && e!=g && e!=h && e!=i
                                                        && f!=g && f!=h && f!=i
                                                                && g!=h && g!=i
                                                                        && h!=i
                            && a*100+b*10+c+d*100+e*10+f==g*100+h*10+i)
            {
                total++;
                printf("%d%d%d+%d%d%d=%d%d%d\n",a,b,c,d,e,f,g,h,i);
            }
          }
  printf("total=%d",total/2);//请想一想为什么要除以2
  return 0;
}
```


上面的程序计算量是$9^9=387 420 489$，哪里还能化简？