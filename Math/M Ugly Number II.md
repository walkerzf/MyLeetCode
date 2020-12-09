## Ugly Number II

> Write a program to find the `n`-th ugly number.
>
> Ugly numbers are **positive numbers** whose **prime factors** only include `2, 3, 5`. 
>
> **Example:**
>
> ```
> Input: n = 10
> Output: 12
> Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
> ```
>
> **Note:** 
>
> 1. `1` is typically treated as an ugly number.
> 2. `n` **does not exceed 1690**.

* The naive approach is to call `isUgly` for every number until you reach the nth one. Most numbers are *not* ugly. Try to focus your effort on **generating only the ugly ones.**
* **An ugly number must be multiplied by either``` 2, 3, or 5``` from a smaller ugly number.**
* The key is how to maintain the order of the ugly numbers. Try a similar approach of merging from three sorted lists:``` L1, L2, and L3.```
* Assume you have``` Uk```, the kth ugly number. Then ```Uk+1``` must be``` Min(L1 * 2, L2 * 3, L3 * 5)```.

## Solution Brute Force

* Way :we check all integer until ugly number count to n
* How: we divide the number by greatest divisible power 

```java
// Java program to find nth ugly number 
class GFG { 
	
	/*This function divides a by greatest 
	divisible power of b*/
	static int maxDivide(int a, int b) 
	{ 
		while(a % b == 0) 
			a = a/b; 
		return a; 
	} 
	
	/* Function to check if a number 
	is ugly or not */
	static int isUgly(int no) 
	{ 
		no = maxDivide(no, 2); 
		no = maxDivide(no, 3); 
		no = maxDivide(no, 5); 
		
		return (no == 1)? 1 : 0; 
	} 
	
	/* Function to get the nth ugly 
	number*/
	static int getNthUglyNo(int n) 
	{ 
		int i = 1; 
		
		// ugly number count 
		int count = 1; 
		
		// check for all integers 
		// until count becomes n 
		while(n > count) 
		{ 
			i++; 
			if(isUgly(i) == 1) 
				count++; 
		} 
		return i; 
	} 
	
	/* Driver program to test above 
	functions */
	public static void main(String args[]) 
	{ 
		int no = getNthUglyNo(150); 
		System.out.println("150th ugly "
					+ "no. is "+ no); 
	} 
} 

// This code has been contributed by 
// Amit Khandelwal (Amit Khandelwal 1) 

```



## Solution Priority Queue

because the positive overflow ,we get the negative ones ,so i use ```long ```

And i compute  many numbers which is never used …

```java
//time 128ms  
class Solution {
    public int nthUglyNumber(int n) {
        PriorityQueue<Long> q =new PriorityQueue<>();
        q.add((long) 2);q.add((long) 3);q.add((long) 5);
        int count = 0;
        long number = 1;
        Set<Long> seen = new HashSet<>();
        int factor [] = new int[]{2,3,5};
        //seen.add(2);seen.add(3);seen.add(5);
        while(count!= n-1){
            number = q.poll();
            //if(seen.contains(number)) continue ;
            for(int i = 0 ;i<3;i++){
                long tmp = factor[i]*number ;
                if(seen.contains(tmp)) continue ;
                else {
                    q.add(tmp);
                    seen.add(tmp);
                }
            }
            count++;
        }
        return (int) number;
    }
}
```

## Solution DP

we can have the equation  ``` k-th number =  Math.min(L1*2 , L2 *3,L3*5)```

**Method 2 (Use Dynamic Programming)**
Here is a time efficient solution with O(n) extra space. The ugly-number sequence is 1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, …
   because every number **can only be divided by 2, 3, 5**, one way to look at the sequence is to split the sequence to three groups as below:
   (1) 1×2, 2×2, 3×2, 4×2, 5×2, …
   (2) 1×3, 2×3, 3×3, 4×3, 5×3, …
   (3) 1×5, 2×5, 3×5, 4×5, 5×5, …

   We can find that every subsequence is the ugly-sequence itself (1, 2, 3, 4, 5, …) multiply 2, 3, 5. Then we use similar merge method as merge sort, to get every ugly number from the three subsequence. Every step we choose the smallest one, and move one step after.

```java
class Solution {
    public int nthUglyNumber(int n) {
        int ugly [] =new int [n];
        ugly[0] = 1;
        int index2 = 0 ;
        int index3 = 0 ;
        int index5 = 0 ;
        int nextMuliple2 = 2 ;
        int nextMuliple3 = 3 ;
        int nextMuliple5 = 5 ;
        //similar to merger sort 
        // we have three sorted rows 
        // we need to find the minimum one 
        for(int i = 1 ;i < n;i++){
            ugly[i] = Math.min(nextMuliple2,Math.min(nextMuliple3,nextMuliple5));
            if(ugly[i]==nextMuliple2){
                index2++;
                nextMuliple2 = ugly[index2]*2;
            }
            if(ugly[i]==nextMuliple3){
                index3++;
                nextMuliple3 = ugly[index3]*3;
            }
            if(ugly[i]==nextMuliple5){
                index5++;
                nextMuliple5 = ugly[index5]*5;
            }
        }
        return ugly[n-1];
    }
}
```

