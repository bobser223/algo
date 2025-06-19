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