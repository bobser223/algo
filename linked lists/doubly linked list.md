
__Двобічно зв’язаний (двозв’язний) список__ – динамічна структура даних, що
складається з елементів одного типу, зв’язаних між собою у строго визначеному
порядку. При цьому _визначено перший та останній елементи у списку, а кожен
елемент списку вказує на наступний і попередній елементи у списку_. Перший
елемент має попереднім елементом посилання на невизначений елемент `nullptr`.
Аналогічно, `nullptr` буде наступним елементом для останнього елементу списку.

### Basic operations

1. Створити список.

2. Визначення, чи порожній список.

3. Зробити поточними перший елемент списку.

4. Зробити поточними останній елемент списку.

5. Перейти до наступного елемента.

6. Перейти до попереднього елемента.

7. Отримати поточний елемент.

8. Вставити новий елемент перед поточним.

9. Вставити новий елемент після поточного.

10. Видалити поточний елемент.


```c++
#include <iostream>
#include <stdexcept>

class DoublyLinkedList {
private:
    struct Node {
        int data;
        Node* prev;
        Node* next;
        Node(int v) : data(v), prev(nullptr), next(nullptr) {}
    };

    Node* head;
    Node* tail;
    int count;

    // Допоміжний: повернути вказівник на вузол з індексом i
    Node* nodeAt(int i) const {
        if (i < 0 || i >= count)
            throw std::out_of_range("Index out of range");
        Node* cur;
        if (i < count/2) {
            cur = head;
            for (int k = 0; k < i; ++k) cur = cur->next;
        } else {
            cur = tail;
            for (int k = count-1; k > i; --k) cur = cur->prev;
        }
        return cur;
    }

public:
    DoublyLinkedList()
        : head(nullptr), tail(nullptr), count(0) {}

    ~DoublyLinkedList() {
        clear();
    }

    int size() const {
        return count;
    }

    bool empty() const {
        return count == 0;
    }

    void push_front(int v) {
        Node* node = new Node(v);
        node->next = head;
        if (head) head->prev = node;
        head = node;
        if (!tail) tail = node;
        ++count;
    }

    void push_back(int v) {
        Node* node = new Node(v);
        node->prev = tail;
        if (tail) tail->next = node;
        tail = node;
        if (!head) head = node;
        ++count;
    }

    int pop_front() {
        if (empty()) throw std::out_of_range("List is empty");
        Node* node = head;
        int val = node->data;
        head = head->next;
        if (head) head->prev = nullptr;
        else      tail = nullptr;
        delete node;
        --count;
        return val;
    }

    int pop_back() {
        if (empty()) throw std::out_of_range("List is empty");
        Node* node = tail;
        int val = node->data;
        tail = tail->prev;
        if (tail) tail->next = nullptr;
        else      head = nullptr;
        delete node;
        --count;
        return val;
    }

    // Вставити v на позицію i (0-based)
    void insertAt(int i, int v) {
        if (i == 0) {
            push_front(v);
        } else if (i == count) {
            push_back(v);
        } else {
            Node* nextNode = nodeAt(i);
            Node* prevNode = nextNode->prev;
            Node* node = new Node(v);
            node->prev = prevNode;
            node->next = nextNode;
            prevNode->next = node;
            nextNode->prev = node;
            ++count;
        }
    }

    // Видалити i-тий елемент, повернути його значення
    int removeAt(int i) {
        if (i == 0) return pop_front();
        if (i == count-1) return pop_back();
        Node* node = nodeAt(i);
        int val = node->data;
        Node* p = node->prev;
        Node* n = node->next;
        p->next = n;
        n->prev = p;
        delete node;
        --count;
        return val;
    }

    // Отримати значення i-ого елемента
    int getAt(int i) const {
        return nodeAt(i)->data;
    }

    // Замінити значення i-ого елемента на v
    void setAt(int i, int v) {
        nodeAt(i)->data = v;
    }

    void clear() {
        Node* cur = head;
        while (cur) {
            Node* tmp = cur;
            cur = cur->next;
            delete tmp;
        }
        head = tail = nullptr;
        count = 0;
    }

    // Для відладки
    void print_forward() const {
        for (Node* cur = head; cur; cur = cur->next)
            std::cout << cur->data << ' ';
        std::cout << '\n';
    }

    void print_reverse() const {
        for (Node* cur = tail; cur; cur = cur->prev)
            std::cout << cur->data << ' ';
        std::cout << '\n';
    }
};
```