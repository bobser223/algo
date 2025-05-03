Стек – це динамічна структура даних із принципом доступу до елементів
__«останнім прийшов – першим пішов»__ (англ. LIFO – last in, first out). Усі операції
у стеку проводяться тільки з одним елементом, що називається верхівкою
стека, та був доданий до стеку останнім.

### Implementation

#### Python
##### using built-in list


```python
class Stack:
	""" Клас, що реалізує стек елементів
	на базі вбудованого списку Python """
	def __init__(self):
		""" Конструктор """
		self.items = []
	
	def empty(self):
		""" Перевіряє чи стек порожній
		:return: True, якщо стек порожній
		"""
		return len(self.items) == 0
	def push(self, item):
		""" Додає елемент у стек
		:param item: елемент, що додається у стек
		:return: None
		"""
		self.items.append(item)
	
	def pop(self):
		""" Забирає верхівку стека
		Сам елемент при цьому прибирається зі стеку
		:return: Верхівку стеку
		"""
		if self.empty():
			raise Exception("Stack: 'pop' applied to empty container")
		return self.items.pop()
```

##### through linked list

```python
class Node:

	""" Допоміжний клас, що реалізує вузол стеку """
	
	def __init__(self, item):
	
		""" Конструктор
		
		:param item: навантаження вузла
		
		"""
		
		self.item = item # створюємо поле для зберігання навантаження
		
		self.next = None # посилання на наступний вузол стеку


class Stack:

	""" Клас, що реалізує стек елементів
	
	як рекурсивну структуру """

	def __init__(self):
		
		""" Конструктор """
		
		self.top_node = None # посилання на верхівку стеку
	
	def empty(self):
	
		""" Перевіряє чи стек порожній
		
		:return: True, якщо стек порожній
		
		"""
		
		return self.top_node is None

	def push(self, item):
	
		""" Додає елемент у стек
		
		:param item: елемент, що додається у стек
		
		"""
		
		new_node = Node(item) # Створюємо новий вузол стеку
		
		new_node.next = self.top_node # Новий вузол
								# має посилатися на поточну верхівку
		
		self.top_node = new_node # змінюємо верхівку стека новим вузлом
		
	def pop(self):

		""" Забирає верхівку стека
		
		Сам вузол при цьому прибирається зі стеку
		
		:return: Навантаження верхівки стеку
		
		"""
		
		if self.empty(): # Якщо стек порожній
			raise Exception("Stack: 'pop' applied to empty container")
		
		current_top = self.top_node # запам'ятовуємо поточну верхівку стека
		
		item = current_top.item # запам'ятовуємо навантаження верхівки
		
		self.top_node = self.top_node.next # замінюємо верхівку наступним вузлом
		
		del current_top # видаляємо вузол, що місить попередню верхівку
```

#### C++
```c++
#include <iostream>  
#include <vector>  
  
struct Node{  
    int value;  
    Node* previous;  
  
};  
  
class stack{  
private:  
    Node* top = nullptr;  
    int counter = 0;  
public:  
    stack()= default;  
    ~stack(){  
        while (top != nullptr){  
            Node* buff = top;  
            top = top ->previous;  
            counter--;  
            delete buff;  
        }    }  
  
    void push(int value){  
        counter++;  
        Node* new_node = new Node;  
        new_node->previous = top;  
        new_node->value = value;  
        top = new_node;  
    }
```