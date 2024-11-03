# Cours C++ : Programmation Orientée Objet – Jour 6

## Thème : Améliorations et Extensions du Jeu Console

---

### Objectifs du jour :
- Approfondir la gestion des interactions entre objets dans le cadre du jeu console.
- Améliorer et étendre les fonctionnalités de l'application pour ajouter de la profondeur et de l’interaction.
- Travailler sur des aspects de conception tels que la gestion de la santé, des points de vie, et l'ajout d'un environnement dynamique.

---

## 1. Améliorations de la Structure du Jeu

Aujourd'hui, nous allons nous concentrer sur l'amélioration du jeu console que nous avons commencé à développer lors de l'exercice 5 du Jour 5. Notre objectif est de rendre le jeu plus interactif, plus complexe, et plus attrayant pour l’utilisateur.

Les améliorations que nous allons apporter comprennent :
- La gestion de la **santé et des points de vie** des personnages.
- L'ajout de **nouveaux types de personnages** et d’interactions.
- La mise en place d'un **système de tour** pour gérer le déroulement du jeu.
- L’introduction d’un **système de score** pour mesurer les performances.

---

## 2. Ajout de la Santé et des Points de Vie

Pour que le jeu devienne plus immersif, chaque personnage pourra maintenant avoir une quantité de santé qui diminue au fil des événements. Voici les étapes pour ajouter la gestion de la santé.

1. **Ajoutez un attribut santé** dans la classe `Personnage`.
2. **Définissez un maximum et un minimum de santé** : un personnage meurt si sa santé atteint 0.
3. **Implémentez une méthode pour diminuer la santé** : Par exemple, après chaque attaque ou tour, la santé peut diminuer.

### Exemple de Code

```cpp
class Personnage {
protected:
    std::string nom;
    int sante;
    int santeMax;

public:
    Personnage(std::string n, int s) : nom(n), sante(s), santeMax(s) {}

    void attaquer(Personnage& cible) {
        int degats = 10;  // Exemple de dégâts
        cible.diminuerSante(degats);
        std::cout << nom << " attaque " << cible.getNom() << " pour " << degats << " points de dégâts." << std::endl;
    }

    void diminuerSante(int degats) {
        sante -= degats;
        if (sante < 0) sante = 0;
    }

    bool estVivant() const {
        return sante > 0;
    }

    std::string getNom() const { return nom; }
    int getSante() const { return sante; }
};
```

---

## 3. Ajout de Nouveaux Types de Personnages et d'Interactions

Pour enrichir le gameplay, nous pouvons introduire des types de personnages différents avec des capacités uniques :

1. **Guerrier** : Peut infliger plus de dégâts.
2. **Mage** : Peut utiliser des sorts qui affectent plusieurs cibles.
3. **Soigneur** : Peut restaurer la santé des autres personnages.

Chacune de ces classes peut hériter de la classe `Personnage` et implémenter des méthodes uniques.

### Exemple de Classe Guerrier

```cpp
class Guerrier : public Personnage {
public:
    Guerrier(std::string nom) : Personnage(nom, 100) {}

    void attaquePuissante(Personnage& cible) {
        int degats = 20;  // Dégâts accrus
        cible.diminuerSante(degats);
        std::cout << nom << " effectue une attaque puissante sur " << cible.getNom() 
                  << " pour " << degats << " points de dégâts." << std::endl;
    }
};
```

---

## 4. Mise en Place d'un Système de Tour

Le jeu progresse à travers un système de tour dans lequel chaque personnage effectue une action (se déplacer, attaquer, se soigner, etc.). Pour chaque tour, on peut :

1. Vérifier quels personnages sont vivants.
2. Donner à chaque personnage la possibilité d’effectuer une action.
3. Mettre à jour l’état du jeu après chaque action.

### Exemple de Boucle de Jeu avec un Système de Tour

```cpp
void boucleDeJeu(std::vector<std::unique_ptr<Personnage>>& personnages) {
    bool jeuActif = true;

    while (jeuActif) {
        for (auto& personnage : personnages) {
            if (personnage->estVivant()) {
                // Exemples d’actions possibles
                personnage->attaquer(*personnages[rand() % personnages.size()]);  // Attaque un personnage aléatoire
            }
        }

        // Suppression des personnages morts du jeu
        personnages.erase(std::remove_if(personnages.begin(), personnages.end(),
                        [](const std::unique_ptr<Personnage>& p) { return !p->estVivant(); }),
                        personnages.end());

        // Arrêtez le jeu si un seul personnage reste
        if (personnages.size() <= 1) {
            jeuActif = false;
            std::cout << "Le jeu est terminé !" << std::endl;
        }
    }
}
```

---

## 5. Système de Score

Ajoutons un score pour chaque personnage basé sur le nombre d’actions réussies (attaques, soins, etc.). Par exemple, un personnage gagne des points à chaque attaque réussie.

1. Ajoutez un attribut `score` dans la classe `Personnage`.
2. Mettez à jour le score dans les méthodes `attaquer` ou `soigner`.
3. Affichez le score final lorsque le jeu se termine.

### Exemple de Gestion de Score

```cpp
class Personnage {
protected:
    std::string nom;
    int sante;
    int score;

public:
    Personnage(std::string n, int s) : nom(n), sante(s), score(0) {}

    void attaquer(Personnage& cible) {
        int degats = 10;
        cible.diminuerSante(degats);
        score += 10;  // Augmentation du score
    }

    int getScore() const { return score; }
};
```

---

## 6. Exercices Pratiques

### Exercice 1 : Ajout d’une Classe de Personnage

- Créez une nouvelle classe `Archer` qui hérite de `Personnage`.
- L'`Archer` peut effectuer une attaque à distance, infligeant des dégâts même si sa cible n'est pas à côté de lui.

### Exercice 2 : Compétence Spéciale

- Ajoutez une compétence spéciale à chaque type de personnage (par exemple, un **Guérisseur** peut restaurer de la santé).
- Testez les compétences dans la boucle de jeu pour voir l’impact sur les autres personnages.

### Exercice 3 : Durée de Vie des Personnages

- Ajoutez une limite de tours pour chaque personnage (exemple : 10 tours maximum).
- Après avoir atteint cette limite, le personnage quitte le jeu.
- Mettez à jour la boucle de jeu pour gérer cette durée de vie.

### Exercice 4 : Système de Bonus

- Ajoutez un système de bonus : si un personnage réussit trois attaques consécutives, il gagne un bonus de santé.
- Mettez à jour le score et la santé de chaque personnage en fonction des bonus.

### Exercice 5 : Affichage des Scores Finaux

- À la fin du jeu, affichez les scores finaux de tous les personnages.
- Déclarez un **gagnant** basé sur le score le plus élevé.
