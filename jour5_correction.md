### Exercice 1 : Ajout d’un Nouvel Ennemi

**Objectif :** Ajouter un deuxième ennemi qui suit également le personnage.

**Solution :**
1. Ajoutez un deuxième objet de type `Ennemi` dans la classe `Jeu`.
2. Modifiez `miseAJourEtat` pour que chaque ennemi suive indépendamment le personnage.
3. Mettez à jour `affichage` pour afficher les positions de chaque ennemi.

```cpp
class Jeu {
private:
    Personnage perso;
    Ennemi ennemi1;
    Ennemi ennemi2; // Deuxième ennemi
    bool enCours;

public:
    Jeu() : perso(0, 0), ennemi1(5, 5), ennemi2(10, 10), enCours(true) {}

    void gestionEntree() {
        char touche;
        std::cout << "Déplacez le personnage (z:haut, s:bas, q:gauche, d:droite) ou x pour quitter : ";
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
        ennemi1.suivrePersonnage(perso);
        ennemi2.suivrePersonnage(perso); // Le deuxième ennemi suit aussi le personnage
    }

    void affichage() {
        system("cls"); 
        perso.afficherPosition();
        ennemi1.afficherPosition();
        ennemi2.afficherPosition(); // Affiche la position du deuxième ennemi
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

---

### Exercice 2 : Système de Score

**Objectif :** Créer un système de score qui augmente à chaque déplacement du personnage.

**Solution :**
1. Ajoutez une classe `Score` avec un attribut `points` et une méthode pour augmenter les points.
2. Intégrez un objet `Score` dans `Jeu` et augmentez le score à chaque déplacement du personnage.
3. Affichez le score dans `affichage`.

```cpp
class Score {
private:
    int points;

public:
    Score() : points(0) {}

    void augmenter(int valeur) {
        points += valeur;
    }

    int getPoints() const {
        return points;
    }
};

class Jeu {
private:
    Personnage perso;
    Ennemi ennemi1;
    Ennemi ennemi2;
    Score score; // Ajout du score
    bool enCours;

public:
    Jeu() : perso(0, 0), ennemi1(5, 5), ennemi2(10, 10), enCours(true) {}

    void gestionEntree() {
        char touche;
        std::cout << "Déplacez le personnage (z:haut, s:bas, q:gauche, d:droite) ou x pour quitter : ";
        std::cin >> touche;
        switch (touche) {
            case 'z': perso.deplacer(0, 1); score.augmenter(1); break;
            case 's': perso.deplacer(0, -1); score.augmenter(1); break;
            case 'q': perso.deplacer(-1, 0); score.augmenter(1); break;
            case 'd': perso.deplacer(1, 0); score.augmenter(1); break;
            case 'x': enCours = false; break;
            default: break;
        }
    }

    void miseAJourEtat() {
        ennemi1.suivrePersonnage(perso);
        ennemi2.suivrePersonnage(perso);
    }

    void affichage() {
        system("cls"); 
        perso.afficherPosition();
        ennemi1.afficherPosition();
        ennemi2.afficherPosition();
        std::cout << "Score actuel : " << score.getPoints() << std::endl; // Affiche le score
    }

    void boucleDeJeu() {
        while (enCours) {
            gestionEntree();
            miseAJourEtat();
            affichage();
        }
        std::cout << "Fin du jeu! Score final : " << score.getPoints() << std::endl;
    }
};
```

---

### Exercice 3 : Détection de Collision

**Objectif :** Détecter la collision entre le personnage et les ennemis pour mettre fin au jeu si une collision est détectée.

**Solution :**
1. Ajoutez une méthode `collisionAvec` dans `Personnage` qui vérifie si un ennemi partage la même position que le personnage.
2. Utilisez cette méthode dans `miseAJourEtat` pour vérifier les collisions.
3. Si une collision est détectée, mettez fin au jeu.

```cpp
class Personnage {
public:
    int x, y;

    Personnage(int posX, int posY) : x(posX), y(posY) {}

    void deplacer(int deltaX, int deltaY) {
        x += deltaX;
        y += deltaY;
    }

    bool collisionAvec(const Ennemi& ennemi) const {
        return x == ennemi.x && y == ennemi.y; // Vérifie si les positions sont identiques
    }

    void afficherPosition() const {
        std::cout << "Position du personnage : (" << x << ", " << y << ")" << std::endl;
    }
};

class Jeu {
private:
    Personnage perso;
    Ennemi ennemi1;
    Ennemi ennemi2;
    Score score;
    bool enCours;

public:
    Jeu() : perso(0, 0), ennemi1(5, 5), ennemi2(10, 10), enCours(true) {}

    void gestionEntree() {
        char touche;
        std::cout << "Déplacez le personnage (z:haut, s:bas, q:gauche, d:droite) ou x pour quitter : ";
        std::cin >> touche;
        switch (touche) {
            case 'z': perso.deplacer(0, 1); score.augmenter(1); break;
            case 's': perso.deplacer(0, -1); score.augmenter(1); break;
            case 'q': perso.deplacer(-1, 0); score.augmenter(1); break;
            case 'd': perso.deplacer(1, 0); score.augmenter(1); break;
            case 'x': enCours = false; break;
            default: break;
        }
    }

    void miseAJourEtat() {
        ennemi1.suivrePersonnage(perso);
        ennemi2.suivrePersonnage(perso);
        
        // Vérifie la collision avec chaque ennemi
        if (perso.collisionAvec(ennemi1) || perso.collisionAvec(ennemi2)) {
            std::cout << "Collision détectée! Fin du jeu." << std::endl;
            enCours = false; // Terminer le jeu si une collision est détectée
        }
    }

    void affichage() {
        system("cls"); 
        perso.afficherPosition();
        ennemi1.afficherPosition();
        ennemi2.afficherPosition();
        std::cout << "Score actuel : " << score.getPoints() << std::endl;
    }

    void boucleDeJeu() {
        while (enCours) {
            gestionEntree();
            miseAJourEtat();
            affichage();
        }
        std::cout << "Fin du jeu! Score final : " << score.getPoints() << std::endl;
    }
};
```
