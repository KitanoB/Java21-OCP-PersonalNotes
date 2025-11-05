# Building Blocks 1

### Concepts clés

**Structure du programme**

* Le point d’entrée d’un programme est la méthode main(String\[] args). Sa signature exacte est à mémoriser (public static void main(…)).&#x20;
* Les déclarations package et import sont optionnelles, mais si présentes, l’ordre doit être : package, puis import, puis la déclaration de classe.&#x20;
* On défini une classe via class NomDeClasse { … }. Une classe peut contenir champs (variables d’instance ou statiques), méthodes, constructeurs, blocs d’initialisation, classes imbriquées.

**Types, variables et portée**

* Types primitifs (int, long, float, double, boolean, char, byte, short) et leurs wrappers (Integer, Long, etc.).
* Variables : déclaration (type + nom), initialisation (valeur). Une variable locale doit être initialisée avant usage.
* Portée (scope) : variable locale (méthode), variable d’instance, variable statique. Comprendre ce que “visible ici” veut dire.
* Initialisation : les champs d’instance ont des valeurs par défaut (0, false, null…) si non initialisés explicitement ; les variables locales non.
* Cycle de vie : création d’un objet via new, assignation de références, éventuellement perte de référence puis ramasse-miettes (garbage collection) — on doit être vigilant sur les fuites de références.

**Objets et références**

* Une variable de type référence pointe sur un objet ou vaut null.
* Lorsqu’on affecte une référence à une autre variable, ce sont les deux références qui peuvent pointer sur le même objet : toute modification via l’une est visible par l’autre.
* Comprendre la différence entre : type primitif vs type référence ; objet immuable vs mutable.

**Bonnes pratiques & pièges fréquents**

* Toujours vérifier que la signature de main est correcte (public static void main(String\[] args)). Sinon le programme compile ou s’exécute mal.&#x20;
* Méfiez-vous de l’ordre des déclarations package/import/éventuels autres éléments.
* Soyez clair avec la portée des variables : ne pas utiliser une variable locale non initialisée.
* Connaître quand un objet devient « eligible » pour la GC (perte de toute référence active). Bien que la GC soit automatique, il est utile de comprendre ce qui la déclenche (références nullifiées, portée sortie, etc.).
* Comprendre que les wrappers (Integer, etc.) sont des objets et que les variables primitives et objets ne sont pas interchangeables sans conversion.

#### Mes notes personnelles / Ce à quoi je ferai attention

* Je vais créer un petit programme “HelloWorld” pour m’assurer que je maîtrise bien la signature main et que je comprends le rôle de String\[] args.
* Ensuite, expérimenter avec diverses variables : primitives, wrappers, objets simples, vérifier le comportement de null, de l’assignation de référence.
* Tester la création d’un objet, l’assignation à plusieurs références, puis mise à null, voir ce que j’en comprends côté cycle de vie.
* Me rappeler que l’ordre package → import → class doit être respecté : bon à garder en tête pour les questions “quel code compile” ou “quel programme fonctionne”.
* Noter mes erreurs éventuelles sur cette section : cela m’aidera pour la révision et pour la communauté (ex : “j’ai confondu portée locale et portée d’instance”).
