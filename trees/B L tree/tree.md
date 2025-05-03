Нижче наведено виправлений та доповнений код червоно-чорного дерева з детальними багаторядковими коментарями **українською** (як ти і просив). У ньому:

1. **Виправлена логіка порівняння** – тепер при вставці, пошуку та видаленні ми орієнтуємось на `key`, а не на `value`.
2. **Реалізовані функції `leftRotate` та `rightRotate`** (повороти), які є ключовими у збалансуванні дерева.
3. **Виправлені помилки у логіці пошуку, видалення та балансування після видалення**.
4. **Додані багаторядкові коментарі українською** – з загальним поясненням логіки алгоритму у найкритичніших ділянках коду.

```cpp
//
// Created by Volodymyr Avvakumov on 15.03.2025.
//

#ifndef ALG_D_SRUCT_BLACK_RED_TREE_H
#define ALG_D_SRUCT_BLACK_RED_TREE_H

#define BLACK false
#define RED true

#endif //ALG_D_SRUCT_BLACK_RED_TREE_H

#include <iostream>

/*
 * ЧЕРВОНО-ЧОРНЕ ДЕРЕВО:
 *  - Кожна вершина або червона, або чорна.
 *  - Корінь дерева завжди чорний.
 *  - Усі NIL-вказівники (тобто відсутні діти) вважаються чорними вершинами.
 *  - Якщо вершина червона, тоді її батько обов'язково чорний.
 *  - Для кожної вершини всі шляхи від неї до нащадків-NIL містять однакову кількість чорних вершин.
 *
 * Ці умови дозволяють тримати дерево максимально збалансованим (за висотою),
 * забезпечуючи вставку, пошук і видалення в середньому за O(log n).
 */

struct Node {
    int key;
    int value;

    Node* parent;
    Node* left;
    Node* right;

    bool color;

    Node() {
        color = BLACK;
        left = nullptr;
        right = nullptr;
        parent = nullptr;
        key = 0;
        value = 0;
    }

    Node(int key, int value) {
        this->key = key;
        this->value = value;
        color = RED;
        left = nullptr;
        right = nullptr;
        parent = nullptr;
    }
};

// Спеціальний "ніби-порожній" вузол (NIL). Він використовується як сторож (sentinel).
// Будь-яке звертання до .left / .right, коли їх "немає", насправді буде вказувати на NIL.
Node* NIL = new Node{};

class Tree {
private:
    Node* root;

    /*
     * Ініціалізуємо новий вузол:
     *  - ключ і значення
     *  - колір за замовчуванням червоний (RED)
     *  - усі покажчики вказують на NIL (щоб вузол був коректно інтегрований)
     */
    static void initNode(Node* node, int key, int value){
        node->key = key;
        node->value = value;
        node->left = NIL;
        node->right = NIL;
        node->parent = NIL;
        node->color = RED;
    }

    // Перевіряємо, що вузол не є NIL.
    static bool exists(Node* n){
        return n != NIL;
    }

    /*
     * Лівий поворот:
     *   Здійснюється довкола вузла x, де y = x->right.
     *   Ми робимо "підйом" y на місце x, а x "опускаємо" вліво від y.
     *
     *   Передумова: x->right != NIL
     *
     *         x               y
     *          \             / \
     *           y     ->    x   ?
     *          /             \
     *         ?               ?
     *
     * Великі шматки робимо докладно:
     *  1) Зв’язок x->right = y->left (бо ліва гілка y "переноситься" вправо від x).
     *  2) Якщо y->left існує, оновлюємо її батька (-> x).
     *  3) Зв’язок y->parent = x->parent (y тепер займає місце x у дереві).
     *  4) Оновлюємо вказівники у батька x, щоб він замість x тепер посилався на y.
     *  5) Робимо x "лівим сином" y.
     *  6) Зв’язок x->parent = y.
     */
    void leftRotate(Node* x) {
        Node* y = x->right; 
        x->right = y->left;  
        if (y->left != NIL) {
            y->left->parent = x;
        }
        y->parent = x->parent;  
        if (x->parent == NIL) {
            root = y;
        } else if (x == x->parent->left) {
            x->parent->left = y;
        } else {
            x->parent->right = y;
        }
        y->left = x;
        x->parent = y;
    }

    /*
     * Правий поворот (дзеркальний варіант лівого):
     *   Здійснюється довкола вузла x, де y = x->left.
     *   Ми робимо "підйом" y на місце x, а x "опускаємо" вправо від y.
     *
     *   Передумова: x->left != NIL
     *
     *         x              y
     *        /              / \
     *       y       ->     ?   x
     *        \                /
     *         ?              ?
     *
     * Аналогічна логіка: 
     *  1) Перенаправляємо x->left на y->right.
     *  2) Якщо y->right існує, оновлюємо її батька на x.
     *  3) y->parent = x->parent (y стає на місце x).
     *  4) Вказівник батька x тепер вказує на y замість x.
     *  5) y->right = x
     *  6) x->parent = y
     */
    void rightRotate(Node* x) {
        Node* y = x->left;
        x->left = y->right;
        if (y->right != NIL) {
            y->right->parent = x;
        }
        y->parent = x->parent;
        if (x->parent == NIL) {
            root = y;
        } else if (x == x->parent->right) {
            x->parent->right = y;
        } else {
            x->parent->left = y;
        }
        y->right = x;
        x->parent = y;
    }

    /*
     * Функція balance:
     *  - Викликається після вставки нового червоного вузла.
     *  - Виправляє можливі порушення (батько та дідусь можуть мати проблеми, якщо батько був червоним).
     *
     * Основні випадки (саме тут червоно-чорне дерево "виправляє" свій колір та структуру):
     *   1) Якщо дядько вузла (uncle) червоний – ми просто перекфарбовуємо батька, дядька у чорний, 
     *      а дідуся у червоний і рухаємось догори (new_node = grandparent).
     *   2) Якщо дядько чорний, а новий вузол – "правий син" (чи "лівий син" у правій гілці), 
     *      робимо поворот та перестановку кольорів (залежно від конкретного випадку).
     */
    void balance(Node* new_node){
        Node* uncle;
        // Поки батько "нового" вузла теж червоний, є ризик порушення правил
        while (new_node->parent->color == RED) {
            // Якщо батько є "лівим сином" дідуся
            if (new_node->parent == new_node->parent->parent->left){
                uncle = new_node->parent->parent->right;
                // 1) Дядько червоний: перекрашуємо і рухаємось угору
                if (uncle->color == RED){
                    new_node->parent->color = BLACK;
                    uncle->color = BLACK;
                    new_node->parent->parent->color = RED;
                    new_node = new_node->parent->parent;
                } 
                // 2) Дядько чорний: можливо, потрібні повороти
                else {
                    // Якщо новий вузол – правий син (лівий випадок)
                    if (new_node == new_node->parent->right) {
                        new_node = new_node->parent;
                        leftRotate(new_node);
                    }
                    // Змінюємо колір батька і дідуся
                    new_node->parent->color = BLACK;
                    new_node->parent->parent->color = RED;
                    rightRotate(new_node->parent->parent);
                }
            }
            // Дзеркальний випадок, якщо батько є "правим сином"
            else {
                uncle = new_node->parent->parent->left;
                if (uncle->color == RED) {
                    new_node->parent->color = BLACK;
                    uncle->color = BLACK;
                    new_node->parent->parent->color = RED;
                    new_node = new_node->parent->parent;
                } else {
                    if (new_node == new_node->parent->left){
                        new_node = new_node->parent;
                        rightRotate(new_node);
                    }
                    new_node->parent->color = BLACK;
                    new_node->parent->parent->color = RED;
                    leftRotate(new_node->parent->parent);
                }
            }
            // Якщо дістались кореня, зупиняємось
            if (new_node == root) {
                break;
            }
        }
        // Корінь завжди має бути чорним
        root->color = BLACK;
    }

    // Рахує кількість дітей (не NIL) у вузла.
    static int childrenCount(Node* node){
        int count = 0;
        if (exists(node->left)) count++;
        if (exists(node->right)) count++;
        return count;
    }

    // Пошук вузла за ключем (повертає вказівник на вузол або NIL).
    Node* searchKey(int key){
        Node* curr = root;
        while (curr != NIL) {
            if (curr->key == key) {
                return curr;
            }
            if (key < curr->key) {
                curr = curr->left;
            } else {
                curr = curr->right;
            }
        }
        return NIL;
    }

    // Повертає мінімальний вузол у піддереві (найлівіший вузол).
    Node* Min(Node* node) {
        while (node->left != NIL) {
            node = node->left;
        }
        return node;
    }

    // Повертає єдину дитину вузла, якщо вона існує, або NIL (якщо у вузла менше ніж 2 дітей).
    static Node* childOrMock(Node* node){
        return (exists(node->left) ? node->left : node->right);
    }

    /*
     * Функція "пересаджування" вузлів:
     *  - Заміщує вузол to_node вузлом from_node, під'єднуючи from_node до батька to_node.
     *  - Використовується при видаленні (коли вирізаємо вузол з дерева).
     */
    void transplantNode(Node* to_node, Node* from_node){
        if (to_node->parent == NIL) {
            root = from_node;
        }
        else if (to_node == to_node->parent->left) {
            to_node->parent->left = from_node;
        }
        else {
            to_node->parent->right = from_node;
        }
        from_node->parent = to_node->parent;
    }

    /*
     * Відновлюємо баланс після видалення чорного вузла:
     *  - Якщо видаляється чорний вузол, може бути порушена умова, що "всі шляхи 
     *    від вершини до NIL містять однакове число чорних вузлів".
     *  - Ця функція "підтягує" чорний колір вгору і робить повороти/перефарбування
     *    для відновлення коректних умов.
     *
     * Типові кроки:
     *   1) Перевіряємо "брата" (sibling) поточного вузла.
     *   2) Якщо брат червоний, робимо поворот і перекрашуємо, щоб отримати чорного брата.
     *   3) Якщо брат і його діти чорні, перекрашуємо брата у червоний і підіймаємось до батька.
     *   4) Інакше робимо ротації для виправлення ситуації (залежно від того, яка дитина брата чорна/червона).
     */
    void fixBalanceAfterDeleting(Node* node){
        while (node != root && node->color == BLACK) {
            if (node == node->parent->left) {
                Node* brother = node->parent->right;

                // Випадок 1: брат червоний -> робимо поворот, щоб отримати чорного брата
                if (brother->color == RED) {
                    brother->color = BLACK;
                    node->parent->color = RED;
                    leftRotate(node->parent);
                    brother = node->parent->right;
                }

                // Випадок 2: обидва діти брата чорні
                if (brother->left->color == BLACK && brother->right->color == BLACK) {
                    brother->color = RED;
                    node = node->parent;
                } 
                else {
                    // Випадок 3: у брата ліва дитина червона, а права – чорна
                    if (brother->right->color == BLACK) {
                        brother->left->color = BLACK;
                        brother->color = RED;
                        rightRotate(brother);
                        brother = node->parent->right;
                    }
                    // Випадок 4: у брата права дитина червона
                    brother->color = node->parent->color;
                    node->parent->color = BLACK;
                    brother->right->color = BLACK;
                    leftRotate(node->parent);
                    node = root;
                }
            }
            // Дзеркальний випадок
            else {
                Node* brother = node->parent->left;
                if (brother->color == RED) {
                    brother->color = BLACK;
                    node->parent->color = RED;
                    rightRotate(node->parent);
                    brother = node->parent->left;
                }
                if (brother->right->color == BLACK && brother->left->color == BLACK) {
                    brother->color = RED;
                    node = node->parent;
                }
                else {
                    if (brother->left->color == BLACK) {
                        brother->right->color = BLACK;
                        brother->color = RED;
                        leftRotate(brother);
                        brother = node->parent->left;
                    }
                    brother->color = node->parent->color;
                    node->parent->color = BLACK;
                    brother->left->color = BLACK;
                    rightRotate(node->parent);
                    node = root;
                }
            }
        }
        node->color = BLACK;
    }

public:
    Tree(){
        // Ініціалізуємо корінь в NIL (порожнє дерево).
        // Це важливо для коректної роботи "balance" та інших функцій.
        root = NIL;
    }

    /*
     * INSERT (вставка нового вузла за заданим key, value).
     *  1) Звичайна бінарна вставка (шукаємо місце в дереві).
     *  2) Створюємо червоний вузол (new_node).
     *  3) Викликаємо balance(new_node), щоб, у разі порушень, дерево відновилося.
     */
    void insert(int key, int value){
        Node* curr_node = root;
        Node* parent = NIL;

        // Рухаємось деревом, шукаючи, куди приєднати новий вузол
        while (exists(curr_node)) {
            parent = curr_node;
            if (key < curr_node->key) {
                curr_node = curr_node->left;
            } else {
                curr_node = curr_node->right;
            }
        }

        // Створюємо новий вузол
        Node* new_node = new Node;
        initNode(new_node, key, value);
        new_node->parent = parent;

        // Якщо дерево було порожнім
        if (parent == NIL) {
            root = new_node;
        }
        else if (key < parent->key) {
            parent->left = new_node;
        }
        else {
            parent->right = new_node;
        }

        // Виклик балансу (можливо, ми порушили забарвлення)
        balance(new_node);
    }

    /*
     * POP (видалення з дерева за ключем).
     *   1) Шукаємо вузол за ключем.
     *   2) Якщо не знайдено – завершуємо.
     *   3) Якщо у вузла менше 2 дітей – спрощений випадок (трансплантація).
     *   4) Інакше шукаємо мінімум у правому піддереві (min_node), переносимо його key/value 
     *      у вузол, який видаляємо, і видаляємо min_node.
     *   5) Якщо видаляємо чорний вузол, треба викликати fixBalanceAfterDeleting.
     */
    void pop(int key){
        Node* node_to_delete = searchKey(key);
        if (!exists(node_to_delete)) return;  // Нічого не робимо, якщо не знайшли

        Node* temp = node_to_delete;
        bool originalColor = temp->color;
        Node* child;

        // Випадок: вузол має < 2 дітей
        if (childrenCount(node_to_delete) < 2) {
            child = childOrMock(node_to_delete);
            transplantNode(node_to_delete, child);
        }
        // Інакше шукаємо мінімум у правому піддереві
        else {
            Node* min_node = Min(node_to_delete->right);
            temp = min_node;
            originalColor = temp->color;

            // "Фізично" переносимо ключ і значення
            node_to_delete->key = min_node->key;
            node_to_delete->value = min_node->value;

            child = childOrMock(min_node);
            transplantNode(min_node, child);
        }

        // Якщо видаляли чорний вузол, виправляємо баланс
        if (originalColor == BLACK){
            fixBalanceAfterDeleting(child);
        }
    }
};
```
---
```c++
#include <iostream>

#define BLACK false
#define RED true

/*
 * Наша глобальна змінна NIL – "порожній" (sentinel) вузол,
 * що використовується замість звичайних nullptr. Він вважається ЧОРНИМ.
 */
struct Node {
    int key;
    bool color;

    Node* parent;
    Node* left;
    Node* right;

    Node() {
        key = 0;
        color = BLACK;
        parent = nullptr;
        left = nullptr;
        right = nullptr;
    }

    Node(int key) {
        this->key = key;
        color = RED;
        parent = nullptr;
        left = nullptr;
        right = nullptr;
    }
};

Node* NIL = new Node();  // Ініціалізуємо глобальний NIL-вузол (за замовчуванням чорний)

class RBTree {
private:
    Node* root;

    // Допоміжна функція ініціалізації нового вузла
    static void initNode(Node* node, int key){
        node->key = key;
        node->color = RED;  // Новий вузол спершу червоний
        node->parent = NIL;
        node->left = NIL;
        node->right = NIL;
    }

    // Перевірка, чи вузол не NIL
    static bool exists(Node* n) {
        return n != NIL;
    }

    /*
     * Лівий поворот: "піднімаємо" правого нащадка x угору.
     *    x             y
     *     \           / \
     *      y   ->    x   ?
     *     /
     *    ?
     */
    void leftRotate(Node* x) {
        Node* y = x->right;
        x->right = y->left;
        if (y->left != NIL) {
            y->left->parent = x;
        }
        y->parent = x->parent;
        if (x->parent == NIL) {
            root = y;
        } else if (x == x->parent->left) {
            x->parent->left = y;
        } else {
            x->parent->right = y;
        }
        y->left = x;
        x->parent = y;
    }

    /*
     * Правий поворот: "піднімаємо" лівого нащадка x угору.
     *      x          y
     *     /          / \
     *    y    ->    ?   x
     *     \
     *      ?
     */
    void rightRotate(Node* x) {
        Node* y = x->left;
        x->left = y->right;
        if (y->right != NIL) {
            y->right->parent = x;
        }
        y->parent = x->parent;
        if (x->parent == NIL) {
            root = y;
        } else if (x == x->parent->right) {
            x->parent->right = y;
        } else {
            x->parent->left = y;
        }
        y->right = x;
        x->parent = y;
    }

    /*
     * Після вставки нового червоного вузла викликаємо balance(new_node),
     * щоб виправити можливі порушення червоно-чорного балансу.
     */
    void balance(Node* new_node) {
        Node* uncle;
        while (new_node->parent->color == RED) {
            if (new_node->parent == new_node->parent->parent->left) {
                uncle = new_node->parent->parent->right;
                if (uncle->color == RED) {
                    // Випадок 1: дядько червоний -> перекрашуємо і рухаємось вище
                    new_node->parent->color = BLACK;
                    uncle->color = BLACK;
                    new_node->parent->parent->color = RED;
                    new_node = new_node->parent->parent;
                } else {
                    // Випадок 2: дядько чорний
                    if (new_node == new_node->parent->right) {
                        new_node = new_node->parent;
                        leftRotate(new_node);
                    }
                    new_node->parent->color = BLACK;
                    new_node->parent->parent->color = RED;
                    rightRotate(new_node->parent->parent);
                }
            } else {
                // Дзеркальний випадок
                uncle = new_node->parent->parent->left;
                if (uncle->color == RED) {
                    new_node->parent->color = BLACK;
                    uncle->color = BLACK;
                    new_node->parent->parent->color = RED;
                    new_node = new_node->parent->parent;
                } else {
                    if (new_node == new_node->parent->left) {
                        new_node = new_node->parent;
                        rightRotate(new_node);
                    }
                    new_node->parent->color = BLACK;
                    new_node->parent->parent->color = RED;
                    leftRotate(new_node->parent->parent);
                }
            }
            if (new_node == root) {
                break;
            }
        }
        root->color = BLACK;
    }

    // Повертає вказівник на вузол за ключем (якщо є) або NIL.
    Node* searchKey(int key) {
        Node* curr = root;
        while (curr != NIL) {
            if (curr->key == key) {
                return curr;
            } else if (key < curr->key) {
                curr = curr->left;
            } else {
                curr = curr->right;
            }
        }
        return NIL;
    }

    // Повертає найлівіший (мінімальний) вузол у піддереві node.
    Node* Min(Node* node) {
        while (node->left != NIL) {
            node = node->left;
        }
        return node;
    }

    // Кількість (не NIL) дітей вузла.
    static int childrenCount(Node* node) {
        int cnt = 0;
        if (exists(node->left)) cnt++;
        if (exists(node->right)) cnt++;
        return cnt;
    }

    // Якщо у вузла 1 дитина або 0, повертаємо ту дитину (або NIL).
    static Node* childOrMock(Node* node) {
        return (exists(node->left) ? node->left : node->right);
    }

    /*
     * Функція пересаджування: замінює вузол to_node на вузол from_node
     * (коректно оновлюючи зв’язки з батьком).
     */
    void transplantNode(Node* to_node, Node* from_node) {
        if (to_node->parent == NIL) {
            root = from_node;
        } else if (to_node == to_node->parent->left) {
            to_node->parent->left = from_node;
        } else {
            to_node->parent->right = from_node;
        }
        from_node->parent = to_node->parent;
    }

    /*
     * Коли видаляємо чорний вузол, це може порушити "чорний баланс".
     * Функція fixBalanceAfterDeleting виправляє дерево за допомогою
     * поворотів та перекрашувань.
     */
    void fixBalanceAfterDeleting(Node* node) {
        while (node != root && node->color == BLACK) {
            if (node == node->parent->left) {
                Node* brother = node->parent->right;
                if (brother->color == RED) {
                    brother->color = BLACK;
                    node->parent->color = RED;
                    leftRotate(node->parent);
                    brother = node->parent->right;
                }
                if (brother->left->color == BLACK && brother->right->color == BLACK) {
                    brother->color = RED;
                    node = node->parent;
                } else {
                    if (brother->right->color == BLACK) {
                        brother->left->color = BLACK;
                        brother->color = RED;
                        rightRotate(brother);
                        brother = node->parent->right;
                    }
                    brother->color = node->parent->color;
                    node->parent->color = BLACK;
                    brother->right->color = BLACK;
                    leftRotate(node->parent);
                    node = root;
                }
            } else {
                // Дзеркальний випадок
                Node* brother = node->parent->left;
                if (brother->color == RED) {
                    brother->color = BLACK;
                    node->parent->color = RED;
                    rightRotate(node->parent);
                    brother = node->parent->left;
                }
                if (brother->left->color == BLACK && brother->right->color == BLACK) {
                    brother->color = RED;
                    node = node->parent;
                } else {
                    if (brother->left->color == BLACK) {
                        brother->right->color = BLACK;
                        brother->color = RED;
                        leftRotate(brother);
                        brother = node->parent->left;
                    }
                    brother->color = node->parent->color;
                    node->parent->color = BLACK;
                    brother->left->color = BLACK;
                    rightRotate(node->parent);
                    node = root;
                }
            }
        }
        node->color = BLACK;
    }

    /*
     * Рекурсивна функція для друку дерева "боком":
     *  - Спочатку рухаємось вправо (більші ключі).
     *  - Виводимо поточний вузол (з відступами).
     *  - Потім рухаємось вліво (менші ключі).
     * Параметр space керує кількістю відступів при виведенні.
     */
    void printTree(Node* node, int space) const {
        if (node == NIL) return;
        space += 5; // Збільшуємо відступ для кожного рівня
        // Спочатку виведемо праве піддерево
        printTree(node->right, space);

        // Тепер виводимо відступи і ключ (а також колір)
        std::cout << std::string(space, ' ')
                  << node->key << (node->color == RED ? " (R)" : " (B)") << "\n";

        // Виведемо ліве піддерево
        printTree(node->left, space);
    }

public:
    // Конструктор ініціалізуємо корінь, щоб він вказував на NIL
    RBTree() {
        root = NIL;
    }

    // Вставка нового вузла за ключем
    void insert(int key) {
        Node* curr = root;
        Node* parent = NIL;

        while (exists(curr)) {
            parent = curr;
            if (key < curr->key) {
                curr = curr->left;
            } else {
                curr = curr->right;
            }
        }

        Node* new_node = new Node;
        initNode(new_node, key);
        new_node->parent = parent;

        if (parent == NIL) {
            // Дерево було порожнім
            root = new_node;
        } else if (key < parent->key) {
            parent->left = new_node;
        } else {
            parent->right = new_node;
        }

        balance(new_node);
    }

    // Видалення вузла за ключем
    void pop(int key) {
        Node* node_to_delete = searchKey(key);
        if (!exists(node_to_delete)) return;

        Node* y = node_to_delete;
        bool originalColor = y->color;
        Node* x;

        // Якщо у вузла ≤ 1 дитини
        if (childrenCount(node_to_delete) < 2) {
            x = childOrMock(node_to_delete);
            transplantNode(node_to_delete, x);
        } else {
            // Шукаємо мінімальний вузол у правому піддереві
            Node* min_node = Min(node_to_delete->right);
            y = min_node;
            originalColor = y->color;

            // Переносимо ключ
            node_to_delete->key = y->key;

            x = childOrMock(y);
            transplantNode(y, x);
        }

        // Якщо видалили чорний вузол, треба відновити баланс
        if (originalColor == BLACK) {
            fixBalanceAfterDeleting(x);
        }
    }

    // Публічна обгортка для друку дерева
    void printTree() const {
        if (root == NIL) {
            std::cout << "Tree is empty.\n";
            return;
        }
        printTree(root, 0);
        std::cout << std::endl;
    }
};

```
---

