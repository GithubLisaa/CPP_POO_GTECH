# Cours C++ : Programmation Orientée Objet – Jour 2

## Thème : Héritage et Polymorphisme

---

### Objectifs du jour :
- Comprendre et implémenter l'héritage en C++.
- Savoir créer des classes dérivées à partir de classes de base.
- Apprendre le polymorphisme pour utiliser des pointeurs de classe de base afin de manipuler des objets dérivés.
- Explorer le rôle des méthodes virtuelles et de la surcharge des méthodes.

---

## 1. Héritage en C++

L'**héritage** permet de créer une nouvelle classe qui hérite des attributs et méthodes d'une classe existante. Cela permet de réutiliser du code et de modeler des relations entre les classes. La classe qui hérite est appelée **classe dérivée**, tandis que la classe de laquelle elle hérite est appelée **classe de base**.

### Syntaxe de l'héritage :

```cpp
class ClasseDeBase {
    // Membres de la classe de base
};

class ClasseDerivee : public ClasseDeBase {
    // Membres spécifiques à la classe dérivée
};
```

L'héritage public (`: public ClasseDeBase`) est le type d'héritage le plus couramment utilisé. Il signifie que les membres publics de la classe de base sont accessibles directement depuis la classe dérivée.

---

## 2. Exemple d'Héritage

### Exemple : Classe Animal et classe Chien

```cpp
#include <iostream>
#include <string>

// Classe de base
class Animal {
public:
    std::string nom;
    int age;

    Animal(std::string n, int a) : nom(n), age(a) {}

    void afficherInfos() {
        std::cout << "Nom: " << nom << ", Âge: " << age << std::endl;
    }

    void parler() {
        std::cout << nom << " fait un bruit." << std::endl;
    }
};

// Classe dérivée
class Chien : public Animal {
public:
    std::string race;

    Chien(std::string n, int a, std::string r) : Animal(n, a), race(r) {}

    void aboyer() {
        std::cout << nom << " aboie!" << std::endl;
    }
};

int main() {
    Chien rex("Rex", 5, "Berger allemand");
    rex.afficherInfos();
    rex.aboyer();
    rex.parler();  // Méthode héritée de la classe Animal

    return 0;
}
```

### Explication :
- La classe `Chien` hérite de la classe `Animal`. Elle peut accéder aux attributs et méthodes de `Animal` (comme `afficherInfos` et `parler`).
- La classe dérivée `Chien` a son propre attribut `race` et une méthode supplémentaire `aboyer`.

**Sortie :**
```
Nom: Rex, Âge: 5
Rex aboie!
Rex fait un bruit.
```

---

## 3. Polymorphisme

Le **polymorphisme** permet d'utiliser des objets dérivés via des pointeurs ou des références à des objets de classe de base. Cela permet de traiter différents types d'objets de manière uniforme, tout en gardant un comportement spécifique à chaque type.

### Exemple de polymorphisme :

```cpp
#include <iostream>
#include <string>

// Classe de base
class Animal {
public:
    std::string nom;

    Animal(std::string n) : nom(n) {}

    // Méthode virtuelle
    virtual void parler() {
        std::cout << nom << " fait un bruit." << std::endl;
    }
};

// Classe dérivée
class Chien : public Animal {
public:
    Chien(std::string n) : Animal(n) {}

    void parler() override {
        std::cout << nom << " aboie!" << std::endl;
    }
};

// Classe dérivée
class Chat : public Animal {
public:
    Chat(std::string n) : Animal(n) {}

    void parler() override {
        std::cout << nom << " miaule!" << std::endl;
    }
};

int main() {
    // Utilisation de pointeurs vers la classe de base
    Animal* animal1 = new Chien("Rex");
    Animal* animal2 = new Chat("Miaou");

    animal1->parler();  // Appelle parler() de Chien
    animal2->parler();  // Appelle parler() de Chat

    // Libérer la mémoire
    delete animal1;
    delete animal2;

    return 0;
}
```

### Explication :
- La méthode `parler` est définie comme **virtuelle** dans la classe `Animal`. Cela signifie que lorsqu'elle est appelée via un pointeur ou une référence à un `Animal`, le C++ déterminera dynamiquement quelle version appeler (celle de `Chien` ou de `Chat`).
- Les méthodes dans les classes dérivées `Chien` et `Chat` surchargent la méthode `parler` de `Animal`.

**Sortie :**
```
Rex aboie!
Miaou miaule!
```

---

## 4. Méthodes virtuelles pures et Classes abstraites

Une **méthode virtuelle pure** est une méthode qui doit être redéfinie dans les classes dérivées. Elle est déclarée en utilisant l'opérateur `= 0`. Une classe contenant une ou plusieurs méthodes virtuelles pures est appelée **classe abstraite**, et elle ne peut pas être instanciée directement.

### Exemple de classe abstraite :

```cpp
#include <iostream>
#include <string>

// Classe abstraite
class Animal {
public:
    std::string nom;

    Animal(std::string n) : nom(n) {}

    // Méthode virtuelle pure
    virtual void parler() = 0;

    void afficherInfos() {
        std::cout << "Nom: " << nom << std::endl;
    }
};

// Classe dérivée
class Chien : public Animal {
public:
    Chien(std::string n) : Animal(n) {}

    void parler() override {
        std::cout << nom << " aboie!" << std::endl;
    }
};

int main() {
    // On ne peut pas instancier Animal directement
    // Animal a("Anonyme");  // Erreur: classe abstraite

    Chien rex("Rex");
    rex.afficherInfos();
    rex.parler();

    return 0;
}
```

### Explication :
- `Animal` est une classe abstraite, car elle contient une méthode virtuelle pure (`parler`).
- `Chien` est une classe dérivée qui doit obligatoirement redéfinir `parler`, sinon elle aussi serait abstraite.

**Sortie :**
```
Nom: Rex
Rex aboie!
```

---

## 5. Exercices Pratiques

### Exercice 1 : Créer une classe Oiseau

1. Créer une classe abstraite `Oiseau` avec une méthode virtuelle pure `voler`.
2. Créer deux classes dérivées `Aigle` et `Colibri`, chacune redéfinissant la méthode `voler`.
3. Utiliser des pointeurs de classe de base pour appeler la méthode `voler`.

### Exercice 2 : Créer une hiérarchie de classes

1. Créer une classe de base `Vehicule` avec des attributs `marque` et `vitesseMax`.
2. Ajouter une méthode virtuelle `demarrer`.
3. Créer deux classes dérivées `Voiture` et `Moto`, chacune redéfinissant `demarrer` pour afficher un message spécifique à chaque type de véhicule.
4. Utiliser des pointeurs ou références pour appeler `demarrer` à partir d'objets `Voiture` et `Moto`.

---

## Résumé du Jour 2

- **Héritage** : Permet de créer une nouvelle classe à partir d'une classe existante. La classe dérivée hérite des attributs et méthodes de la classe de base.
- **Polymorphisme** : Permet de manipuler des objets de classes dérivées à travers des pointeurs ou des références de classe de base, tout en permettant un comportement spécifique à chaque classe.
- **Méthodes virtuelles** : Permettent de redéfinir le comportement d'une méthode dans une classe dérivée.
- **Classe abstraite** : Une classe qui contient au moins une méthode virtuelle pure et qui ne peut pas être instanciée directement.

Ces concepts sont essentiels pour modéliser des relations complexes entre objets et faciliter la réutilisation du code.
```
