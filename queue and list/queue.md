### definition
Черга – динамічна структура даних із принципом доступу до елементів
__"першим прийшов – першим пішов" (англ. FIFO – first in, first out)__.

> такий собі антогоніст [[stack|стеку]]

---
### базові дії при роботі з чергою
Класична реалізація черги вимагає реалізувати структуру даних з такими
операціями:

1. Створення порожньої черги.

2. Операція __empty()__ – визначення, чи є черга порожньою.

3. Операція __enqueue(item)__ – додати елемент item до кінця черги.

4. Операція __dequeue()__ – взяти елемент з початку черги.

---
### Bad implementation
> **вказівка**: тут вставка $O(n)$

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
			raise Exception("Queue: 'dequeue' applied to empty container")
		return self.items.pop(0)
```
#### Implementation
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
---
---

## Deque

> або черга з двома кінцями

Черга з двома кінцями (двостороння черга або дек, deque від англ. double ended
queue) – динамічна структура даних, елементи якої можуть:

- додаватись як у початок (голову), так і в кінець (хвіст);

- вилучатися як з початку, так і з кінця.
---
### Базові дії при роботі з деком
Класична реалізація деку вимагає опису структури даних, що підтримує такі
операції:

1. Створення порожнього деку.

2. Операція empty()– визначення, чи є дек порожнім.

3. Операція appendleft(item) – додати елемент item до початку дека.

4. Операція popleft() – взяти елемент з початку дека.

5. Операція append(item) – додати елемент item до кінця дека.

6. Операція pop() – взяти елемент з кінця дека.
---
### bad implementation
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

### implementation
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