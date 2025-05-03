__вказівка:__ використовувати  std::vector$<bool>$ під капотом це те ж саме

### Idea
Ми дуже часто зустрічаємося з наступною проблемою: нам потрібно видалити елемент з середини масиву, але це займає занадто багато операцій {мінімум $O(n)$} таким чином ми вводимо додатковий масив такої ж довжини зі значеннями int/bool та за допомогою нього визначаємо які елементи вже "видалені" але, на жаль, це займає занадто багато пам'яті. Нам насправді потрібен лише один біт, а ми використовуємо як мінімум байт. Тут і з'являється побітова маска - __число кожен біт якого відповідає за дискретний стан основного масиву__

### Implementation
```c++
#include <iostream>  
  
  
class bit_mask{  
private:  
    int num;  
    int size;  
public:  
    explicit bit_mask(int arr_size) : num(0), size(arr_size) {}  
  
    ~bit_mask() = default;  
  
    void set_one(int position){  
        num = num | (1 << (size - position));  
    }  
    
    void change_bit(int position){  
        num = num ^ (1 << (size - position));  
    }  
    
    void set_zero(int position){  
        num = num & ~(1 << (size - position));  
    }  
    
    void print() const {  
        for (int i = size - 1; i >= 0; --i) {  
            std::cout << ((num >> i) & 1);  
        }        
        std::cout << std::endl;  
    }  
  
  
};
```