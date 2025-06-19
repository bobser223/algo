```c++
#include <iostream>  
#include <vector>  
#include <queue>  
#include <stack>  
  
using namespace std;  
  
#define INF 100500  
  
// 0 based  
  
struct edge{  
    int to;  
    int weight;  
  
    bool operator<(const edge& other) const {  
        return weight > other.weight;  
    }};  
  
  
  
class weighted_graph_list{  
private:  
    vector<vector<edge>> graph;  
    size_t g_size;  
    bool oriented;  
public:  
  
    void add_edge(int from, int to, int weight){  
        graph[from].emplace_back(to, weight);  
        if (!oriented)  
            graph[to].emplace_back(from, weight);  
    }  
    std::vector<int> belman_ford(int start){  
        int n = g_size;  
  
        std::vector<int> distances(g_size, INF);  
        distances[start] = 0;  
  
        for (int iter = 0; iter < n-1; iter++){  
            for (int v = 0; v < g_size; v++){  
                for(auto [neighbour, weight]: graph[v]){  
                    if (distances[v] + weight < distances[neighbour])  
                        distances[neighbour] = distances[v] + weight;  
                }            }        }  
        return distances;  
    }  
    std::vector<int> dijkstra(int start){  
        std::vector<int> distances(g_size, INF);  
        distances[start] = 0;  
  
  
        priority_queue<edge> pq;  
        pq.emplace(start, 0);  
  
  
        while (!pq.empty()){  
            auto [curr, curr_w] = pq.top(); pq.pop();  
  
            // Якщо це запис застарів — пропускаємо  
            if (curr_w > distances[curr])  
                continue;  
  
  
  
            for (auto [neighbour, nei_w]: graph[curr]){  
  
                if (curr_w + nei_w < distances[neighbour]){  
                    distances[neighbour] = curr_w + nei_w;  
                    pq.emplace(neighbour, curr_w + nei_w);  
                }            }        }  
        return distances;  
  
  
    }  
  
    std::vector<std::vector<int>> floyd_warshall(){  
  
        vector<vector<int>> distance(g_size, std::vector<int>(g_size, INF));  
  
        for (int v = 0; v < g_size; v++){  
            distance[v][v] = 0;  
            for (auto [nei, nei_w]: graph[v]){  
                distance[v][nei] = nei_w;  
            }        }  
        for (int k = 0; k < g_size; ++k)  
            for (int i = 0; i < g_size; ++i)  
                for (int j = 0; j < g_size; ++j)  
                    if (distance[i][k] < INF && distance[k][j] < INF) // не обовязково  
                        distance[i][j] = min(distance[i][j], distance[i][k] + distance[k][j]);  
  
  
  
        for (int v = 0; v < g_size; ++v) { // перевірка на негативні цикли  
            if (distance[v][v] < 0) {  
                throw runtime_error("Graph contains a negative-weight cycle");  
            }        }  
  
        return distance;  
  
    }  
    std::vector<int> prim_algo(){  
            }  
  
  
  
};
```