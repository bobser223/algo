побудувати всі можливі n-значні числа, що складаються з цифр 0 та 1
```python
n = int(input())
l = 1 << n -1
r = 1 << n

for i in range(l, r):
	print(bin(i))
	
```

Програма що визначає кількість тризначних натуральних чисел, сума цифр яких дорівнює n (n >= 1)

```python
n = int(input())
counter = 0
for i in range(1, 10):
	for j in range(10):
		for k in range(10):
			if i + k + j == n:
				counter += 1

print(counter)
```



```python

def sequences(lst: list, k, n):


	if k > n:
		print(*lst)
		return

	for pos in range(k):
		lst_next = lst[:]
		lst_next.insert(pos, k)
		sequences(lst_next, k+1, n)

n = int(input())
lst = []
sequences(lst, 1, n)

	
```


За заданим натуальним числом n виведемо усі перестиновки з цілих чисел від 1 до n у лексографічному порядку
```python

def sequences(lst: list, n):


	k = len(lst)
	
	if k == n:
		print(*lst)
		return
	for i in range(1, n+1):
		if i not in lst:
			lst_next = lst[:]
			lst_next.append(pos, k)
			sequences(lst: list, n)
			
n = int(input())
lst = []
sequences(lst, 1, n)
```


