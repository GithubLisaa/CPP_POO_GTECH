### Exercice 1 : Manager d'objets

1. **Classe Objet** : Créez une classe `Objet` avec un attribut `nom` et un constructeur qui prend un nom en paramètre.
2. **Manager d'instances** : Créez un `ObjetManager` capable d'ajouter et de supprimer dynamiquement des objets.
3. **Testez le Manager** : Créez plusieurs objets et supprimez-les au besoin pour vérifier le bon fonctionnement de la gestion des instances.

#### Code :

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

class Objet {
public:
    std::string nom;

    Objet(const std::string& nom) : nom(nom) {
        std::cout << "Objet " << nom << " créé.\n";
    }

    ~Objet() {
        std::cout << "Objet " << nom << " détruit.\n";
    }
};

class ObjetManager {
private:
    std::vector<Objet*> objets;

public:
    ~ObjetManager() {
        for (auto objet : objets) {
            delete objet;
        }
        objets.clear();
    }

    Objet* ajouterObjet(const std::string& nom) {
        Objet* objet = new Objet(nom);
        objets.push_back(objet);
        return objet;
    }

    void supprimerObjet(Objet* objet) {
        auto it = std::find(objets.begin(), objets.end(), objet);
        if (it != objets.end()) {
            delete *it;
            objets.erase(it);
        }
    }
};

int main() {
    ObjetManager manager;
    Objet* obj1 = manager.ajouterObjet("Objet1");
    Objet* obj2 = manager.ajouterObjet("Objet2");

    manager.supprimerObjet(obj1);

    return 0;  // Les objets restants seront automatiquement détruits
}
```

---

### Exercice 2 : Utilisation de `shared_ptr`

1. **Classe Ressource** : Créez une classe `Ressource` avec un attribut `nom`.
2. **Utilisation de `shared_ptr`** : Créez un programme qui utilise des `shared_ptr` pour partager une instance de `Ressource` entre plusieurs variables.
3. **Affichez le nombre de références** : Vérifiez le nombre de références à différents moments.

#### Code :

```cpp
#include <iostream>
#include <memory>

class Ressource {
public:
    std::string nom;

    Ressource(const std::string& nom) : nom(nom) {
        std::cout << "Ressource " << nom << " créée.\n";
    }

    ~Ressource() {
        std::cout << "Ressource " << nom << " détruite.\n";
    }
};

int main() {
    std::shared_ptr<Ressource> res1 = std::make_shared<Ressource>("Ressource1");
    std::cout << "Nombre de références : " << res1.use_count() << "\n";  // 1

    {
        std::shared_ptr<Ressource> res2 = res1;
        std::cout << "Nombre de références : " << res1.use_count() << "\n";  // 2
    }

    std::cout << "Nombre de références : " << res1.use_count() << "\n";  // 1

    return 0;
}
```

---

### Exercice 3 : Singleton

1. **Classe `ConfigManager`** : Créez un Singleton `ConfigManager` pour gérer la configuration d'une application.
2. **Configuration unique** : `ConfigManager` ne doit avoir qu'une seule instance dans le programme.

#### Code :

```cpp
#include <iostream>
#include <string>

class ConfigManager {
private:
    static ConfigManager* instance;
    std::string nomApplication;
    std::string version;

    // Constructeur privé pour empêcher la création directe
    ConfigManager() : nomApplication("MonApp"), version("1.0") {}

public:
    static ConfigManager* getInstance() {
        if (instance == nullptr) {
            instance = new ConfigManager();
        }
        return instance;
    }

    void afficherConfig() const {
        std::cout << "Nom de l'application : " << nomApplication << "\n";
        std::cout << "Version : " << version << "\n";
    }
};

// Initialisation du pointeur unique à nullptr
ConfigManager* ConfigManager::instance = nullptr;

int main() {
    ConfigManager* config = ConfigManager::getInstance();
    config->afficherConfig();

    return 0;
}
```

**Explications** :
- **Constructeur privé** : Empêche la création directe de `ConfigManager` en dehors de `getInstance`.
- **Méthode `getInstance`** : Gère l'instanciation unique de `ConfigManager`.
