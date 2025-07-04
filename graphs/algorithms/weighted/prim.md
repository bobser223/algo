
### 🌳 Алгоритм Прима

**Ідея алгоритму Прима** дуже проста і полягає в тому, що, **починаючи з деякої вершини**,  
на кожному кроці алгоритму до побудованого на попередніх етапах дерева **додається ребро з найменшою вагою**.

---

Цей процес **триває доти, доки дерево не охопить усі вершини вихідного графа**.

---
## Capacity
### 📊 Алгоритмічна складність

Алгоритмічна складність залежить від того, **як реалізований граф** та **як здійснюється вибір вершини з найменшою "ціною" додавання до дерева**.

---

#### 🔹 Вибір вершини послідовним перебором

Вибір вершини здійснюється **послідовним перебором**.

Нехай $n$ – кількість вершин у графі, $m$ – кількість ребер у графі $$(m \leq n(n - 1))$$.

- Для **матриці суміжності**:
  $$
  O(n^2)
  $$

- Для **списку суміжності**:
  $$
  O(n^2)
  $$

---

#### 🔹 Вибір вершини з використанням пріоритетної черги

Вибір вершини здійснюється **пріоритетною чергою** на базі **двійкової купи**.

Нехай $n$ – кількість вершин, $m$ – кількість ребер $$(m \leq n(n - 1))$$.

- Для **матриці суміжності**:
  $$
  O(n^2 + m \log n)
  $$

- Для **списку суміжності**:
  $$
  O((n + m) \log n)
  $$
