## Windy Number

> ## 题目背景
>
> windy 定义了一种 windy 数。
>
> ## 题目描述
>
> 不含前导零且相邻两个数字之差至少为 22 的正整数被称为 windy 数。windy 想知道，在 a*a* 和 b*b* 之间，包括 a*a* 和 b*b* ，总共有多少个 windy 数？
>
> ## 输入格式
>
> 输入只有一行两个整数，分别表示 a*a* 和 b*b*。
>
> ## 输出格式
>
> 输出一行一个整数表示答案。
>
> ## 输入输出样例
>
> **输入 #1**复制
>
> ```
> 1 10
> ```
>
> **输出 #1**复制
>
> ```
> 9
> ```
>
> **输入 #2**复制
>
> ```
> 25 50
> ```
>
> **输出 #2**复制
>
> ```
> 20
> ```
>
> ## 说明/提示
>
> #### 数据规模与约定
>
> 对于全部的测试点，保证 1≤*a*≤*b*≤2×109。

## Solution DP

* we need to preprocess to calculate the  digit number is``` i ```and MSB is ```j``` ‘s possible answers 
* for the zero case ,it is useful for higher bit 

```c++
#include <cmath>
#include <memory.h>
#include <iostream>

using namespace std;

int p, q, dp[15][15], a[15];

void init() {
    for(int i =0;i<15;i++){
        for(int j =0;j<15;j++){
            dp[i][j]=0;
        }
    }
    for (int i = 0; i <= 9; i++) dp[1][i] = 1;
    for (int i = 2; i <= 10; i++) {
        //intuition the first digit is zero is invalid
        //but it is useful for the next iteration
        for (int j = 0; j <= 9; j++) {
            for (int k = 0; k <= 9; k++) {
                if (abs(j - k) >= 2) dp[i][j] += dp[i - 1][k];
            }
        }
    }
}

int work(int x) {

    memset(a, 0, sizeof(a));
    //first to know the digit the x
    int len = 0;
    int ans = 0;
    while (x) {
        a[++len] = x % 10;
        x /= 10;
    }
    //compute the digit <len
    for (int i = 1; i < len; i++) {
        for (int j = 1; j <= 9; j++) {
            ans += dp[i][j];
        }
    }
    //compute the digit ==len
    for (int i = len; i >= 1; i--) {
        for (int j = ((i == len) ? 1 : 0); j < a[i]; j++) {
            if (i == len) { ans += dp[i][j]; }
            else {
                if (abs(j - a[i + 1]) >= 2) ans += dp[i][j];
            }
        }
        //the if is very important 。。
        if(i!=len) if (abs(a[i] - a[i + 1]) < 2) break;
    }

    return ans;
}

int main() {
    init();
    cin >> p >> q;
    cout << work(q + 1) - work(p) << endl;
    return 0;
}
```

