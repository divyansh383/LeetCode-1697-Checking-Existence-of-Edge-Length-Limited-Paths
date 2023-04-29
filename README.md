# LeetCode-1697-Checking-Existence-of-Edge-Length-Limited-Paths
# Problem 
An undirected graph of n nodes is defined by edgeList, where edgeList[i] = [ui, vi, disi] denotes an edge between nodes ui and vi with distance disi. Note that there may be multiple edges between two nodes.

Given an array queries, where queries[j] = [pj, qj, limitj], your task is to determine for each queries[j] whether there is a path between pj and qj such that each edge on the path has a distance strictly less than limitj .

Return a boolean array answer, where answer.length == queries.length and the jth value of answer is true if there is a path for queries[j] is true, and false otherwise.

# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->
The problem requires determining if there exists a path of length less than the given limit between two nodes. We can use a variation of Union-Find algorithm to solve this problem. First, we sort the edges by their distance in ascending order. Then, we sort the queries by their distance limit in ascending order. We iterate over the sorted queries and for each query, we add edges to our Union-Find data structure that have distance less than the current query limit. Then we check if the source and destination nodes of the query are connected in the Union-Find data structure. If they are, we add true to the result array, otherwise, we add false.
# Approach
<!-- Describe your approach to solving the problem. -->
Create a disjoint set data structure.
Sort the edges by their distance.
Sort the queries by their distance limit.
Iterate over the sorted queries.
For each query, add edges to the disjoint set data structure that have distance less than the current query limit.
Check if the source and destination nodes of the query are connected in the disjoint set data structure.
Add the result to the result array.
# Complexity
- Time complexity:
<!-- Add your time complexity here, e.g. $$O(n)$$ -->
The time complexity of this approach is O((E+Q)log(E)), where E is the number of edges and Q is the number of queries. This is because we sort the edges and queries, which takes O(Elog(E)) and O(Qlog(Q)) time respectively. Then we iterate over the sorted queries and add edges to the disjoint set data structure, which takes O(E) time. Finally, we check if the source and destination nodes of the query are connected in the disjoint set data structure, which takes O(1) time. Therefore, the overall time complexity is O((E+Q)log(E)).

- Space complexity:
<!-- Add your space complexity here, e.g. $$O(n)$$ -->
 The space complexity is O(N), where N is the number of nodes, because we need to create a disjoint set data structure with N nodes.

# Code
```java
class disjointSet{
    int[] set;
    int[] rank;
    disjointSet(int n){
        set=new int[n];
        rank=new int[n];
        for(int i=0;i<n;i++){set[i]=i;}
        
    }
    int find(int x){
        if(x==set[x]){return x;}
        set[x] = find(set[x]); 
        return set[x];
    }
    void union(int x, int y) {
        int parentX = find(x);
        int parentY = find(y);
        if (parentX == parentY) {
            return;
        }
        if (rank[parentX] < rank[parentY]) {
            set[parentX] = parentY;
        } else {
            set[parentY] = parentX;
            if (rank[parentX] == rank[parentY]) {
                rank[parentX]++;
            }
        }
    }
}

class Solution {
    public boolean[] distanceLimitedPathsExist(int n, int[][] edgeList, int[][] queries) {
        disjointSet djs=new disjointSet(n);

        Arrays.sort(edgeList, new Comparator<int[]>(){
            @Override
            public int compare(int[] a,int[] b){
                return Integer.compare(a[2],b[2]);
            }
        });
        int[][] q=new int[queries.length][4];
        for(int i=0;i<queries.length;i++){
            q[i][0]=queries[i][2];
            q[i][1]=queries[i][0];
            q[i][2]=queries[i][1];
            q[i][3]=i;
        }
        Arrays.sort(q, new Comparator<int[]>(){
            @Override
            public int compare(int[] a,int[] b){
                return Integer.compare(a[0],b[0]);
            }
        });

        boolean[] soln =new boolean[queries.length];
        int edge_idx=0;
        for(int[] query : q){
            int limit=query[0],src=query[1],dest=query[2],q_idx=query[3];
            while(edge_idx<edgeList.length && edgeList[edge_idx][2]<limit){
                djs.union(edgeList[edge_idx][0],edgeList[edge_idx][1]);
                edge_idx++;
            }
            soln[q_idx]=(djs.find(src)==djs.find(dest));
        }
        return soln;
    }
}


```
