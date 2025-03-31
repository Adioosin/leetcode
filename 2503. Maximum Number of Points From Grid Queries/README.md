[2503. Maximum Number of Points From Grid Queries](https://leetcode.com/problems/maximum-number-of-points-from-grid-queries/description/?envType=daily-question&envId=2025-03-30)

## Solution

``` java
class Solution {

    int d[][] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    public int[] maxPoints(int[][] grid, int[] queries) {
        int n = queries.length;
        int resOrder[] = new int[n];
        // Set<Integer> set = new HashSet<>();
        for (int i = 0; i < n; i++) {
            resOrder[i] = queries[i];
            // set.add(queries[i]);
        }
        // queries = new int[set.size()];
        // int j = 0;
        // for (int i: set){
        //     queries[j] = i;
        //     j++;
        // }
        Arrays.sort(queries);
        // System.out.println(Arrays.toString(queries));
        Map<Integer, Integer> res = new HashMap<>();
        Queue<int[]> q = new PriorityQueue<>((a, b) -> a[0] - b [0]);
        q.add(new int[]{grid[0][0], 0, 0});
        int qpos = 0;
        while (qpos < queries.length) {
            // System.out.println(pending);
            // for (Pair p: pending) {
            //     q.offer(p);
            // }
            // pending = new HashSet<>();
            if (qpos == 0)
                res.put(queries[qpos], 0);
            else 
                res.put(queries[qpos], res.get(queries[qpos - 1]));
            while (!q.isEmpty() && q.peek()[0] < queries[qpos]) {
                int[] p = q.poll();
                int x = p[1];
                int y = p[2];
                if (grid[x][y] == 0) continue;
                if(grid[x][y] < queries[qpos]) {
                    res.put(queries[qpos], res.getOrDefault(queries[qpos], 0) + 1);
                    grid[x][y] = 0;
                    for (int k = 0; k < d.length; k++) {
                        int newX = x + d[k][0];
                        int newY = y + d[k][1];
                        if (newX >= 0 && newX < grid.length && newY >= 0 && newY < grid[0].length && grid[newX][newY] != 0) {
                            q.offer(new int[]{grid[newX][newY], newX, newY});
                        }
                    }
                } 
                // else {
                //     if (!pending.contains(p)) {
                //         pending.add(p);
                //     }
                // }
                // System.out.println("q: " + q);
            }
            qpos++;
        }
        // System.out.println(res);
        for (int i = 0; i < n; i++) {
            resOrder[i] = res.getOrDefault(resOrder[i], 0);
        }
        // System.out.println(Arrays.toString(res));
        return resOrder;
    }

}
```