## Дуже детальний конспект для Obsidian

Нижче наведено **Markdown-конспект** з детальними поясненнями, які можна вставити до Obsidian. Він містить розділи з кодом та детальними коментарями, корисними для швидкого перегляду й розуміння алгоритму.

````markdown
# ЧЕРВОНО-ЧОРНЕ ДЕРЕВО

Основна ідея червоно-чорного дерева — **забезпечити баланс** у висоті шляхом дотримання певних правил забарвлення вузлів (червоний/чорний) і виконання поворотів при вставці та видаленні.  

> **Важливі інваріанти:**
> 1. Корінь завжди чорний.
> 2. Усі NIL-вказівники вважаються чорними вузлами.
> 3. Якщо вузол червоний, то його батько — чорний.
> 4. Кожен шлях від вершини до `NIL` має однакову кількість чорних вузлів.

## Клас Tree
Містить:
- `root` — вказівник на корінь дерева (чорного кольору або `NIL`, коли дерево порожнє).
- Статичний `NIL` — “порожній” вузол, який виступає як sentinel.

```cpp
class Tree {
private:
    Node* root;
    // ...
public:
    Tree(){
        root = NIL;
    }
    // ...
};
````

---

## Вставка (Insert)

1. **Пошук місця вставки**:
    
    ```cpp
    while (exists(curr_node)) {
        parent = curr_node;
        if (key < curr_node->key) {
            curr_node = curr_node->left;
        } else {
            curr_node = curr_node->right;
        }
    }
    ```
    
    Ми рухаємось від кореня `root` донизу, шукаючи належне місце. Порівнюємо `key` з поточним `curr_node->key`.
    
2. **Створення нового вузла**:
    
    ```cpp
    Node* new_node = new Node;
    initNode(new_node, key, value);
    new_node->parent = parent;
    ```
    
    Використовуємо допоміжну функцію `initNode`, яка встановлює `key`, `value`, робить колір `RED`, та вказівники на `NIL`.
    
3. **Прикріплення нового вузла**:
    
    ```cpp
    if (parent == NIL) {
        root = new_node;
    } else if (key < parent->key) {
        parent->left = new_node;
    } else {
        parent->right = new_node;
    }
    ```
    
4. **Балансування (balance)**:
    
    ```cpp
    balance(new_node);
    ```
    
    Викликаємо спеціальну функцію, що усуває можливі порушення правил червоно-чорного дерева (наприклад, якщо батько і новий вузол обидва червоні).
    

---

## Функція `balance(new_node)`

Більшість “магії” відбувається тут. Якщо батько червоний — порушення, адже червоний вузол не може мати червоного батька.

```cpp
while (new_node->parent->color == RED) {
    if (new_node->parent == grandparent->left) {
        uncle = grandparent->right;

        if (uncle->color == RED) {
            /* Перефарбування: батько, дядько -> чорні, дідусь -> червоний */
        } else {
            /* Можливі повороти */
        }
    } else {
        /* Дзеркальний випадок */
    }
}

