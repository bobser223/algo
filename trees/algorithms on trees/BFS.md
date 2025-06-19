
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