
```python
def bubble_sort(arr):
	n = len(arr)
	for pass_num in range(n - 1, 0, -1):
		for i in range(pass_num):
			if arr[i] > arr[i + 1]:
				arr[i], arr[i + 1] = arr[i + 1], arr[i]
```