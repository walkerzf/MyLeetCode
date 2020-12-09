## Motivation

* 给定一个一维数组，求解它的 ```range sum query```  RSQ
  * navie Implement ：O(N) per query
  * use dp to ppre-compute the prefix sum  O(1)
  * what if the values of elements can change ?
* Fenwick Tree was supposed to solve the prefix sum problem
  * The idea is to store Patial sum in each node and get total sum by traversaling the tree from the leaf to root .the Tree have the heighr
  * Query:O(logN)
  * UPdate:O(logN)

## 图解

![image-20200323102517407](C:\Users\15524\AppData\Roaming\Typora\typora-user-images\image-20200323102517407.png)

## 代码

```java
class FenwickTree {
    private int []sum;
    
    private int  lowbit(int x){
        return  x&(-x);
    }
    
    public  FenwickTree(int n){
        sum=new int[n+1];
    }
	//更新fenwick Tree  查询到根节点
    public  void update(int i, int delta){
        while(i<sum.length){
            sum[i]+=delta;
            i+=lowbit(i);
        }
    }
   //查询Fenwick Tree，查询到0
    public  int query(int i){
        int res=0;
        while(i>0){
            res+=sum[i];
            i-=lowbit(i);
        }
        return  res ;
    }
}
```

## [307. Range Sum Query - Mutable](https://leetcode-cn.com/problems/range-sum-query-mutable/)

```java
//time  21ms 47.8MB
class NumArray {
    private int [] sum;
    private int [] num;
    public int lowbit(int x){
        return x&(-x);
    }
    public NumArray(int[] nums) {
        sum=new int[nums.length+1];
        num=new int[nums.length];
        for(int i=1;i<sum.length;i++){
            update(i-1,nums[i-1]);
             num[i-1]=nums[i-1];
        }
    }
    
    public void update(int i, int val) {
        int delta=val-num[i];
        num[i]=val;
        int j=i+1;
        while(j<sum.length){
            sum[j]+=delta;
            j+=lowbit(j);
        }
    }
    
    public int sumRange(int i, int j) {
        int res=0;
        int l=i;
        int m=j+1;
        while(m>0){
            res+=sum[m];
            m-=lowbit(m);
        }
        while(l>0){
            res-=sum[l];
            l-=lowbit(l);
        }
        return res;
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```

## [315. Count of Smaller Numbers After Self](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/)

### Brute force

```java
//TLE
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> res=new LinkedList<>();
        int count=0;
        for(int i=0;i<nums.length;i++){
            count=0;
            for(int j=i+1;j<nums.length;j++){
                if(nums[j]<nums[i]) count++;
            }
            res.add(count);
        }
        return res;
    }
}
```

## BIT Binary Index Tree/ Fenwick Tree

```java
//time 14ms 44.4MB
class Solution {
    class FenwickTree{
        private int sum[];
        private int n;
        public FenwickTree(int n){
            sum=new int[n+1];
        }
        public int lowbit (int x){
            return x&(-x);
        }
        public void update (int i,int delta){
            while(i<sum.length){
                sum[i]+=delta;
                i+=lowbit(i);
            }
        }
        public int query(int i){
            int res=0;
            while(i>0){
                res+=sum[i];
                i-=lowbit(i);                
            }
            return res;
        }
    }

    public List<Integer> countSmaller(int[] nums) {
        Set<Integer> sorted =new HashSet<>();
        HashMap<Integer ,Integer> m=new HashMap<>();
        List<Integer> res=new LinkedList<>();
        for(int i=0;i<nums.length;i++){
            sorted.add(nums[i]);
        }
        int index=0;
        int s[]=new int[sorted.size()];
        for(int a:sorted){
            s[index++]=a;
        }
        Arrays.sort(s);
        int rank=1;

        //rank map
        for(int i=0;i<s.length;i++){
            m.put(s[i],rank++);
        }

        //频率数组
        FenwickTree f=new FenwickTree(rank);
        for(int i=nums.length-1;i>=0;i--){
            f.update(m.get(nums[i]),1);
            res.add(f.query(m.get(nums[i])-1));
        }
        Collections.reverse(res);
        return res;
    }

}
```

