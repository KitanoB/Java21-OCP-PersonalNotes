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

# Création d'objet

### 1. Différences entre déclaration, instanciation et initialisation

```
String name;              // déclaration
name = "Java";            // initialisation
String lang = new String("Java");  // instanciation + initialisation
```

Piège fréquent :

Ne pas confondre création d’un objet (new) avec la déclaration d’une variable.

```
String s;
System.out.println(s);  // Erreur : variable locale non initialisée
```

***

### 2. Variables d’instance vs locales

| Type                    | Valeur par défaut        | Initialisation obligatoire ? |
| ----------------------- | ------------------------ | ---------------------------- |
| Champ (instance/static) | Oui (ex. 0, false, null) | Non                          |
| Locale                  | Non                      | Oui avant utilisation        |

Piège :

```
public class Test {
    int x;
    public static void main(String[] args) {
        int y;
        System.out.println(new Test().x);  // 0
        System.out.println(y);             // Compilation error
    }
}
```

***

### 3. Constructeurs : pièges classiques

#### a. Constructeur implicite

S’il n’y a aucun constructeur défini, le compilateur ajoute un constructeur par défaut sans paramètre.

Mais :

```
public class Dog {
    public Dog(String name) { }
}

Dog d = new Dog();  // Erreur : aucun constructeur sans argument
```

***

#### b. Appel du constructeur parent

* Chaque constructeur appelle implicitement super() (sauf si un autre appel est fait).
* Si la superclasse n’a pas de constructeur sans argument, il faut l’appeler explicitement.

```
class A {
    A(int x) { }
}
class B extends A {
    B() { } // Erreur : super() inexistant
}
```

Solution :

```
B() { super(5); }
```

***

#### c. Ordre d’exécution

1. Champs et blocs statiques de la superclasse
2. Champs et blocs statiques de la sous-classe
3. Champs et blocs d’instance de la superclasse
4. Constructeur de la superclasse
5. Champs et blocs d’instance de la sous-classe
6. Constructeur de la sous-classe



#### d. Faux constructeurs

```
class A {
    public void A() { }
}
```

Bien faire attention aux faux construteurs. Oracle adore ce genre de piège.\
Pour qu'un constructeur soit valide, il ne doit pas contenir de new(), de void, ou autre supercherie de ce type.

```
class A {
    A() { }
}
```

***

### 4. Initialisation d’objets et blocs d’initialisation

```
class Cat {
    { 
      System.out.println("Instance init"); 
    }
    static { 
      System.out.println("Static init"); 
    }
    Cat() { 
      System.out.println("Constructor"); 
    }
}
new Cat();
```

Résultat :

```
Static init
Instance init
Constructor
```

Le bloc static ne s’exécute qu’une seule fois, à la première utilisation de la classe.

***

### 5. Références et objets : deux notions distinctes

```
StringBuilder a = new StringBuilder("hi");
StringBuilder b = a;
b.append(" there");
System.out.println(a); // "hi there"
```

Piège classique : les deux références pointent le même objet.

***

### 6. Variables null

```
String s = null;
System.out.println(s.length()); // NullPointerException
```

Mais attention aux autoboxings piège :

```
Integer i = null;
int x = i;  // NullPointerException lors de l’unboxing
```

***

### 7. Réaffectation de références

```
String a = "x";
String b = a;
a = "y";
System.out.println(b); // "x"
```

Les objets immuables comme String ne changent pas, mais les références peuvent pointer ailleurs.

***

### 8. Appels en cascade

Exemple valide :

```
new StringBuilder("hi").append("!").reverse();
```

Mais attention :

```
StringBuilder sb = new StringBuilder("a");
sb.append("b").deleteCharAt(0).append("c");
System.out.println(sb); // "bc"
```

→ Le même objet est modifié à chaque appel.

Les questions pièges jouent souvent sur la confusion entre chaîne immuable (String) et mutable (StringBuilder).

***

### 9. Création d’objet sans référence

```
new String("test");  // valide mais l’objet devient aussitôt éligible au GC
```

Cela compile et s’exécute, mais aucune référence ne garde l’objet.

***

### 10. Cas particuliers à repérer à l’examen

| Cas                                    | Effet                                            |
| -------------------------------------- | ------------------------------------------------ |
| new dans un bloc statique              | Exécuté une seule fois à la première utilisation |
| Création d’objet dans une méthode      | Créé à chaque appel                              |
| Constructeur privé                     | Interdit la création externe                     |
| Objet créé mais référence réassignée   | Objet précédent devient éligible au GC           |
| this() et super() dans un constructeur | Doivent être la première instruction             |

***

### 11. Exemple de piège complet

```
class A {
    A() { System.out.println("A"); }
}
class B extends A {
    B() {
        this("B");
        System.out.println("2");
    }
    B(String s) {
        System.out.println(s);
    }
}
public class Test {
    public static void main(String[] args) {
        new B();
    }
}
```

Sortie :

```
A
B
2
```

Oracle adore ce type de question :

mix entre constructeurs chaînés, héritage, et ordre d’exécution.
