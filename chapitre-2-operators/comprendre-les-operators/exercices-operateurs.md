# Exercices Operateurs

### Exercice 1 — Priorité et incréments

```
int x = 3;
int y = x++ + ++x * 2;
System.out.println(x + ", " + y);
```

#### Correction

1. Évaluation gauche → droite, indépendamment de la priorité.
2. x++ renvoie 3 puis x devient 4.
3. ++x est évalué ensuite → x devient 5 et renvoie 5.
4. Priorité : \* avant + → calcul : 5 \* 2 = 10.
5. Expression complète : 3 + 10 = 13.\


Résultat :

```
x = 5
y = 13
```

Ce que teste Oracle : comprendre que la priorité ne change pas l’ordre d’évaluation, seulement le regroupement.

***

### Exercice 2 — Promotion numérique

```
byte a = 5;
byte b = 10;
byte c = a + b;
System.out.println(c);
```

#### Correction

* a + b → promotion arithmétique en int.
* Résultat : int → assignation à byte non permise.



Le code ne compile pas.

Correction attendue :

```
byte c = (byte)(a + b);
```

Ce que teste Oracle : même deux byte ou char ou short produisent un int.

***

### Exercice 3 — Comparaison de wrappers

```
Integer a = 100;
Integer b = 100;
Integer c = 200;
Integer d = 200;

System.out.println(a == b);
System.out.println(c == d);
```

#### Correction

*   La JVM met en cache les entiers de -128 à 127.

    → a et b pointent le même objet : true.
*   200 est hors cache.

    → c et d sont deux objets différents : false\


Sortie :

```
true
false
```

Ce que teste Oracle : mélange == et autoboxing.

***

### Exercice 4 — Courts-circuits

```
String s = null;
boolean result = (s != null) && s.startsWith("A");
System.out.println(result);
```

#### Correction

* && est un opérateur court-circuitant.
* Comme s != null est false, la partie droite n’est jamais évaluée.
* Pas de NPE.

Sortie :

```
false
```

Ce que teste Oracle : différence entre && et &.

***

### Exercice 5 — Ternaires et unification de type

```
Integer x = null;
int y = (x == null) ? 0 : x;
System.out.println(y);
```

#### Correction

* Les deux branches doivent s’unifier en type.
* Ici : 0 (int literal) et x (Integer).
* Java choisit le type int, car Integer peut être converti par _unboxing_.
* Mais comme la branche x n’est pas évaluée, aucune NPE n’est lancée.



Sortie :

```
0
```

Ce que teste Oracle :

* unification de type dans le ternaire,
* unboxing repoussé au moment où la branche est évaluée.

***

### Exercice 6 — Bits et modulo des décalages

```
int r = 1 << 35;
System.out.println(r);
```

#### Correction

* Déplacement sur int → seul le modulo 32 est pris en compte.
* 35 % 32 = 3 → équivaut à 1 << 3 = 8.

Sortie :

```
8
```

Ce que teste Oracle : règle méconnue du modulo pour <<, >>, >>>.

***

### Exercice 7 — Effets de bord et ordre

```
int a = 1;
int b = a++ + a++ + ++a;
System.out.println(a + ", " + b);
```

#### Correction détaillée

* Expression évaluée strictement gauche → droite :

1. a++ → renvoie 1, a = 2
2. a++ → renvoie 2, a = 3
3. ++a → a = 4, renvoie 4



* Somme : 1 + 2 + 4 = 7.

Sortie :

```
a = 4
b = 7
```

Ce que teste Oracle : accumulation de post/prefix dans une même expression.
