# Exercices Operateurs plus durs

### Exercice 1 — && vs &, NPE potentielle

```
public class Test1 {
    public static void main(String[] args) {
        String s = null;
        boolean b1 = (s != null) && s.length() > 0;
        boolean b2 = (s != null) &  s.length() > 0;
        System.out.println("b1=" + b1);
        System.out.println("b2=" + b2);
    }
}
```

Question :

Que se passe-t-il à l’exécution ?

* Ligne b1 :
  * && est court-circuitant.
  * s != null est false, donc s.length() n’est jamais appelé.
  * b1 = false, pas d’exception.
* Ligne b2 :
  * & (sur des boolean) n’est pas court-circuitant.
  * s != null est évalué (false), puis s.length() est tout de même évalué.
  * s vaut null → NullPointerException.

Résultat :

* b1=false est calculé, mais le programme s’arrête ensuite sur un NullPointerException lors du calcul de b2.
* b2 n’est jamais affiché.

***

### Exercice 2 — Ternaires, unification de type et autoboxing

```
public class Test2 {
    public static void main(String[] args) {
        Integer a = 1;
        Double  b = 2.0;

        var r1 = true  ? a : b;
        var r2 = false ? a : b;

        System.out.println(r1.getClass().getSimpleName());
        System.out.println(r2.getClass().getSimpleName());
    }
}
```

Question :

Que s’affiche-t-il ? Explique le type réel de r1 et r2.

#### Correction

* Expression ternaire true ? a : b :
  * Types : Integer et Double.
  * Il n’y a pas de type wrapper commun plus spécifique → promotion vers un type numérique commun.
  * La règle : combinaison de wrappers numériques → promotion vers double (primitive), puis boxing éventuel.
  * Donc r1 et r2 sont en réalité de type Double (autoboxing après la promotion primitive).

\
Affichage :

```
Double
Double
```

***

### Exercice 3 — instanceof avec pattern matching et portée

```
public class Test3 {
    public static void main(String[] args) {
        Object obj = "hello";
        if (obj instanceof String s && s.length() > 3) {
            System.out.println(s.toUpperCase());
        } else {
            System.out.println(s.length());
        }
    }
}
```

Question :

Ce code compile-t-il ? Si non, pourquoi ?

#### Correction

* Dans if (obj instanceof String s && s.length() > 3), le pattern s est valide dans la partie droite de && et dans le bloc if.
* Mais dans le else, la variable s n’est pas en portée : elle n’existe que dans le bloc où la condition instanceof String s && ... est vraie.
* Dans le else, le compilateur ne connaît pas s → erreur de compilation.

Ligne fautive :

```
System.out.println(s.length()); // ❌ s ne peut pas être résolu ici
```

***

### Exercice 4 — Affectations composées et overflow silencieux

```
public class Test4 {
    public static void main(String[] args) {
        int x = Integer.MAX_VALUE - 1;
        x += 10;
        System.out.println(x);
    }
}
```

Question :

Quelle est la valeur affichée ?

#### Correction

* Integer.MAX\_VALUE = 2\_147\_483\_647
* x = 2\_147\_483\_646
* x += 10 → addition en int avec overflow, pas d’exception.

Calcul en int :

2\_147\_483\_646 + 10 = 2\_147\_483\_656

En 32 bits signé, cela wrappe :

2\_147\_483\_656 − 2^32 = 2\_147\_483\_656 − 4\_294\_967\_296 = −2\_147\_483\_640



Affichage :

```
-2147483640
```

***

### Exercice 5 — Méthodes, ordre d’évaluation et effets de bord

```
public class Test5 {
    public static void main(String[] args) {
        int x = 1;
        int result = f(x) + f(++x) + f(x++);
        System.out.println("x=" + x + ", result=" + result);
    }

    static int f(int v) {
        System.out.println("f(" + v + ")");
        return v * 10;
    }
}
```

Question :

Qu’affiche le programme, dans l’ordre exact ?

#### Correction

Évaluation strictement de gauche à droite :

1. f(x)
   * x vaut 1
   * affiche f(1)
   * retourne 10
   * x reste 1
2. f(++x)
   * ++x → x devient 2
   * appel f(2)
   * affiche f(2)
   * retourne 20
3. f(x++)
   * x++ → transmet la valeur actuelle 2, puis x devient 3
   * appel f(2)
   * affiche f(2)
   * retourne 20

Somme :

* result = 10 + 20 + 20 = 50
* x = 3 à la fin



Affichage dans l’ordre :

```
f(1)
f(2)
f(2)
x=3, result=50
```

***

### Exercice 6 — Décalages et négatifs

```
public class Test6 {
    public static void main(String[] args) {
        int x = -8;
        int a = x >> 1;
        int b = x >>> 1;
        System.out.println(a);
        System.out.println(b);
    }
}
```

Question :

Quelles valeurs sont affichées pour a puis b ?

#### Correction

*   x = -8 → en binaire (32 bits) :

    11111111 11111111 11111111 11111000

1. x >> 1 : décalage arithmétique (propage le bit de signe, donc des 1 à gauche)
   * Résultat : 11111111 11111111 11111111 11111100 → -4
2. x >>> 1 : décalage logique (remplit par 0 à gauche)
   * Résultat : 01111111 11111111 11111111 11111100
   * Valeur positive : 2\_147\_483\_644



Affichage :

```
-4
2147483644
```

***

### Exercice 7 — Ternaires imbriqués et types

```
public class Test7 {
    public static void main(String[] args) {
        int x = 5;
        var v = x > 0 ? x < 5 ? 1L : 2 : 3.0;
        System.out.println(v);
        System.out.println(v.getClass().getSimpleName());
    }
}
```

Question :

Que s’affiche-t-il ? Quel est le type réel de v ?

#### Correction

Expression :

```
v = x > 0 ? (x < 5 ? 1L : 2) : 3.0;
```

* x = 5
  * x > 0 → true, donc on regarde la branche (x < 5 ? 1L : 2)
  * x < 5 → false → on prend 2

Donc la valeur finale est 2 venant de l’expression interne.\


Type :

*   Branche interne : 1L (long) et 2 (int).

    → unification : long (widening de int vers long).
*   Expression globale : long (résultat du ternaire interne) et 3.0 (double).

    → unification : double (widening de long vers double).\


Donc v est de type Double (autoboxing du double).



Affichage :

```
2.0
Double
```

***
