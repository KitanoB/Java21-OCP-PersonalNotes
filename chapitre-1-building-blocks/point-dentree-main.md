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

# Point d'Entrée - Main

### 1. Concepts clés

#### **Structure du programme**

*   Le point d’entrée d’un programme Java est la méthode \


    ```
    public static void main(String[] args).
    ```

Attention, certaines variantes sont possibles et souvent proposées lors des examens (vu en OCA8) comme par exemple:&#x20;

```
public static void main(String... args)
public static void main(String args[])
public final static main(String args[])
public final static main(String... args)
public final static main(String[] args)
public static final main(String args[])
public static final main(String... args)
public static final main(String[] args)
public static final main(final String args[])
public static final main(final String... args)
public static final main(final String[] args)
public static main(final String args[])
public static main(final String... args)
public static main(final String[] args)
```

`final` est un _`modifiers`_ et complètement optionel. Il peut être placé aussi après `static` ou avant sans générer d'erreur de compilation ou d'execution. \
La création d'un tableau de `String` peut être, elle aussi, effectuée de plusieurs manière et donc peut entrainer des variantes dans la définition de cette methode.&#x20;

* **Les déclarations `package` et `import` sont optionnelles**, mais si présentes, l’ordre doit être : `package`, puis import, puis la déclaration de classe.&#x20;
  * PIC -> Package import class
* On défini une classe via `class NomDeClasse { … }.` \
  Une classe peut contenir champs (variables d’instance ou statiques), méthodes, constructeurs, blocs d’initialisation, classes imbriquées.

