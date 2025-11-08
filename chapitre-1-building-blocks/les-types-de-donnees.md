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

# Les types de données

### 1. Types primitifs — tailles, conversions et effets de bord

| Type    | Taille          | Exemple de piège                                            |
| ------- | --------------- | ----------------------------------------------------------- |
| byte    | 8 bits          | Overflow silencieux : (byte)130 == -126                     |
| short   | 16 bits         | Attention à la promotion implicite en int                   |
| int     | 32 bits         | Calculs d’indice ou boucle peuvent overflow sans erreur     |
| long    | 64 bits         | Ajouter L sinon int par défaut : long x = 3000000000; // KO |
| float   | 32 bits         | Approximation : 0.1f + 0.2f != 0.3f                         |
| double  | 64 bits         | Précision : 0.1 + 0.2 != 0.3                                |
| char    | 16 bits Unicode | C’est un entier signé (U+0000 à U+FFFF)                     |
| boolean | —               | Jamais converti implicitement en nombre                     |

En pratique :

* Évite les conversions implicites (notamment sur les byte/short).
* Les calculs numériques sont toujours faits au moins en int.
* L’overflow ne déclenche pas d’exception → souvent un bug silencieux.

***

### 2. Conversions et promotion numérique

#### Promotion automatique

```
byte b = 10;
int i = b;      // élargissement
double d = i;   // élargissement
```

#### Réduction (narrowing)

```
int i = 200;
byte b = (byte) i; // overflow (-56)
```

Promotion arithmétique :

Toute opération arithmétique sur des types plus petits que int → résultat en int.

```
byte a = 1, b = 2;
byte c = a + b; // KO car (a + b) est int
byte c = (byte)(a + b); // OK
```

Pertinent IRL : multiplication ou addition sur des compteurs byte/short/char dans du code bas niveau (I/O, protocole, buffer).

***

### 3. Autoboxing / Unboxing : les vrais pièges

#### Ce qui marche :

```
Integer i = 10;      // autoboxing
int x = i;           // unboxing
```

#### Ce qui casse :

```
Integer n = null;
int x = n;  // NullPointerException
```

#### Comparaisons subtiles :

```
Integer a = 100;
Integer b = 100;
System.out.println(a == b); // true (cache -128 à 127)
Integer c = 200;
Integer d = 200;
System.out.println(c == d); // KO false
```

En prod :

Toujours préférer equals() pour comparer des wrappers.

Ne pas abuser d’== hors primitives.

***

### 4. Littéraux numériques et séparateurs

#### Points à retenir :

* \_  (underscore) autorisé entre chiffres, pas au début, à la fin, ni à côté du point ou du suffixe :

```
double x = 1_000.25;    // OK
double y = _1000.25;    // KO
double z = 1000_.25;    // KO

```

* Bases supportées :

```
int a = 0b1010;   // binaire
int b = 07;       // octal
int c = 0xFF;     // hexadécimal
```

Peu d’usage quotidien, mais utile pour lire des tests bas niveau ou du code d’infra.

***

### 5. Casting entre primitives : toujours explicite vers plus petit

```
double d = 9.7;
int i = (int) d;  // OK 9 (troncature, pas arrondi)
```

Pas d’arrondi implicite → erreurs silencieuses fréquentes.

***

### 6. var (depuis Java 10)

* Type inféré au moment de la compilation.
* Ne change rien à la nature statique et typée du langage.\


Exemples :

```
var x = 10;         // int
var s = "hello";    // String
var list = List.of(1,2,3); // List<Integer>
```

Pièges :

```
var n = null;     // ❌ Impossible
var i;            // ❌ Non initialisée
var o = (Object) "hi";  // OK mais type = Object
```

Règle pro : var améliore la lisibilité _si_ le type est évident dans la ligne.

***

### 7. String, immutabilité et optimisations

* String est immuable.
* Les littéraux sont internés (pool de chaînes).
* new String("x") crée un nouvel objet, inutile 99 % du temps.

Exemple :

```
String a = "Java";
String b = "Java";
String c = new String("Java");

System.out.println(a == b); // OK true
System.out.println(a == c); // KO false
```

En pratique : évite new String() et utilise equals() pour comparer.

***

### 8. StringBuilder et StringBuffer

* StringBuilder : mutable, non synchronisé (préféré en général).
* StringBuffer : synchronisé (legacy, utile pour code thread-safe ancien).

```
StringBuilder sb = new StringBuilder("a");
sb.append("b").append("c");
System.out.println(sb);  // "abc" renvoie une nouvelle chaîne à chaque appel.
```

***

### 9. var + immutabilité : piège silencieux

```
var list = List.of(1, 2, 3);
list.add(4); // KO UnsupportedOperationException (liste immuable)
```

Le type inféré ici est List\<Integer>, pas mutable — piégeux si tu crois que c’est un ArrayList.

***

### 10. Points réellement utiles à un ingénieur

| Thème                | À maîtriser vraiment                           |
| -------------------- | ---------------------------------------------- |
| Conversion numérique | overflow silencieux, promotion automatique     |
| Autoboxing           | cache des entiers, NPE sur unboxing            |
| String               | immutabilité, intern pool                      |
| StringBuilder        | mutable et non thread-safe                     |
| var                  | inférence locale, mais typage fort             |
| final                | rend la référence immuable, pas l’objet        |
| Wrapper vs primitive | comportements différents dans les comparaisons |
