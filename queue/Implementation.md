> це кастрований однозвязний список
### Idea
__Черга__ – динамічна структура даних із принципом доступу до елементів _"першим прийшов – першим пішов" (англ. FIFO – first in, first out)._

_Взагалі є два варіанти реалізації_, але один з них, а саме _через масив маргінальний_, тому чергу реалізовують як [[рекурсивна структура даних|рекурсивну структуру даних]] 

#### Operations
Класична реалізація черги вимагає реалізувати структуру даних з такими операціями:
1. Створення порожньої черги.
2. Операція empty() – визначення, чи є черга порожньою.
3. Операція enqueue(item) – додати елемент item до кінця черги.
4. Операція dequeue() – взяти елемент з початку черги.

### Implementation

#### Python
##### built-in list(array) 
```python
class Queue:
	""" Клас, що реалізує чергу елементів
	на базі вбудованого списку Python """
	def __init__(self):
		""" Конструктор """
		self.items = [] # Список елементів черги
	
	def empty(self):
		""" Перевіряє чи черга порожня
		:return: True, якщо черга порожня
		"""
		return len(self.items) == 0
	
	def enqueue(self, item):
		""" Додає елемент у чергу (у кінець)
		:param item: елемент, що додається
		:return: None
		"""
		self.items.append(item)
	def dequeue(self):
		""" Прибирає перший елемент з черги
		Сам елемент при цьому прибирається із черги
		:return: Перший елемент черги
		"""
		if self.empty():
			raise Exception("Queue: 'dequeue'
		return self.items.pop(0)
```

##### linked list
```python
class Node:

	""" Допоміжний клас, що реалізує вузол черги """
	
	def __init__(self, item):
	
		""" Конструктор
		
		:param item: навантаження вузла
		
		"""
		
		self.item = item # створюємо поле для зберігання навантаження
		
		self.next = None # посилання на наступний вузол черги


class Queue:
	""" Клас, що реалізує чергу елементів
	як рекурсивну структуру """
	def __init__(self):
		""" Конструктор """
		self.front = None # Посилання на початок черги
		self.back = None # Посилання на кінець черги
	def empty(self):
		""" Перевіряє чи черга порожня
		:return: True, якщо черга порожня
		"""
		# Насправді досить перевіряти лише одне з полів front або back
		return self.front is None and self.back is None
	def enqueue(self, item):
		""" Додає елемент у чергу (в кінець)
		:param item: елемент, що додається
		:return: None
		"""
		new_node = Node(item) # Створюємо новий вузол черги
		if self.empty(): # Якщо черга порожня
		self.front = new_node # новий вузол робимо початком черги
		else:
		self.back.next = new_node # останній вузол черги
		
		# посилається на новий вузол
		self.back = new_node # Останній вузол вказує на новий вузол
	def dequeue(self):
		""" Прибирає перший елемент з черги
		:return: Навантаження голови черги (перший елемент черги)
		"""
		if self.empty():
			raise Exception("Queue: 'dequeue' applied to empty container")
		current_front = self.front # запам'ятовуємо поточну голову черги
		item = current_front.item # запам'ятовуємо навантаження першого вузла
		self.front = self.front.next # замінюємо перший вузол наступним
		del current_front # видаляємо запам'ятований вузол
		if self.front is None: # Якщо голова черги стала порожньою
			self.back = None # Черга порожня => хвіст черги теж порожній
		return item

	
```


#### C++
```c++
#include <iostream>


// top -> . . . . . . -> bottom


using std::cin;
using std::cout;
using std::endl;

class Queue {
private:
    struct Node {
        int value;
        Node* next;
        Node(int v, Node* ptr = nullptr)
            : value(v), next(ptr) {}
    };

    Node* top;
    Node* bottom;
    int SIZE;

public:
    Queue()
        : top(nullptr), bottom(nullptr), SIZE(0) {}

    ~Queue() {
        clear();  // Очистка памяти
    }

    void push(int n) {
        Node* tmp = new Node(n);
        SIZE++;

        if (!top) {
            top = bottom = tmp;
            return;
        }

        bottom->next = tmp;
        bottom = tmp;
    }

    void pop() {
        if (SIZE == 0 || !top) {
            cout << "error" << endl;
            return;
        }
        SIZE--;

        cout << top->value << endl;
        Node* tmp = top;
        top = top->next;
        delete tmp;
        if (!top) {
            bottom = nullptr;
        }
    }

    void front() const {
        if (SIZE == 0 || !top) {
            cout << "error" << endl;
            return;
        }
        cout << top->value << endl;
    }

    void size() const {
        cout << SIZE << endl;
    }

    void clear() {
        while (top) {
            Node* tmp = top;
            top = top->next;
            delete tmp;
        }
        bottom = nullptr;
        SIZE = 0;
    }
};
```