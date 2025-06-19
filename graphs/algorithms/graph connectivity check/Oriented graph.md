## Слабка зв'язність
1) додаємо зворотні ребра ($\forall v_i, v_j: v_i \rightarrow v_j$ додаємо ребро $v_i \leftarrow v_j$), тобто перетворюємо граф на орієнтований. 
2) Використовуємо алгоритм з [[Not oriented graph]]

P.S. краще створювати буферний граф, а не додавати ребра в поточний.

## Сильна зв'язність
### Теорема
Нехай у орієнтованому графі G для довільно-фіксованої вершини v виконують такі твердження:
1) Всі вершини графа є досяжними з вершини v.  
2) Вершина v є досяжною з будь-якої вершини графа.  

Тоді граф є __сильно зв’язним.__

Першу умову перевірити легко – досить лише запустити пошук у глибину/ширину з будь-якої фіксованої вершини графа v і, по завершенню його роботи, переконатися, що всі вершини протягом його роботи були відвідані.

Для перевірки другої умови теореми потрібно побудувати граф Gᵀ, що є транспонованим по відношенню до початкового, далі запустити пошук з вершини v та, по завершенню його роботи, переконатися, що всі вершини були відвідані.

```c++
const GraphAdjacencyList transpose(const GraphAdjacencyList& g) {  
    // Якщо граф неорієнтований, його транспонований = сам граф (повертаємо копію)  
    if (!g.is_oriented) {  
        return g;  
    }  
    // Створюємо новий орієнтований граф того ж розміру  
    GraphAdjacencyList gt(g.g_size, /*oriented=*/true);  
  
    // Для кожного ребра u -> v у g додаємо ребро v -> u у gt  
    for (int u = 0; u < g.g_size; ++u) {  
        for (int v : g.neighbors[u]) {  
            gt.neighbors[v].push_back(u);  
        }    }  
    return gt;  
}  
  
friend bool DFS_connection_check(const GraphAdjacencyList& g, int start) {  
    if (g.g_size == 0) return true;    // або false — залежить від вашої задачі на порожній граф  
  
    vector<bool> visited(g.g_size, false);  
    visited[start] = true;  
  
    stack<int> st;  
    st.push(start);  
  
    while (!st.empty()) {  
        int curr = st.top();  
        st.pop();  
  
        for (int nei : g.neighbors[curr]) {  
            if (!visited[nei]) {  
                visited[nei] = true;  
                st.push(nei);  
            }        }    }  
    for (bool was : visited) {  
        if (!was) return false;  
    }    return true;  
}  
  
  
bool strong_connectivity_check_oriented(){  
    if (!is_oriented) {  
        // user is an idiot;  
        return 0;  
    }  
    if (g_size <= 1) {  
        return true;  
    }  
    if (!DFS_connection_check(*this, 0)) return false;  
  
    auto transposed_graph = transpose(*this);  
  
    if (!DFS_connection_check(transposed_graph, 0)) return false;  
  
    return true;  
  
  
}
```
## Пошук компонент сильної зв’язності

Алгоритм Косараджу пошуку компонент сильної зв’язності графа. Він базується на кількох кроках, наведених нижче:

1. Запускаємо пошук у глибину для графа G, та обчислюємо “час” виходу з кожної вершини.
2. Будуємо Gᵀ (транспонований граф).
3. Запускаємо пошук у глибину для графа Gᵀ, при цьому в основному циклі пошуку в глибину досліджуємо кожну вершину в порядку спадання “часу” її виходу, отриманому на кроці 1.
4. Кожне дерево лісу, знайденого на кроці 3, є компонентою сильної зв’язності.
