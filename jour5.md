# Cours C++ : Programmation Orientée Objet – Jour 5

## Thème : Boucle de jeu et Architecture d’une application

---

### Objectifs du jour :
- Comprendre la boucle de jeu, essentielle dans les applications interactives comme les jeux.
- Définir une architecture de base pour structurer les éléments d’un jeu.
- Apprendre à gérer le flux principal d’une application, notamment la gestion des entrées et l’actualisation de l’affichage.
- Structurer les interactions entre objets pour favoriser une bonne organisation et évolutivité du code.

---

## 1. Introduction à la Boucle de Jeu

La boucle de jeu est au cœur de tout jeu vidéo. Elle contrôle le flux du jeu, gère les événements, met à jour l’état du jeu et affiche le résultat à l’écran. Cette boucle permet de garder le jeu interactif et réactif en continuant d'exécuter ces étapes jusqu'à la fin du jeu.

### Structure de la Boucle de Jeu

La boucle de jeu se divise en trois étapes principales :
1. **Gestion des entrées** : lire les actions des joueurs (comme les commandes clavier).
2. **Mise à jour de l’état** : actualiser les positions, les actions et les événements du jeu.
3. **Affichage** : afficher les résultats à l’écran (dans notre cas, sur la console).

### Exemple d’une Boucle de Jeu Simplifiée

```cpp
#include <iostream>
#include <conio.h> // Pour utiliser _kbhit() et _getch() sous Windows
#include <chrono>
#include <thread>

bool jeuEnCours = true;

void gestionEntree() {
    if (_kbhit()) { // Vérifie si une touche est appuyée
        char touche = _getch();
        if (touche == 'q') { // Appuyer sur 'q' pour quitter
            jeuEnCours = false;
        }
    }
}

void miseAJourEtat() {
    // Logique de mise à jour du jeu, par exemple, déplacer un personnage, gérer le score, etc.
}

void affichage() {
    std::cout << "Affichage de l'état du jeu..." << std::endl;
}

int main() {
    while (jeuEnCours) {
        gestionEntree();
        miseAJourEtat();
        affichage();
        std::this_thread::sleep_for(std::chrono::milliseconds(100)); // Pause pour ralentir la boucle
    }
    std::cout << "Fin du jeu!" << std::endl;
    return 0;
}
```

### Explication

- La boucle s’exécute tant que `jeuEnCours` est vrai.
- La fonction `gestionEntree` lit les entrées de l’utilisateur. Si l’utilisateur appuie sur "q", la variable `jeuEnCours` passe à `false` et termine la boucle.
- `miseAJourEtat` représente la logique du jeu, comme la mise à jour de la position des personnages.
- `affichage` permettrait de redessiner l’état du jeu à l’écran.

---

## 2. Structure d'une Application de Jeu

Il est important de structurer le code d'un jeu pour le rendre modulaire et évolutif. Une architecture de base pourrait inclure des classes comme `Jeu`, `Personnage`, `Ennemi`, `Niveau`, etc.

### Exemples de Classes et leurs Rôles

- **Classe Jeu** : contrôle la boucle de jeu et la gestion globale.
- **Classe Personnage** : représente le joueur avec des attributs comme `pointsDeVie`, `position`, et des méthodes comme `deplacer`.
- **Classe Ennemi** : représente les ennemis, avec des comportements spécifiques.
- **Classe Niveau** : contient les informations de chaque niveau et les objets qu’il contient.

---

## 3. Exemple d’Architecture pour un Mini-Jeu Console

Voici une structure basique pour un jeu console où un personnage peut se déplacer et des ennemis peuvent réagir.

### Classe `Personnage`

```cpp
#include <iostream>

class Personnage {
public:
    int x, y; // Position du personnage

    Personnage(int posX, int posY) : x(posX), y(posY) {}

    void deplacer(int deltaX, int deltaY) {
        x += deltaX;
        y += deltaY;
    }

    void afficherPosition() const {
        std::cout << "Position du personnage : (" << x << ", " << y << ")" << std::endl;
    }
};
```

### Classe `Ennemi`

```cpp
class Ennemi {
public:
    int x, y;

    Ennemi(int posX, int posY) : x(posX), y(posY) {}

    void suivrePersonnage(const Personnage& perso) {
        if (x < perso.x) x++;
        else if (x > perso.x) x--;
        
        if (y < perso.y) y++;
        else if (y > perso.y) y--;
    }

    void afficherPosition() const {
        std::cout << "Position de l'ennemi : (" << x << ", " << y << ")" << std::endl;
    }
};
```

### Classe `Jeu`

```cpp
class Jeu {
private:
    Personnage perso;
    Ennemi ennemi;
    bool enCours;

public:
    Jeu() : perso(0, 0), ennemi(5, 5), enCours(true) {}

    void gestionEntree() {
        char touche;
        std::cout << "Déplacez le personnage (z:haut, s:bas, q:gauche, d:droite) ou q pour quitter : ";
        std::cin >> touche;
        switch (touche) {
            case 'z': perso.deplacer(0, 1); break;
            case 's': perso.deplacer(0, -1); break;
            case 'q': perso.deplacer(-1, 0); break;
            case 'd': perso.deplacer(1, 0); break;
            case 'x': enCours = false; break;
            default: break;
        }
    }

    void miseAJourEtat() {
        ennemi.suivrePersonnage(perso);
    }

    void affichage() {
        system("clear"); // Utilisez "cls" sur Windows
        perso.afficherPosition();
        ennemi.afficherPosition();
    }

    void boucleDeJeu() {
        while (enCours) {
            gestionEntree();
            miseAJourEtat();
            affichage();
        }
        std::cout << "Fin du jeu!" << std::endl;
    }
};
```

### Fonction `main`

```cpp
int main() {
    Jeu jeu;
    jeu.boucleDeJeu();
    return 0;
}
```

### Explication

- La classe `Personnage` représente le joueur avec une position `(x, y)` et une méthode `deplacer`.
- La classe `Ennemi` représente un ennemi qui suit le personnage en ajustant sa position pour se rapprocher de celle du personnage.
- La classe `Jeu` gère le flux du jeu : gestion des entrées du joueur, mise à jour de l’état des objets et affichage de l’état actuel du jeu. La méthode `boucleDeJeu` contient la boucle principale.

---

## 4. Exercices Pratiques

### Exercice 1 : Ajout d’un Nouvel Ennemi

1. Ajoutez un deuxième ennemi dans le jeu avec une position initiale différente.
2. Modifiez la logique de `miseAJourEtat` pour que chaque ennemi suive indépendamment le personnage.
3. Affichez les positions de chaque ennemi dans la fonction `affichage`.

### Exercice 2 : Système de Score

1. Créez une nouvelle classe `Score` qui contient un attribut `points`.
2. Dans la boucle de jeu, augmentez le score à chaque déplacement du personnage.
3. Affichez le score actuel dans la fonction `affichage`.

### Exercice 3 : Détection de Collision

1. Ajoutez une méthode `collisionAvec` dans la classe `Personnage` qui prend les coordonnées d’un ennemi en paramètre et retourne `true` si le personnage et l’ennemi occupent la même position.
2. Utilisez cette méthode dans la classe `Jeu` pour détecter une collision entre le personnage et les ennemis. Si une collision est détectée, terminez la partie.

---
