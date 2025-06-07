---
title: "Challenge Rootme Web Client 5 à 7"
date: 2025-06-07 00:000 +0800
categories: [Rootme, Web Client]
tags: [CTF, Web]
toc: true
image: assets/img/posts/Rootme/Rootme_WebClient-1-4/Rootme_logo.jpg
---

Sur cette page, je détail la résolution des challenges **Root Me**,  catégorie **Web Client**  :

- **Javascript - Obfuscation 1**  
- **Javascript - Obfuscation 2**
- **Javascript - Native code**  

---

## 1. **Javascript - Obfuscation 1**

Comme pour le troiseme challenge  **JavaScript – Source**, une popup JavaScript apparaît, demandant un mot de passe et empêchant l’accès au site.

On peut saisir n’importe quelle valeur pour faire disparaitre la popup et avoir accés au Devtools.

dans la code HTML on y retrouve le Script java qui contient le Mot de passe obfusqué :

<img src="assets/img/posts/Rootme/Rootme_WebClient-5-7/Challenge5.png" alt= "Challenge5">

En observant ce script, on peut observer que :
- La variable `pass` contient une chaîne encodée.
- Le script affiche une **popup** (prompt) demandant à l'utilisateur de saisir un mot de passe.
- Si la saisie de l'utilisateur (h) correspond à la version décodée de la variable pass, un message indique que le mot de passe est correct. Sinon, un message d'erreur s'affiche.

Il faut donc décoder `%63%70%61%73%62%69%65%6e%64%75%72%70%61%73%73%77%6f%72%64`.

On observe également cette ligne :

`if(h == unescape(pass))`

En consultant les ressources du challenge, on apprend que la fonction `unescape` est une ancienne fonction JavaScript utilisée pour décoder du texte encodé au format URL

Il suffit donc d’appeler cette fonction dans la console DevTools pour obtenir le mot de passe en clair :

<img src="assets/img/posts/Rootme/Rootme_WebClient-5-7/Flag5.png" alt= "Flag5">

---

## 2. **Javascript - Obfuscation 2**

Dans ce challenge, la page web est une page blanche, sans popup affichée à l’entrée.
Nous allons donc directement dans les **DevTools**, pour observer dans le code HTML la présence d’une fonction `unescape`

<img src="assets/img/posts/Rootme/Rootme_WebClient-5-7/Challenge6.png" alt= "Challenge6">

Comme dans le challenge précédent, nous décidons d’appeler la fonction `unescape` dans la console, suivie de la valeur indiquée :

`unescape("unescape%28%22String.fromCharCode%2528104%252C68%252C117%252C102%252C106%252C100%252C107%252C105%252C49%252C53%252C54%2529%22%29")`

Ce qui nous donne comme résultat :

`unescape("String.fromCharCode%28104%2C68%2C117%2C102%2C106%2C100%2C107%2C105%2C49%2C53%2C54%29")`

Ce résultat est un second `unescape`.
Je décide de réitérer l’opération, mais avec cette nouvelle valeur, pour obtenir :

`String.fromCharCode(104,68,117,102,106,100,107,105,49,53,54)`

À ce stade, je ne connais pas la fonction `String.fromCharCode`.
Après quelques recherches, j’apprends qu’il s’agit d’une fonction qui permet de convertir des codes numériques (ASCII ou Unicode) en caractères.

Je décide donc d’appeler cette fonction dans la console, et j’obtiens une suite de caractères qui correspond au mot de passe attendu.



<img src="assets/img/posts/Rootme/Rootme_WebClient-5-7/Flag6.png" alt= "Flag6">

--- 

## 3. **Javascript - Native code**

Ce challenge propose à nouveau une popup Js de connexion 

<img src="assets/img/posts/Rootme/Rootme_WebClient-5-7/Challenge7.png" alt= "Challenge7">

Peu importe la valeur saisie, nous sommes redirigés vers une page blanche. En inspectant le code source, on observe une balise `script` contenant du code JavaScript obfusqué :

<img src="assets/img/posts/Rootme/Rootme_WebClient-5-7/jsscript.png" alt= "jsscript">

En essayant de comprendre le code, je remarque plusieurs  `;`qui me font penser à des déclarations de variables.

Je sépare le code en plusieurs blocs pour obtenir dans un premier temps :
`É=-~-~[],ó=-~É,Ë=É<<É,þ=Ë+~[]`

Je comprends donc que ce code nous donne la valeur de É, ó, Ë et þ.
En déclarant ces variables dans la console, on obtient :
`É=2,ó=3,Ë=8,þ=7;`

À partir de là, je commence à remplacer dans le code les valeurs que je connais. Après plusieurs essais pour comprendre la suite, je bloque et décide de faire une pause.

En revenant, je décide d'exécuter l'intégralité du code obfusqué dans la console DevTools. Le script se lance alors et affiche une popup de connexion.

Je prends le script, l'enregistre dans un fichier .js et l'exécute avec Node.js. Bingo !

Le script, ne pouvant pas interagir avec une interface utilisateur, me retourne une erreur tout en affichant le code qu'il a essayé d'exécuter :

<img src="assets/img/posts/Rootme/Rootme_WebClient-5-7/Flag7.png" alt= "Flag7">

On peut lire que le mot de passe attendu est : `toto123lol`

