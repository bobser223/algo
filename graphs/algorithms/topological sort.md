
__Лише для орієнтованих графів !!!__

Топологічне сортування — це впорядкування вершин орієнтованого ациклічного графа (DAG) у такий спосіб, щоб для кожної орієнтованої дуги u→vu \to v вершина uu стояла перед vv. В результаті отримуємо лінійний порядок, який зберігає спрямованість усіх зв’язків у графі.


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

	void DFS_for_topological_sort(int vertex, std::vector<int>& colors, std::stack<int>& sorted){  
	    if (colors[vertex] == BLACK) return;  
	  
	    if (colors[vertex] == GREY){  
	        // we have a cycle topological sort is unreal  
	        // exception handling    }  
	  
	    colors[vertex] = GREY;  
	    for (int neighbor: neighbors[vertex]) {  
	  
	        DFS_for_topological_sort(neighbor, colors, sorted);  
	    }  
	  
	    colors[vertex] = BLACK;  
	    sorted.push(vertex);  
	}  
	  
	std::vector<int> topological_sort(){  
	    std::vector<int> colors(g_size, WHITE);  
	    std::stack<int> st;  
	    for (int v = 1; v <= g_size; v++)  
	        DFS_for_topological_sort(v, colors, st);  
	  
	    //////// reversing stack  
	  
	    std::vector<int> result;  
	  
	    while (!st.empty()){  
	        result.push_back(st.top());  
	        st.pop();  
	    }  
	    return result;
	}
};
```