root->color = BLACK;
```

---

## Повороти (`leftRotate` / `rightRotate`)

### Лівий поворот

```cpp
void leftRotate(Node* x) {
    Node* y = x->right;
    x->right = y->left;
    if (y->left != NIL) {
        y->left->parent = x;
    }
    y->parent = x->parent;
    if (x->parent == NIL) {
        root = y;
    } else if (x == x->parent->left) {
        x->parent->left = y;
    } else {
        x->parent->right = y;
    }
    y->left = x;
    x->parent = y;
}
```

> - **Крок 1:** `y = x->right` — виділяємо вузол, який “підніметься” вгору.
> - **Крок 2:** `x->right = y->left` — з’єднуємо праве піддерево `x` із лівим піддеревом `y`.
> - **Крок 3:** коригуємо батьківські вказівники.
> - **Крок 4:** робимо `y` батьком `x`.

### Правий поворот

```cpp
void rightRotate(Node* x) {
    Node* y = x->left;
    x->left = y->right;
    if (y->right != NIL) {
        y->right->parent = x;
    }
    y->parent = x->parent;
    if (x->parent == NIL) {
        root = y;
    } else if (x == x->parent->right) {
        x->parent->right = y;
    } else {
        x->parent->left = y;
    }
    y->right = x;
    x->parent = y;
}
```

> Аналогічно, тільки “дзеркально”.

---

## Видалення (pop)

Шукаємо вузол за `key`, якщо він існує — видаляємо.

```cpp
void pop(int key){
    Node* node_to_delete = searchKey(key);
    if (!exists(node_to_delete)) return;
    // ...
}
```

### Три основні випадки:

1. **Менше 2 дітей**
    
    ```cpp
    if (childrenCount(node_to_delete) < 2) {
        child = childOrMock(node_to_delete);
        transplantNode(node_to_delete, child);
    }
    ```
    
    Трансплантуємо єдиного сина (або `NIL`) на місце вузла, що видаляється.
    
2. **2 дітей**
    
    ```cpp
    else {
        Node* min_node = Min(node_to_delete->right);
        node_to_delete->key = min_node->key;
        node_to_delete->value = min_node->value;
        child = childOrMock(min_node);
        transplantNode(min_node, child);
    }
    ```
    
    Шукаємо найменший вузол у правому піддереві. Потім переносимо його вміст у `node_to_delete` і видаляємо власне `min_node`.
    
3. **Відновлення балансу**  
    Якщо видаляємо **чорний** вузол, викликаємо:
    
    ```cpp
    if (originalColor == BLACK) {
        fixBalanceAfterDeleting(child);
    }
    ```
    

---

## Функція `fixBalanceAfterDeleting(node)`

Якщо вузол був чорним, кількість чорних вузлів на шляху до `NIL` могла змінитися.

```cpp
while (node != root && node->color == BLACK) {
    if (node == node->parent->left) {
        brother = node->parent->right;
        if (brother->color == RED) {
            // Робимо брата чорним, батька червоним, поворот leftRotate
        }
        if (both children of brother are BLACK) {
            // Зміна кольору брата -> RED, перехід вище
        } else {
            // Робимо відповідні ротації (rightRotate / leftRotate) і перекрашування
        }
    } else {
        // Дзеркальний випадок
    }
}
node->color = BLACK;
```

---

## Заключення

Таким чином, **червоно-чорне дерево** — це **самобалансувальне** бінарне дерево пошуку. Завдяки поворотам і чергуванню кольорів воно зберігає логарифмічну висоту навіть у гіршому випадку.

З точки зору реалізації, ключові моменти — **коректна робота з `NIL`**, **зміни батьківських вказівників**, і **дотримання кольорових правил** після кожної операції (вставки або видалення).

```
Цим завершується короткий (але водночас детальний) конспект 
для Obsidian по реалізації червоно-чорного дерева на C++.
```