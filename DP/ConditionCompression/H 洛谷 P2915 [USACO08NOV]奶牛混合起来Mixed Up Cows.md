## [洛谷 P2915 [USACO08NOV\]奶牛混合起来Mixed Up Cows](https://www.luogu.org/problemnew/show/P2915)

> ## 题目描述
>
> Each of Farmer John's N (4 <= N <= 16) cows has a unique serial number S_i (1 <= S_i <= 25,000). The cows are so proud of it that each one now wears her number in a gangsta manner engraved in large letters on a gold plate hung around her ample bovine neck.
>
> Gangsta cows are rebellious and line up to be milked in an order called 'Mixed Up'. A cow order is 'Mixed Up' if the sequence of serial numbers formed by their milking line is such that the serial numbers of every pair of consecutive cows in line differs by more than K (1 <= K <= 3400). For example, if N = 6 and K = 1 then 1, 3, 5, 2, 6, 4 is a 'Mixed Up' lineup but 1, 3, 6, 5, 2, 4 is not (since the consecutive numbers 5 and 6 differ by 1).
>
> How many different ways can N cows be Mixed Up?
>
> For your first 10 submissions, you will be provided with the results of running your program on a part of the actual test data.
>
> POINTS: 200
>
> 约翰家有N头奶牛，第i头奶牛的编号是Si，每头奶牛的编号都是唯一的。这些奶牛最近 在闹脾气，为表达不满的情绪，她们在挤奶的时候一定要排成混乱的队伍。在一只混乱的队 伍中，相邻奶牛的编号之差均超过K。比如当K = 1时，1, 3, 5, 2, 6, 4就是一支混乱的队伍， 而1, 3, 6, 5, 2, 4不是，因为6和5只差1。请数一数，有多少种队形是混乱的呢？
>
> #### 输入格式
>
> \* Line 1: Two space-separated integers: N and K
>
> \* Lines 2..N+1: Line i+1 contains a single integer that is the serial number of cow i: S_i
>
> #### 输出格式
>
> #### \* Line 1: A single integer that is the number of ways that N cows can be 'Mixed Up'. The answer is guaranteed to fit in a 64 bit integer.
>
> #### 输入输出样例
>
> #### **输入 #1**
>
> ```
> 4 1 
> 3 
> 4 
> 2 
> 1 
> ```
>
> #### **输出 #1*
>
> ```
> 2 
> ```
>
> #### 说明/提示
>
> The 2 possible Mixed Up arrangements are:
>
> 3 1 4 2
>
> 2 4 1 3
>
> #### ·题目分析
>
> 我们考虑将当前选取的所有奶牛当做一个状态，而前一个选取的奶牛当做另一个状态。枚举所有的奶牛，如果它还没有被选取，且满足与上一个选取的奶牛差超过K，我们就将这个状态的方案数累加进下一个状态的方案数。其中边界为对于只有i的集合，最后选取的奶牛为i时，方案数为1。

## Solution

>  首先我们能够一眼看到4 <= N <= 16，那么就是它了，我们要压缩的状态就是它了
>
> 那么之后能我们用这个状态表示什么呢，我们要表示的显然是每只奶牛是否在队伍中 比如说10吧，转成二进制后就是1010，这就代表了第一只和第三只奶牛已经在队伍中，而第二只和第四只还没有在队伍中
>
> 那么就有一些状态可以初始化了，对于那些只有一只奶牛在队伍中，即某个状态转为二进制后只有一个1的状态我们就可以初始化为1
>
> 但是我们要判断的是这个队伍是否**“混乱”，**混乱的定义是：相邻奶牛的编号之差均超过K，于是我们在由一个状态得到一个新状态时一定要去判断这个原状态加上那个我们新添加到队伍末尾的那只奶牛后是否还是混乱的 那么我们要怎么做呢 显然我们的dp数组里存的不止是状态了，还应该存一个能帮助我们判断的东西 那就想想我们一旦新在队伍末尾加入一个奶牛后是怎么判断的呢
>
> **很显然只要新加入的这只奶牛和原本队尾那只奶牛的编号差大于k就可以使队伍继续混乱下去了，因为我们必须保证那个原来的状态是合法的，也就是说原来的那个队伍是混乱的**
>
> 于是我们的dp数组就呼之欲出了
>
> 我们设 f[i][j]表示以第i只奶牛为结尾的状态为j的队伍混乱的方案数是多少
>
> 而我们怎么转移状态呢，我们知道对于每一个状态都有很多结尾，于是我们用两个循环，一个枚举状态，一个枚举结尾的奶牛
>
> 当然我们还需要判断这个情况是否存在，比如说f[2][10]吧，它表示10这个状态也就是1010，以第二只奶牛为结尾的方案数，这种情况显然是不存在的，因为在1010这个状态中第二只奶牛根本没有被选择，根本不可能成为结尾，所以对于这种情况我们需要进行判断
>
> 在最后我们统计答案的时候要把枚举各种奶牛作为结尾且所有奶牛均被选择的情况
>
> 于是就很简单了
>
> 之后就是代码了



```c++
#include<iostream>
#include<cstring>
#include<cstdio>
#define re register
#define int long long
#define maxn 17
using namespace std;
int k;
int n,a[maxn],f[maxn][1<<maxn];
inline int read()
{
	char c=getchar();
	int x=0;
	while(c<'0'||c>'9') c=getchar();
	while(c>='0'&&c<='9')
	  x=(x<<3)+(x<<1)+c-48,c=getchar();
	return x;
}
signed main()
{
	n=read();
	k=read();
    // a[i] is the S[i]
	for(re int i=1;i<=n;i++)
		a[i]=read();
	for(re int i=1;i<=n;i++)
		f[i][1<<(n-i)]=1;
	for(re int i=1;i<(1<<n);i++)//枚举状态 
		for(re int j=1;j<=n;j++)//枚举结尾奶牛 
		{	
            //if initialize have existed 
            if(f[j][i]) continue;
            //if invalid that is the state i the exact bit is not 1 
            //we should continue 
            //判断这种情况是否存在 
            if(!(i&(1<<(n-j)))) continue;
            // we use xor to get the only bit is different 
            // so the current state will from the previous the state but not end with current end  
            int m=i^(1<<(n-j));
            for(re int g=1;g<=n;g++)
            //枚举这个状态m的结尾 
            //we iterative the end about the state 
            {
                if(g==j) continue;
               //we perform  the change about the condition 
                if(a[j]-a[g]>k||a[g]-a[j]>k) f[j][i]+=f[g][m];
                //符合混乱的条件，进行转移 
            }
        }
	int ans=0;
	for(re int i=1;i<=n;i++)
	ans+=f[i][(1<<n)-1];
	cout<<ans<<endl;
	return 0;
}
```

