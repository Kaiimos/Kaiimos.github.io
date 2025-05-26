---
title: "Hangman game"
date: 2025-05-21 00:000 +0800
 00:000 +0800
categories: [Exercises, Python]
tags: [Python, Game]
toc: true
image: /assets/img/posts//Python-pendu/python_classic.jpeg
---

Pendant ma formation sur la plateforme Dyma, on m’a demandé de créer un jeu du Pendu entièrement en Python. En plus des fonctionnalités de base, j’ai ajouté plusieurs fonctionnalités supplémentaires.

<a href="https://github.com/Kaiimos/Hangman_Game" target="_blank">--> **Github** <--</a>



# Hangman-Game.py

---
<video autoplay loop muted playsinline style="max-width: 600px;">
  <source src="/assets/video/HangmanGame.mkv" type="video/mp4">
  Your browser does not support HTML5 video.
</video>

## V1 - Initial Version

1. **Choix du mot à deviner**  
Le jeu sélectionne aléatoirement un mot à partir d'une liste codée et stockée directement dans le code :
   ```python
   mots = ["python", "programmation", "ordinateur", "intelligence"]
   mot_a_deviner = random.choice(mots)
   ```

2. **Initialisation du jeu**  
   Des variables sont créées pour suivre les lettres devinées  (`lettres_trouvees`), les lettres déjà essayées (`lettres_essayees`), et le nombre de tentatives restantes (`chances`):

   ```python
   lettres_trouvees = ["_" for _ in mot_a_deviner]
   lettres_essayees = set()
   chances = 6
   ```

3. **Tour du joueur**  
   La boucle continue tant que le joueur a encore des chances et que le mot n’a pas été complètement deviné. À chaque tour, le joueur doit deviner une lettre :
   ```python
   lettre = input("Devinez une lettre : ").lower()
   ```

4. **Validation**  
  On vérifie si la saisie est valide (une seule lettre, non déjà essayée).

   ```python
   if not lettre.isalpha() or len(lettre) != 1:
       print("Veuillez entrer une seule lettre.")
   ```   

5. **Vérification de la lettre**  
   Si la lettre est dans le mot, elle est révélée. Sinon, le joueur perd une tentative.

   ```python
   if lettre in mot_a_deviner:
       for i, l in enumerate(mot_a_deviner):
           if l == lettre:
               lettres_trouvees[i] = lettre
   else:
       chances -= 1
   ```
6. **Fin de la partie**  
   Le jeu se termine lorsque le joueur devine le mot ou n’a plus de tentatives

   ```python
   if "_" not in lettres_trouvees:
       print("Félicitations ! Vous avez deviné le mot :", mot_a_deviner)
   else:
       print("Vous avez perdu. Le mot était :", mot_a_deviner)
   ```
---

##  V2 - Ajout d’un système de niveaux et de listes de mots externes

### Nouvelles fonctionnalités :
- **Système de niveaux** Les joueurs peuvent choisir entre trois niveaux (Facile, Moyen, Difficile).
- **Liste de mots externe** Les mots sont désormais importés depuis des fichiers externes, ce qui facilite l’ajout de plus de variété.

Voici le code mis à jour pour cette fonctionnalité :

```python
niveau = ""
while niveau not in ["1", "2", "3"]:
    niveau = input("Choisissez un niveau : ").strip()

    if niveau not in ["1", "2", "3"]:
        print("Niveau invalide. Veuillez entrer 1, 2 ou 3.")

niveaux_noms = {
    "1": "Facile",
    "2": "Moyen",
    "3": "Difficile"
}

fichiers_niveaux = {
    "1": "words_list/mots_faciles.txt",
    "2": "words_list/mots_moyens.txt",
    "3": "words_list/mots_difficiles.txt"
}
nom_fichier = fichiers_niveaux[niveau]


with open(nom_fichier, "r", encoding="utf-8") as f:
    mots = [ligne.strip().lower() for ligne in f if ligne.strip()]
```

---

##  V3 - ASCII Art 

Cette version ajoute une représentation visuelle du pendu qui évolue à chaque mauvaise réponse.
Nous utilisons une liste PENDU_PICS contenant toutes les étapes du dessin, de 0 à 6 erreurs. À chaque mauvaise lettre, l’étape suivante est affichée :

```python
print(PENDU_PICS[6 - chances])
```
---


