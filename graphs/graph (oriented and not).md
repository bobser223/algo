# defenition
__Графом G__ називається _пара G = (V, E)_
V = {$v_1, v_2, .., v_n$} - множина вершин графу (vertexes)
E = {$e_1, e_2, .., e_m$} - множина ребер графа (edges)
# Aditional info
__Вершина__ (або вузол) графу – це
математична абстракція, яка _моделює
об’єкти_.
Вершина, як правило, має _ім’я, яке
називається ключем_.
__Навантаження вершини__ –
це _додаткова інформація_, що міститься у
вершині графа та _не впливає на його
структуру_.

__порядок графа__ - _кількість вершин_


---
__Ребра (або дуги)__ графу визначають
_відношення (тобто з’єднання)_ між
парами вершин.

_Кількість ребер_ графа називається
__розміром графу__

__Ребра__ графу _можуть бути:_

- _однонаправленими (дугами)_

- _двонаправленими (ребрами)_

однонаправлене ребро графа вказує на
односторонній зв’язок пари вершин
графу

Граф, у якого _хоча б одне ребро якого є
дугою_, __називається орієнтованим__

Якщо _рух_ по ребру між двома вершинами може
здійснюватися _у обох напрямках_, то ребро
називається __двонаправленим__

Граф _всі ребра_ якого _двонаправлені_
називається __неорієнтованим графом.__

У неорієнтованих графах _вершини сполучені_
ребром називають __суміжними (або сусідами)__.

__Суміжними__ називають ребра, що мають
_спільну вершину_

Для неорієнтованих графів вважається, що рух
_з вершини безпосередньо у себе_ не має сенсу.
Для орієнтованих графів такий спосіб
допускається.
При цьому вершина вважається суміжною собі
Дуги що позначають такий перехід називаються __петлями__

__Степінь вершини__ у неорієнтованому графі це _кількість ребр що виходять_ з цієї вершини, степінь позначається як deg(vertex_name)

У орієнтованому графі є __напівстепінь входу__ $deg^{+}(vertex)$ - _кількість дуг що входять у цю вершину_ та __напівстепінь виходу__ $deg^{-}(vertex)$ - _кількість дуг що виходять з цієї вершини_

Ребра можуть мати __вагу__.
Вага _визначає «вартість» переміщення_ від
однієї вершини до іншої.
Граф, _ребра якого мають вагу_ називається
__зваженим__

---
**Маршрутом** в графі називається послідовність вершин і ребер, яка має такі властивості:  
1) вона починається і закінчується вершиною;  
2) вершини і ребра в ній чергуються;  
3) будь-яке ребро цієї послідовності має своїми кінцями дві вершини: що безпосередньо передує йому в цій послідовності і наступну, що йде відразу за ним.

Маршрут, що веде з вершини $v_i \in V$ у точку $v_j \in V$ будемо позначати:  
$$
v_i \textrightarrow v_j
$$


**Шляхом** називається такий маршрут, в якому жодне ребро не зустрічається двічі.

Кажуть, що вершина $v_j$ досяжна з вершини $v_i$, якщо існує шлях з вершини $v_i$ у вершину $v_j$.

**Довжиною шляху у не зваженому графі** називається кількість ребер у цьому шляху.


__Шлях__ називається __простим__, якщо кожна
_вершина_ у ньому _відвідується лише раз_.


**Довжиною шляху у зваженому графі** є сума ваг усіх ребер, що входять до цього шляху.
**Шлях** називається **замкненим** або **циклічним**, якщо він починається та закінчується у одній і тій же вершині.

Замкненим буде шлях:  
$$
3 \rightarrow 5 \rightarrow 4 \rightarrow 3
$$

**Замкнений шлях** називається **простим**, якщо кожна вершина, за виключенням початкової та кінцевої, у ньому відвідується лише один раз.  
Простий циклічний шлях називається **циклом**.

---

**Граф, що не містить циклів**, називається **ациклічним**.

---

**Неорієнтований граф** називається **зв’язним**, якщо у ньому будь-яка вершина є досяжною з будь-якої іншої.

**Неорієнтований граф** називається **незв’язним**, якщо він не є зв’язним.

---

> **Для орієнтованих графів поняття зв’язності не є коректним.**

- **Орієнтований граф** називається **сильно-зв’язним**, якщо в ньому існує шлях з будь-якої вершини до будь-якої іншої. У такому разі говорять, що граф містить одну компоненту сильної зв’язності.

