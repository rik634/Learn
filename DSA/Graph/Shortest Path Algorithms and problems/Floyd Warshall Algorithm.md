# Floyd Warshall Algorithm
- The problem is to find the shortest distances between every pair of vertices in a given edge-weighted directed graph. The graph is represented as an adjacency matrix of size n*n. Matrix[i][j] denotes the weight of the edge from i to j. If Matrix[i][j]=-1, it means there is no edge from i to j.

### Logic
- Dijkstra's and bellman ford are the single shource shortest path algorithm. While, "Floyd Warshall Algorithm" is the multi source shortest path algorithm.
- It also helps to detect the negative weight cycle.
- This algorithm states to check via every node/vertex and find the shortest path.
- The algorithm is not much intuitive as the other ones. It is more of a brute force, where all combination of paths have been tried to get the shortest paths.
- 

### Solution
```
void shortest_distance(vector<vector<int>>&matrix) {
		int n = matrix.size();
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				if (matrix[i][j] == -1) {
					matrix[i][j] = 1e9;
				}
				if (i == j) matrix[i][j] = 0;
			}
		}

		for (int k = 0; k < n; k++) {
			for (int i = 0; i < n; i++) {
				for (int j = 0; j < n; j++) {
					matrix[i][j] = min(matrix[i][j],
					                   matrix[i][k] + matrix[k][j]);
				}
			}
		}




		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				if (matrix[i][j] == 1e9) {
					matrix[i][j] = -1;
				}
			}
		}
	}
```
- Time Complexity: O(V^3)
- Space Complexity: O(V^2) (For storing the adjacency matrix of given graph)

- If there are no negative weights present in the graph, we can use Dijkstra's algorithm. For each one source, time complexity is E*LogV. For V number of sources, time complexity will be V*E*logV.



### Intuition
1. How to detect a negative weight cycle:
   - If the shortest distance between node i and i is less than 0, then neGative cycle exists.
2. 