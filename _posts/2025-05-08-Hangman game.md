---
title: "Hangman game"
date: 2025-05-08 00:000 +0800
categories: [Exercises, Python]
tags: [Python, Game]
toc: true
image: /assets/img/posts//Python-pendu/python_classic.jpeg
---

During my training on the Dyma platform, I was asked to create a Hangman game entirely in Python. In addition to the basic functionality, I added several extra features.

<a href="https://github.com/Kaiimos/Hangman_Game" target="_blank">--> **Github** <--</a>



# Hangman-Game.py

## 📝 How It Works

The script randomly selects a word from a list, and you must guess it letter by letter.  
You have **6 attempts** before you lose the game.

---

## 🛠️ Improvements

**List of project improvements :**

- **~~💾 Word import from an external file~~**  ✅
- **~~🎯 **Level system~~** ✅
- 🎨 **Hangman drawing** (ASCII art)  
- 🧹 **Cleaner Interface** 

---

## V1 Before Improvements

1. **Choosing a word to guess**  
   The game randomly selects a word from a predefined list:

   ```python
   mots = ["python", "programmation", "ordinateur", "intelligence"]
   mot_a_deviner = random.choice(mots)
   ```

2. **Initializing the game**  
   Variables are created to track the guessed letters (`lettres_trouvees`), the letters already tried (`lettres_essayees`), and the number of remaining attempts (`chances`):

   ```python
   lettres_trouvees = ["_" for _ in mot_a_deviner]
   lettres_essayees = set()
   chances = 6
   ```

3. **Player’s turn**  
   The loop continues as long as the player has chances left and the word has not been fully guessed. On each turn, the player must guess a letter:

   ```python
   lettre = input("Devinez une lettre : ").lower()
   ```

4. **Validating the letter**  
   The game checks if the input is correct (a single letter and not already tried):

   ```python
   if not lettre.isalpha() or len(lettre) != 1:
       print("Veuillez entrer une seule lettre.")
   ```   

5. **Checking the letter**  
   If the letter is in the word, it is revealed. Otherwise, the player loses one attempt:

   ```python
   if lettre in mot_a_deviner:
       for i, l in enumerate(mot_a_deviner):
           if l == lettre:
               lettres_trouvees[i] = lettre
   else:
       chances -= 1
   ```
6. **End of the game**  
   The game ends when the player guesses the word or runs out of attempts:

   ```python
   if "_" not in lettres_trouvees:
       print("Félicitations ! Vous avez deviné le mot :", mot_a_deviner)
   else:
       print("Vous avez perdu. Le mot était :", mot_a_deviner)
   ```
---

##  V.2 Level System and Word List Import

In this version, the game has been improved to allow dynamic word selection, and the player can now choose a difficulty level.

### New Features:
- **Level System:** Players can choose between three levels (Easy, Medium, Hard).
- **External Word List:** Words are now imported from external files, making it easier to add more variety.

Here is the updated code for this feature:

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

1. **Level Selection**  
   If the input is invalid (“4”, “easy”, or blank), an error message is shown. The `.strip()` method cleans any accidental spaces from the input.

   ```python
   niveau = ""
   while niveau not in ["1", "2", "3"]:
       niveau = input("Choisissez un niveau : ").strip()

       if niveau not in ["1", "2", "3"]:
           print("Niveau invalide. Veuillez entrer 1, 2 ou 3.")
   ```

   This while loop ensures the player enters a valid choice:
    - `1` for Easy
    - `2` for Medium
    - `3` for Hard

    Once the level is chosen, the game loads a list of words from an external file corresponding to the selected difficulty. This allows the game to vary the difficulty and offer a greater challenge as the player progresses.

2.  **Difficulty Levels Mapping**  
    This dictionary maps the level number to its corresponding difficulty name:

    ```python
    niveaux_noms = {
        "1": "Facile",
        "2": "Moyen",
        "3": "Difficile"
    }
    ```

3.  **Mapping Levels to Word Lists**  
    This dictionary associates each difficulty level with a specific text file that contains the words for that level:

    ```python
    fichiers_niveaux = {
        "1": "words_list/mots_faciles.txt",
        "2": "words_list/mots_moyens.txt",
        "3": "words_list/mots_difficiles.txt"
    }
    ```

4.  **Loading Words from File**  
    This section loads the words from the selected file based on the difficulty level:

    ```python
    nom_fichier = fichiers_niveaux[niveau]
    with open(nom_fichier, "r", encoding="utf-8") as f:
        mots = [ligne.strip().lower() for ligne in f if ligne.strip()]
    ```

    - `nom_fichier` is the file path corresponding to the selected level.  
    - `with open` reads the file in UTF-8 encoding.  
    - The list comprehension loads the words, strips extra spaces, and converts them to lowercase.  

    This ensures that words are loaded dynamically based on the chosen difficulty level.

---

##  V.3 Hangman drawing (ASCII art) 
work in progress

---

##  V.4 Cleaner interface
work in progress

