```c++
vector<tuple<int,int,int>> prim_mst(int p) {
    vector<int> parent(size+1, -1);
    vector<tuple<int,int,int>> mst;
    min_heap pq;
    pq.push(p, 0);
    vector<bool> used(size+1, false);

    while (!pq.empty() && mst.size() < size-1) {
        auto [v_min, w_min] = pq.extract_min();
        used[v_min] = true;

        // якщо це не корінь — записуємо ребро (parent→v_min) з вагою w_min
        if (parent[v_min] != -1) {
            mst.emplace_back(parent[v_min], v_min, w_min);
        }

        for (auto [neighbour, weight] : graph[v_min]) {
            if (used[neighbour]) continue;
            if (!pq.contains(neighbour) || weight < pq.get_priority(neighbour)) {
                pq.push(neighbour, weight);
                parent[neighbour] = v_min;
            }
        }
    }

    return mst;
}

```