# DFS
Пошук в глибину (англ. Depth-first
search, DFS) полягає у тому, щоб,
починаючи з кореня дерева,
_заглиблюватися у дерево наскільки
це можливо_, перш ніж здійснити
перехід до наступної (сусідньої)
вершини.

## Recursion
```C++
void do_something(){}  
  
void BFS(Tree* t){  
    for (auto kid: kids){  
        do_something();  
        BFS(kid);  
    }
}
```

```python
def DFS(tree: Tree):
	""" Обхід дерева в глибину
	:param tree: корінний вузол дерева (піддерева) """
	
	# запускаємо DFS для всіх дітей кореня
	for child in tree.getChildren():
		DFS(child)
	
	# Опрацьовуємо корінь
	print(tree.key(), end=" -> ")
```
## Stack

```c++
void do_something(){}  
  
void BFS(Tree* t){  
    std::stack<Tree*> st;  
    st.push(t);  
    while(!st.empty()){  
        Tree* curr = st.top(); st.pop();  
        do_something();  
        for (auto kid: curr->kids){  
            st.push(kid);  
        }    
    }
}
```

# BFS
Стратегія пошуку в ширину (англ.
breadth-first search, BFS) полягає у
тому, щоб _проходити вузли дерева
по рівнях_ – спочатку корінь, потім
всі вершини на першому рівні, потім
всі вершини на другому і так далі.
```c++
void do_something(){}  
  
void BFS(Tree* t){  
    std::queue<Tree*> q;  
    q.push(t);  
  
    while(!q.empty()){  
        Tree* curr = q.front(); q.pop();  
        do_something();  
        for (auto kid: curr->kids){  
            q.push(kid);  
        } 
    }
}
```

```python
def BFS(tree: Tree):
	""" Обхід дерева в ширину
	:param tree: дерево
	"""
	
	q = Queue() # Черга для опрацьованих вузлів
	q.enqueue(tree) # Додаємо у чергу корінь дерева
	
	while not q.empty(): # Поки черга не порожня
	
		current = q.dequeue() # Беремо перший елемент з черги
		
		print(current.key(), end=" -> ") # Опрацьовуємо взятий елемент
		
		# Додаємо в чергу всіх дітей поточного вузла
		for child in current.getChildren():
			q.enqueue(child)
```