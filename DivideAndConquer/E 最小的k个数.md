## 题干

> 输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
>
>  
>
> **示例 1：**
>
> ```
> 输入：arr = [3,2,1], k = 2
> 输出：[1,2] 或者 [2,1]
> ```
>
> **示例 2：**
>
> ```
> 输入：arr = [0,1,2,1], k = 1
> 输出：[0]
> ```
>
>  
>
> **限制：**
>
> - `0 <= k <= arr.length <= 10000`
> - `0 <= arr[i] <= 10000`

## Solution 直接用排序算法

```java
//time 7ms 42.8MB
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        int []res=new int [k];
        Arrays.sort(arr);
        for(int i=0;i<k;i++){
            res[i]=arr[i];
        }
        return res;
    }
}
```

## Solution 优先队列 最小堆

```
//time 22ms 43.4MB
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(k==0||arr.length==0) return new int[0];
        if(k==arr.length) return arr;
        int []res=new int [k];
        PriorityQueue<Integer> p=new PriorityQueue<>((v1, v2) -> v2 - v1);
        for(int i=0;i<k;i++){
            p.add(arr[i]);
        }
        for(int i=k;i<arr.length;i++){
            if(arr[i]<p.peek()){
                p.poll();
                p.add(arr[i]);
            }
        }
        for(int i=0;i<k;i++){
            res[i]=p.poll();
        }
        return res;
    }
}
```

## Solution 快速选择

```java
//time 3ms 43MB
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(k==0||arr.length==0) return new int[0];
        if(k==arr.length) return arr;
        quickSort(arr,0,arr.length-1,k);
        int []res=new int [k];
        for(int i=0;i<k;i++){
            res[i]=arr[i];
        }
        return res;
    }


    private void quickSort(int []arr,int start,int end,int k){
        if(start>end) return;
        int left=start;
        int right=end;
        int pivot=arr[start];
        while(left<right){
            while(left<right&&arr[right]>pivot) right--;
            if(left<right) arr[left++]=arr[right];
            while(left<right&&arr[left]<pivot) left++;
            if(left<right) arr[right--]=arr[left];
        }
        arr[left]=pivot;
        if(left==k-1){
            return;
        }else if(left>k-1) quickSort(arr,start,left-1,k);
        else quickSort(arr,left+1,end,k);
    }

}
```

