### За заданим числом n необхідно вивести усі перестановки з цілих чисел від 1 до n
``` python

def sequences(lst: list, k, n):
	"""
	:param lst: підсписок перестановок
	:param k: елемент для вставки
	:param n: найбільший елемент послідовності
	"""
	
	if k > n: # Якщо всі елементи вже вичерпано
		print(*lst)
		return
	
	# Вставляємо елемент k у всі можливі позиції списку
	# отриманого на попередніх ітераціях
	
	for pos in range(k):
		lst_next = lst[:] # Копіюємо список
		lst_next.insert(pos, k) # вставляємо елемент
		sequences(lst_next, k + 1, n) # рекурсивне додавання наступного числа.

n = int(input())
lst = []
sequences(lst, 1, n)

```



### Теж саме але у лексографічному порядку 


```python

def sequences(lst : list, n):
	"""
	:param lst: підсписок перестановок
	:param n: найбільший елемент послідовності
	"""
	
	k = len(lst) # Визначаємо кількість членів поточної підпослідовності
	
	if k == n: # Якщо всі елементи вже вичерпано
		print(*lst)
		return
	
	for i in range(1, n + 1):
		if i not in lst: # Якщо елемент i не міститься у підпослідовності
			lst_next = lst[:] # Копіюємо список
			lst_next.append(i) # Додаємо до нього елемент i
			sequences(lst_next, n) # Додавання рекурсивно інших членів послідовності.

n = int(input())
lst = []
sequences(lst, n)
```

