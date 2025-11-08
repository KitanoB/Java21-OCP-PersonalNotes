---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: false
---

# Packages

### 1. Rappel de base

Un package est un espace logique pour organiser les classes.

Déclaré tout en haut du fichier, avant tout autre code :

```
package com.example.utils;

public class Helper { }
```

***

### 2. Le mot-clé  (Reserved Keyword)

Package doit être en première ligne.

C’est un classique de l’examen :

```
// Ne compile pas
import java.util.*;
package com.exam; // Erreur : package doit précéder tout import
```

L’ordre correct :

```
package com.exam;
import java.util.*;
public class Test { }
```

***

### 3. Le package doit correspondre au chemin du fichier

Autre piège typique : le compilateur ne vérifie pas toujours, mais le lanceur (java), lui, si. (Runtime exeception)

Exemple :

```
src/com/app/Main.java
```

Contenu :

```
package com.app;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

Compilé depuis src :

```
javac com/app/Main.java
```

Lancé depuis src :

```
java com.app.Main   // OK
```

Si tu fais :

```
java Main           // ❌ Erreur : class not found
```

***

### 4. Packages par défaut (default package)

S'il n'y a pas de déclaration de package, la classe appartient au package par défaut.

Exemple :

```
public class Hello { }
```

Les classes du package par défaut ne peuvent pas être importées depuis un package nommé.

```
// fichier dans package com.test
package com.test;

// Ne compile pas si Hello est dans le package par défaut
import Hello;
```

***

### 5. Deux classes de même nom dans des packages différents

Examen classique :

```
package a;
public class Dog { }

package b;
public class Dog { }

package zoo;
import a.Dog;
import b.*;  // aussi contient Dog

public class Test {
    Dog d;  // Ambigu — ne compile pas
}
```

Solution : préciser le nom complet :

```
a.Dog d;
```

***

### 6. Visibilité et protected entre packages

Piège fréquent sur la visibilité des membres :

* protected = accessible :
  * dans le même package
  * ou par héritage (même dans un autre package)

Exemple :

```
package animals;
public class Animal {
    protected void eat() { }
}
```

Autre fichier :

```
package zoo;
import animals.Animal;

public class Dog extends Animal {
    void bark() {
        eat(); // ok (héritage)
    }
}
```

Mais :

```
package zoo;
import animals.Animal;

public class Vet {
    void check() {
        Animal a = new Animal();
        a.eat();  // pas accessible : pas même package et pas héritée
    }
}
```

***

### 7. Même package = même nom exact

Le nom complet du package doit être identique, caractère par caractère.

```
package com.app;
```

et

```
package com.App;
```

→ ce sont deux packages différents (Java est sensible à la casse).

***

### 8. Fichiers multiples et compilation

Quand plusieurs fichiers sont dans le même package :

```
// src/com/example/A.java
package com.example;
public class A { }

// src/com/example/B.java
package com.example;
public class B { }
```

Commande :

```
javac com/example/*.java
```

Si package est oublié dans un seul fichier du lot, la compilation échoue.

***

### 9. Organisation typique dans un projet modulaire

En Java moderne (17 et plus), on tend vers une hiérarchie cohérente :

```
com.company.project.module
```

Exemple :

```
package com.mybank.core;
```

Cela évite les conflits avec d’autres librairies (bon à savoir pour l’examen).
