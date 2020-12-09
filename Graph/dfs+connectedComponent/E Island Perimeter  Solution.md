## Island Perimeter

> You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water.
>
> Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).
>
> The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.
>
>  
>
> **Example:**
>
> ```
> Input:
> [[0,1,0,0],
>  [1,1,1,0],
>  [0,1,0,0],
>  [1,1,0,0]]
> 
> Output: 16
> 
> Explanation: The perimeter is the 16 yellow stripes in the image below:
> ```

* The intuition for me is to calculate the cell of  the land , i assume the repeat line number is ```cellNumber-1``` ,the assumption is false !!\
* so we can iterative the land ,and count the right and down neighbor 
* or we can calculate the line of the land ,the boundary condition is  out of the grid or the neighbor is  water ,we should tag the visited``` grid[i] [j]== -1``` ,to avoid dead loop and avoid to miss some repeat line  

## Solution DFS to calculate the  free line

```java
class Solution {
    int n = 0;
    int sub = 0;
    int row;
    int col;
    int direction[][]= new int [][]{{0,1},{0,-1},{-1,0},{1,0}};
    public int islandPerimeter(int[][] g) {
        if(g.length==0||g[0].length==0) return 0;
        row = g.length;
        col= g[0].length;
        for(int i = 0 ;i<row;i++){
            for(int j = 0 ;j < col ;j++ ){
                if(g[i][j]==1){ 
                    dfs(g,i,j);
                    return n;
                }
            }
        }
        return -1 ;
    }
    private void dfs(int [][] g ,int r ,int c){
        //avoid repeat
        //important
        if(r<0||r>=row||c<0||c>=col||g[r][c]==0){
            n++;
            return ;
        }else if(g[r][c]==1){
             g[r][c]=-1;
            for(int i = 0 ; i < 4 ;i++){
                int newr = r + direction[i][0];
                int newc = c + direction[i][1];
                dfs(g,newr,newc);
            }
        }
        
    }
}
```

## Solution Iterative in the grid 

```java
public static int islandPerimeter(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;
        int result = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                //calculate the right and down cell
                //we can also check four directions and minus one every single time 
                if (grid[i][j] == 1) {
                    result += 4;
                    if (i > 0 && grid[i-1][j] == 1) result -= 2;
                    if (j > 0 && grid[i][j-1] == 1) result -= 2;
                }
            }
        }
        return result;
    }
```

