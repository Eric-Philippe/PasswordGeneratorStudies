# 1. Mots de passe et nombres aléatoires

Vous étudierez les bonnes pratiques au quotidien sur les mots de passe avec une présentation rapide d’outils de gestions comme KeePass et de plugin de navigateurs utiles.

Vous aborderez ensuite la notion de robustesse de ceux-ci avec les notions d’entropie et complexité.

Vous présenterez les principaux générateurs de nombres pseudo-aléatoires en donnant des exemples concrets dans des langages comme Python, Java ou encore des frameworks connus. Vous aborderez aussi la génération de nombres aléatoires dans les systèmes d’exploitation avec /dev/random et /dev/urandom sous Linux (voir https://lwn.net/Kernel/Index/#Random_numbers et https://www.2uo.de/myths-about-urandom/).

Vous expliquerez et conduirez des tests statistiques pour qualifier les nombres obtenus. Vous aborderez le test du khi-deux déjà vu en probas mais aussi d’autres.

# Compte-Rendu

Merci de vous référer au fichier [Notebook Jupyter](report.ipynb) pour le compte-rendu.

# Bibliographie

- [KeePass](https://keepass.info/)
- [Unix random numbers](https://lwn.net/Kernel/Index/#Random_numbers)
- [Khi deux](https://fr.wikipedia.org/wiki/Test_du_%CF%87%C2%B2)
- [LCG](https://fr.wikipedia.org/wiki/Algorithme_congruentiel_lin%C3%A9aire)
- [LCG](https://en.wikipedia.org/wiki/Linear_congruential_generator)
- [Lava Lamp Encryption](https://www.cloudflare.com/learning/ssl/lava-lamp-encryption/)
- [Advanced Encryption Standard](https://csrc.nist.gov/files/pubs/fips/197/final/docs/fips-197.pdf)

# Lancer le code

Afin de lancer le code, il vous faut Python 3.6+, et un serveur [Jupyter](https://jupyter.org/).

```bash
# Installez les dépendances avec
> pip install -r requirements.txt

# puis lancez le serveur avec
> jupyter notebook.

# Une page web devrait s'ouvrir, cliquez sur le fichier `report.ipynb`
# et lancez les cellules avec le bouton "Run" ou en appuyant sur Shift+Enter
```
