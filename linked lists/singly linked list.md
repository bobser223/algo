__Класичний (зв’язний або однозв’язний) список__ – це динамічна структура
даних, що складається з елементів (як правило одного типу), пов’язаних між
собою у строго визначеному порядку: _кожен елемент списку вказує на
наступний елемент списку_.

### Basic operations
Базовий набір дій над однозв’язним списком є таким:

1. Створити список.

2. Операція визначення, чи порожній список.

3. Додати елемент у початок списку.

4. Взяти перший елемент списку (без зміни всього списку).

5. Отримати хвіст списку.

```c++
#include <iostream>

template<typename T>
class SinglyLinkedList {
private:
    struct Node {
        T data;
        Node* next;
        Node(const T& d) : data(d), next(nullptr) {}
    };
    Node* head;

public:
    SinglyLinkedList() : head(nullptr) {}
    ~SinglyLinkedList() { clear(); }

    // Чи порожній список?
    bool empty() const { 
        return head == nullptr; 
    }

    // Додати на початок
    void push_front(const T& value) {
        Node* node = new Node(value);
        node->next = head;
        head = node;
    }

    // Додати в кінець
    void push_back(const T& value) {
        Node* node = new Node(value);
        if (empty()) {
            head = node;
        } else {
            Node* cur = head;
            while (cur->next)
                cur = cur->next;
            cur->next = node;
        }
    }

    // Видалити з початку
    bool pop_front() {
        if (empty()) return false;
        Node* tmp = head;
        head = head->next;
        delete tmp;
        return true;
    }

    // Видалити з кінця
    bool pop_back() {
        if (empty()) return false;
        if (head->next == nullptr) {
            delete head;
            head = nullptr;
            return true;
        }
        Node* prev = head;
        Node* cur  = head->next;
        while (cur->next) {
            prev = cur;
            cur  = cur->next;
        }
        prev->next = nullptr;
        delete cur;
        return true;
    }

    // Чи містить значення?
    bool contains(const T& value) const {
        for (Node* cur = head; cur; cur = cur->next)
            if (cur->data == value)
                return true;
        return false;
    }

    // Видалити перше входження значення
    bool remove(const T& value) {
        if (empty()) return false;
        if (head->data == value) {
            pop_front();
            return true;
        }
        Node* prev = head;
        Node* cur  = head->next;
        while (cur) {
            if (cur->data == value) {
                prev->next = cur->next;
                delete cur;
                return true;
            }
            prev = cur;
            cur  = cur->next;
        }
        return false;
    }

    // Підрахунок елементів
    int size() const {
        int count = 0;
        for (Node* cur = head; cur; cur = cur->next)
            ++count;
        return count;
    }

    // Очищення списку
    void clear() {
        while (pop_front());
    }

    // Вивід у консоль
    void print() const {
        for (Node* cur = head; cur; cur = cur->next)
            std::cout << cur->data << " ";
        std::cout << "\n";
    }

    // Доступ за індексом (повертає nullptr, якщо індекс за межами)
    T* get_at(int index) const {
        Node* cur = head;
        int i = 0;
        while (cur) {
            if (i == index)
                return &cur->data;
            cur = cur->next;
            ++i;
        }
        return nullptr;
    }
};
```