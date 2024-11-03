# Cours C++ : Programmation Orientée Objet – Jour 3

## Thème : Gestion de la mémoire et Managers d’instances

---

### Objectifs du jour :
- Comprendre la gestion de la mémoire en C++ (allocation dynamique, pointeurs, libération de la mémoire).
- Apprendre à implémenter un **Manager d'instances** pour gérer les objets dynamiques de manière centralisée.
- Explorer l'utilisation des **pointeurs intelligents** (`smart pointers`) pour éviter les fuites de mémoire.
- Apprendre à utiliser des structures comme les **Singletons** pour la gestion des instances uniques dans un programme.

---

## 1. Gestion de la Mémoire en C++

En C++, la gestion de la mémoire est essentielle pour éviter les fuites de mémoire et garantir l'efficacité du programme. L'allocation dynamique permet de réserver de la mémoire pour des objets qui ne sont pas alloués statiquement ou sur la pile.

### Allocation dynamique

L'opérateur `new` permet d'allouer de la mémoire dynamiquement sur le **tas** (heap), tandis que `delete` libère cette mémoire.

#### Exemple basique d'allocation dynamique :

```cpp
#include <iostream>

int main() {
    int* ptr = new int;  // Allocation dynamique d'un entier
    *ptr = 42;           // Affectation de la valeur

    std::cout << "Valeur de ptr: " << *ptr << std::endl;

    delete ptr;          // Libération de la mémoire
    return 0;
}
```

**Explication :**
- `new` alloue un bloc de mémoire et renvoie un pointeur vers ce bloc.
- L'opérateur `delete` libère la mémoire précédemment allouée avec `new`.

**Sortie :**
```
Valeur de ptr: 42
```

### Allocation dynamique d'un tableau :

```cpp
int* tableau = new int[5];  // Allocation dynamique d'un tableau de 5 entiers
```

N'oubliez pas de libérer la mémoire avec l'opérateur `delete[]` pour les tableaux dynamiques :

```cpp
delete[] tableau;  // Libération de la mémoire du tableau
```

---

## 2. Fuites de Mémoire et Gestion Automatique avec des Pointeurs Intelligents

L'un des problèmes les plus courants avec l'allocation dynamique est l'oubli de libérer la mémoire, ce qui entraîne des **fuites de mémoire**. Les **pointeurs intelligents** (`smart pointers`) aident à éviter cela en gérant automatiquement la libération de la mémoire lorsque le pointeur n'est plus utilisé.

### Types de Pointeurs Intelligents en C++ :

1. **`std::unique_ptr`** : 
   - Gère un objet à travers un seul pointeur. Lorsque le pointeur est détruit, la mémoire est automatiquement libérée.
   - Il ne peut pas être copié, seulement déplacé.

2. **`std::shared_ptr`** : 
   - Permet à plusieurs pointeurs de partager la propriété d'un même objet. Lorsque le dernier `shared_ptr` qui pointe vers l'objet est détruit, la mémoire est libérée.

3. **`std::weak_ptr`** :
   - Est utilisé pour éviter les références circulaires qui peuvent survenir avec `shared_ptr`. Il n'incrémente pas le compteur de références.

### Exemple avec `unique_ptr` :

```cpp
#include <iostream>
#include <memory>  // Nécessaire pour les pointeurs intelligents

int main() {
    std::unique_ptr<int> ptr = std::make_unique<int>(42);
    std::cout << "Valeur du pointeur unique: " << *ptr << std::endl;
    
    // Le pointeur sera automatiquement détruit en sortant de la portée
    return 0;
}
```

### Exemple avec `shared_ptr` :

```cpp
#include <iostream>
#include <memory>  // Nécessaire pour les pointeurs intelligents

int main() {
    std::shared_ptr<int> ptr1 = std::make_shared<int>(42);
    std::shared_ptr<int> ptr2 = ptr1;  // Partage la même instance

    std::cout << "Valeur de ptr1: " << *ptr1 << std::endl;
    std::cout << "Valeur de ptr2: " << *ptr2 << std::endl;

    std::cout << "Nombre de références: " << ptr1.use_count() << std::endl;  // Affiche 2

    return 0;
}
```

---

## 3. Manager d’Instances

