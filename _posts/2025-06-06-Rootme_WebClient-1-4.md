---
title: "Challenge Rootme Web Client 1 à 4"
date: 2025-06-06 00:000 +0800
categories: [Rootme, Web Client]
tags: [CTF, Web]
toc: true
image: assets/img/posts/Rootme/Rootme_WebClient-1-4/WebClient.png
---

Sur cette page, je détail la résolution des quatres premiers challenges **Root Me**,  catégorie **Web Client**  :

- **HTML – Boutons désactivés**  
- **JavaScript – Authentification**  
- **JavaScript – Source**  
- **JavaScript – Authentification 2**

---

## 1. **HTML – Boutons désactivés**  

Ce premier challenge, intitulé `HTML – Boutons désactivés` indique une manipulation du code HTML.
L’énoncé `Contournement avec style` peut également faire référence aux balises `style` en HTML.

La page du challenge affiche une simple interface web comportant deux champs `input` désactivés :
<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Challenge1.png" alt= "Challenge1">

En inspectant le DevTools de la page on observe deux Balises :
<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/DevTools.png" alt= "DevTools">

`input disabled="" type="text" name="auth-login" value=""`

`input disabled="" type="submit" value="Member access" name="authbutton"`

Il suffit de supprimer les attributs `disabled` de ces deux lignes pour réactiver les champs.
Retournez ensuite sur la page web, remplissez le champ `auth-login`, validez avec `Member access` et le flag apparaît.

<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Flag1.png" alt= "Flag1">

---

## 2. **JavaScript – Authentification**  
Dans ce deuxième challenge, nous avons une page de connexion :
<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Challenge2.png" alt= "Challenge2">

En entrant, par exemple, `test:test`, une popup JavaScript apparaît :
<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Popup2.png" alt= "Popup Js">

En regardant dans le fichier `login`.js du DevTools, à la ligne 8, on peut voir le nom d'utilisateur et le mot de passe inscrit en clair.
Le Flag correspond au Password.
<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Flag2.png" alt= "Flag2">

---

## 3. **JavaScript – Source**
Dans ce troisième challenge, une popup JavaScript apparaît, demandant un mot de passe et empêchant l’accès au site :

<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Popup3.png" alt= "Popup Js connexion">

On peut saisir n’importe quelle valeur, puis valider ou annuler afin d’accéder aux DevTools.
Dans le code source, le mot de passe est visible en clair.

<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Flag3.png" alt= "Flag3">

---

## 4. **JavaScript – Authentification 2**

Dans ce quatrième challenge, le site présente un champ `login`, suivi d'une popup JavaScript demandant un nom d’utilisateur et un mot de passe. Si les identifiants sont incorrects, un message d’erreur s'affiche.

<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Challenge4.png" alt= "Challenge4">

Comme pour les challenges précédents, la solution se trouve dans les DevTools, dans le fichier `login.js`, via l’onglet Network.
On peut y observer une boucle qui vérifie si les identifiants saisis dans la popup correspondent aux valeurs codées en dur à la ligne 4, au format `username:password`.

<img src="assets/img/posts/Rootme/Rootme_WebClient-1-4/Flag4.png" alt= "Flag4">

Le mot de passe sert de Flag.

