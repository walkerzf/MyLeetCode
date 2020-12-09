## [ Count Submatrices With All Ones](https://leetcode-cn.com/problems/count-submatrices-with-all-ones/)

> Given a``` rows * columns``` matrix mat of ones and zeros, return how many submatrices have all ones.
>
> ```
> Example 1:
> 
> Input: mat = [[1,0,1],
>               [1,1,0],
>               [1,1,0]]
> Output: 13
> Explanation:
> There are 6 rectangles of side 1x1.
> There are 2 rectangles of side 1x2.
> There are 3 rectangles of side 2x1.
> There is 1 rectangle of side 2x2. 
> There is 1 rectangle of side 3x1.
> Total number of rectangles = 6 + 2 + 3 + 1 + 1 = 13.
> Example 2:
> 
> Input: mat = [[0,1,1,0],
>               [0,1,1,1],
>               [1,1,1,0]]
> Output: 24
> Explanation:
> There are 8 rectangles of side 1x1.
> There are 5 rectangles of side 1x2.
> There are 2 rectangles of side 1x3. 
> There are 4 rectangles of side 2x1.
> There are 2 rectangles of side 2x2. 
> There are 2 rectangles of side 3x1. 
> There is 1 rectangle of side 3x2. 
> Total number of rectangles = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24.
> Example 3:
> 
> Input: mat = [[1,1,1,1,1,1]]
> Output: 21
> Example 4:
> 
> Input: mat = [[1,0,1],[0,1,0],[1,0,1]]
> Output: 5
> ```
>
> 
>
> ```
> Constraints:
> 
> 1 <= rows <= 150
> 1 <= columns <= 150
> 0 <= mat[i][j] <= 1
> ```



## Solution Brute Force

use dp array to record the left ones and up ones to help brute force 

## Solution compress the region  to column

```java
//time 33ms
class Solution {
    public int numSubmat(int[][] mat) {
        int row = mat.length ;
        int col = mat[0].length;
        int data [] = new int [col];
        int res = 0;
        for(int i = 0 ;i < row ; i++){
            Arrays.fill(data,0);
            for(int j = i ; j < row ;j++){
                // row - > [i,j]
                for(int l = 0 ;l< col ;l++){
                    //0 -> delimeter
                    //data [l] accumlate the result of row 
                    data[l] += mat[j][l]^1;
                }
                res += cal(data);
            }
        }
        return res; 
    }
    private int cal(int data[]){
        int res  = 0;
        int n =data.length ;
        int len = 0;
        for(int i = 0 ;i<data.length ;i++){
            // 1 + 2 + 3 + ...+len
            //continous subArray
            if(data[i]>=1){
                res += len*(len+1)/2;
                len = 0;
            }else{
                len++;
            }
        }
        res +=len*(len+1)/2;
        return res; 
    }
}
```

## Solution prefix Sum

```java
//time 7ms
class Solution {
    public int numSubmat(int[][] mat) {
        int row = mat.length;
        int col = mat[0].length;
        int left[][] = new int[row][col];
        int res = 0;
        //prefix sum
        for(int i = 0 ;i<row; i++){
            for(int j = 0 ;j<col;j++){
                if(j==0){
                    left[i][j] = mat[i][j];
                }else 
                    left[i][j] = mat[i][j]==1?left[i][j-1]+1:0;
            }
        }
        for(int i = 0 ;i<row;i++){ 
            for(int j =  0 ;j<col;j++){
                if(mat[i][j]==1){
                    int minWidth = Integer.MAX_VALUE;
                    for(int l = i;l>=0;l--){
                        if(left[l][j]==0) break;
                        minWidth= Math.min(minWidth,left[l][j]);
                        res+=minWidth;
                    }
                }
            }
        }
    
        return res;
    } 
}
```

