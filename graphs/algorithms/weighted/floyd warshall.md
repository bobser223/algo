### 🔁 Алгоритм Флойда – Воршелла

**Алгоритм Флойда – Воршелла** дозволяє знайти **найкоротші шляхи від всіх вершин графа до всіх вершин графа**.

---

Алгоритм Флойда – Воршелла **порівнює всі можливі шляхи** в графі між кожною парою вершин.

- Вважається, що **відстань від вершини до себе — нульова**.
- **Відстані між сусідніми вершинами** задаються вагами ребер.
- Відстані до всіх інших вершин графу — **нескінченні**.

---

Нехай найкоротший шлях від вершини $i$ до вершини $j$, що використовує вершини  
$\{1, 2, \dots, k\}$, дорівнює $path(i, j, k)$.  
Тоді, додавши до цього переліку вершину $k + 1$, найкоротшим шляхом буде:

$$
path(i, j, k + 1) = \min \left(path(i, j, k),\ path(i, k + 1, k) + path(k + 1, j, k)\right)
$$

---
## Capacity
**Алгоритмічна складність** буде:

$$
O(n^3)
$$

у найгіршому випадку, де $n$ — кількість вершин у графі.

## Implementation
```c++
std::vector<std::vector<int>> floyd_warshall(){  
  
    vector<vector<int>> distance(g_size, std::vector<int>(g_size, INF));  
  
    for (int v = 0; v < g_size; v++){  
        distance[v][v] = 0;  
        for (auto [nei, nei_w]: graph[v]){  
            distance[v][nei] = nei_w;  
        }    }  
    for (int k = 0; k < g_size; ++k)  
        for (int i = 0; i < g_size; ++i)  
            for (int j = 0; j < g_size; ++j)  
                if (distance[i][k] < INF && distance[k][j] < INF) // не обовязково  
                    distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j]);  
  
  
  
    for (int v = 0; v < g_size; ++v) { // перевірка на негативні цикли  
        if (distance[v][v] < 0) {  
            throw runtime_error("Graph contains a negative-weight cycle");  
        }    }  
  
    return distance;  
  
}
```