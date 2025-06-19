__text in markdown with latex__

## 🌲 Дерево відрізків

Нехай задано деякий індексований масив даних  
$$
a = \{a_i : i \in [0, n]\}.
$$

### 📌 Визначення

**Дерево відрізків** – структура даних, що дозволяє за час  
$$
O(\log n)
$$  
знаходити значення деякої цільової асоціативної функції для елементів масиву $a$ на проміжку $[i, j]$,  
$$
0 \le i \le j \le n,
$$  
тобто:
$$
f(a_i, a_{i+1}, \dots, a_j).
$$

📎 **Приклади цільових функцій**:
- Сума
- Добуток
- Мінімум / максимум
- Найбільший спільний дільник (НСД)
- Інші асоціативні функції

---

### 🌳 Структура дерева

- Дерево відрізків — це **повне бінарне дерево**, де кожна вершина відповідає певному відрізку масиву.
- **Корінь дерева** охоплює весь масив.
- Кожен вузол, що охоплює більше ніж один елемент, має **двох синів**, які охоплюють ліву та праву половину його діапазону.
- **Листки дерева** відповідають окремим елементам масиву.

---

### ⚡ Побудова дерева для суми

Для масиву з $k$ елементів дерево відрізків міститиме $2n$ вершин, де  
$$
n = 2^p \text{ — найменший степінь двійки, такий що } k \le n.
$$

Висота дерева:
$$
\log n.
$$

---

### 🔍 Головна властивість

Кожен **неперервний відрізок** масиву з $p$ елементів можна представити за допомогою не більш ніж  
$$
2 \log p
$$  
вершин дерева.

📌 Наприклад, для відрізку $[1, 6]$ масиву:
```
Індекс:   0  1  2  3  4  5  6  7  
Масив:    2  7  6  4  1  3  5  8
```
Для обчислення суми на цьому відрізку буде достатньо **4-х вузлів** дерева.

---

### 💾 Представлення в пам’яті комп’ютера

- Дерево є **повним бінарним**, тому як і **бінарна купа**, може бути представлене звичайним масивом розміром $2n$.
- Листки (елементи вхідного масиву) зберігаються у **другій половині масиву**:
  - $a_0$ — на позиції $n$
  - $a_1$ — на позиції $n+1$
  - ...
- Внутрішні вузли зберігаються на позиціях $2$ до $n-1$
- Корінь — у позиції $1$
- **Позиція $0$** — не використовується

---

### 🛠️ Операції з деревом відрізків

1. **Побудова** дерева на основі вхідного масиву
2. **Оновлення** значення одного з елементів
3. **Запит** на обчислення цільової функції на заданому відрізку



# Implementation

## C++
```c++
#include <vector>  
  
using std::vector;  
  
static const int NEUTRAL_ELEMENT = 0;  
  
class DecisionTree{  
private:  
    vector<int> items;  
    size_t _size;  
  
    void fill_tree(){  
        for (size_t i = _size - 1; i > 0; i--)  
                                    // any binary operation can be used  
            items[i] = items[2*i + 1] + items[2*i+ 2];  
    }  
public:  
    explicit DecisionTree(const vector<int>& vec){  
  
        size_t n = vec.size();  
        size_t m = 1;  
  
        while(m < n)  
            m <<= 1;  
  
        _size = m;  
        items.resize(_size, NEUTRAL_ELEMENT);  
  
        for (size_t i = 0; i < n; i++){  
            items[i+_size] = vec[i];  
        }  
        fill_tree();  
    }  
  
    void update(size_t position, int value){  
  
  
        position += _size;  
        items[position] = value;  
  
        while (position > 1){  
            position /= 2;  
  
            items[position] = items[2*position + 1] + items[2*position + 2];  
        }    }  
    int get_binary_op(size_t left, size_t right)  {  
  
        int result = NEUTRAL_ELEMENT;  
  
        left += _size;  
        right += _size;  
        while (left <= right){  
            if (left % 2 == 1) result += items[left];  
            if (right % 2 == 0) result += items[right];  
  
            left = (left + 1) / 2;  
            right = (right - 1) / 2;  
        }  
        return result;  
    }  
};


```


## Python

``` python
from math import log2

class SegmentTree:
    def __init__(self, array):
        """
        Конструктор
        :param array: Вхідний масив елементів
        """
        k = len(array)                           # кількість елементів у масиві даних
        n = (1 << int(log2(k - 1)) + 1)          # доводимо розмірність масиву до степені 2
        self.mItems = 2 * n * [0]                # масив для збереження вузлів дерева

        # записуємо дані вхідного масиву у листки дерева
        for i in range(k):
            self.mItems[n + i] = array[i]

        # записуємо дані для внутрішніх вузлів дерева
        for i in range(n - 1, 0, -1):
            self.mItems[i] = self.mItems[2 * i] + self.mItems[2 * i + 1]

        self.mSize = n                           # запам’ятовуємо розмірність масиву

    def update(self, pos, x):
        """
        Замінає значення ключа у вхідному масиві
        :param pos: позиція елемента вхідного масиву, що потрібно замінити
        :param x: нове значення
        """
        pos += self.mSize
        self.mItems[pos] = x                     # Змінюємо значення у листі

        # перераховуємо значення у внутрішніх вузлах
        i = pos // 2                             # Позиція батьківського вузла
        while i > 0:
            self.mItems[i] = self.mItems[2 * i] + self.mItems[2 * i + 1]
            i //= 2

    def sum(self, left, right):
        """
        Сума елементів відрізку від left до right вхідного масиву
        :param left: ліва межа відрізку
        :param right: права межа відрізку
        """
        left += self.mSize
        right += self.mSize

        res = 0
        while left <= right:
            if left % 2 == 1:                    # якщо left - правий син свого батька
                res += self.mItems[left]
            if right % 2 == 0:                   # якщо right - лівий син свого батька
                res += self.mItems[right]

            # підіймаємося у дереві на рівень вище
            left = (left + 1) // 2
            right = (right - 1) // 2

        return res

```