- **Орієнтований граф** називається **односторонньо-зв’язним**, якщо для будь-яких двох його вершин $v_i$ та $v_j$ існує хоча б один зі шляхів: або від $v_i$ до $v_j$, або від $v_j$ до $v_i$.

- **Орієнтований граф** називається **слабо-зв’язним**, якщо є зв’язним неорієнтований граф, отриманий з нього заміною орієнтованих ребер на неорієнтовані.

# Implementation

## Adjacency List ( список суміжності)
### C++
```C++
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
    explicit GraphAdjacencyList(int size, bool oriented=false){  
        // вершини починаються з нуля, якщо з одиниці, то тоді resize(size + 1) та neighbors[0] завжди пустий  
        neighbors.resize(size);  
        is_oriented = oriented;  
        g_size = size;  
    }  
    ~GraphAdjacencyList() = default;  
  
  
    void add(int vertex, int edge){  
        neighbors[vertex].push_back(edge);  
        if (!is_oriented)  
            neighbors[edge].push_back(vertex);  
  
    }  
    void removeEdge(int vertex, int edge){  
        neighbors[vertex].erase(  
            std::remove(neighbors[vertex].begin(), neighbors[vertex].end(), edge),  
            neighbors[vertex].end()  
        );        if (!is_oriented)  
            neighbors[edge].erase(  
                std::remove(neighbors[edge].begin(), neighbors[edge].end(), vertex),  
                neighbors[edge].end()  
            );    }  
  
  
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
                }            }        }  
    }  
  
    void BFS(int vertex){  
        do_sth(vertex);  
  
        queue<int> q;  
        q.push(vertex);  
  
        vector<bool> used(g_size, false);  
        used[vertex] = true;  
  
        while(!q.empty()){  
            int curr = q.front(); q.pop();  
            for (int neighbor: neighbors[curr]){  
                if (!used[neighbor]){  
                    do_sth(neighbor);  
                    q.push(neighbor);  
                    used[neighbor] = true;  
                }            }  
        }    }  
  
};
```
## Matrix
### C++
```c++
#include <vector>  
#include <iostream>  
#include <algorithm>  
#include <queue>  
#include <stack>  
  
using namespace std;  

class GraphMatrix{  
private:  
    void do_sth(int vertex){}  
    vector<vector<int>> matrix;  
    int g_size;  
    bool is_oriented;  
public:  
    explicit GraphMatrix(int size, bool oriented=false){  
        g_size = size;  
        matrix.resize(size, std::vector(size, 0));  
        is_oriented = oriented;  
    }  
    ~GraphMatrix() = default;  
  
    void add(int vertex, int edge){  
        // якщо вершини рахуємо з 1 то тут варто зробити vertex--; edge--;  
        matrix[vertex][edge] = 1;  
        if (!is_oriented){  
            matrix[edge][vertex] = 1;  
        }    }  
    void removeEdge(int vertex, int edge){  
        // якщо вершини рахуємо з 1 то тут варто зробити vertex--; edge--;  
        matrix[vertex][edge] = 0;  
  
        if (!is_oriented){  
            matrix[edge][vertex] = 0;  
        }    }  
    void DFS(int vertex){  
        // якщо вершини рахуємо з 1 то тут варто зробити vertex--;  
        do_sth(vertex);  
  
        stack<int> st;  
        st.push(vertex);  
  
        vector<bool> used(g_size, false);  
        used[vertex] = true;  
  
        while(!st.empty()){  
            int curr = st.top(); st.pop();  
  
            for (int neighbor = 0; neighbor < g_size; neighbor++){  
                if (matrix[curr][neighbor] == 1 && !used[neighbor]){  
                    do_sth(neighbor);  
                    st.push(neighbor);  
                    used[neighbor] = true;  
                }            }        }    }  
    void BFS(int vertex){  
        // якщо вершини рахуємо з 1 то тут варто зробити vertex--;  
        do_sth(vertex);  
  
        queue<int> q;  
        q.push(vertex);  
  
        vector<bool> used(g_size, false);  
        used[vertex] = true;  
  
        while(!q.empty()){  
            int curr = q.front(); q.pop();  
  
            for (int neighbor = 0; neighbor < g_size; neighbor++){  
                if (matrix[curr][neighbor] == 1 && !used[neighbor]){  
                    do_sth(neighbor);  
                    q.push(neighbor);  
                    used[neighbor] = true;  
                }            }        }    }  
  
  
  
  
};
```
