# Compte-Rendu

## 1. Introduction

### 1.1

Ce document relate le compte-rendu du sujet de Modélisation Mathématiques de la troisième année de BUT Informatique de l'IUT de Blagnac.
Il sera appuyé par le fichier `main.ipynb` qui contient toutes les sources utilisées pour la réalisation de ce document.

### 1.2 Consignes

Vous étudierez les bonnes pratiques au quotidien sur les mots de passe avec une présentation rapide d’outils de gestions comme KeePass et de plugin de navigateurs utiles.

Vous aborderez ensuite la notion de robustesse de ceux-ci avec les notions d’entropie et complexité.

Vous présenterez les principaux générateurs de nombres pseudo-aléatoires en donnant des exemples concrets dans des langages comme Python, Java ou encore des frameworks connus. Vous aborderez aussi la génération de nombres aléatoires dans les systèmes d’exploitation avec /dev/random et /dev/urandom sous Linux (voir https://lwn.net/Kernel/Index/#Random_numbers et https://www.2uo.de/myths-about-urandom/).

Vous expliquerez et conduirez des tests statistiques pour qualifier les nombres obtenus. Vous aborderez le test du khi-deux déjà vu en probas mais aussi d’autres.

## 2. Sommaire

1. Introduction
   1.1. Consignes
2. Sommaire

## 3. Bonne pratiques du quotidien..

### 3.1 .. en tant qu'utilisateur non aguerri

#### 3.1.1. Les pratiques à éviter

| Pratique                                             | Explication                                                | Exemple                                                                       |
| ---------------------------------------------------- | ---------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Utiliser le même mot de passe pour plusieurs comptes | Si un compte est compromis, tous les autres le sont aussi  | Utiliser le même mot de passe pour son compte Facebook et son compte bancaire |
| Utiliser des mots de passe trop simples              | Les mots de passe simples sont plus facilement crackés     | Utiliser le mot de passe `"password"` pour ses comptes                        |
| Utiliser des mots de passe trop courts               | Les mots de passe courts sont plus facilement crackés      | Utiliser le mot de passe `"salut"` pour ses comptes                           |
| Utiliser des patterns dans les mots de passe         | Les patterns sont plus facilement crackés                  | Utiliser le mot de passe `"123456"` pour ses comptes                          |
| Utiliser des dates dans les mots de passe            | Les dates sont plus facilement crackés                     | Utiliser le mot de passe `"1998"` pour ses comptes                            |
| Utiliser des informations personnelles               | Les informations personnelles sont plus facilement crackés | Utiliser le mot de passe `"prenom.nom"` pour ses comptes                      |

#### 3.1.2. Les pratiques à adopter

| Pratique                                       | Explication                                                                       | Exemple                                                                                                     |
| ---------------------------------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Utiliser un générateur de mot de passe         | Les générateurs de mot de passe génèrent des mots de passe aléatoires             | Utiliser des services navigateurs comme Firefox Lockwise ou des services en ligne comme LastPass ou KeePass |
| Utiliser un gestionnaire de mot de passe       | Les gestionnaires de mot de passe stockent les mots de passe de manière sécurisée | Utiliser différents services comme Dashlane, LastPass ou KeePass                                            |
| Utiliser la double authentification            | La double authentification permet de sécuriser les comptes                        | Utiliser la double authentification à l'aide de services comme Authy ou Google Authenticator                |
| Faire un mot de passe de plus de 12 caractères | Les mots de passe longs sont plus difficiles à crackés                            | Utiliser des mots de passe de plus de 12 caractères                                                         |

### 3.2 Présentation d'outils de gestion

#### 3.2.1 KeePass

KeePass est un gestionnaire de mot de passe open-source. Il permet de stocker des mots de passe de manière sécurisée dans une base de données chiffrée. Il est disponible sur Windows, Linux, macOS, Android et iOS.

##### 3.2.1.1 Technologies utilisées

KeePass est écrit en C# et utilise le framework .NET. Il utilise le chiffrement AES et Twofish pour chiffrer les bases de données.

**Qu'est ce que le chiffrement AES ?**

L'AES (Advanced Encryption Standard) est un algorithme de chiffrement symétrique. Il est utilisé pour chiffrer les données de la base de données de KeePass.

**Comment fonctionne l'AES ?**

L'AES fonctionne par blocs de 128 bits. Il utilise une clé de 128, 192 ou 256 bits. Il utilise 10, 12 ou 14 tours de chiffrement en fonction de la taille de la clé.

```py
# Simplified AES implementation in Python

def aes_encrypt(data, key):
    # Split data into 128-bit blocks
    blocks = split_into_blocks(data, 128)

    # Generate round keys
    round_keys = generate_round_keys(key)

    # Perform 10 rounds of encryption
    for i in range(10):
        # Perform SubBytes
        blocks = sub_bytes(blocks)

        # Perform ShiftRows
        blocks = shift_rows(blocks)

        # Perform MixColumns
        blocks = mix_columns(blocks)

        # Perform AddRoundKey
        blocks = add_round_key(blocks, round_keys[i])

    # Perform SubBytes
    blocks = sub_bytes(blocks)

    # Perform ShiftRows
    blocks = shift_rows(blocks)

    # Perform AddRoundKey
    blocks = add_round_key(blocks, round_keys[10])

    # Join blocks
    return join_blocks(blocks)
```

#### 3.2.2 Firefox Lockwise

Firefox Lockwise est un gestionnaire de mot de passe intégré à Firefox. Il permet de stocker des mots de passe de manière sécurisée dans une base de données chiffrée. Il est disponible sur Windows, Linux, macOS, Android et iOS. La base de données est chiffrée avec AES-256.

#### 3.2.3 Dashlane

Dashlane est un gestionnaire de mot de passe propriétaire. Il permet de stocker des mots de passe de manière sécurisée dans une base de données chiffrée. Il est disponible sur Windows, Linux, macOS, Android et iOS.
La base de données est chiffrée également avec AES-256.

_On remarque que le chiffrement AES, particulièrement AES-256, est très utilisé dans les gestionnaires de mot de passe._

## 4. Robustesse des mots de passe

### 4.1 Introduction

Dans la plupart des cas, les mots de passe sont stockés dans une base de données. Il est donc important de les chiffrer pour éviter qu'ils ne soient compromis en cas de fuite de données. De ce postulat on hash les mots de passe. La quasi-totalité des librairies fournissant des méthodes pour hasher un mot de passe ne fournissent que une méthode pour hasher un mot de passe, et une seconde pour comparer un texte avec un hash. L'encryption n'est alord qu'unilatérale, on ne peut pas décrypter un hash pour retrouver le mot de passe.

**Exemple :**

```js
// Javascript

const bcrypt = require("bcrypt");

// Le salage rajoute une information aléatoire au mot de passe pour éviter les attaques par dictionnaire
const saltRounds = 10;
const myPassword = "password";

// Hash le mot de passe
const hash = bcrypt.hashSync(myPassword, saltRounds);

// Compare le mot de passe avec le hash
const isTheGoodPassword = bcrypt.compareSync(myPassword, hash);
```

```js
/**
 * @Notice
 * La méthode `compare` agit uniquement en tant que raccourcis à un code équivalent à:
 */
const compareSyncSimplified(hashedPass, pass) {
    const hashedInput = bcrypt.hashSync(pass, saltRounds);
    // Retourne true si les deux hash sont identiques
    return hashedInput === hashedPass;
}
```

### 4.2 Entropie

#### 4.2.1 Définition

L'entropie est une mesure de la quantité d'information contenue dans un message. Elle est exprimée en bits. Plus un message est complexe, plus son entropie est élevée.

#### 4.2.2 Calcul de l'entropie

L'entropie d'un message peut être calculée à l'aide de la formule suivante :

$$ H = - \sum\_{i=1}^{n} p_i \log_2 p_i $$

Où $p_i$ est la probabilité d'apparition du symbole $i$ dans le message.

#### 4.2.3 Exemple

Soit le message suivant :

$$ M = "abracadabra" $$
$$ n = 11 $$
$$ p*a = \frac{5}{11} $$
$$ p_b = \frac{2}{11} $$
$$ p_c = \frac{1}{11} $$
$$ p_d = \frac{1}{11} $$
$$ p_r = \frac{2}{11} $$
$$ p*{\text{espace}} = \frac{0}{11} $$
$$ p\_{\text{autres}} = \frac{0}{11} $$

$$ H = - \sum\_{i=1}^{n} p_i \log_2 p_i $$

$$ H = - \left( \frac{5}{11} \log_2 \frac{5}{11} + \frac{2}{11} \log_2 \frac{2}{11} + \frac{1}{11} \log_2 \frac{1}{11} + \frac{1}{11} \log_2 \frac{1}{11} + \frac{2}{11} \log_2 \frac{2}{11} + \frac{0}{11} \log_2 \frac{0}{11} + \frac{0}{11} \log_2 \frac{0}{11} \right) $$

$$ H = - \left( \frac{5}{11} \times -2.3219 + \frac{2}{11} \times -2.8074 + \frac{1}{11} \times -3.3219 + \frac{1}{11} \times -3.3219 + \frac{2}{11} \times -2.8074 + \frac{0}{11} \times -\infty + \frac{0}{11} \times -\infty \right) $$

$$ H = - \left( -1.0608 - 0.5116 - 0.3019 - 0.3019 - 0.5116 \right) $$

$$ H = - \left( -2.6878 \right) $$

$$ H = 2.6878 $$

L'entropie du message $M$ est de $2.6878$ bits.

**Ordre de grandeur de l'entropie**

| Entropie | Exemple                                                                        |
| -------- | ------------------------------------------------------------------------------ |
| 0        | Message composé d'un seul symbole                                              |
| 1        | Message composé de deux symboles ayant la même probabilité d'apparition        |
| 2        | Message composé de quatre symboles ayant la même probabilité d'apparition      |
| 3        | Message composé de huit symboles ayant la même probabilité d'apparition        |
| 4        | Message composé de seize symboles ayant la même probabilité d'apparition       |
| 5        | Message composé de trente-deux symboles ayant la même probabilité d'apparition |

Donc notre message $M$ a une entropie de $2.6878$ bits, ce qui est relativement faible.

#### 4.2.4 Entropie de Shannon

L'entropie de Shannon est une mesure de l'entropie d'un message. Elle est exprimée en bits. Plus un message est complexe, plus son entropie de Shannon est élevée. elle gère également le cas où un symbole n'apparait pas dans le message, c'est à dire un cas il exclut les termes relatifs aux caractères absents.

<kbd>[Cf. A. de `main.ipynb`](main.ipynb#a-calcul-de-lentropie)</kbd>

### 4.3 Complexité

#### 4.3.1 Définition

La complexité d'un mot de passe est une mesure de la difficulté à le deviner. Elle est exprimée en bits. Plus un mot de passe est complexe, plus sa complexité est élevée.

#### 4.3.2 Calcul de la complexité

La complexité d'un mot de passe peut être calculée à l'aide de la formule suivante :

$$ C = \log_2 \left( \frac{1}{p} \right) $$

Où $p$ est la probabilité de deviner le mot de passe.

#### 4.3.3 Exemple

Soit le mot de passe suivant :

$$ M = "abracadabra" $$
$$ n = 11 $$
$$ p*a = \frac{5}{11} $$
$$ p_b = \frac{2}{11} $$
$$ p_c = \frac{1}{11} $$
$$ p_d = \frac{1}{11} $$
$$ p_r = \frac{2}{11} $$
$$ p*{\text{espace}} = \frac{0}{11} $$
$$ p\_{\text{autres}} = \frac{0}{11} $$

$$ C = \log_2 \left( \frac{1}{p} \right) $$
$$ C = \log_2 \left( \frac{1}{\frac{5}{11}} \right) $$
$$ C = \log_2 \left( \frac{11}{5} \right) $$
$$ C = \log_2 \left( 2.2 \right) $$
$$ C = 2.1375 $$
$$ C = 2.1375 \text{ bits} $$

La complexité du mot de passe $M$ est de $2.1375$ bits.

<kbd>[Cf. B. de `main.ipynb`](main.ipynb#b-calcul-de-la-complexité)</kbd>

**Ordre de grandeur de la complexité**

| Complexité | Exemple                                                                             |
| ---------- | ----------------------------------------------------------------------------------- |
| 0          | Mot de passe composé d'un seul symbole                                              |
| 1          | Mot de passe composé de deux symboles ayant la même probabilité d'apparition        |
| 2          | Mot de passe composé de quatre symboles ayant la même probabilité d'apparition      |
| 3          | Mot de passe composé de huit symboles ayant la même probabilité d'apparition        |
| 4          | Mot de passe composé de seize symboles ayant la même probabilité d'apparition       |
| 5          | Mot de passe composé de trente-deux symboles ayant la même probabilité d'apparition |

Donc notre mot de passe $M$ a une complexité de $2.1375$ bits, ce qui est relativement faible.

## 5. Générateurs de nombres pseudo-aléatoires

**Lexique :**

- `PRNG` - Pseudo-Random-Number-Generator (_Générateur de nombres pseudos-aléatoires_)
- `LGC` - Linear Congruential Generators (_Générateurs congruentiels linéaires_)

### 5.1 Générateurs congruentiels linéaires

#### 5.1.1 Définition

Le générateur congruentiel linéaire est un des plus anciens générateurs de nombres pseudo-aléatoires mais pour autant très simple à comprendre; il est basé sur une suite récurrente linéaire définie par la relation de récurrence suivante :

$$ X\_{n+1} = (aX_n + c) \mod m $$

C'est à dire :

- Multiplier le nombre précédent par un nombre $a$.
- Ajouter un nombre $c$.
- Prendre le reste de la division euclidienne par un nombre $m$.

<kbd>[Cf. C.1 de `main.ipynb`](main.ipynb#C-générateurs-congruentiels-linéaires)</kbd>

Avec le LCG, on peut remarquer certains problèmes :

- Tous les nombres sont impairs, nous ne touchons donc pas du tout uniformément tous les nombres (tout modèle observable étant l'antithèse du hasard).

- La taille de l'échantillon est très limitée - on peut remarquer qu'on boucle sur les mêmes nombres.

Tous ces problèmes sont issues des paramètres entrés dans le LCG. C'est ici qu'intervient sa version plus récente qu'est le LCG Drand48

<kbd>[Cf. C.2 de `main.ipynb`](main.ipynb#C.2-fonction-de-génération-de-nombres-aléatoires-lcg-drand48)</kbd>

Dans le `Drand48 LCG`, nous n'obtenons pas seulement le numéro suivant dans la séquence, ici nous décalons tous les bits de 16 places vers la droite, les 16 bits les plus à droite étant supprimés en conséquence.

Exemple de shift de bits :

```py
# Si nous avons `1017 >> 3`
    # 1017 = 11 1111 1001 en base 2
    # Avec le shift de 3 bits vers la droite, nous obtenons : 111 1111
    # qui représente 127 en base 10

# Le shift de bits aide à avoir un générateur 16 bits avec une sortie de 8 bits
```
