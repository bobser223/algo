
# Realisation
Дерево можна реалізовувати через _масив_, але це нормально лише в деяких випадках, а саме бінарної купи та дерева відрізків, зазвичай це _лайно_.


Зазвичай дерево це [[рекурсивна структура даних]], тут є два варіанти:
1) `Node`
2) `Tree` =  `Node`

## Node

## Tree = Node

### Впорядковане
#### C++ (не ідеоматично, але просто)
```C++

#include <iostream>  
#include <vector>  
  
  
class Tree{  
    int value;  
    std::vector<Tree*> kids;  
  
    explicit Tree(int val): value(val){}  
  
    ~Tree(){  
        for (auto kid: kids)  
            delete kid;  
    }  
    void set_key(int val){  
        value = val;  
    }  
    int key() const{  
        return value;  
    }  
    void add_kid(Tree& t){  
        kids.push_back(&t);  
    }  
    bool remove_child(int key){  
        for (auto kid: kids){  
            if (kid ->value == key){  
                delete kid; 
                return true; 
            }        
        } 
        return false;
    }
          
    std::vector<Tree*> get_children(){  
        return kids;  
    }  
    Tree* get_child(int key){  
        for (auto kid: kids){  
            if (kid->value == key)  
                return kid;  
        }        return nullptr;  
    }  
  
  
  
  
};
```

#### Python
```python
class Node:
	""" Клас, що реалізує вузол дерева """
	
	def __init__(self, key=None):
		"""
		Конструктор - створює вузол дерева
		:param key: ключ вузла
		"""
		self.mKey = key
	
	def empty(self):
		"""
		Перевіряє чи вузол порожній
		:return: True, якщо вузол порожній
		"""
		return self.mKey is None
	def setKey(self, key):
		"""
		Встановлює ключ для вузла
		:param key: нове значення ключа
		"""
		self.mKey = key
		
	def key(self):
		"""
		Повертає ключ вузла
		:return: ключ вузла
		"""
		return self.mKey

class Tree(Node):
	"""
	Клас, що реалізує структуру даних дерево
	"""
	
	def __init__(self, key=None):
		"""
		Конструктор - створює вузол дерева
		:param key: ключ вузла, що створюється
		"""
		super().__init__(key)
		self.mChildren = []
	
	def empty(self):
		"""
		Перевіряє чи дерево порожнє
		:return: True, якщо дерево порожнє
		"""
		return super().empty() and len(self.mChildren) == 0

	def addChild(self, child: Tree):
		"""
		Додає до поточного вузла заданий вузол (разом з відповідним піддеревом)
		:param child: вузол (піддерево), що додається
		"""
		self.mChildren.append(child)
		
	def removeChild(self, key):
		"""
		Видаляє у поточному вузлі вузол-дитину
		:param key: ключ вузла, що видаляється
		:return: False, якщо вузол не містить дитину заданим ключем
		"""
		for child in self.mChildren:
			if child.key() == key:
				self.mChildren.remove(child)
				return True
		return False

	def getChild(self, key):
		"""
		За заданим ключем, повертає вузол зі списку дітей
		:param key: ключ вузла
		:return: знайдений вузол якщо його знайдено, None у іншому випадку
		"""
		for child in self.mChildren:
			if child.key() == key:
				return child
		return None # якщо ключ не знайдено

	def getChildren(self):
		"""
		Повертає список дітей поточного вузла
		:return: Список дітей
		"""
		return self.mChildren


```

### Не впорядковане

Якщо ж постає задача про реалізацію невпорядкованого дерева, то потрібно в
реалізації вузла список, що містить послідовність його дітей, замінити
невпорядкованою колекцією, наприклад, словником.

Ключем у словнику буде ключ вузла, а значенням – відповідний вузол дитини.

```C++


```

```python
class UnorderedTree(Node):
	"""
	Клас, що реалізує структуру даних невпорядковане дерево
	"""
	
	def __init__(self, key=None):
		"""
		Конструктор - створює вузол дерева
		:param key: ключ вузла, що створюється
		"""
		super().__init__(key) # Виклик конструктора батьківського класу
		self.mChildren = {} # словник дітей вузла, місить пари {ключ: піддерево}
	
	def addChild(self, child: UnorderedTree):
		"""
		Додає до поточного вузла заданий вузол (разом з відповідним піддеревом)
		:param child: вузол (піддерево), що додається
		"""
		self.mChildren[child.key()] = child
```