# Possible Bipartition
## https://leetcode.com/problems/possible-bipartition

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
