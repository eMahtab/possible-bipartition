# Possible Bipartition
## https://leetcode.com/problems/possible-bipartition

We want to split a group of n people (labeled from 1 to n) into two groups of any size. Each person may dislike some other people, and they should not go into the same group.

Given the integer n and the array dislikes where dislikes[i] = [ai, bi] indicates that the person labeled ai does not like the person labeled bi, return true if it is possible to split everyone into two groups in this way.

 
```
Example 1:

Input: n = 4, dislikes = [[1,2],[1,3],[2,4]]
Output: true
Explanation: group1 [1,4] and group2 [2,3].

Example 2:
Input: n = 3, dislikes = [[1,2],[1,3],[2,3]]
Output: false

Example 3:
Input: n = 5, dislikes = [[1,2],[2,3],[3,4],[4,5],[1,5]]
Output: false
``` 

## Constraints:
```
1. 1 <= n <= 2000
2. 0 <= dislikes.length <= 104
3. dislikes[i].length == 2
4. 1 <= dislikes[i][j] <= n
5. ai < bi
6. All the pairs of dislikes are unique.
```

# Implementation : BFS
```java
class Solution {
    public boolean possibleBipartition(int n, int[][] dislikes) {
        int[] visited = new int[n];
        Arrays.fill(visited, -1);
        Map<Integer,List<Integer>> graph = new HashMap<>();
        for(int[] dislike : dislikes) {
            int u = dislike[0] - 1;
            int v = dislike[1] - 1;
            graph.putIfAbsent(u, new ArrayList<Integer>());
            graph.putIfAbsent(v, new ArrayList<Integer>());
            graph.get(u).add(v);
            graph.get(v).add(u);
        }
        for(int vertex = 0; vertex < n; vertex++) {
            if(visited[vertex] == -1) {
                boolean isBipartite = isBipartite(graph, vertex, visited);
                if(!isBipartite)
                    return false;
            }
        }
        return true;
    }
    
    private boolean isBipartite(Map<Integer,List<Integer>> graph, int vertex, int[] visited) {
        Queue<int[]> q = new ArrayDeque<>();
        q.add(new int[]{vertex, 0});
        
        while(!q.isEmpty()) {
            int[] node = q.remove();
            int v = node[0];
            if(visited[v] != -1) {
                if(visited[v] != node[1])
                    return false;
            } else {
                visited[v] = node[1];
            }
            
            if(graph.get(v) != null) {
                for(int neighbor : graph.get(v)) {
                    if(visited[neighbor] == -1) {
                        q.add(new int[]{neighbor, node[1] + 1});
                    }
               }   
            }
        }
        return true;
    }
}
```

# References :
https://github.com/eMahtab/is-graph-bipartite
