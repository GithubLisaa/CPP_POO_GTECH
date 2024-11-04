## **Correction de l'Exercice 1 : Créer une classe Voiture**

### Énoncé :
1. Créer une classe `Voiture` avec les attributs suivants :
   - `marque` (chaîne de caractères)
   - `kilometrage` (entier)
   - `carburant` (flottant représentant le pourcentage restant).
2. Ajouter les méthodes suivantes :
   - `afficherInfos` : affiche les informations sur la voiture.
   - `rouler` : réduit le carburant de 10% à chaque appel et augmente le kilométrage.
3. Utiliser un **constructeur** pour initialiser les attributs, et gérer l'affichage de destruction à l'aide du **destructeur**.

### Solution :

```cpp
#include <iostream>
#include <string>

class Voiture {
private:
    std::string marque;
    int kilometrage;
    float carburant;

public:
    // Constructeur
    Voiture(std::string m, int k, float c) : marque(m), kilometrage(k), carburant(c) {}

    // Destructeur
    ~Voiture() {
        std::cout << "La voiture " << marque << " est détruite." << std::endl;
    }

    // Méthode pour afficher les informations
    void afficherInfos() {
        std::cout << "Marque: " << marque << ", Kilométrage: " << kilometrage
                  << " km, Carburant: " << carburant << "%" << std::endl;
    }

    // Méthode pour simuler la conduite (rouler)
    void rouler() {
        if (carburant >= 10) {
            carburant -= 10;  // Réduit le carburant de 10%
            kilometrage += 100;  // Augmente le kilométrage de 100 km
            std::cout << "Vous avez roulé 100 km. Carburant restant: " << carburant << "%" << std::endl;
        } else {
            std::cout << "Pas assez de carburant pour rouler." << std::endl;
        }
    }
};

int main() {
    // Création d'une instance de Voiture
    Voiture voiture1("Toyota", 5000, 50.0);

    // Afficher les infos initiales
    voiture1.afficherInfos();

    // Simuler la conduite
    voiture1.rouler();
    voiture1.rouler();
    voiture1.rouler();
    voiture1.rouler();  // Essayer de rouler plusieurs fois

    // Afficher les infos finales
    voiture1.afficherInfos();

    return 0;
}
```

### Explication :
- **Constructeur** : Initialise les attributs `marque`, `kilometrage`, et `carburant`.
- **Destructeur** : Informe que la voiture est détruite à la fin du programme.
- **Méthode `afficherInfos`** : Affiche les détails de la voiture (marque, kilométrage, et carburant).
- **Méthode `rouler`** : Réduit le carburant de 10% et augmente le kilométrage de 100 km si le carburant est suffisant. Sinon, un message d'alerte est affiché.

### Exemple de sortie :

```
Marque: Toyota, Kilométrage: 5000 km, Carburant: 50%
Vous avez roulé 100 km. Carburant restant: 40%
Vous avez roulé 100 km. Carburant restant: 30%
Vous avez roulé 100 km. Carburant restant: 20%
Vous avez roulé 100 km. Carburant restant: 10%
Marque: Toyota, Kilométrage: 5400 km, Carburant: 10%
La voiture Toyota est détruite.
```

---

## **Correction de l'Exercice 2 : Implémenter l'encapsulation**

### Énoncé :
1. Modifier la classe `Voiture` pour que tous ses attributs soient privés.
2. Ajouter des méthodes publiques pour modifier et accéder aux attributs.

### Solution :

```cpp
#include <iostream>
#include <string>

class Voiture {
private:
    std::string marque;
    int kilometrage;
    float carburant;

public:
    // Constructeur
    Voiture(std::string m, int k, float c) : marque(m), kilometrage(k), carburant(c) {}

    // Destructeur
    ~Voiture() {
        std::cout << "La voiture " << marque << " est détruite." << std::endl;
    }

    // Méthode pour afficher les informations
    void afficherInfos() {
        std::cout << "Marque: " << marque << ", Kilométrage: " << kilometrage
                  << " km, Carburant: " << carburant << "%" << std::endl;
    }

    // Getter et Setter pour les attributs privés
    std::string getMarque() const {
        return marque;
    }

    int getKilometrage() const {
        return kilometrage;
    }

    float getCarburant() const {
        return carburant;
    }

    void setMarque(const std::string &m) {
        marque = m;
    }

    void setKilometrage(int k) {
        if (k >= 0) {
            kilometrage = k;
        }
    }

    void setCarburant(float c) {
        if (c >= 0 && c <= 100) {
            carburant = c;
        }
    }
};

int main() {
    // Création d'une instance de Voiture
    Voiture voiture1("Honda", 10000, 60.0);

    // Afficher les infos initiales
    voiture1.afficherInfos();

    // Modifier les attributs via les setters
    voiture1.setMarque("Tesla");
    voiture1.setKilometrage(20000);
    voiture1.setCarburant(80.0);

    // Afficher les nouvelles infos après modification
    voiture1.afficherInfos();

    return 0;
}
```

### Explication :
- **Encapsulation** : Les attributs `marque`, `kilometrage`, et `carburant` sont déclarés en `private`. Ils ne peuvent être modifiés ou accédés directement de l'extérieur.
- **Getters et Setters** : Des méthodes publiques (`getMarque`, `getKilometrage`, `getCarburant`, `setMarque`, `setKilometrage`, `setCarburant`) sont créées pour accéder et modifier les attributs privés de manière sécurisée.
- Les setters valident les valeurs avant de les accepter (par exemple, `kilometrage` ne peut pas être négatif et `carburant` doit être compris entre 0 et 100).

### Exemple de sortie :

```
Marque: Honda, Kilométrage: 10000 km, Carburant: 60%
Marque: Tesla, Kilométrage: 20000 km, Carburant: 80%
La voiture Tesla est détruite.
```

---

### **Résumé des points importants :**
- L'encapsulation protège les données et empêche l'accès direct aux attributs via des getters et setters.
- L'utilisation de **constructeurs** et **destructeurs** permet de gérer l'initialisation et la destruction des objets.
- Les méthodes définissent le comportement des objets, permettant ainsi de modifier les données de manière contrôlée.
