Для перевірки _не орієнтованого_ графа на звязність варто зробити наступне:
1) Створюємо список відвіданих вершин
2) Проходимось [[algorithms and data structures/graphs/algorithms/DFS|DFS]] (стартуємо з довільної вершини)
3) Якщо залишилась хоча б одна не відвідана, то граф не звязний, інакше - звязний

Ідая підрахунку компонент звязності абсолютно такаж, але кожен раз коли ми зустрічаємо не відвідану вершину після [[algorithms and data structures/graphs/algorithms/DFS|DFS]], ну тобто пункту __2)__ ми інкрементуємо рахунок компонент звязності.

Ідейно пошук самих компонент звязності дуже простий, ми просто замість простого масиву використовуємо 'std::unordered_map<номер вершини, номер її компоненти звязності'  і все, код я навів 

## implementation
### Count

```c++

class GraphAdjacencyList{  
private:  
    void do_sth(int vertex){}  
  
    bool is_oriented;  
    vector<vector<int>> neighbors;  
    int g_size;
public:

	...
	
	void DFS_for_connectivity_check_not_oriented(size_t v, vector<bool>& visited){  
	    stack<int> st;  
	    st.push(v);  
	  
	    visited[v] = true;  
	  
	    while (!st.empty()){  
	        int curr = st.top(); st.pop();  
	  
	        for (int neighbor: neighbors[curr]){  
	            if (!visited[neighbor]){  
	                st.push(neighbor);  
	                visited[neighbor] = true;  
	            }        }    }}  
	  
	bool connectivity_check_not_oriented(){  
	    if (is_oriented) {  
	        // user is an idiot;  
	        return 0;  
	    }  
	    if (g_size <= 1) {  
	        return true;  
	    }  
	    vector<bool> visited(g_size+1, false);  
	    visited[0] = true;  
	  
	    DFS_for_connectivity_check_not_oriented(1, visited);  
	  
	    for(bool is_visited: visited){  
	        if (!is_visited) return false;  
	    }  
	    return true;  
	}  
	  
	size_t connectivity_components_count_not_oriented(){  
	    if (is_oriented) {  
	        // user is an idiot;  
	        return -1;  
	    }  
	    size_t  count = 0;  
	  
	    vector<bool> visited(g_size+1, false);  
	    visited[0] = true;  
	  
	    for(size_t v = 1; v < g_size+1; v++){  
	        if (!visited[v]){  
	            count++;  
	            DFS_for_connectivity_check_not_oriented(v, visited);  
	        }    }  
	    return count;  
	}
};
```

### Elements
```c++
void DFS_for_connectivity_components_found_not_oriented(  
        std::unordered_map<int, int>& visited, int v, int conn_comp  
        ){  
  
    for (int neighbor: neighbors[v]){  
        if (visited.find(neighbor) == visited.end()){  
            visited[neighbor] = conn_comp;  
            DFS_for_connectivity_components_found_not_oriented(visited, neighbor, conn_comp);  
        }    }  
}  
  
unordered_map<int, int> connectivity_components_found_not_oriented(){  
    if (is_oriented) {  
        // user is an idiot;  
    }  
  
    size_t  count = 0;  
    //                <vertex_number, component_number>  
    std::unordered_map<int, int> visited;  
  
    for(size_t v = 0; v < g_size; v++){  
        if (visited.find(v) == visited.end()){  
            count++;  
            visited[v] = count;  
            DFS_for_connectivity_components_found_not_oriented(visited, v, count);  
        }    }  
    return visited;  
}
```