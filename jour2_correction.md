## **Correction de l'Exercice 1 : Créer une classe Oiseau**

### Énoncé :
1. Créer une classe abstraite `Oiseau` avec une méthode virtuelle pure `voler`.
2. Créer deux classes dérivées `Aigle` et `Colibri`, chacune redéfinissant la méthode `voler`.
3. Utiliser des pointeurs de classe de base pour appeler la méthode `voler`.

### Solution :

```cpp
#include <iostream>
#include <string>

// Classe abstraite
class Oiseau {
public:
    std::string nom;

    Oiseau(std::string n) : nom(n) {}

    // Méthode virtuelle pure
    virtual void voler() = 0;  // méthode virtuelle pure, classe abstraite

    void afficherNom() {
        std::cout << "Nom: " << nom << std::endl;
    }
};

// Classe dérivée Aigle
class Aigle : public Oiseau {
public:
    Aigle(std::string n) : Oiseau(n) {}

    void voler() override {
        std::cout << nom << " vole haut dans le ciel!" << std::endl;
    }
};

// Classe dérivée Colibri
class Colibri : public Oiseau {
public:
    Colibri(std::string n) : Oiseau(n) {}

    void voler() override {
        std::cout << nom << " vole rapidement d'une fleur à l'autre!" << std::endl;
    }
};

int main() {
    // Pointeur de classe de base (polymorphisme)
    Oiseau* oiseau1 = new Aigle("Aigle Royal");
    Oiseau* oiseau2 = new Colibri("Petit Colibri");

    // Utilisation du polymorphisme pour appeler voler()
    oiseau1->voler();
    oiseau2->voler();

    // Libération de la mémoire
    delete oiseau1;
    delete oiseau2;

    return 0;
}
```

### Explication :
- La classe `Oiseau` est une classe abstraite car elle contient la méthode virtuelle pure `voler`.
- Les classes dérivées `Aigle` et `Colibri` doivent redéfinir cette méthode.
- Le polymorphisme permet d'utiliser des pointeurs vers `Oiseau` pour manipuler des objets de type `Aigle` et `Colibri`, tout en appelant leur version respective de la méthode `voler`.

### Exemple de sortie :
```
Aigle Royal vole haut dans le ciel!
Petit Colibri vole rapidement d'une fleur à l'autre!
```

---

## **Correction de l'Exercice 2 : Créer une hiérarchie de classes pour les véhicules**

### Énoncé :
1. Créer une classe de base `Vehicule` avec les attributs `marque` et `vitesseMax`.
2. Ajouter une méthode virtuelle `demarrer`.
3. Créer deux classes dérivées `Voiture` et `Moto`, chacune redéfinissant `demarrer` pour afficher un message spécifique à chaque type de véhicule.
4. Utiliser des pointeurs ou références pour appeler `demarrer` à partir d'objets `Voiture` et `Moto`.

### Solution :

```cpp
#include <iostream>
#include <string>

// Classe de base Vehicule
class Vehicule {
protected:
    std::string marque;
    int vitesseMax;

public:
    Vehicule(std::string m, int v) : marque(m), vitesseMax(v) {}

    // Méthode virtuelle
    virtual void demarrer() {
        std::cout << "Le véhicule démarre." << std::endl;
    }

    void afficherInfos() {
        std::cout << "Marque: " << marque << ", Vitesse Max: " << vitesseMax << " km/h" << std::endl;
    }
};

// Classe dérivée Voiture
class Voiture : public Vehicule {
public:
    Voiture(std::string m, int v) : Vehicule(m, v) {}

    void demarrer() override {
        std::cout << "La voiture " << marque << " démarre avec une vitesse max de " << vitesseMax << " km/h." << std::endl;
    }
};

// Classe dérivée Moto
class Moto : public Vehicule {
public:
    Moto(std::string m, int v) : Vehicule(m, v) {}

    void demarrer() override {
        std::cout << "La moto " << marque << " démarre avec une vitesse max de " << vitesseMax << " km/h." << std::endl;
    }
};

int main() {
    // Utilisation du polymorphisme
    Vehicule* vehicule1 = new Voiture("Tesla", 250);
    Vehicule* vehicule2 = new Moto("Yamaha", 180);

    // Afficher les infos et démarrer les véhicules
    vehicule1->afficherInfos();
    vehicule1->demarrer();

    vehicule2->afficherInfos();
    vehicule2->demarrer();

    // Libération de la mémoire
    delete vehicule1;
    delete vehicule2;

    return 0;
}
```

### Explication :
- La classe `Vehicule` est une classe de base qui contient les attributs communs `marque` et `vitesseMax`, ainsi qu'une méthode virtuelle `demarrer`.
- Les classes dérivées `Voiture` et `Moto` redéfinissent la méthode `demarrer` pour personnaliser le comportement du démarrage.
- Le polymorphisme permet d'utiliser des pointeurs vers `Vehicule` pour manipuler des objets `Voiture` et `Moto`, tout en appelant leur méthode `demarrer` respective.

### Exemple de sortie :
```
Marque: Tesla, Vitesse Max: 250 km/h
La voiture Tesla démarre avec une vitesse max de 250 km/h.
Marque: Yamaha, Vitesse Max: 180 km/h
La moto Yamaha démarre avec une vitesse max de 180 km/h.
```

---

### **Résumé des points importants dans la correction :**
- L'héritage permet de créer des hiérarchies de classes où les classes dérivées héritent des attributs et des méthodes de la classe de base.
- Le polymorphisme permet d'utiliser des pointeurs ou des références de la classe de base pour manipuler des objets dérivés, tout en conservant un comportement spécifique à chaque classe dérivée grâce aux méthodes virtuelles.
- Les méthodes virtuelles pures, lorsqu'elles sont définies dans une classe de base, forcent les classes dérivées à les redéfinir, ce qui rend la classe de base abstraite.
