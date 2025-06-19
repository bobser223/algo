Алгоритм допомагає визначити _найкоротший шлях_ від фіксованої вершини до всіх інших

Ідея хвильового алгоритму полягає у тому, що ми запускаємо [[algorithms and data structures/graphs/algorithms/BFS|BFS]] і при цьому для
кожної вершини, крім того чи відвідана вона чи ні, ще будемо пам’ятати відстань від
стартової точки до неї. Для цього будемо вважати, що відстань від стартової точки до
себе є нулем. А далі під час додавання точки до черги (якщо вона ще не була
відвідана) будемо встановлювати для неї цю відстань на одиницю більшу ніж для
поточної.

> **складність** теж саме що і в BFS: $O(n^2)$ для графа через матрицю, $O(n+m)$ (m -кількість ребр) для графа через список
> 

## Implementation

> **посилання на оригінальний граф** [[graph (oriented and not)]]



```c++
#include <vector>  
#include <iostream>  
#include <algorithm>  
#include <queue>  
#include <stack>  

#define NOT_VISITED -1

  
using namespace std;  
  
  
class GraphAdjacencyList{  
private:  
    void do_sth(int vertex){}  
  
    bool is_oriented;  
    vector<vector<int>> neighbors;  
    int g_size;  
  
public:  


	...

  
	vector<int> wave(size_t start){  
	    queue<int> q;  
	    q.push(start);  
	  
	    vector<int> distances(g_size, NOT_VISITED);  
	    distances[start] = 0;  
	  
	  
	    while(!q.empty()){  
	        int curr = q.front(); q.pop();  
	  
	        for (int neighbor: neighbors[curr]){  
	            if (distances[neighbor] == NOT_VISITED){  
	                distances[neighbor] = distances[curr] + 1;  
	                q.push(neighbor);  
	            }        
	        }    
	    }    
	    return distances;  
	}
};
```