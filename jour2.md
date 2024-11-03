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
Qu'est-ce que l'héritage ?

L'héritage est un principe fondamental de la programmation orientée objet qui permet à une classe (appelée classe dérivée ou sous-classe) d'hériter des propriétés (attributs et méthodes) d'une autre classe (appelée classe de base ou super-classe). Cela permet de réutiliser le code et de créer des hiérarchies de classes, facilitant ainsi la gestion des objets et leur extension.
Avantages de l'héritage

    Réutilisation du code : Vous pouvez réutiliser les méthodes et les attributs de la classe de base dans la classe dérivée.
    Extensibilité : Les classes dérivées peuvent ajouter de nouvelles fonctionnalités ou modifier celles de la classe de base.
    Organisation : L'héritage permet de structurer le code en hiérarchies, rendant le programme plus clair et plus facile à maintenir.

Types d'héritage

L'héritage peut être classé en trois types principaux selon la manière dont les membres de la classe de base sont accessibles dans la classe dérivée : public, protected et private.
1. Héritage public

    Accessibilité :
        Les membres publics de la classe de base deviennent des membres publics de la classe dérivée.
        Les membres protégés de la classe de base deviennent des membres protégés de la classe dérivée.
        Les membres privés de la classe de base restent inaccessibles à la classe dérivée.

Exemple :

cpp

#include <iostream>

class Base {
public:
    void afficherPublic() { std::cout << "Membre public de Base\n"; }
protected:
    void afficherProtege() { std::cout << "Membre protégé de Base\n"; }
private:
    void afficherPrive() { std::cout << "Membre privé de Base\n"; }
};

class Derivee : public Base {
public:
    void afficher() {
        afficherPublic();   // Accessible
        afficherProtege();  // Accessible
        // afficherPrive(); // Inaccessible, ca causerait une erreur
    }
};

int main() {
    Derivee d;
    d.afficher();
    return 0;
}

Sortie :

arduino

Membre public de Base
Membre protégé de Base

2. Héritage protégé

    Accessibilité :
        Les membres publics et protégés de la classe de base deviennent des membres protégés de la classe dérivée.
        Les membres privés de la classe de base restent inaccessibles à la classe dérivée.

Exemple :

cpp

class Base {
public:
    void afficherPublic() { std::cout << "Membre public de Base\n"; }
protected:
    void afficherProtege() { std::cout << "Membre protégé de Base\n"; }
private:
    void afficherPrive() { std::cout << "Membre privé de Base\n"; }
};

class Derivee : protected Base {
public:
    void afficher() {
        afficherPublic();   // Accessible
        afficherProtege();  // Accessible
        // afficherPrive(); // Inaccessible
    }
};

int main() {
    Derivee d;
    d.afficher();
    // d.afficherPublic(); // Inaccessible, ca causerait une erreur
    return 0;
}

Sortie :

arduino

Membre public de Base
Membre protégé de Base

3. Héritage privé

    Accessibilité :
        Les membres publics et protégés de la classe de base deviennent des membres privés de la classe dérivée.
        Les membres privés de la classe de base restent inaccessibles à la classe dérivée.

Exemple :

cpp

class Base {
public:
    void afficherPublic() { std::cout << "Membre public de Base\n"; }
protected:
    void afficherProtege() { std::cout << "Membre protégé de Base\n"; }
private:
    void afficherPrive() { std::cout << "Membre privé de Base\n"; }
};

class Derivee : private Base {
public:
    void afficher() {
        afficherPublic();   // Accessible
        afficherProtege();  // Accessible
        // afficherPrive(); // Inaccessible
    }
};

int main() {
    Derivee d;
    d.afficher();
    // d.afficherPublic(); // Inaccessible, ca causerait une erreur
    return 0;
}

Sortie :

arduino

Membre public de Base
Membre protégé de Base

Résumé des types d'héritage
Type d'héritage	Membre public	Membre protégé	Membre privé
Héritage public	Public	Protégé	Inaccessible
Héritage protégé	Protégé	Protégé	Inaccessible
Héritage privé	Privé	Privé	Inaccessible
Conclusion

L'héritage en C++ est un outil puissant qui facilite la réutilisation du code et l'organisation des programmes. Le choix du type d'héritage (public, protected ou private) influence la façon dont les membres de la classe de base sont accessibles dans la classe dérivée. Il est donc essentiel de choisir le type approprié en fonction des besoins de votre conception.
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
