> кастрований двозвязний список


### Idea
__Черга з двома кінцями__ (двостороння черга або дек, deque від англ. double ended queue) – динамічна структура даних, елементи якої можуть

1) додаватись як у початок (голову), так і в кінець (хвіст);

2) вилучатися як з початку, так і з кінця.

Тобто це структура що може слугувати і стеком і чергою одночасно, реалізується (нормальними людьми)
як [[рекурсивна структура даних]], а саме як обгортка над [[doubly linked list|двозвязним списком ]].


### Operations
Класична реалізація деку вимагає опису структури даних, що підтримує такі операції:

1. Створення порожнього деку.
2. Операція empty()– визначення, чи є дек порожнім.
3. Операція appendleft(item) – додати елемент item до початку дека.
4. Операція popleft() – взяти елемент з початку дека.
5. Операція append(item) – додати елемент item до кінця дека.
6. Операція pop() – взяти елемент з кінця дека.


### Implementation

#### Python
##### Built-in list(array) O(n)
```python
class Deque:

	def __init__(self):
		""" Конструктор деку
		Створює порожній дек.
		"""
		self.items = [] # Список елементів деку
	def empty(self):
		""" Перевіряє чи дек порожній
		:return: True, якщо дек порожній
		"""
		return len(self.items) == 0
	def append(self, item):
		""" Додає елемент у кінець деку
		:param item: елемент, що додається
		:return: None
		"""
		self.items.append(item)
	def pop(self):
		""" Повертає елемент з кінця деку.
		:return: Останній елемент у деку
		"""
		if self.empty():
			raise Exception("Deque: 'pop' applied to empty container")
		return self.items.pop()
	def appendleft(self, item):
		""" Додає елемент до початку деку
		:param item: елемент, що додається
		:return: None
		"""
		self.items.insert(0, item)
	def popleft(self):
		""" Повертає елемент з початку деку.
		:return: Перший елемент у деку
		"""
		if self.empty():
			raise Exception("Deque: 'popleft' applied to empty container")
		return self.items.pop(0)
```
##### linked list O(1)
```python
class Node:
	""" Допоміжний клас - вузол деку """
	def __init__(self, item):
		""" Конструктор вузла деку
		:param item: Елемент деку
		"""
		self.item = item # поле, що містить елемент деку
		self.next = None # наступний вузол
		self.prev = None # попередній вузол


class Deque:
""" Реалізує дек як рекурсивну структуру. """
	def __init__(self):
		""" Конструктор деку - Створює порожній дек.
		"""
		self.front = None # Посилання на перший елемент деку
		self.back = None # Посилання на останній елемент деку
	def empty(self):
		""" Перевіряє чи дек порожній
		:return: True, якщо дек порожній
		"""
		return self.front is None and self.back is None

	def appendleft(self, item):
		""" Додає елемент до початку деку
		:param item: елемент, що додається
		:return: None
		"""
		node = Node(item) # створюємо новий вузол деку
		node.next = self.front # наступний вузол нового - елемент, що є першим
		if not self.empty(): # якщо додаємо до непорожнього деку
			self.front.prev = node # новий вузол стає попереднім для першого
		else:
			self.back = node # якщо додаємо дані до порожнього деку,
							 # новий вузол є останнім
		
		self.front = node # новий вузол стає першим у деку
		
	def popleft(self):
		""" Вилучає елемент з початку деку.
		:return: Перший елемент у деку
		"""
		if self.empty():
			raise Exception('popleft: Дек порожній')
		node = self.front # node - перший вузол деку
		item = node.item # запам'ятовуємо навантаження
		self.front = node.next # першим стає наступний вузлом деку
		if self.front is None: # якщо в деку був 1 елемент
			self.back = None # дек стає порожнім
		else:
			self.front.prev = None # інакше перший елемент посилається на None
		del node # Видаляємо вузол
		return item

	# методи append та pop повністю симетричні appendleft та popleft відповідно

	def append(self, item):
		""" Додає елемент у кінець деку
		:param item: елемент, що додається
		:return: None
		"""
		...
	
	def pop(self):
		""" Повертає елемент з кінця деку.
		:return: Останній елемент у деку
		"""
		...

```

#### C++ O(1)
```c++

// top -> . . . . . . -> bottom  
//     <-             <-

#include <string>
#include <iostream>

using std::cin;
using std::cout;
using std::endl;

class Deque {
private:
    struct Node {
        int value;
        Node* next;
        Node* previous;
        Node(int v) : value(v), next(nullptr), previous(nullptr) {}
    };

    Node* top;
    Node* bottom;
    int SIZE;

public:
    Deque()
        : top(nullptr), bottom(nullptr), SIZE(0) {}

    ~Deque() {
        clear();
    }

    void push_back(int value) {
        SIZE++;
        if (!top) {
            top = bottom = new Node(value);
            return;
        }
        Node* curr = new Node(value);
        curr->previous = bottom;
        bottom->next = curr;
        bottom = curr;
    }

    void push_front(int value) {
        SIZE++;
        if (!top) {
            top = bottom = new Node(value);
            return;
        }
        Node* curr = new Node(value);
        curr->next = top;
        top->previous = curr;
        top = curr;
    }

    void pop_front() {
        if (!top) {
            cout << "error" << endl;
            return;
        }
        cout << top->value << endl;
        Node* tmp = top;
        top = top->next;
        delete tmp;
        if (top)
            top->previous = nullptr;
        else
            bottom = nullptr;
        --SIZE;
    }

    void pop_back() {
        if (!bottom) {
            cout << "error" << endl;
            return;
        }
        cout << bottom->value << endl;
        Node* tmp = bottom;
        bottom = bottom->previous;
        delete tmp;
        if (bottom)
            bottom->next = nullptr;
        else
            top = nullptr;
        --SIZE;
    }

    void front() const {
        if (!top) {
            cout << "error" << endl;
        } else {
            cout << top->value << endl;
        }
    }

    void back() const {
        if (!bottom) {
            cout << "error" << endl;
        } else {
            cout << bottom->value << endl;
        }
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