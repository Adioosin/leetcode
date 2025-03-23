[2685. Count the Number of Complete Components](https://leetcode.com/problems/count-the-number-of-complete-components/description/?envType=daily-question&envId=2025-03-22)

## Solution

``` java
class Solution {
    public int countCompleteComponents(int n, int[][] edges) {
        int parent[] = new int[n];
        int sz[] = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            sz[i] = 1;
        }
        int arr[] = new int[n];
        for (int i = 0; i < edges.length; i++) {
            union(edges[i][0], edges[i][1], parent, sz);
            arr[edges[i][0]] += 1;
            arr[edges[i][1]] += 1;
        }
        Set<Integer> s = new HashSet<>();
        Set<Integer> remove = new HashSet<>();
        for(int i = 0; i < n; i++) {
            find(i, parent);
            s.add(parent[i]);
            if (arr[i] != sz[parent[i]] - 1) {
                remove.add(parent[i]);
            }
        }
        // System.out.println(Arrays.toString(parent));
        // System.out.println(Arrays.toString(sz));
        // System.out.println(Arrays.toString(arr));
        s.removeAll(remove);
        return s.size();
    }

    public void union(int x, int y, int parent[], int sz[]) {
        int parentX = find(x, parent);
        int parentY = find(y, parent);
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

    public int find(int i, int[] parent) {
        if (parent[i] == i) return i;
        int p = find(parent[i], parent);
        parent[i] = p;
        return p;
    }
}
```