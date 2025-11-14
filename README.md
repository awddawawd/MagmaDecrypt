Voici une proposition de README.md pour un dépôt GitHub, basée sur le document que vous avez fourni.

-----

# Magma Decrypt

Ce projet est une implémentation en C++ d'un programme de déchiffrement basé sur les propriétés d'un [magma](https://fr.wikipedia.org/wiki/Magma_\(math%C3%A9matiques\)) (une structure algébrique non associative).

Le programme lit une grille de symboles et une table de combinaison, puis applique un algorithme de déchiffrement récursif qui réduit la grille jusqu'à ce que ses dimensions deviennent impaires.

## 📖 Principe de Déryptage

[cite\_start]Le projet vise à décrypter un motif codé dans une grille de symboles[cite: 6]. [cite\_start]Ces symboles proviennent d'un ensemble fini de **$nbS$** symboles[cite: 6].

[cite\_start]Les symboles se combinent selon une table de composition (similaire à la Figure 1a du document) qui respecte les règles suivantes[cite: 7]:

  * [cite\_start]**Élément neutre** : Le premier symbole (s1) agit comme un élément neutre[cite: 9].
  * [cite\_start]**Commutativité** : L'opération est commutative (la table est symétrique)[cite: 10].
  * [cite\_start]**Idempotence** : Un symbole combiné avec lui-même donne ce même symbole[cite: 11].
  * **Non-associativité** : L'opération n'est **pas** garantie d'être associative. [cite\_start]$(a \cdot b) \cdot c$ peut être différent de $a \cdot (b \cdot c)$[cite: 12].

[cite\_start]Le décryptage consiste à remplacer chaque bloc de **$2 \times 2$** symboles de la grille par un unique symbole, en utilisant une clef de déchiffrement qui dicte l'ordre des combinaisons[cite: 15]. [cite\_start]Ce processus est répété tant que la grille résultante a un nombre pair de lignes et de colonnes[cite: 16, 191].

-----

## 🚀 Fonctionnalités

[cite\_start]Le programme exécute trois tâches principales dans l'ordre (conformément à la Figure 2)[cite: 129]:

1.  **Lecture et Vérification :** Lit les paramètres depuis l'entrée standard. [cite\_start]Il vérifie la validité des données (nombre de symboles, dimensions de la grille, validité des symboles, etc.)[cite: 130, 131]. [cite\_start]En cas d'erreur, il affiche un message prédéfini et s'arrête[cite: 136].
2.  [cite\_start]**Affichage Initial :** Affiche la table de composition complète des $nbS$ symboles, suivie de la grille initiale à décrypter[cite: 130, 185].
3.  **Décryptage :** Exécute la boucle de décryptage. [cite\_start]À chaque étape, il affiche la grille nouvellement décryptée[cite: 124, 190]. [cite\_start]Le processus s'arrête lorsque le nombre de lignes ou de colonnes devient impair[cite: 16].

-----

## 💾 Format des Données d'Entrée

[cite\_start]Le programme lit les données exclusivement depuis **l'entrée standard** (clavier ou redirection de fichier)[cite: 99, 164]. Les données doivent être fournies dans l'ordre suivant :

1.  [cite\_start]**$nbS$** : Le nombre de symboles (un entier $> 2$ et $\le 64$)[cite: 168].
2.  [cite\_start]**Liste des $nbS$ symboles** : Une chaîne unique contenant les $nbS$ symboles (caractères ASCII imprimables entre $0x21$ et $0x7D$, tous distincts)[cite: 169, 170].
3.  [cite\_start]**Symboles de complètement** : Une chaîne de $nbt$ symboles pour compléter la table de combinaison (en respectant la commutativité, l'idempotence et l'élément neutre)[cite: 171, 172].
4.  [cite\_start]**$nbL$ et $nbC$** : Le nombre de lignes et de colonnes de la grille (entiers $\ge 2$ et $\le 64$)[cite: 175].
5.  [cite\_start]**Grille** : $nbL$ lignes, chacune contenant une chaîne de $nbC$ symboles appartenant à l'ensemble des $nbS$ symboles[cite: 176].
6.  [cite\_start]**Clef de décryptage** : Un unique caractère alphanumérique qui définit l'ordre des combinaisons[cite: 179].

-----

## 🔑 Logique de la Clef de Déryptage

[cite\_start]La clef de décryptage est un caractère unique qui détermine comment combiner les 4 éléments (nommés $a$, $b$, $c$, $d$) d'un bloc $2 \times 2$[cite: 194, 196].

[cite\_start]Il existe deux familles de combinaisons, déterminées par l'analyse binaire du caractère de la clef[cite: 197]:

  * **Combinaison Séquentielle (Bit $2^6$ = 1)**

      * [cite\_start]Le bit de poids fort $2^7$ est 0[cite: 202].
      * [cite\_start]Le bit $2^6$ vaut **1**[cite: 204].
      * [cite\_start]Les 6 bits restants (3 paires de 2 bits) codent la séquence des 3 premiers opérandes[cite: 204].
      * [cite\_start]Codes binaires : $a = 00$, $b = 01$, $c = 10$, $d = 11$[cite: 205].
      * [cite\_start]**Exemple** : Le caractère `X` (motif binaire `01011000`)[cite: 206]:
          * Bit $2^6$ = 1 (séquentiel).
          * Motifs : $01$ (b), $10$ (c), $00$ (a).
          * [cite\_start]Ordre : $((b \cdot c) \cdot a) \cdot d$ (d est le dernier opérande non utilisé)[cite: 206, 207].

  * **Combinaison Hiérarchique (Bit $2^6$ = 0)**

      * [cite\_start]Le bit de poids fort $2^7$ est 0[cite: 202].
      * [cite\_start]Le bit $2^6$ vaut **0**[cite: 208].
      * [cite\_start]Les 3 combinaisons hiérarchiques possibles sont codées par les caractères ASCII `0`, `1` et `2`[cite: 209]:
          * [cite\_start]`0` : $(a \cdot b) \cdot (c \cdot d)$ [cite: 210, 200]
          * [cite\_start]`1` : $(a \cdot c) \cdot (b \cdot d)$ [cite: 209, 200]
          * [cite\_start]`2` : $(a \cdot d) \cdot (b \cdot c)$ [cite: 209, 200]

-----

## 🛠️ Contraintes Techniques

Ce projet doit respecter les contraintes d'implémentation suivantes :

  * [cite\_start]**Langage** : C++ (compilé avec l'option `-std=c++17`)[cite: 215].
  * [cite\_start]**Fichiers** : Tout le code doit être contenu dans un **unique fichier source**[cite: 216].
  * [cite\_start]**Structures de données** : L'utilisation de `vector` est obligatoire pour mémoriser la table de combinaison et les grilles[cite: 217].
  * [cite\_start]**Entrées/Sorties** : Utilisation exclusive de l'entrée standard (`cin`) et de la sortie standard (`cout`)[cite: 99]. [cite\_start]**Aucune lecture ou écriture de fichier** n'est autorisée dans le code[cite: 100].
  * **Variables** : Les variables doivent être déclarées localement. [cite\_start]L'utilisation de variables globales est proscrite (sauf pour les constantes `constexpr` ou `#define`)[cite: 225, 231].

-----

## ⚙️ Compilation et Exécution

### Compilation

Pour compiler le projet (en supposant que le fichier source s'appelle `magma_decrypt.cc`) :

```bash
g++ -std=c++17 -o proj magma_decrypt.cc
```

### Exécution

[cite\_start]Le programme s'exécute en utilisant la redirection de l'entrée standard pour fournir les données d'un fichier de test (ex: `t00.txt`)[cite: 105]:

```bash
./proj < t00.txt
```

-----

## ✅ Vérification des Tests

Pour vérifier la sortie du programme, vous pouvez rediriger la sortie standard vers un fichier (ex: `aff00.txt`) et la comparer au fichier de sortie attendu (ex: `out00.txt`).

1.  [cite\_start]**Exécuter le test et capturer la sortie**[cite: 248]:

    ```bash
    ./proj < t00.txt > aff00.txt
    ```

2.  [cite\_start]**Comparer les fichiers de sortie**[cite: 250]:

    ```bash
    diff -s out00.txt aff00.txt
    ```

3.  [cite\_start]**Résultat attendu si le test réussit**[cite: 252]:

    ```
    Files out00.txt and aff00.txt are identical
    ```
