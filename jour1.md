# Cours C++ : Programmation Orientée Objet – Jour 1

## Thème : Les Classes et Objets

---

### Objectifs du jour :
- Comprendre ce qu'est une classe et un objet en C++.
- Apprendre à créer des classes avec des attributs et des méthodes.
- Savoir utiliser des constructeurs et des destructeurs pour initialiser et nettoyer les objets.
- Explorer le concept d'encapsulation et les niveaux de visibilité (`public`, `private`, `protected`).

---

## 1. Introduction à la Programmation Orientée Objet (POO)

La **programmation orientée objet (POO)** est un paradigme de programmation qui repose sur l'utilisation de **classes** et d'**objets** pour modéliser des concepts du monde réel. En C++, ce paradigme permet de structurer le code de manière modulaire et réutilisable.

La POO en C++ repose sur quatre principes fondamentaux : l’encapsulation, l’héritage, le polymorphisme et l’abstraction. L’encapsulation consiste à regrouper les données et les fonctions qui manipulent ces données au sein de classes, protégeant ainsi l'intégrité de l'objet en restreignant l'accès direct à ses attributs. Cela permet une gestion plus sécurisée des données et limite les erreurs potentielles en rendant certains aspects d’un objet inaccessibles depuis l’extérieur. L'héritage, quant à lui, permet de créer de nouvelles classes à partir de classes existantes, facilitant la réutilisation du code et la création de structures hiérarchiques. Grâce à l'héritage, on peut éviter de dupliquer le code et favoriser une organisation plus logique.

Le polymorphisme permet à des objets de différentes classes d'être manipulés à travers une interface commune, offrant ainsi une grande flexibilité et facilitant l'extension des fonctionnalités du programme sans altérer son architecture. En C++, ce concept se manifeste sous forme de polymorphisme statique (surcharge de fonctions) et de polymorphisme dynamique (utilisation de pointeurs et références de classes de base). L’abstraction, enfin, est utilisée pour simplifier des systèmes complexes en exposant uniquement les détails essentiels à l'utilisateur final, laissant les détails internes au sein de la classe. Ce principe permet aux développeurs de se concentrer sur l'interaction avec l'objet plutôt que sur son fonctionnement interne.

La POO apporte de nombreux avantages dans le développement en C++. En structurant le code en objets, elle rend le programme plus intuitif et facile à maintenir. Les classes facilitent la réutilisation du code, permettant de construire des modules indépendants qui peuvent être testés et débogués individuellement, ce qui améliore la fiabilité du code. De plus, grâce à l'encapsulation et à l'abstraction, les modifications peuvent être apportées aux classes sans perturber le reste du programme, réduisant ainsi le risque de bugs. Le C++, grâce à sa prise en charge des pointeurs et des fonctions virtuelles, offre une implémentation efficace du polymorphisme, permettant de concevoir des systèmes extensibles et adaptables. En somme, la POO en C++ fournit des outils puissants pour développer des applications robustes, modulaires et évolutives.

---

## 2. Les Concepts de Base : Classe et Objet

- **Classe** : Une classe est un modèle ou un plan qui définit des **attributs** (données) et des **méthodes** (comportements) d'un certain type d'objet. En d'autres termes, une classe est une abstraction qui regroupe des données et des fonctions.
  
- **Objet** : Un objet est une **instance** d'une classe. Chaque objet possède ses propres valeurs d'attributs mais partage les mêmes méthodes définies dans la classe.

### Exemple : Classe Animal

```cpp
class Animal {
public:
    // Attributs
    std::string nom;
    int age;

    // Méthodes
    void afficherInfos() {
        std::cout << "Nom: " << nom << ", Âge: " << age << std::endl;
    }
};
```

Dans cet exemple, `Animal` est une classe qui a deux **attributs** : `nom` et `age`, et une **méthode** `afficherInfos` pour afficher ces informations.

### Créer un Objet (Instance d'une Classe)

Une fois que la classe est définie, vous pouvez créer des **objets** en déclarant des instances de cette classe :

```cpp
int main() {
    Animal chat;  // Création d'un objet chat
    chat.nom = "Miaou";
    chat.age = 3;
    
    chat.afficherInfos();  // Appel de la méthode afficherInfos()
    
    return 0;
}
```

**Sortie :**
```
Nom: Miaou, Âge: 3
```

---

## 3. Attributs et Méthodes

Les **attributs** sont des variables définies au sein d'une classe et représentent les caractéristiques des objets. Les **méthodes** sont des fonctions définies dans la classe qui permettent aux objets d'effectuer des actions.

- **Attributs** : Variables membres d’une classe.
- **Méthodes** : Fonctions membres qui définissent les comportements de la classe.

### Exemple : Ajout d'une méthode supplémentaire

```cpp
class Animal {
public:
    std::string nom;
    int age;

    void afficherInfos() {
        std::cout << "Nom: " << nom << ", Âge: " << age << std::endl;
    }
    
    void manger() {
        std::cout << nom << " est en train de manger." << std::endl;
    }
};

int main() {
    Animal chien;
    chien.nom = "Rex";
    chien.age = 5;

    chien.afficherInfos();  // Affiche les infos
    chien.manger();         // Affiche que Rex est en train de manger

    return 0;
}
```

