### вирішення колізій

1) з відкритою адресацією
2) з ланцюжками

#### з відкритою адресацією
під час розвязання колізій проводиться пошук іншого вільного місця для розміщення елементу.
(лінійне зондування - йти в якусь сторону по мапі, поки не знайдеться вільна комірка)

O(n) в найгіршому випадку, що вставка що перевірка, а особливо перевірка наявності.

```python
class HashTable:
	def __init__(self):
		self.size = 11
		self.curr_size = 0
		self.keys = [None] * self.size
		self.data = [None] * self.size

	def hash(self, key):
		return key % self.size

	def put(self, key, value):

		current = self.hash(key)
		while self.keys[current] is not None:
			if self.keys[current] == key:
				self.values[current] = value
				return
			curent += 1 # можна вийти за межі масиву
		
		self.keys[current] == key
		self.values[current] = value
		self.curr_size  += 1

	def get(self.key):
	
		current = self.hash(key)
		while self.keys[current] is not None:
			if self.keys[current] == key:
				return self.values[current]
			curent += 1 # можна вийти за межі масиву
		
		return None

```


#### з ланцюжками

кожна комірка це елемент це список (лінкед)


```python

class Node:

```


```python
class HashTable:
	def __init__(self):
		self.size = 11
		self.curr_size = 0
		self.keys = [None] * self.size
		self.data = [None] * self.size

	def hash(self, key):
		return key % self.size
	def get(self, key):
		hash_key = self.hash(key)
		slot = self.slots[hash_key]

		while slot is not None:
			if slot.key == key:
				return slot.value
			slot = slot.next
			return None

	def put(self, key, value):
		hash_key = self.hash(key)
		slot = self.slots[hash_key]
		while slot is not None:
			if slot.key == key:
				slot.value = value
				slot.valid = True
				return
			slot = slot.next

			slot = Node(key, vlue)
			slot.next = self.slots[hash_key]
			self.slots[hash_key] = slot
			
```