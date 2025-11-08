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

# Imports

Les règles d’import en Java 17 sont globalement identiques à celles de Java 8.

***

### 1. Les bases inchangées

* Les classes du même package sont accessibles sans import.
*   Les classes du package java.lang sont automatiquement importées.

    → Pas besoin d’écrire import java.lang.String;.
* Pour utiliser une classe d’un autre package, tu dois :
  * L’importer explicitement :

```
import java.util.List;
```

* Ou utiliser son full qualified name :

```
java.util.List list = new java.util.ArrayList<>();
```

***

### 2 . Wildcard \*

*   import java.util.\*; importe toutes les classes et interfaces du package java.util,

    mais pas les sous-packages :

```
import java.util.*;  // Ok pour List, Map, Set...
// Pas ok pour java.util.concurrent.* (sous-package différent)

```

* Il est impossible d'utiliser plusieurs wildcards dans un import comme par exemple `fr.amn.*.*`

***

### 3. Conflits d’imports

Quand deux classes portent le même nom dans des packages différents, il faut être explicite:

```
import java.util.*;
import java.sql.*;   // les deux contiennent une classe Date

Date date;  // ❌ Ambigu — ne compile pas
```

Solution : utiliser le nom complet :

```
java.util.Date utilDate;
java.sql.Date sqlDate;
```

***

### 4. Imports redondants ou inutiles

* Un import est inutile si la classe est déjà dans le même package.
* Un import est redondant si la classe est déjà accessible autrement (ex. java.lang.\*).
* Ce n’est pas une erreur, seulement un _warning_.

Exemple :

```
package myapp;

import myapp.*;      // inutile
import java.lang.*;  // inutile aussi
```

***

### 5. static import

Permet d’importer des membres statiques (méthodes, constantes, etc.) :

```
import static java.lang.Math.PI;
import static java.lang.Math.*;  // importe tout (sin, cos, sqrt...)

System.out.println(PI);
System.out.println(sqrt(25));
```

En cas de conflit de noms, il faut préciser la classe :

```
import static java.lang.Math.*;
import static java.lang.StrictMath.*;

sqrt(4);           // ❌ Ambigu
Math.sqrt(4);      // Ok
```

***

### 6. Ce qui n’est pas à connaîtrepour l’OCP 17

*   Les modules Java (JPMS) utilisent requires, pas import.

    → Ce n’est pas au programme pour cette partie.
* Pas besoin de connaître d’outils de gestion d’imports (IDE, etc.).

