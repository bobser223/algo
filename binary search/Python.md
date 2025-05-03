### Basic

```python
def binary_search(arr, target):
	left = 0
	right = len(arr) - 1
	while left < right:
		m = (left + right) // 2
		if arr[m] < target:
			left = m + 1
		else:
			right = m
	return arr[right] == target
```

### Check

```python

def binary_search(arr, target):
	left = 0
	right = len(arr) - 1
	while left <= right:
	
		m = left +  (right - left) // 2 # коректна реалізація
		
		if arr[m] < target:
			left = m + 1
		elif arr[m] > target:
			right = m - 1
		else:
			return m
	return None
```


### bsearch_leftmost

бінарний пошук для відшуканна найпершого входження заданого числа



```python
def bsearch_leftmost(arr, target):
	left = 0
	right = len(arr) #фіктивний елемент
	while left < right:
	
		m = left +  (right - left) // 2 # коректна реалізація
		
		if arr[m] < target:
			left = m + 1
		else:
			right = m
	return left

```

можна вийти за межі масиву (тобто __left__ в ретурні буде == довжині масиву), якщо __target__ більше за всі елементи масиву 


#### найправіше входження

```python
def bsearch_rightmost(arr, target):
	left = 0
	right = len(arr) #фіктивний елемент
	while left < right:
	
		m = left +  (right - left) // 2 # коректна реалізація
		
		if arr[m] <= target:
			left = m + 1
		else:
			right = m
	return left - 1

```
якщо __target__ відсутній, то повернемл найперше входження елемента що м менше за __target__ 


якщо всі елемнти масиву більші за __target__ , то повернеться -1