## Двійкова (бінарна) купа

Бінарне дерево є двійковою купою, якщо виконуються такі умови:
1. Будь-яке його піддерево (ліве чи праве) є двійковою купою.
2. Значення ключа будь-якого вузла є не меншим (макс-купа) або не більшим (мін-купа) за значення ключів його дітей.
3. Глибина всіх листків дерева відрізняється не більше ніж на 1.
4. При додаванні нових вузлів останній рівень заповнюється зліва направо (без «дірок»).

---

## Операції з двійковою купою

Для реалізації над пріоритетною чергою доступні такі операції:
1. Створити купу.
2. `empty()` — перевірка на порожність.
3. `insert(key)` — вставка ключа в купу.
4. `extract_minimum()` — видалення й повернення найменшого ключа.

Час виконання операцій `insert` та `extract_minimum` — **O(log n)**, де *n* — кількість елементів у купі.

Додаткові корисні операції:
- `update(key, newkey)` — заміна ключа в купі новим.
- `get_minimum()` — повернення найменшого ключа без видалення.

---

## Повне бінарне дерево (ПБД)

Бінарне дерево називається **повним бінарним деревом (ПБД)**, якщо всі його вузли мають рівно двох дітей, крім листків, що розташовуються на однаковій глибині.

---

## Нумерація вузлів ПБД

Якщо вузол має номер *n*, то його діти мають індекси:
- `2n` — лівий син
- `2n + 1` — правий син

Батько вузла з індексом *n*:
- `⌊n / 2⌋`


# Implementation

```c++
//  
// Created by Volodymyr Avvakumov on 28.05.2025.  
//  
#include <vector>  
#include <algorithm>  
  
#define SOMETHING 100500  
  
using namespace std;  
  
  
class Heap{  
private:  
    vector<int> vec;  
    static int size;  
  
  
    void sift_up(int idx=size){  
        int curr = idx;  
        while (curr != 0){  
            int parent = curr;  
            curr /= 2;  
            if (vec[parent] < vec[curr])  
                std::swap(vec[parent],vec[curr]);  
            else  
                break;  
        }    }  
    void sift_down(int idx=1){  
        int curr = idx;  
        while (curr*2 < size){  
            int left = curr*2;  
            int right = curr*2+1;  
  
            int min;  
  
            if (vec[left] < vec[right])  
                min = left;  
            else  
                min = right;  
  
            if (vec[curr] > vec[min])  
                std::swap(vec[curr], vec[min]);  
            else  
                break;  
  
            curr = min;  
        }    }  
public:  
    Heap(){  
        vec.push_back(SOMETHING);  
        size = 0;  
    }  
    ~Heap() = default;  
  
    void insert(int value){  
        size++;  
        vec.push_back(value);  
        sift_up();  
    }  
    static int get_size(){  
        return size;  
    }  
    int get_minimum(){  
        return vec[1];  
    }  
    int extract_minimum(){  
        int deleted_value = vec[1];  
        size--;  
        vec[1] = vec.back();  
        vec.pop_back();  
        sift_down();  
  
        return deleted_value;  
    }  
    void update(int key, int new_key){  
        int idx = -1;  
        for (int i = 1; i <= size; ++i) {  
            if (vec[i] == key) {  
                idx = i;  
                break;  
            }        }        if (idx == -1) {  
            return;  
        }  
        int old = vec[idx];  
        vec[idx] = new_key;  
  
        if (new_key < old) {  
            sift_up(idx);  
        } else {  
            sift_down(idx);  
        }  
  
    }  
  
};
```

---

## Майже повне бінарне дерево

Бінарне дерево називається **майже повним бінарним деревом**, якщо дерево, утворене в результаті видалення всіх вузлів останнього рівня, є **повним бінарним деревом**.
