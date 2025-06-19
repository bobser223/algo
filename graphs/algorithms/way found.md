Знову ж таки звичайний [[algorithms and data structures/graphs/algorithms/BFS|BFS]], але ми використовуємо список батьків, щоб отримати щось приблизно таке _from_ $\leftarrow v_1 \leftarrow v_2 \leftarrow ... \leftarrow$ _to_, ну а далі ми розветаємо цей масив "посилань", ну тобто отримуємо \[_from_, $v_1$, $v_2$, ... , _to_ \]  це і є найкоротший шлях з _from_ з _to_

> **складність** теж саме що і в BFS: $O(n^2)$ для графа через матрицю, $O(n+m)$ (m -кількість ребр) для графа через список


## Implementation

> **посилання на оригінальний граф** [[graph (oriented and not)]]



``` c++
#include <vector>  
#include <iostream>  
#include <algorithm>  
#include <queue>  
#include <stack>  

  
#define NOT_VISITED -1  
#define NO_PARENT -1
  
using namespace std;  
  
  
class GraphAdjacencyList{  
private:  
    void do_sth(int vertex){}  
  
    bool is_oriented;  
    vector<vector<int>> neighbors;  
    int g_size;  
  
public:  

	...

	std::vector<int> way(size_t from, size_t to){  
	    vector<int> parent(g_size, NO_PARENT);  
	    parent[from] = from; // stub value  
	  
	    queue<int> q;  
	    q.push(from);  
	  
	    while (!q.empty()){  
	        int curr = q.front(); q.pop();  
	  
	        for (int neighbor: neighbors[curr]){  
	            if (parent[neighbor] == NO_PARENT){ // similar to !used[neighbor]  
	                parent[neighbor] = curr;  
	                q.push(neighbor);  
	            }        }    }  
	    if (parent[to] == NO_PARENT){  
	        // exception handling  
	        return {};  
	    }  
	    //////////////////// "reversing" parent vector //////////  
	  
	    int curr = to;  
	    stack<int> way_st;  
	    while(curr != from){  
	        way_st.push(curr);  
	        curr = parent[curr];  
	    }    way_st.push(from);  
	  
	  
	    std::vector<int> result;  
	  
	    while(!way_st.empty()){  
	        result.push_back(way_st.top());  
	        way_st.pop();  
	    }  
	    return result;  
	  
	}
};
```