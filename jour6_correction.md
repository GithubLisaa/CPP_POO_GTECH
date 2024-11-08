# Correction du Jour 6 : Amélioration du Jeu Console


```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cstdlib>
#include <ctime>
#include <cmath>

class Personnage {
protected:
    int hp;
    int x, y;
    std::string type;

public:
    Personnage(int hp, std::string type) : hp(hp), x(rand() % 20), y(rand() % 20), type(type) {}

    virtual void attaquer(Personnage& autre) = 0;
    virtual void capaciteSpeciale(std::vector<Personnage*>& personnages) = 0;

    bool estVivant() const {
        return hp > 0;
    }

    double distanceAvec(const Personnage& autre) const {
        return sqrt(pow(x - autre.x, 2) + pow(y - autre.y, 2));
    }

    void deplacer(int dx, int dy) {
        x += dx;
        y += dy;
        std::cout << type << " se deplace en (" << x << ", " << y << ").\n";
    }

    std::string getType() const {
        return type;
    }

    void recevoirDegats(int degats) {
        hp -= degats;
        std::cout << type << " recoit " << degats << " points de degats, HP restants : " << hp << "\n";
    }
};

class Guerrier : public Personnage {
public:
    Guerrier() : Personnage(100, "Guerrier") {}

    void attaquer(Personnage& autre) override {
        if (distanceAvec(autre) <= 1.5) {
            autre.recevoirDegats(10);
            std::cout << "Le Guerrier attaque au corps-a-corps.\n";
        }
        else {
            std::cout << "Le Guerrier est trop loin pour attaquer.\n";
        }
    }

    void capaciteSpeciale(std::vector<Personnage*>& personnages) override {
        std::cout << "Le Guerrier utilise sa capacite speciale : attaque puissante !\n";
        for (auto& cible : personnages) {
            if (cible != this && cible->estVivant() && distanceAvec(*cible) <= 1.5) {
                cible->recevoirDegats(20);
            }
        }
    }
};

class Mage : public Personnage {
public:
    Mage() : Personnage(70, "Mage") {}

    void attaquer(Personnage& autre) override {
        autre.recevoirDegats(10);
        std::cout << "Le Mage lance une attaque magique.\n";
    }

    void capaciteSpeciale(std::vector<Personnage*>& personnages) override {
        std::cout << "Le Mage utilise sa capacite speciale : attaque double !\n";
        int cibleAttaquees = 0;
        for (auto& cible : personnages) {
            if (cible != this && cible->estVivant()) {
                cible->recevoirDegats(10);
                if (++cibleAttaquees == 2) break;
            }
        }
    }
};

class Guerisseur : public Personnage {
public:
    Guerisseur() : Personnage(80, "Guerisseur") {}

    void attaquer(Personnage& autre) override {
        if (distanceAvec(autre) <= 1.5) {
            autre.recevoirDegats(5);
            std::cout << "Le Guerisseur attaque faiblement.\n";
        }
        else {
            std::cout << "Le Guerisseur est trop loin pour attaquer.\n";
        }
    }

    void capaciteSpeciale(std::vector<Personnage*>&) override {
        std::cout << "Le Guerisseur utilise sa capacite speciale : se soigne !\n";
        hp += 15;
        if (hp > 80) hp = 80;
        std::cout << "HP du Guerisseur apres soin : " << hp << "\n";
    }
};

class Archer : public Personnage {
public:
    Archer() : Personnage(60, "Archer") {}

    void attaquer(Personnage& autre) override {
        if (distanceAvec(autre) <= 5.0) {
            autre.recevoirDegats(8);
            std::cout << "L'Archer attaque a distance.\n";
        }
        else {
            std::cout << "L'Archer est trop loin pour attaquer.\n";
        }
    }

    void capaciteSpeciale(std::vector<Personnage*>& personnages) override {
        std::cout << "L'Archer utilise sa capacite speciale : tir a longue portee !\n";
        for (auto& cible : personnages) {
            if (cible != this && cible->estVivant() && distanceAvec(*cible) <= 8.0) {
                cible->recevoirDegats(12);
            }
        }
    }
};

class Jeu {
private:
    std::vector<Personnage*> personnages;

public:
    ~Jeu() {
        for (auto personnage : personnages)
            delete personnage;
    }

    void init(int nombrePersonnages) {
        srand(time(0));
        for (int i = 0; i < nombrePersonnages; ++i) {
            int classe = rand() % 4;
            switch (classe) {
            case 0: personnages.push_back(new Guerrier()); break;
            case 1: personnages.push_back(new Mage()); break;
            case 2: personnages.push_back(new Guerisseur()); break;
            case 3: personnages.push_back(new Archer()); break;
            }
        }
    }

    void tourDeJeu() {
        for (auto personnage : personnages) {
            if (!personnage->estVivant()) continue;

            int action = rand() % 3;
            switch (action) {
            case 0:  // Deplacer
                personnage->deplacer(rand() % 3 - 1, rand() % 3 - 1);
                break;
            case 1:  // Attaquer
                for (auto& cible : personnages) {
                    if (cible != personnage && cible->estVivant()) {
                        personnage->attaquer(*cible);
                        break;
                    }
                }
                break;
            case 2:  // Utiliser capacite speciale
                personnage->capaciteSpeciale(personnages);
                break;
            }
            if (std::count_if(personnages.begin(), personnages.end(), [](Personnage* p) { return p->estVivant(); }) <= 1) {
                break;
            }
        }
    }

    void lancerJeu(int nombrePersonnages) {
        init(nombrePersonnages);
        while (std::count_if(personnages.begin(), personnages.end(), [](Personnage* p) { return p->estVivant(); }) > 1) {
            tourDeJeu();
            std::cout << "\nAppuyez sur Entree pour passer au tour suivant...\n";
            std::cin.ignore();  // Pause pour attendre l'appui sur Entree
        }
        for (auto p : personnages) {
            if (p->estVivant()) {
                std::cout << p->getType() << " a gagne la bataille royale!\n";
                break;
            }
        }
    }
};

int main() {
    Jeu jeu;
    int nombrePersonnages;
    std::cout << "Entrez le nombre de personnages : ";
    std::cin >> nombrePersonnages;
    std::cin.ignore();  // Ignorer le retour a la ligne apres le nombre
    jeu.lancerJeu(nombrePersonnages);
    return 0;
}

```
