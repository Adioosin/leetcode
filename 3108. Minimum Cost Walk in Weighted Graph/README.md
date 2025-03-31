[3108. Minimum Cost Walk in Weighted Graph] (https://leetcode.com/problems/minimum-cost-walk-in-weighted-graph/description/?envType=daily-question&envId=2025-03-30)

## Solution

``` java
class Solution {
    public int[] minimumCost(int n, int[][] edges, int[][] query) {
        int[] parent = new int[n];
        int[] sz = new int[n];
        int[] w = new int[n];
        int[] res = new int[query.length];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            sz[i] = 1;
            w[i] = -1;
        }
        for(int i = 0; i < edges.length; i++) {
            union(edges[i], parent, sz);
        }
        for (int i = 0; i < n; i++) {
            find(i, parent);
        }
        for(int i = 0; i < edges.length; i++) {
            if (w[parent[edges[i][0]]] == -1) {
                w[parent[edges[i][0]]] = edges[i][2];
            } else {
                w[parent[edges[i][0]]] &= edges[i][2];
            }
        }
        // System.out.println(Arrays.toString(parent));
        // System.out.println(Arrays.toString(sz));
        // System.out.println(Arrays.toString(w));
        for (int i = 0; i < query.length; i++) {
            if (find(query[i][0], parent) != find(query[i][1], parent)) {
                res[i] = -1;
            } else {
                res[i] = w[find(query[i][0], parent)];
            }
        }
        return res;
    }

    void union(int[] edge, int parent[], int sz[]) {
        int parentX = find(edge[0], parent);
        int parentY = find(edge[1], parent);
        if (parentX != parentY) {
            if (sz[parentX] >= sz[parentY]) {
                parent[parentY] = parentX;
                sz[parentX] += sz[parentY];
            } else {
                parent[parentX] = parentY;
                sz[parentY] += sz[parentX];
            }
        }
    }

    int find(int x, int[]parent) {
        if (x == parent[x]) {
            return x;
        }
        int p = find(parent[x], parent);
        parent[x] = p;
        return p;
    }

}

```