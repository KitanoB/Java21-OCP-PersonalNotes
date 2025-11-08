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

### Concepts clés

**Structure du programme**

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
* On défini une classe via class `NomDeClasse { … }`. Une classe peut contenir champs (variables d’instance ou statiques), méthodes, constructeurs, blocs d’initialisation, classes imbriquées.

**Types, variables et portée**

* Types `primitifs` (i`nt, long, float, double, boolean, char, byte, short`) et leurs wrappers (I`nteger, Long, etc.`).
* Variables : déclaration (type + nom), initialisation (valeur). Une variable locale doit être initialisée avant usage.
* Portée (`scope`) : variable locale (méthode), variable d’instance, variable statique. Comprendre ce que “visible ici” veut dire.
* Initialisation : les champs d’instance ont des valeurs par défaut (0, false, null…) si non initialisés explicitement ; les variables locales non.
* Cycle de vie : création d’un objet via new, assignation de références, éventuellement perte de référence puis ramasse-miettes (garbage collection) — on doit être vigilant sur les fuites de références.

**Objets et références**

* Une variable de type référence pointe sur un objet ou vaut null.
* Lorsqu’on affecte une référence à une autre variable, ce sont les deux références qui peuvent pointer sur le même objet : toute modification via l’une est visible par l’autre.
* Comprendre la différence entre : type primitif vs type référence ; objet immuable vs mutable.

**Pièges fréquents**

* Toujours vérifier que la signature de main est correcte (public static void main(String\[] args)). Sinon le programme compile ou s’exécute mal.&#x20;
* Méfiez-vous de l’ordre des déclarations package/import/éventuels autres éléments.
* Soyez clair avec la portée des variables : ne pas utiliser une variable locale non initialisée.
* Connaître quand un objet devient « eligible » pour la GC (perte de toute référence active). Bien que la GC soit automatique, il est utile de comprendre ce qui la déclenche (références nullifiées, portée sortie, etc.).
* Comprendre que les wrappers (Integer, etc.) sont des objets et que les variables primitives et objets ne sont pas interchangeables sans conversion.