**Sortie :**
```
Nom: Rex, Âge: 5
Rex est en train de manger.
```

---

## 4. Constructeurs et Destructeurs

### Constructeur :
Un **constructeur** est une méthode spéciale appelée automatiquement lors de la création d’un objet. Il permet d'initialiser les attributs de la classe.

**Syntaxe :**
Le constructeur a le même nom que la classe et n'a pas de type de retour.

```cpp
class Animal {
public:
    std::string nom;
    int age;

    // Constructeur
    Animal(std::string n, int a) : nom(n), age(a) {}
    
    void afficherInfos() {
        std::cout << "Nom: " << nom << ", Âge: " << age << std::endl;
    }
};

int main() {
    Animal chien("Rex", 5);  // Utilisation du constructeur

    chien.afficherInfos();

    return 0;
}
```

**Sortie :**
```
Nom: Rex, Âge: 5
```

### Destructeur :
Un **destructeur** est une méthode spéciale appelée automatiquement lorsque l'objet est détruit (en fin de programme ou lorsque l'objet sort de la portée). Il permet de libérer des ressources si nécessaire (comme la mémoire allouée dynamiquement).

**Syntaxe :**
Le destructeur a le même nom que la classe, précédé du symbole `~`.

```cpp
class Animal {
public:
    std::string nom;
    int age;

    Animal(std::string n, int a) : nom(n), age(a) {}

    // Destructeur
    ~Animal() {
        std::cout << nom << " est détruit." << std::endl;
    }

    void afficherInfos() {
        std::cout << "Nom: " << nom << ", Âge: " << age << std::endl;
    }
};

int main() {
    Animal chien("Rex", 5);
    chien.afficherInfos();

    // À la fin du programme, le destructeur est automatiquement appelé
    return 0;
}
```

**Sortie :**
```
Nom: Rex, Âge: 5
Rex est détruit.
```

---

## 5. Encapsulation et Visibilité

L'**encapsulation** est un principe fondamental de la POO qui consiste à **protéger les données** d'une classe en contrôlant leur accès. En C++, cela se fait grâce aux modificateurs d'accès : `public`, `private`, et `protected`.

- **public** : Les membres publics sont accessibles de l'extérieur de la classe.
- **private** : Les membres privés ne sont accessibles que depuis les méthodes de la classe elle-même.
- **protected** : Comme `private`, mais accessible aussi par les classes dérivées.

### Exemple : Attributs privés et méthodes publiques

```cpp
class Animal {
private:
    std::string nom;
    int age;

public:
    Animal(std::string n, int a) : nom(n), age(a) {}

    // Méthode pour accéder aux attributs privés
    void afficherInfos() {
        std::cout << "Nom: " << nom << ", Âge: " << age << std::endl;
    }
};

int main() {
    Animal chien("Rex", 5);
    chien.afficherInfos();  // OK
    
    // Erreur si on tente d'accéder à nom directement
    // chien.nom = "Max";  // Erreur: 'nom' est privé

    return 0;
}
```

**Sortie :**
```
Nom: Rex, Âge: 5
```

L'accès direct à `nom` et `age` n'est pas possible car ils sont déclarés comme privés. Il faut utiliser des méthodes publiques pour interagir avec eux, respectant ainsi l'encapsulation.

---

## 6. Exercices Pratiques

### Exercice 1 : Créer une classe Voiture

1. Créer une classe `Voiture` avec les attributs suivants :
   - `marque` (chaîne de caractères)
   - `kilometrage` (entier)
   - `carburant` (flottant représentant le pourcentage restant)

2. Ajouter les méthodes suivantes :
   - `afficherInfos` : affiche les informations sur la voiture
   - `rouler` : réduit le carburant de 10% à chaque appel et augmente le kilométrage

3. Utiliser un **constructeur** pour initialiser les attributs, et gérer l'affichage de destruction à l'aide du **destructeur**.

### Exercice 2 : Implémenter l'encapsulation

1. Modifier la classe `Voiture` pour que tous ses attributs soient privés.
2. Ajouter des méthodes publiques pour modifier et accéder aux attributs.

---

## Résumé du Jour 1

- **Classe** : Plan qui regroupe des attributs et des méthodes.
- **Objet** : Instance d’une classe.
- **Attributs** : Variables membres de la classe.
- **Méthodes** : Fonctions membres définissant le comportement d’un objet.
- **Constructeur** : Méthode spéciale appelée lors de l'initialisation d'un objet.
- **Destructeur** : Méthode spéciale appelée lors de la destruction d'un objet.
- **Encapsulation** : Technique pour restreindre l'accès direct aux attributs via des méthodes publiques.

Ces concepts de base sont essentiels à maîtriser avant d’aborder des sujets plus avancés comme l’héritage et le polymorphisme.
```
