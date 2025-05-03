```python
def qsort(array, a, b):

	if a >= b: return
	
	pivot = array[a + (b - a) // 2] # опорний елемент
	left = a # лівий маркер
	right = b # правий маркер
	
	while True:
		while array[left] < pivot: # Рухаємося зліва на право, поки не знайдемо
			left += 1 # елемент, що більший або рівний за опорний
		
		while pivot < array[right]: # Рухаємося справа на ліво, поки не знайдемо
			right -= 1 # елемент,що менший або рівний за опорний
		
		if left >= right: # маркери вказують на один і той де елемент або
			break # лівий маркер займає позицію праворуч від правого
		
		array[left], array[right] = array[right], array[left] # міняємо місцями
		left += 1 # зміщуємо лівий маркер вправо
		right -= 1 # зміщуємо правий маркер вліво
		
		# рекурсивно повторюємо процедуру для лівого та правого підсписків
		qsort(array, a, right)
		qsort(array, right + 1, b)
```

```c
#include <stdio.h>  
#include <stdlib.h>  
#include <time.h>  
  
void print_arr(int* arr, size_t size){  
    printf("[ ");  
    for(int i =0; i< size; i++)  
        printf("%d, ", arr[i]);  
  
    printf("]\n");  
}  
  
void swap(int* arr, int first_position, int second_position){  
  
    // the same as arr[first_position], arr[second_position] = arr[second_position], arr[first_position]  
  
    int buffer = arr[first_position];  
  
    arr[first_position] = arr[second_position];  
    arr[second_position] = buffer;  
}  
  
int* quick_sort(int* arr_to_sort, int begin, int end){  
    if (begin >= end) return arr_to_sort;  
  
    int pivot = arr_to_sort[begin + (end - begin) / 2];  
  
    int left = begin;  
    int right = end;  
  
    while (1){  
        while (arr_to_sort[left] < pivot)  
            left++;  
  
        while (arr_to_sort[right] > pivot)  
            right--;  
  
        if (left >= right)  
            break;  
  
        swap(arr_to_sort, left, right);  
  
        left++;  
        right--;  
  
    }  
    quick_sort(arr_to_sort, begin, right);  
    quick_sort(arr_to_sort, right+ 1, end);  
  
    return arr_to_sort;  
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
  
    clock_t start = clock();  
  
    sorted = quick_sort(arr_to_sort,0, arr_size - 1);  
  
    print_arr(sorted, arr_size);  
  
    clock_t end = clock();  
    double time_taken = ((double)(end - start)) / CLOCKS_PER_SEC;  
    printf("Sorting took %f seconds.\n", time_taken);  
  
    free(sorted);  
    // free(arr_to_sort); already freed abow sorted* == arr_to_sort  
}
```

$$P(z) = \begin{cases}  
0, & z \leq 0, \  
\frac{z^2}{8}, & 0 < z \leq 2, \  
z - 1 - \frac{z^2}{8}, & 2 < z \leq 4, \  
1, & z > 4.  
\end{cases}$$