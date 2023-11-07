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
