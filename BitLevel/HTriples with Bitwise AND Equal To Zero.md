## [Triples with Bitwise AND Equal To Zero](https://leetcode-cn.com/problems/triples-with-bitwise-and-equal-to-zero/)

> Given an array of integers A, find the number of triples of indices (i, j, k) such that:
>
> ```
> 0 <= i < A.length
> 0 <= j < A.length
> 0 <= k < A.length
> A[i] & A[j] & A[k] == 0, where & represents the bitwise-AND operator.
> ```
>
> ```
> Example 1:
> 
> Input: [2,1,3]
> Output: 12
> Explanation: We could choose the following i, j, k triples:
> (i=0, j=0, k=1) : 2 & 2 & 1
> (i=0, j=1, k=0) : 2 & 1 & 2
> (i=0, j=1, k=1) : 2 & 1 & 1
> (i=0, j=1, k=2) : 2 & 1 & 3
> (i=0, j=2, k=1) : 2 & 3 & 1
> (i=1, j=0, k=0) : 1 & 2 & 2
> (i=1, j=0, k=1) : 1 & 2 & 1
> (i=1, j=0, k=2) : 1 & 2 & 3
> (i=1, j=1, k=0) : 1 & 1 & 2
> (i=1, j=2, k=0) : 1 & 3 & 2
> (i=2, j=0, k=1) : 3 & 2 & 1
> 
> (i=2, j=1, k=0) : 3 & 1 & 2
> ```
>
> 
>
> ```
> Note:
> 
> 1 <= A.length <= 1000
> 0 <= A[i] < 2^16
> ```

## Solution O(M*2^16*N)

TLE for c++

```c++
class Solution {
public:
    int countTriplets(vector<int>& A) {
        int N = 1<<16;
        int size  = A.size();
        int dp[4][N];
        for(int i = 0;i<4;i++)  memset(dp[i],0,sizeof dp[i]);
        dp[0][N-1] =1;
        for(int i = 1; i<=3;i++){
            for(int l = 0 ; l <size ;l++){
            for(int j = 0 ;j<N;j++){
                dp[i][j&A[l]] +=dp[i-1][j];
               }
            }
        }
        return dp[3][0];
    }
};
```

## Solution Map o(N2)

```c++
int countTriplets(vector<int>& A, int cnt = 0) {
  int tuples[1 << 16] = {};
  for (auto a : A)
    for (auto b : A) ++tuples[a & b];
  for (auto a : A)
    for (auto i = 0; i < (1 << 16); ++i)
      if ((i & a) == 0) cnt += tuples[i];
  return cnt;
}
```

```c++
int countTriplets(vector<int>& A, int cnt = 0) {
  unordered_map<int, int> tuples;
  for (auto a : A)
    for (auto b : A) ++tuples[a & b];
  for (auto a : A)
    for (auto t : tuples)
      if ((t.first & a) == 0) cnt += t.second;
  return cnt;
}
```

## Solution 

```c++
class Solution {
    public int countTriplets(int[] nums) {
		int res = 0;
		int max = 0;

		for (int x : nums) {
			max = Math.max(max, x);
		}

		int max_len = Integer.toBinaryString(max).length();
		int mask = 1;
		for (int i = 0; i < max_len - 1; i++) {
			mask <<= 1;
			mask++;
		}
		int[] map = new int[mask + 1];
		for (int x : nums) {
			for (int y : nums) {

				map[x & y]++;
			}
		}
		for (int x : nums) {
			if (x == 0) {
				res += nums.length * nums.length;
				continue;
			}
			x ^= mask;
            //similar to cut the branch this is for the possible U (and test all subsets)
			for (int i = x; i > 0; i = (i - 1) & x) {

				res += map[i];

			}
			res += map[0];
		}
		return res;
	}
}
```

```c++
class Solution {
public:
    int countTriplets(vector<int>& A) {
        int res = 0;
        int m = 0;
        int len = A.size();
        for(int i = 0;i<len;i++) m = max(m,A[i]);
        int shift = 1;
        while((1<<shift)<=m){
            shift++;
        }
        int maxstate = (1<<shift) -1;
        int p[maxstate+1];
        memset(p,0,sizeof p);
        for(int i = 0;i<len;i++){
            for(int j =0;j<len;j++){
                p[A[i]&A[j]]++;
            }
        }
        for(int i = 0;i<len;i++){
            int num  = A[i];
            if(num==0) {res+=(len*len); continue;}
            int x = num^maxstate;
            for(int i = x;i>0;i=x&(i-1)){
                res+=p[i];
            }
            res+=p[0];
        }
        return res;
    }
};
```

