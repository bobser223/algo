Ідейно те ж саме що і в [[algorithms and data structures/trees/algorithms on trees/DFS|дереві]], але нам варто використати список вже відвіданих вершин (краще `std::vector<bool>`)

> **Складність** $O(n^2)$ для графа через матрицю, $O(n+m)$ (m -кількість ребр) для графа через список

> **Увага**
 в прикладі буде використано граф на основі списку суміжності
 
## Implementation

> **посилання на оригінальний граф** [[graph (oriented and not)]]


```c++
#include <vector>  
#include <iostream>  
#include <algorithm>  
#include <queue>  
#include <stack>  
  
using namespace std;  
  
  
class GraphAdjacencyList{  
private:  
    void do_sth(int vertex){}  
  
    bool is_oriented;  
    vector<vector<int>> neighbors;  
    int g_size;  
  
public:  


	...

    void DFS(int vertex){  
        do_sth(vertex);  
  
        stack<int> st;  
        st.push(vertex);  
  
        vector<bool> used(g_size, false);  
        used[vertex] = true;  
  
        while(!st.empty()){  
            int curr = st.top(); st.pop();  
  
            for (int neighbor: neighbors[curr]){  
                if (!used[neighbor]){  
                    do_sth(neighbor);  
                    st.push(neighbor);  
                    used[neighbor] = true;  
                }
	        }
	    }  
    }
};
```