Un **Manager d'instances** est un modèle de conception permettant de centraliser la création, la gestion et la destruction des instances d'objets dans un programme. Cela facilite la gestion de la mémoire et l'organisation du code.

### Exemple de Manager d'Instances pour une classe `Personnage` :

```cpp
#include <iostream>
#include <vector>

class Personnage {
public:
    std::string nom;

    Personnage(std::string n) : nom(n) {
        std::cout << nom << " créé." << std::endl;
    }

    ~Personnage() {
        std::cout << nom << " détruit." << std::endl;
    }
};

// Manager d'instances pour la classe Personnage
class PersonnageManager {
private:
    std::vector<Personnage*> personnages;  // Stocke les pointeurs vers les instances

public:
    ~PersonnageManager() {
        for (auto personnage : personnages) {
            delete personnage;  // Libère chaque personnage
        }
        personnages.clear();
    }

    Personnage* creerPersonnage(std::string nom) {
        Personnage* p = new Personnage(nom);
        personnages.push_back(p);  // Ajoute à la liste des instances
        return p;
    }

    void detruirePersonnage(Personnage* personnage) {
        auto it = std::find(personnages.begin(), personnages.end(), personnage);
        if (it != personnages.end()) {
            delete *it;  // Libère la mémoire
            personnages.erase(it);  // Supprime du vecteur
        }
    }
};

int main() {
    PersonnageManager manager;
    Personnage* p1 = manager.creerPersonnage("Guerrier");
    Personnage* p2 = manager.creerPersonnage("Mage");

    // On peut détruire les instances à tout moment
    manager.detruirePersonnage(p1);

    // Le destructeur du manager libérera les personnages restants
    return 0;
}
```

### Explication :
- Le **Manager d'instances** centralise la création et la destruction d'instances de `Personnage`.
- Lorsque le `PersonnageManager` est détruit (fin du programme), il libère toutes les instances de `Personnage` qu'il a créées.

---

## 4. Singleton

Un **Singleton** est une classe qui ne peut avoir qu'une seule instance dans tout le programme. Cela est utile lorsqu'une seule instance d'une classe doit exister, comme pour un gestionnaire de ressources ou un logger.

### Exemple de Singleton en C++ :

```cpp
#include <iostream>

class Logger {
private:
    static Logger* instance;
    Logger() {}  // Constructeur privé pour empêcher les instanciations directes

public:
    static Logger* getInstance() {
        if (instance == nullptr) {
            instance = new Logger();
        }
        return instance;
    }

    void log(std::string message) {
        std::cout << "Log: " << message << std::endl;
    }
};

// Initialisation du pointeur statique à nullptr
Logger* Logger::instance = nullptr;

int main() {
    // On récupère l'unique instance du Logger
    Logger* logger = Logger::getInstance();
    logger->log("Ceci est un message de log.");

    return 0;
}
```

### Explication :
- Le constructeur de `Logger` est privé, empêchant la création d'instances multiples.
- `getInstance()` renvoie toujours la même instance unique de `Logger`.
- C'est un bon moyen d'assurer qu'une classe n'ait qu'une seule instance active dans un programme.

---

## 5. Exercices Pratiques

### Exercice 1 : Manager d'objets
1. Créez une classe `Objet` avec un attribut `nom` et un constructeur qui prend un nom en paramètre.
2. Créez un **Manager d'instances** qui peut ajouter et supprimer des objets dynamiquement.
3. Testez le Manager en créant plusieurs objets et en les supprimant au besoin.

### Exercice 2 : Utilisation de `shared_ptr`
1. Créez une classe `Ressource` avec un attribut `nom`.
2. Créez un programme qui utilise des `shared_ptr` pour partager une instance de `Ressource` entre plusieurs variables.
3. Affichez le nombre de références à l'instance partagée à différents moments du programme.

### Exercice 3 : Singleton
1. Créez un Singleton `ConfigManager` qui gère la configuration d'une application (par exemple, le nom de l'application et sa version).
2. Assurez-vous que `ConfigManager` n'a qu'une seule instance dans tout le programme.

---

## Résumé du Jour 3

- **Gestion de la mémoire** : Utilisation de `new` et `delete` pour l'allocation et la libération dynamique de la mémoire.
- **Pointeurs intelligents** : Utilisation de `unique_ptr`
