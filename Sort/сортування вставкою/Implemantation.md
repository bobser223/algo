```python
def insertion_sort(arr):

	n = len(arr)
	for i in range(1, n):
		currValue = arr[i]
		position = i
		while position > 0:
			if arr[position - 1] > currValue:
				arr[position] = arr[position - 1]
			else:
				break
			position -= 1
		arr[position] = currValue
```
