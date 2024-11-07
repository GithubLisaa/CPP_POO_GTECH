# Exercices Pratiques sur les Pointeurs en C++

## Exercice 1 : Gestion dynamique de mémoire avec `unique_ptr`

**Contexte :** Vous travaillez sur une application de gestion d'inventaire pour une bibliothèque. Vous devez créer des objets représentant des livres, chaque livre ayant un titre et un nombre de pages. Chaque livre doit être géré dynamiquement, mais sans risque de fuite de mémoire.

**Objectif :**
1. Créez une classe `Livre` avec deux attributs : `titre` (chaîne de caractères) et `pages` (entier).
2. Dans la fonction `main`, allouez dynamiquement deux livres en utilisant `std::unique_ptr`.
3. Assignez des valeurs aux attributs des livres.
4. Affichez les informations de chaque livre.

**Contraintes :**
- Utilisez `std::unique_ptr` pour gérer chaque livre.
- Pas besoin de libérer explicitement la mémoire, `unique_ptr` doit s’en charger automatiquement.

**Solution attendue :**
```cpp
#include <iostream>
#include <memory>
#include <string>

class Livre {
public:
    std::string titre;
    int pages;

    Livre(const std::string& t, int p) : titre(t), pages(p) {}

    void afficher() const {
        std::cout << "Titre : " << titre << ", Pages : " << pages << std::endl;
    }
};

int main() {
    // Allocation dynamique de livres avec unique_ptr
    std::unique_ptr<Livre> livre1 = std::make_unique<Livre>("Le Petit Prince", 98);
    std::unique_ptr<Livre> livre2 = std::make_unique<Livre>("1984", 328);

    // Affichage des informations des livres
    livre1->afficher();
    livre2->afficher();

    // Les unique_ptrs sont automatiquement libérés ici
    return 0;
}
```

---

## Exercice 2 : Gestion de tableaux dynamiques avec `new` et `delete`

**Contexte :** Vous développez une fonctionnalité de statistiques qui prend en entrée un tableau dynamique de notes d'étudiants pour calculer la moyenne. Le nombre de notes est saisi par l’utilisateur, et vous devez gérer la mémoire dynamique pour stocker ces notes.

**Objectif :**
1. Demandez à l'utilisateur d'entrer le nombre de notes.
2. Allouez dynamiquement un tableau d’entiers de cette taille.
3. Demandez à l'utilisateur de saisir chaque note dans le tableau.
4. Calculez et affichez la moyenne des notes.
5. Libérez la mémoire du tableau.

**Contraintes :**
- Utilisez `new` et `delete[]` pour allouer et libérer la mémoire.

**Solution attendue :**
```cpp
#include <iostream>

int main() {
    int nombreNotes;

    // Demande du nombre de notes
    std::cout << "Entrez le nombre de notes : ";
    std::cin >> nombreNotes;

    // Allocation dynamique d'un tableau pour les notes
    int* notes = new int[nombreNotes];

    // Entrée des notes
    for (int i = 0; i < nombreNotes; ++i) {
        std::cout << "Entrez la note " << i + 1 << " : ";
        std::cin >> notes[i];
    }

    // Calcul de la moyenne
    int somme = 0;
    for (int i = 0; i < nombreNotes; ++i) {
        somme += notes[i];
    }
    double moyenne = static_cast<double>(somme) / nombreNotes;
    std::cout << "Moyenne des notes : " << moyenne << std::endl;

    // Libération de la mémoire
    delete[] notes;

    return 0;
}
```

---

## Exercice 3 : Gestion de la mémoire partagée avec `shared_ptr` et `weak_ptr`

**Contexte :** Vous développez un jeu vidéo avec des personnages. Chaque personnage peut avoir un « allié » (un autre personnage) et chaque personnage peut également être l’allié d’un autre. Cela peut facilement créer des références circulaires.

**Objectif :**
1. Créez une classe `Personnage` avec un attribut `nom` (chaîne de caractères) et un attribut `std::weak_ptr<Personnage> allié`.
2. Dans `main`, créez deux personnages (`p1` et `p2`) avec des noms différents.
3. Assignez `p2` comme allié de `p1`, et `p1` comme allié de `p2`.
4. Affichez le nom de l'allié de chaque personnage sans provoquer de fuite de mémoire grâce à `weak_ptr`.

**Contraintes :**
- Utilisez `std::shared_ptr` pour chaque personnage principal et `std::weak_ptr` pour les alliances.

**Solution attendue :**
```cpp
#include <iostream>
#include <memory>
#include <string>

class Personnage {
public:
    std::string nom;
    std::weak_ptr<Personnage> allie;  // Pointeur faible pour éviter les cycles

    Personnage(const std::string& n) : nom(n) {}

    void afficherAllie() const {
        if (auto alliePartage = allie.lock()) {
            std::cout << nom << " a pour allié : " << alliePartage->nom << std::endl;
        } else {
            std::cout << nom << " n'a pas d'allié." << std::endl;
        }
    }
};

int main() {
    // Création des personnages
    std::shared_ptr<Personnage> p1 = std::make_shared<Personnage>("Héros");
    std::shared_ptr<Personnage> p2 = std::make_shared<Personnage>("Ami");

    // Définition des alliances (références croisées)
    p1->allie = p2;
    p2->allie = p1;

    // Affichage des alliés
    p1->afficherAllie();
    p2->afficherAllie();

    // Les shared_ptr sont automatiquement libérés ici sans fuite de mémoire
    return 0;
}
```
