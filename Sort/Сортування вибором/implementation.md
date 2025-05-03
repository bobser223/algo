```python

def selection_sort(arr):
	n = len(arr)
	for i in range(n - 1, 0, -1):
		maxpos = 0
		for j in range(1, i + 1):
			if arr[maxpos] < arr[j]:
				maxpos = j
		arr[i], arr[maxpos] = arr[maxpos], arr[i]
		
```

