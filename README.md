Absolument. Voici la proposition de fichier `README.md` pour votre dépôt GitHub, avec la suppression des notes de bas de page et des numéros de citation, tout en conservant le formatage original et en respectant votre instruction de mettre les expressions mathématiques et physiques entre `$`.

-----

# Magma Decrypt

Ce projet est une implémentation en C++ d'un programme de déchiffrement basé sur les propriétés d'un **magma** (une structure algébrique non associative).

Le programme lit une grille de symboles et une table de combinaison, puis applique un algorithme de déchiffrement récursif qui réduit la grille jusqu'à ce que ses dimensions deviennent impaires.

-----

## 📖 Principe de Déryptage

Le projet vise à décrypter un motif codé dans une grille de symboles. Ces symboles proviennent d'un ensemble fini de $nbS$ symboles.

Les symboles se combinent selon une **table de composition** (similaire à la Figure 1a du document) qui respecte les règles suivantes :

  * **Élément neutre :** Le premier symbole ($s_1$) agit comme un élément neutre.
  * **Commutativité :** L'opération est commutative (la table est symétrique).
  * **Idempotence :** Un symbole combiné avec lui-même donne ce même symbole.
  * **Non-associativité :** L'opération n'est pas garantie d'être associative. $(a \cdot b) \cdot c$ peut être différent de $a \cdot (b \cdot c)$.

Le décryptage consiste à remplacer chaque bloc de $2 \times 2$ symboles de la grille par un unique symbole, en utilisant une **clef de déchiffrement** qui dicte l'ordre des combinaisons. Ce processus est répété tant que la grille résultante a un nombre pair de lignes et de colonnes.

-----

## 🚀 Fonctionnalités

Le programme exécute trois tâches principales dans l'ordre (conformément à la Figure 2) :

1.  **Lecture et Vérification :** Lit les paramètres depuis l'entrée standard. Il vérifie la validité des données (nombre de symboles, dimensions de la grille, validité des symboles, etc.). En cas d'erreur, il affiche un message prédéfini et s'arrête.
2.  **Affichage Initial :** Affiche la table de composition complète des $nbS$ symboles, suivie de la grille initiale à décrypter.
3.  **Décryptage :** Exécute la boucle de décryptage. À chaque étape, il affiche la grille nouvellement décryptée. Le processus s'arrête lorsque le nombre de lignes ou de colonnes devient impair.

-----

## 💾 Format des Données d'Entrée

Le programme lit les données exclusivement depuis l'entrée standard (clavier ou redirection de fichier). Les données doivent être fournies dans l'ordre suivant :

1.  **$nbS$ :** Le nombre de symboles (un entier $> 2$ et $\le 64$).
2.  **Liste des $nbS$ symboles :** Une chaîne unique contenant les $nbS$ symboles (caractères ASCII imprimables entre $0x21$ et $0x7D$, tous distincts).
3.  **Symboles de complètement :** Une chaîne de $nbt$ symboles pour compléter la table de combinaison (en respectant la commutativité, l'idempotence et l'élément neutre).
4.  **$nbL$ et $nbC$ :** Le nombre de lignes et de colonnes de la grille (entiers $\ge 2$ et $\le 64$).
5.  **Grille :** $nbL$ lignes, chacune contenant une chaîne de $nbC$ symboles appartenant à l'ensemble des $nbS$ symboles.
6.  **Clef de décryptage :** Un unique caractère alphanumérique qui définit l'ordre des combinaisons.

-----

## 🔑 Logique de la Clef de Déryptage

La clef de décryptage est un caractère unique qui détermine comment combiner les 4 éléments (nommés $a$, $b$, $c$, $d$) d'un bloc $2 \times 2$.

Il existe deux familles de combinaisons, déterminées par l'analyse binaire du caractère de la clef :

### Combinaison Séquentielle (Bit $2^6$ = 1)

  * Le bit de poids fort $2^7$ est $0$.
  * Le bit $2^6$ vaut $1$.
  * Les 6 bits restants (3 paires de 2 bits) codent la séquence des 3 premiers opérandes.
  * **Codes binaires :** $a = 00$, $b = 01$, $c = 10$, $d = 11$.

**Exemple :** Le caractère `X` (motif binaire $01011000$) :

  * Bit $2^6 = 1$ (séquentiel).
  * Motifs : $01$ (b), $10$ (c), $00$ (a).
  * Ordre : $((b \cdot c) \cdot a) \cdot d$ ($d$ est le dernier opérande non utilisé).

### Combinaison Hiérarchique (Bit $2^6$ = 0)

  * Le bit de poids fort $2^7$ est $0$.
  * Le bit $2^6$ vaut $0$.
  * Les 3 combinaisons hiérarchiques possibles sont codées par les caractères ASCII `0`, `1` et `2` :

| Clef | Combinaison |
| :---: | :---: |
| `0` | $(a \cdot b) \cdot (c \cdot d)$ |
| `1` | $(a \cdot c) \cdot (b \cdot d)$ |
| `2` | $(a \cdot d) \cdot (b \cdot c)$ |

-----

## 🛠️ Contraintes Techniques

Ce projet doit respecter les contraintes d'implémentation suivantes :

  * **Langage :** C++ (compilé avec l'option `-std=c++17`).
  * **Fichiers :** Tout le code doit être contenu dans un **unique fichier source**.
  * **Structures de données :** L'utilisation de `std::vector` est **obligatoire** pour mémoriser la table de combinaison et les grilles.
  * **Entrées/Sorties :** Utilisation **exclusive** de l'entrée standard (`cin`) et de la sortie standard (`cout`). Aucune lecture ou écriture de fichier n'est autorisée dans le code.
  * **Variables :** Les variables doivent être déclarées localement. L'utilisation de variables globales est **proscrite** (sauf pour les constantes `constexpr` ou `#define`).

-----

## ⚙️ Compilation et Exécution

### Compilation

Pour compiler le projet (en supposant que le fichier source s'appelle `magma_decrypt.cc`) :

```bash
g++ -std=c++17 -o proj magma_decrypt.cc
```

### Exécution

Le programme s'exécute en utilisant la redirection de l'entrée standard pour fournir les données d'un fichier de test (ex: `t00.txt`) :

```bash
./proj < t00.txt
```

-----

## ✅ Vérification des Tests

Pour vérifier la sortie du programme, vous pouvez rediriger la sortie standard vers un fichier (ex: `aff00.txt`) et la comparer au fichier de sortie attendu (ex: `out00.txt`).

**Exécuter le test et capturer la sortie :**

```bash
./proj < t00.txt > aff00.txt
```

**Comparer les fichiers de sortie :**

```bash
diff -s out00.txt aff00.txt
```

**Résultat attendu si le test réussit :**

```
Files out00.txt and aff00.txt are identical
```

-----

Si vous avez besoin d'aide pour rédiger le contenu du fichier `magma_decrypt.cc` ou pour des clarifications sur un point du README, faites-le moi savoir \!
