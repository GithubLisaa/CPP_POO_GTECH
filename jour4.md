# Cours C++ : Programmation Orientée Objet – Jour 4

## Thème : Polymorphisme et Interfaces en C++

---

### Objectifs du jour
- Comprendre le concept de polymorphisme en programmation orientée objet.
- Savoir utiliser des fonctions virtuelles et des classes abstraites.
- Mettre en pratique le polymorphisme dans des scénarios concrets.
- Comprendre l’importance des interfaces et comment les implémenter en C++.

---

## 1. Introduction au Polymorphisme

En C++, le polymorphisme est un concept clé qui permet de manipuler des objets de différentes classes de manière uniforme. Le polymorphisme permet d'utiliser une interface commune pour des objets de types différents, offrant ainsi une flexibilité dans le code.

### Types de Polymorphisme
1. **Polymorphisme de compilation (ou surcharge)** :
   - Inclut la surcharge de fonctions et d'opérateurs.
   - Résolu lors de la compilation.
2. **Polymorphisme d'exécution (ou polymorphisme dynamique)** :
   - Utilise des fonctions virtuelles et des pointeurs de base.
   - Résolu à l'exécution grâce à la liaison dynamique.

Dans ce cours, nous nous concentrons sur le polymorphisme dynamique, car il est fondamental dans la création de hiérarchies de classes flexibles.

---

## 2. Fonction virtuelle

Une **fonction virtuelle** est une fonction déclarée dans une classe de base et redéfinie dans une classe dérivée. Elle permet de réaliser le polymorphisme en C++. Une fonction virtuelle indique au compilateur que l'appel à cette fonction sera résolu dynamiquement en fonction du type réel de l'objet.

### Exemple de Fonction Virtuelle

```cpp
#include <iostream>

class Animal {
public:
    virtual void parler() const { // Fonction virtuelle
        std::cout << "Cet animal fait un bruit." << std::endl;
    }
};

class Chien : public Animal {
public:
    void parler() const override { // Redéfinition de la fonction
        std::cout << "Le chien aboie." << std::endl;
    }
};

class Chat : public Animal {
public:
    void parler() const override {
        std::cout << "Le chat miaule." << std::endl;
    }
};

int main() {
    Animal* animal1 = new Chien();
    Animal* animal2 = new Chat();

    animal1->parler(); // Affiche : Le chien aboie.
    animal2->parler(); // Affiche : Le chat miaule.

    delete animal1;
    delete animal2;

    return 0;
}
```

**Explication** :
- **virtual** dans `Animal::parler()` permet la redéfinition de cette fonction dans les classes dérivées.
- L’utilisation de `override` dans les classes dérivées `Chien` et `Chat` indique que ces méthodes remplacent la version de `Animal`.

### Remarque
Si `parler` n’est pas virtuel, l’appel `animal1->parler()` ou `animal2->parler()` exécuterait toujours la version de `Animal`, même si `animal1` et `animal2` sont des objets `Chien` et `Chat`.

---

## 3. Classe Abstraite et Interface

Une **classe abstraite** est une classe qui contient au moins une fonction virtuelle pure (une fonction sans implémentation dans la classe de base). Elle ne peut pas être instanciée directement et sert de modèle pour d’autres classes.

### Syntaxe d’une Classe Abstraite

```cpp
class Forme {
public:
    virtual void dessiner() const = 0; // Fonction virtuelle pure
};
```

La fonction `dessiner` est une **fonction virtuelle pure** en ajoutant `= 0` à la fin de sa déclaration.

### Exemple de Classe Abstraite

Imaginons un exemple où `Forme` est une classe abstraite et `Cercle` et `Rectangle` sont des classes dérivées.

```cpp
#include <iostream>

class Forme {
public:
    virtual void dessiner() const = 0; // Fonction virtuelle pure
};

class Cercle : public Forme {
public:
    void dessiner() const override {
        std::cout << "Dessiner un cercle." << std::endl;
    }
};

class Rectangle : public Forme {
public:
    void dessiner() const override {
        std::cout << "Dessiner un rectangle." << std::endl;
    }
};

int main() {
    Forme* forme1 = new Cercle();
    Forme* forme2 = new Rectangle();

    forme1->dessiner(); // Affiche : Dessiner un cercle.
    forme2->dessiner(); // Affiche : Dessiner un rectangle.

    delete forme1;
    delete forme2;

    return 0;
}
```

**Explication** :
- `Forme` est une classe abstraite avec une fonction virtuelle pure `dessiner`.
- `Cercle` et `Rectangle` sont des classes concrètes qui dérivent de `Forme` et redéfinissent `dessiner`.

---

## 4. Utilisation Pratique des Interfaces

En C++, les **interfaces** sont souvent définies par des classes abstraites qui contiennent uniquement des fonctions virtuelles pures. Les interfaces facilitent la création de code flexible et modulaire en permettant à plusieurs classes d'implémenter la même interface.

### Exemple de Création d'une Interface

Créons une interface `Affichable` qui définit une méthode `afficher`.

```cpp
#include <iostream>

class Affichable {
public:
    virtual void afficher() const = 0; // Méthode virtuelle pure
};

class Produit : public Affichable {
public:
    void afficher() const override {
        std::cout << "Affichage d'un produit." << std::endl;
    }
};

class Client : public Affichable {
public:
    void afficher() const override {
        std::cout << "Affichage d'un client." << std::endl;
    }
};

int main() {
    Affichable* obj1 = new Produit();
    Affichable* obj2 = new Client();

    obj1->afficher(); // Affiche : Affichage d'un produit.
    obj2->afficher(); // Affiche : Affichage d'un client.

    delete obj1;
    delete obj2;

    return 0;
}
```

---

## 5. Exercices Pratiques

### Exercice 1 : Polymorphisme et Fonction Virtuelle

1. Créez une classe `Vehicule` avec une fonction virtuelle `klaxonner`.
2. Créez deux classes dérivées, `Voiture` et `Moto`, qui redéfinissent `klaxonner` pour afficher un message différent.
3. Testez le polymorphisme en utilisant des pointeurs de type `Vehicule`.

### Exercice 2 : Classe Abstraite

1. Créez une classe abstraite `Animal` avec une fonction virtuelle pure `seDeplacer`.
2. Créez deux classes concrètes, `Oiseau` et `Poisson`, qui redéfinissent `seDeplacer` pour afficher la manière dont ils se déplacent.
3. Testez en créant des instances de `Oiseau` et `Poisson` à travers des pointeurs de type `Animal`.

### Exercice 3 : Interface Affichable

1. Créez une interface `Affichable` avec une méthode `afficher`.
2. Créez une classe `Document` et une classe `Image` qui implémentent `Affichable`.
3. Dans chaque classe, redéfinissez `afficher` pour afficher un message unique, puis testez en utilisant des pointeurs vers `Affichable`.

---

```
