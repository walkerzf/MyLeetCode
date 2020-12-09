## Prison Cells After N Days

> There are 8 prison cells in a row, and each cell is either occupied or vacant.
>
> Each day, whether the cell is occupied or vacant changes according to the following rules:
>
> - If a cell has two adjacent neighbors that are both occupied or both vacant, then the cell becomes occupied.
> - Otherwise, it becomes vacant.
>
> (Note that because the prison is a row, the first and the last cells in the row can't have two adjacent neighbors.)
>
> We describe the current state of the prison in the following way: `cells[i] == 1` if the `i`-th cell is occupied, else `cells[i] == 0`.
>
> Given the initial state of the prison, return the state of the prison after `N` days (and `N` such changes described above.)
>
>  
>
> **Example 1:**
>
> ```
> Input: cells = [0,1,0,1,1,0,0,1], N = 7
> Output: [0,0,1,1,0,0,0,0]
> Explanation: 
> The following table summarizes the state of the prison on each day:
> Day 0: [0, 1, 0, 1, 1, 0, 0, 1]
> Day 1: [0, 1, 1, 0, 0, 0, 0, 0]
> Day 2: [0, 0, 0, 0, 1, 1, 1, 0]
> Day 3: [0, 1, 1, 0, 0, 1, 0, 0]
> Day 4: [0, 0, 0, 0, 0, 1, 0, 0]
> Day 5: [0, 1, 1, 1, 0, 1, 0, 0]
> Day 6: [0, 0, 1, 0, 1, 1, 0, 0]
> Day 7: [0, 0, 1, 1, 0, 0, 0, 0]
> ```
>
> **Example 2:**
>
> ```
> Input: cells = [1,0,0,1,0,0,1,0], N = 1000000000
> Output: [0,0,1,1,1,1,1,0]
> ```
>
>  
>
> **Note:**
>
> 1. `cells.length == 8`
> 2. `cells[i]` is in `{0, 1}`
> 3. `1 <= N <= 10^9`

## Solution 

*  the first and last index must be zero 
* we have six bits ,so we have 2^6 conditions 
* there must be a loop
* mod 14

```java
class Solution {
    //when N%14 ==0 the cells will keep the same 
    //so we need to preprocessor
    public int[] prisonAfterNDays(int[] cells, int N) {
        if(N%14==0) N=14;
        else N=N%14;
        for(int i = 1; i <= N ; i++){
            int tmp[] = new int[cells.length];
            for(int j = 1;j<cells.length-1;j++){
                if(( cells[j-1]^cells[j+1])==0 ){
                    tmp[j] = 1 ;
                }
            }
            cells = tmp ;            
        }
        cells[0] = 0;
        cells[7] = 0;
        return cells;
    }
}
```

```javascript
    public int[] prisonAfterNDays(int[] cells, int N) {
        //initilize 
        for (N = (N - 1) % 14 + 1; N > 0; --N) {
            int[] cells2 = new int[8];
            for (int i = 1; i < 7; ++i)
                cells2[i] = cells[i - 1] == cells[i + 1] ? 1 : 0;
            cells = cells2;
        }
        return cells;
    }
```

## Solution Map

* use map to search loop 

```java
   public int[] prisonAfterNDays(int[] cells, int N) {
        Map<String, Integer> seen = new HashMap<>();
        while (N > 0) {
            int[] cells2 = new int[8];
            seen.put(Arrays.toString(cells), N--);
            for (int i = 1; i < 7; ++i)
                cells2[i] = cells[i - 1] == cells[i + 1] ? 1 : 0;
            cells = cells2;
            if (seen.containsKey(Arrays.toString(cells))) {
                N %= seen.get(Arrays.toString(cells)) - N;
            }
        }
        return cells;
    }
```

