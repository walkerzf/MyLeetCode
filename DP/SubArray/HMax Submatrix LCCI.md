## [. Max Submatrix LCCI](https://leetcode-cn.com/problems/max-submatrix-lcci/)

> Given an NxN matrix of positive and negative integers, write code to find the submatrix with the largest possible sum.
>
> Return an array [r1, c1, r2, c2], where r1, c1 are the row number and the column number of the submatrix's upper left corner respectively, and r2, c2 are the row number of and the column number of lower right corner. If there are more than one answers, return any one of them.
>
> Note: This problem is slightly different from the original one in the book.
>
> ```
> Example:
> 
> Input:
> [
>    [-1,0],
>    [0,-1]
> ]
> Output: [0,1,0,1]
> Note:
> 
> 1 <= matrix.length, matrix[0].length <= 200
> ```

## Solution Brute Force 

* we calculate all subMatrix sum to compare ,so we have many repeat calculation s

## Solution use the subArray in 1-D

* we can fix the upper row and lower row ,and we see the sub Col as the one number ,use the dp to calculate 

```java
class Solution {
    private int addOneCol(int [][]matrix ,int row1,int row2,int col){
        int ret = 0;
        for(int i =row1;i<=row2;i++){
            ret+=matrix[i][col];
        }
        return ret;
    }
    private void updateRes(int res[],int row1,int row2,int col1,int col2){
        res[0] = row1;
        res[1] = col1;
        res[2] = row2;
        res[3] = col2;
    }
    public int[] getMaxMatrix(int[][] matrix) {
        if(matrix.length==0||matrix[0].length==0)  return new int[]{};
        int res[]= new int[4];
        int row = matrix.length;
        int col = matrix[0].length;
        //n4 is impossible 
        //so we try n2
        //we fix the row number 
        int max = Integer.MIN_VALUE;
        int sum = 0 ;
        int begin = 0;
        int temp[] = new int[col];
        for(int i = 0;i<row;i++){
            //one trick to calculate the sum of sub col
            Arrays.fill(temp,0);
            for(int j =i;j<row;j++){
                sum = 0;
                begin = 0;
                for(int l=0;l<col;l++){
                    temp[l] +=matrix[j][l];
                    if(sum<=0){
                        sum=temp[l];
                        begin = l;
                    }else{
                        sum+=temp[l];
                    }
                    if(max<sum){
                        max =sum;
                        updateRes(res,i,j,begin,l);
                    }
                }
            }
        }
        return res;
    }
}
```

