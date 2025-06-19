# Означення
Дерево називається __бінарним__, якщо кожен
його вузол має _не більше ніж двох дітей_.

## Впорядкованість
Бінарні дерева вважаються
впорядкованими і діти його вузлів
називаються спеціальним чином – лівим
та правим сином

Інша термінологія для бінарних дерев
повністю повторює відповідну
термінологію для дерев



## Методи згідно з кліщем
1) Створення дерева (виклик конструктора).

2) Метод empty() – перевіряє чи дерево порожнє, тобто чи має вузол ключ та хоча б одного з дітей.

3) Метод item() – повертає ключ (навантаження) поточного вузла.

4) Методи leftChild() та rightChild() – повертають ліве/праве піддерево

5) Методи leftItem() та rightItem() – повертають навантаження лівого/правого сина

6) Методи setLeft() та setRight() – встановлюють навантаження або піддерево для лівого/правого сина

7) Методи hasLeft () та hasRight () – перевіряють чи має вузол лівого/правого сина

8) Метод hasNoChildren(self) – перевіряє чи вузол має дітей

9) Методи removeLeft() та removeRight() – видаляють лівого/правого сина вузла

# Implementation

```python
class BinaryTree:

	""" Реалізує бінарне дерево """
	
	def __init__(self, item=None, left=None, right=None):
	
		""" Конструктор
			
		Створює вузол дерева - корінь та ініціалізує його заданими даними
		
		:param item: Навантаження вузла
		
		:param left: ліве піддерево
		
		:param right: праве піддерево
		
		"""
		
		self.mItem = item # навантаження кореня дерева
		
		self.mLeftChild = None # створюємо поле для лівого сина
		
		self.mRightChild = None # створюємо поле для правого сина
		
		if left is not None:
		
			self.setLeft(left) # встановлюємо лівого сина
		if right is not None:
			self.setRight(right) # встановлюємо правого сина
	def empty(self):

		""" Перевіряє чи дерево порожнє, чи має воно навантаження та дітей
		
		:return: True, якщо дерево немає навантаження та дітей
		
		"""
		
		return (self.mItem is None
		
			and self.mLeftChild is None
		
			and self.mRightChild is None)
		
	def item(self):
		
		""" Повертає навантаження поточного вузла дерева
		
		:return: Навантаження
		
		"""
		
		if self.empty():
		
			raise Exception('BinaryTree: Дерево порожнє')
		
		return self.mItem
		
	def setNode(self, item):
		
		""" Змінює поточний вузол
		
		:param item: Нове піддерево або значення навантаження
		
		:return: None
		
		"""
		
		if isinstance(item, BinaryTree): # якщо item є деревом
		
			self.mItem = item.item() # змінюємо дані
			
			self.mLeftChild = item.leftChild() # змінюємо ліве піддерево
			
			self.mRightChild = item.rightChild() # змінюємо праве піддерево
			
		else:
		
			self.mItem = item

	def leftItem(self):

		""" Повертає навантаження лівого сина
		
		:return: Навантаження лівого сина
		
		"""
		
		if self.hasLeft():
		
			return self.mLeftChild.item()
		
	def rightItem(self):
		
		""" Повертає навантаження правого сина
		
		:return: Навантаження правого сина
		
		"""
		
		if self.hasRight():
		
			return self.mRightChild.item()

	def hasLeft(self):

		""" Чи містить дерево лівого сина
		
		:return: True, якщо дерево має лівого сина.
		
		"""
		
		return self.mLeftChild is not None

	def hasRight(self):
	
		""" Чи містить дерево правого сина
		
		:return: True, якщо дерево має правого сина.
		
		"""
		
		return self.mRightChild is not None
	
	def hasNoChildren(self):
	
		""" Визначає чи має дерево дітей
		
		:return: True, якщо дерево немає дітей.
		
		"""
		
		return self.mLeftChild is None and self.mRightChild is None

	def leftChild(self):
		
		""" Повертає ліве піддерево поточного вузла
		
		:return: Ліве піддерево поточного вузла
		
		"""
		
		return self.mLeftChild
		
	def rightChild(self):
		
		"""Отримати праве піддерево."""
		
		return self.mRightChild
		
	def removeLeft(self):
		
		""" Видаляє лівого сина """
		
		self.mLeftChild = None
		
	def removeRight(self):
		
		""" Видаляє правого сина """

		self.mRightChild = None
		
	def setLeft(self, item):
		
		""" Змінює лівого сина.
		
		:param item: Навантаження або піддерево
		
		:return: None
		
		"""
		
		if isinstance(item, BinaryTree): # якщо item є деревом
		
			self.mLeftChild = item # змінюємо все піддерево
		
		elif self.hasLeft(): # якщо дерево містить лівого сина
		
			self.mLeftChild.setNode(item) # замінюємо вузол
		
		else: # якщо дерево немає лівого сина
		
			self.mLeftChild = BinaryTree(item) # створюємо дерево з вузлом item
		
		# та робимо його лівим сином
		```