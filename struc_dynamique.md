# Structures de Données Dynamiques en C++

Les structures de données dynamiques en C++ permettent de gérer des collections de données dont la taille ou la structure peut changer dynamiquement pendant l'exécution. Voici une vue d'ensemble des structures principales en C++.

---

## 1. Listes Chaînées

Une liste chaînée est une collection de nœuds, où chaque nœud contient une valeur et un pointeur vers le nœud suivant (pour une liste simplement chaînée) ou vers le nœud précédent et suivant (pour une liste doublement chaînée).

### a. Liste Chaînée Simple

Dans une liste chaînée simple, chaque nœud contient deux parties : la valeur (ou donnée) et un pointeur vers le nœud suivant.

### Code d'implémentation

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next; // Pointeur vers le prochain nœud
    
    Node(int val) : data(val), next(nullptr) {}
};

class LinkedList {
private:
    Node* head;

public:
    LinkedList() : head(nullptr) {}

    void insertAtHead(int val) {
        Node* newNode = new Node(val);
        newNode->next = head;
        head = newNode;
    }

    void insertAtTail(int val) {
        Node* newNode = new Node(val);
        if (!head) {
            head = newNode;
            return;
        }
        Node* temp = head;
        while (temp->next) temp = temp->next;
        temp->next = newNode;
    }

    void display() {
        Node* temp = head;
        while (temp) {
            cout << temp->data << " -> ";
            temp = temp->next;
        }
        cout << "NULL" << endl;
    }

    void deleteNode(int val) {
        if (!head) return;
        if (head->data == val) {
            Node* toDelete = head;
            head = head->next;
            delete toDelete;
            return;
        }
        Node* temp = head;
        while (temp->next && temp->next->data != val) temp = temp->next;
        if (temp->next) {
            Node* toDelete = temp->next;
            temp->next = temp->next->next;
            delete toDelete;
        }
    }

    ~LinkedList() {
        while (head) {
            Node* temp = head;
            head = head->next;
            delete temp;
        }
    }
};

int main() {
    LinkedList list;
    list.insertAtHead(3);
    list.insertAtTail(5);
    list.insertAtTail(10);
    list.display();

    list.deleteNode(5);
    list.display();

    return 0;
}
```

### b. Liste Doublement Chaînée

Chaque nœud de la liste contient deux pointeurs : un vers le nœud suivant et un autre vers le nœud précédent.

```cpp
struct DNode {
    int data;
    DNode* prev;
    DNode* next;
    
    DNode(int val) : data(val), prev(nullptr), next(nullptr) {}
};

class DoublyLinkedList {
private:
    DNode* head;

public:
    DoublyLinkedList() : head(nullptr) {}

    void insertAtHead(int val) {
        DNode* newNode = new DNode(val);
        if (head) {
            newNode->next = head;
            head->prev = newNode;
        }
        head = newNode;
    }

    void insertAtTail(int val) {
        DNode* newNode = new DNode(val);
        if (!head) {
            head = newNode;
            return;
        }
        DNode* temp = head;
        while (temp->next) temp = temp->next;
        temp->next = newNode;
        newNode->prev = temp;
    }

    void display() {
        DNode* temp = head;
        while (temp) {
            cout << temp->data << " <-> ";
            temp = temp->next;
        }
        cout << "NULL" << endl;
    }

    ~DoublyLinkedList() {
        while (head) {
            DNode* temp = head;
            head = head->next;
            delete temp;
        }
    }
};
```

---

## 2. Piles (Stack) et Files (Queue)

### a. Pile (Stack)

Une pile est une structure de données de type LIFO (Last In, First Out) où le dernier élément ajouté est le premier à être retiré.

```cpp
#include <stack>
stack<int> s;
s.push(10);
s.pop();
s.top();
```

### b. File (Queue)

Une file est une structure de données de type FIFO (First In, First Out) où le premier élément ajouté est le premier à être retiré.

```cpp
#include <queue>
queue<int> q;
q.push(10);
q.pop();
q.front();
```

---

## 3. Arbres Binaires

Un arbre binaire est une structure de données hiérarchique où chaque nœud a au maximum deux enfants (gauche et droite).

```cpp
struct TreeNode {
    int data;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int val) : data(val), left(nullptr), right(nullptr) {}
};

class BinaryTree {
public:
    TreeNode* root;

    BinaryTree() : root(nullptr) {}

    void insert(TreeNode*& node, int val) {
        if (!node) {
            node = new TreeNode(val);
            return;
        }
        if (val < node->data)
            insert(node->left, val);
        else
            insert(node->right, val);
    }

    void display(TreeNode* node) {
        if (!node) return;
        display(node->left);
        cout << node->data << " ";
        display(node->right);
    }
};

int main() {
    BinaryTree tree;
    tree.insert(tree.root, 50);
    tree.insert(tree.root, 30);
    tree.insert(tree.root, 70);
    tree.insert(tree.root, 20);
    tree.insert(tree.root, 40);

    cout << "In-order traversal: ";
    tree.inOrderDisplay(tree.root);
    cout << endl;

    return 0;
}
```

---

## 4. Table de Hachage (Hash Table)

Une table de hachage est une structure de données qui permet un accès rapide aux données grâce à une fonction de hachage.

```cpp
#include <vector>
#include <list>

class HashTable {
private:
    vector<list<int>> table;
    int size;

    int hashFunction(int key) {
        return key % size;
    }

public:
    HashTable(int s) : size(s) {
        table.resize(size);
    }

    void insert(int key) {
        int index = hashFunction(key);
        table[index].push_back(key);
    }

    bool search(int key) {
        int index = hashFunction(key);
        for (int element : table[index])
            if (element == key) return true;
        return false;
    }

    void display() {
        for (int i = 0; i < size; ++i) {
            cout << "Bucket " << i << ": ";
            for (int element : table[i])
                cout << element << " ";
            cout << endl;
        }
    }
};

int main() {
    HashTable ht(7);
    ht.insert(10);
    ht.insert(20);
    ht.insert(15);
    ht.insert(7);

    cout << "Hash Table:" << endl;
    ht.display();

    cout << "Search 10: " << (ht.search(10) ? "Found" : "Not Found") << endl;

    return 0;
}
```

---

## 5. Vecteurs

Les vecteurs sont des structures dynamiques en C++ fournies par la bibliothèque `<vector>`. Ils permettent de gérer des collections de données de taille variable et offrent un accès rapide par index.

### Caractéristiques

- **Taille dynamique**
- **Accès indexé**
- **Gestion automatique de la mémoire**

### Exemple de base

```cpp
#include <vector>
vector<int> vec;
vec.push_back(10);
vec.insert(vec.begin(), 5);
vec.pop_back();
vec.erase(vec.begin());
vec.clear();
vec.reserve(100);
vec.shrink_to_fit();
```

### Opérations de Base

- **Ajout :** `push_back(valeur)`, `insert(position, valeur)`
- **Suppression :** `pop_back()`, `erase(position)`, `clear()`
- **Accès :** `operator[]`, `at(index)`, `front()`, `back()`

### Itérateurs

Les vecteurs supportent les itérateurs pour parcourir les éléments.

```cpp
vector<int> vec = {1, 2, 3, 4};
for (auto it = vec.begin(); it != vec.end(); ++it) {
    cout << *it << " ";
}
```

### Avantages et Inconvénients des Vecteurs

**Avantages**
- Flexibilité de taille
- Accès rapide par index
- Gestion automatique de la mémoire

**Inconvénients**
- Réallocation coûteuse en cas de dépassement de capacité
- Performance moins bonne pour insertion et suppression au milieu
