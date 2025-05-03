## python 1
```python

def merge(left_half, right_half):
	# Злиття двох відсортованих списків
	i = 0
	j = 0
	k = 0

	array = [0] * (len(right_half) + len(left_half))
	
	while i < len(left_half) and j < len(right_half):
		if left_half[i] < right_half[j]:
			array[k] = lefthalf[i]
			i += 1
	else:
		array[k] = righthalf[j]
		j += 1
		k += 1
	
	while i < len(left_half):
		array[k] = left_half[i]
		i += 1
		k += 1
	while j < len(right_half):
		array[k] = right_half[j]
		j += 1
		k += 1
	return array


def merge_sort(array):
	""" Реалізує алгоритм сортування злиттям
	:param array: Масив (список однотипових елементів)
	:return: None
	"""
	print("Splitting ", array)
	if len(array) > 1:
		# Розбиття списку навпіл
		mid = len(array) // 2
		left_half = array[:mid]
		right_half = array[mid:]
		
		# Рекурсивний виклик сортування для кожної з половин
		merge_sort(left_half)
		merge_sort(right_half)
		return merge(left_half, right_half)
		
	

	
```

## python 2

```python
def merge_sort(array):
	""" Реалізує алгоритм сортування злиттям
	:param array: Масив (список однотипових елементів)
	:return: None
	"""
	print("Splitting ", array)
	if len(array) > 1:
		# Розбиття списку навпіл
		mid = len(array) // 2
		left_half = array[:mid]
		right_half = array[mid:]
		
		# Рекурсивний виклик сортування для кожної з половин
		merge_sort(left_half)
		merge_sort(right_half)
		# Злиття двох відсортованих списків
		i = 0
		j = 0
		k = 0
		
		while i < len(left_half) and j < len(right_half):
			if left_half[i] < right_half[j]:
				array[k] = lefthalf[i]
				i += 1
		else:
			array[k] = righthalf[j]
			j += 1
			k += 1
		
		while i < len(left_half):
			array[k] = left_half[i]
			i += 1
			k += 1
		while j < len(right_half):
			array[k] = right_half[j]
			j += 1
			k += 1
		
```

## c 1
```c
#include <stdio.h>  
#include <stdlib.h>  
  
void print_arr(int* arr, size_t size){  
    printf("[ ");  
    for(int i =0; i< size; i++)  
        printf("%d, ", arr[i]);  
  
    printf("]\n");  
}  
  
int* copy_arr(int* arr, size_t begin, size_t end){  
    int* copy = (int*)calloc(abs((int)begin - (int)end), sizeof (int));  
  
    int j = 0;  
    for (int i = begin; i < end; i++){  
        copy[j] = arr[i];  
        j++;  
    }  
    return copy;  
}  
  
int* merge(int* arr_a, int* arr_b,size_t a_size, size_t b_size){  
    int* merged = (int*)calloc(a_size + b_size, sizeof (int));  
    int i = 0, j = 0, k = 0;  
  
    while (i < a_size && j < b_size){  
  
        if (arr_a[i] < arr_b[j]){  
            merged[k] = arr_a[i];  
            i ++;  
        } else {  
            merged[k] = arr_b[j];  
            j ++;  
        }  
        k ++;  
    }  
    while(i < a_size){  // while(i < a_size) merged[k++] = arr_a[i++]; 
        merged[k] = arr_a[i];  
        i++;  
        k++;  
    }  
    while(j < b_size){  // while(i < b_size) merged[k++] = arr_b[j++]; 
        merged[k] = arr_b[j];  
        j++;  
        k++;  
    }  
  
    free(arr_a);  
    free(arr_b);  
  
    return merged;  
}  
  
  
int* merge_sort(int* arr_to_sort, size_t arr_size){  
    if (arr_size == 1 || arr_size == 0)  
        return arr_to_sort;  
  
    size_t middle = arr_size / 2;  
  
    int* left_part = copy_arr(arr_to_sort, 0, middle);  
    int* right_part = copy_arr(arr_to_sort, middle, arr_size);  
  
    left_part = merge_sort(left_part, middle);  
    right_part = merge_sort(right_part, arr_size - middle);  
  
  
    return merge(left_part, right_part, middle, arr_size - middle);  
  
  
}  
  
  
  
  
int main(){  
    size_t arr_size = 7;  
    int* arr_to_sort = (int*)calloc(arr_size, sizeof (int));  
  
    arr_to_sort[0] = 9;  
    arr_to_sort[1] = 10;  
    arr_to_sort[2] = 7;  
    arr_to_sort[3] = -1;  
    arr_to_sort[4] = 4;  
    arr_to_sort[5] = 3;  
    arr_to_sort[6] = 2;  
  
    print_arr(arr_to_sort, arr_size);  
    int* sorted;  
  
    sorted = merge_sort(arr_to_sort, arr_size);  
  
    print_arr(sorted, arr_size);  
  
    free(sorted);  
    free(arr_to_sort);  
}
```
