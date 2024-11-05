# Cours sur les Pointeurs en C++

## 1. Qu'est-ce qu'un pointeur ?

Un pointeur est une variable qui contient l'adresse mémoire d'une autre variable. Cette adresse est un numéro unique représentant un emplacement en mémoire où la variable est stockée. Au lieu de stocker directement une valeur (comme un entier ou un caractère), un pointeur contient une adresse. 

Les pointeurs permettent de :
- **Passer des variables par référence** : Transmettre directement l'adresse d'une variable à une fonction, permettant de modifier la variable d'origine.
- **Manipuler des structures de données dynamiques** : Utilisés pour les listes chaînées, arbres, graphes.
- **Optimiser la gestion de la mémoire** : Contrôle précis de l’utilisation de la mémoire.

---

## 2. Déclaration et initialisation des pointeurs

Pour déclarer un pointeur, on utilise `*` après le type de la variable qu’il doit pointer.

```cpp
int* p1;     // Pointeur vers un entier
double* p2;  // Pointeur vers un double
char* p3;    // Pointeur vers un char

Un pointeur doit être initialisé avec l’adresse d’une variable, sinon il contiendra une adresse aléatoire, causant un comportement indéfini.


---

3. L'opérateur & (adresse de)

L'opérateur & renvoie l’adresse mémoire d’une variable.

int x = 5;
int* p = &x; // 'p' stocke l'adresse de 'x'


---

4. L'opérateur * (déréférencement)

L'opérateur * devant un pointeur permet d'accéder à la valeur stockée à l’adresse pointée.

int x = 5;
int* p = &x;
cout << *p << endl; // Affiche la valeur de 'x', donc 5


---

5. Null Pointers (Pointeurs nuls)

Un pointeur nul ne pointe vers aucune adresse valide. Utilisez nullptr pour indiquer qu'un pointeur est nul.

int* p = nullptr; // 'p' est initialisé à nullptr


---

6. Types de pointeurs

a) Pointeurs constants

Un pointeur constant ne peut pas changer l’adresse vers laquelle il pointe.

int x = 10;
int y = 20;
int* const p = &x; // 'p' est un pointeur constant
p = &y; // Erreur

b) Pointeurs vers des constantes

Un pointeur vers une constante peut changer d’adresse mais ne peut modifier la valeur pointée.

const int x = 10;
const int* p = &x; // 'p' est un pointeur vers une constante
*p = 20; // Erreur

c) Pointeur constant vers une constante

Ce pointeur ne peut ni changer d’adresse ni modifier la valeur pointée.

const int x = 10;
const int* const p = &x; // 'p' est un pointeur constant vers une constante


---

7. Pointeurs et tableaux

Le nom d’un tableau est un pointeur vers son premier élément. On peut utiliser l’arithmétique des pointeurs pour parcourir le tableau.

int arr[] = {10, 20, 30};
int* p = arr; // 'p' pointe vers le premier élément de 'arr'

cout << *p << endl;       // Affiche 10
cout << *(p + 1) << endl;  // Affiche 20
cout << *(p + 2) << endl;  // Affiche 30


---

8. Pointeurs et fonctions

Les pointeurs permettent de passer des arguments par référence aux fonctions.

void increment(int* p) {
    (*p)++;
}

int main() {
    int x = 5;
    increment(&x); // Passe l'adresse de x
    cout << x << endl; // Affiche 6
    return 0;
}


---

9. Allocation dynamique de mémoire

Utilisez new pour allouer de la mémoire dynamiquement et delete pour la libérer.

int* p = new int;   // Alloue dynamiquement un entier
*p = 10;

delete p;           // Libère la mémoire allouée

// Allocation dynamique d'un tableau
int* arr = new int[5];
arr[0] = 1;
arr[1] = 2;

delete[] arr;       // Libère la mémoire du tableau


---

10. Pointeurs intelligents (Smart Pointers)

Les pointeurs intelligents (<memory>) gèrent automatiquement la mémoire et évitent les erreurs de gestion de mémoire. Il en existe trois principaux :

a) std::unique_ptr

Pointeur exclusif, gérant une seule ressource qui est libérée automatiquement lorsqu'il est détruit.

std::unique_ptr<int> p = std::make_unique<int>(10);
cout << *p << endl; // Affiche 10

Utilisez unique_ptr pour les objets qui ne seront partagés par aucune autre instance. On ne peut pas copier un unique_ptr, mais on peut le transférer avec std::move.

b) std::shared_ptr

Pointeur partagé, plusieurs shared_ptr peuvent partager la même ressource, libérée quand le dernier shared_ptr est détruit.

std::shared_ptr<int> p1 = std::make_shared<int>(10);
std::shared_ptr<int> p2 = p1; // p2 partage la même ressource que p1

Le shared_ptr utilise un compteur de référence pour suivre combien d'instances partagent la ressource.

c) std::weak_ptr

Pointeur qui fonctionne avec shared_ptr pour référencer un objet sans en modifier le compteur de référence. Utile pour éviter les cycles de référence.

std::shared_ptr<int> p1 = std::make_shared<int>(10);
std::weak_ptr<int> wp = p1; // Référence sans compter


---

11. Erreurs courantes avec les pointeurs

Déréférencement d’un pointeur nul : Accéder à un pointeur nul (nullptr) provoque un crash.

Fuites de mémoire : Oublier de libérer la mémoire allouée dynamiquement.

Déréférencement d'un pointeur non initialisé : Utiliser un pointeur sans assignation initiale.

Pointeur pendu (dangling pointer) : Un pointeur qui pointe vers une mémoire libérée, causant un comportement indéfini.
