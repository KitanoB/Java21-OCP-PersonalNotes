# Comprendre les opérators

## 1. Catégories principales d’opérateurs

* Affectation : =, +=, -=, \*=, /=, %=, &=, |=, ^=, <<=, >>=, >>>=
* Arithmétiques : +, -, \*, /, %
* Incrément/Décrément : ++, -- (pré et post)
* Comparaison : ==, !=, <, >, <=, >=
* Logiques : &&, ||, !, &, |, ^
* Bits : &, |, ^, \~, <<, >>, >>>
* Ternaires : ?:
* Instance/type : instanceof, cast

***

## 2. Priorité et ordre d’évaluation

#### L’ordre de priorité important pour l’examen

1. Post-incrément/decrément (x++, x--)
2. Pré-incrément/décrément (++x, --x)
3. Cast, instanceof
4. Multiplicatifs (\*, /, %)
5. Additifs (+, -)
6. Comparaison (<, >, <=, >=, instanceof)
7. Égalité (==, !=)
8. Bits (&, ^, |)
9. Logiques courts-circuits (&&, ||)
10. Ternaire
11. Affectation

#### Ce qu’il faut retenir

*   Les opérateurs ne changent pas l’ordre d’évaluation.

    Les expressions sont toujours évaluées de gauche à droite.
* La priorité influence le regroupement mais jamais l’ordre d’évaluation.

***

## 3. Incrément/Décrément : pré/post

```
int x = 3;
int y = x++ + 2; // y = 5, x = 4
int z = ++x + 2; // z = 7, x = 5
```

Pièges classiques :

* Effet de bord multiple dans une même expression (toujours éviter)
* Interaction avec les tableaux :

```
int[] a = {10, 20};
int v = a[x++]; // index évalué avant incrément
```

***

## 4. Opérateurs d’affectation composés

#### Forme longue vs composée

```
byte b = 1;
b = (byte)(b + 1); // cast explicite nécessaire
b += 1;            // pas de cast nécessaire
```

Pourquoi ?

* += effectue d’abord une conversion implicite vers le type de gauche après calcul.

À retenir :

* Les opérateurs composés sont plus permissifs que l’affectation simple.

***

## 5. Comparaisons : primitives vs références

#### Primitives == compare la valeur.

#### Références == compare la référence, pas l’état de l’objet.

Piège typique :

```
Integer a = 100;
Integer b = 100;     // mêmes objets (cache)
Integer c = 200;
Integer d = 200;     // objets différents
a == b; // true
c == d; // false
```

→ Toujours utiliser .equals() sauf cas particulier.

***

## 6. Logiques courts-circuits vs logiques bit-à-bit

#### &&  et || (courts-circuits)

Ne évaluent la droite que si nécessaire.

```
if (obj != null && obj.isValid()) { } // ligne droite sûre
```

#### & et |

Toujours évalués des deux côtés même pour booléens.

```
if (obj != null & obj.isValid()) { } // risque de NPE
```

#### ^

XOR logique ou bit-à-bit, selon type.



Points à connaître :

* &&, || imposent que les opérandes soient boolean.
* &, |, ^ appliqués sur entiers fonctionnent en mode bit-à-bit.

***

## 7. Opérateurs bitwise

* \~x : négation bit-à-bit
* << : décalage à gauche (remplit par 0)
* \>> : décalage arithmétique (étend le bit de signe)
* \>>> : décalage logique (remplit par 0)



Piège OCP :

*   Les décalages ne sont appliqués que sur les 5 bits de poids faible pour un int,

    6 bits pour un long.



Ex :

```
1 << 35  // équivaut à 1 << (35 % 32) = 1 << 3 = 8
```

***

## 8. Conversions, promotion numérique et opérateurs

Les règles fondamentales qu’Oracle adore utiliser :

#### 1. Promotion arithmétique

Toute opération sur des types plus petits que int → promotion en int.

```
byte a = 1, b = 2;
byte c = (byte) (a + b); // nécessaire
```

#### 2. char,byte,short → promos en int

Même pour l’opérateur + sur char.

```
char c = 'A';
int x = c + 1; // ok
```

***

## 9. Casts et instanceof

#### instanceof (avant Java 14)

```
obj instanceof String
```

#### instanceof avec pattern matching (OCP 17)

```
if (obj instanceof String s) {
    System.out.println(s.length());
}
```

À retenir :

* Le cast implicite via le pattern n’est valide que dans le bloc où il est prouvé.
* Les champs masqués ou les blocs s’exécutant après un return invalident parfois la portée.

***

## 10. Ternaire

```
result = test ? val1 : val2;
```

Points essentiels :

* Le ternaire doit produire un type unifié (union type).
* Le compilateur choisit le type commun le plus spécifique.
* Attention à l’autoboxing dans le ternaire\


Piège :

```
Integer a = null;
int x = (a == null) ? 0 : a; // pas d’unboxing avant le choix du type → ok
```

***

## 11. Expressions complexes et ordre d’évaluation

Java garantit :

* Évaluation strictement gauche → droite
* Application des effets de bord dans cet ordre

Exemple :

```
int x = 1;
int y = x++ + ++x; // (1) + (3) = 4, x = 3
```

Le piège : mélanger plusieurs effets de bord sur la même variable rend la lisibilité et la prévisibilité plus difficiles ; Oracle aime tester la compréhension de la règle.

