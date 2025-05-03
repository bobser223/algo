
### Editorial
Nothing to explain. Idea is just to find is the target < or > than the middle of an array (std::vector here) if < we do the same for the firs part of given arr, similar for >.
###  Recursion


```c++
#include <vector>  
#include <iostream>  
  
using std::cout;  
using std::endl;  
  
int binary_search_recursion(const std::vector<int>& sorted_vector, int target, int start, int end){  
    if (start > end)  
        return -1; // failed, no target in the vector  
  
    int middle = (start + end ) / 2;  
  
    if (sorted_vector[middle] == target)  
        return middle; // bingo  
  
    if (target < sorted_vector[middle])  
        return binary_search_recursion(sorted_vector, target, start, middle -1);  
  
  
    if (target > sorted_vector[middle])  
        return binary_search_recursion(sorted_vector, target, middle + 1, end);  
  
}

int main(){  
    std::vector<int> vec = {1, 2, 4, 6, 7, 8};  
    cout << binary_search_recursion(vec, 3, 0, vec.size()-1) << endl;  
    cout << binary_search_recursion(vec, 6, 0, vec.size()-1) << endl;  
}

```

```console
-1
3 // becouse we count from 0
```

### Loop

```c++

#include <vector>  
#include <iostream>  
  
using std::cout;  
using std::endl;  
  
int binary_search(const std::vector<int>& sorted_vector, int target){  
    int left = 0, right = sorted_vector.size();  
  
    while(left <= right){  
        int middle = (left + right) / 2;  
        if (sorted_vector[middle] == target){  
            return middle;  
        } else if (target < sorted_vector[middle]){  
            right = middle - 1;  
        } else if (target > sorted_vector[middle]){  
            left = middle + 1;  
        }    }  
    return -1;  
  
}  
  
  
int main(){  
    std::vector<int> vec = {1, 2, 4, 6, 7, 8};  
     
  
    cout << binary_search(vec, 3) << endl;  
    cout << binary_search(vec, 6) << endl;  
}
```