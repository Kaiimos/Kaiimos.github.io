---
title: "Challenge Rootme Web Client 8"
date: 2025-06-09 00:000 +0800
categories: [Rootme, Web Client]
tags: [CTF, Web]
toc: true
image: assets/img/posts/Rootme/Rootme_WebClient-1-4/WebClient.png
---

Dans ce challenge, nous allons explorer ce qu’est Webpack.

À ce stade, je ne connais pas encore cet outil. Je fais donc quelques recherches, et voici ce que j’apprends :

Webpack est un bundler pour les applications JavaScript. Il prend tous les fichiers d’un projet (JavaScript, CSS, images, etc.) et les regroupe en un ou plusieurs fichiers optimisés appelés bundles, afin qu’ils puissent être facilement chargés dans un navigateur.

---


Le challenge affiche une page web qui explique la différence entre un canard classique et un canard mandarin, mais rien d’autre ne semble visible à l’écran.

On ouvre alors les DevTools, et on remarque  que le code HTML fait appel à trois fichiers JavaScript :

- manifest.js
- vendor.js
- app.js

<img src="assets/img/posts/Rootme/Rootme_WebClient-8/Challenge8.png" alt= "Challenge8">

Dans l’onglet Network, on peut consulter le contenu de chacun de ces fichiers.

- manifest.js : Il gère l’ordre de chargement des modules, les dépendances internes, et la liaison entre les différents fichiers.
- vendor.js : Contient les bibliothèques tierces (par exemple : Vue, React, Lodash, jQuery, etc.).
- app.js : Contient le code source de l’application — celui que nous allons cibler.

---

# 📦 À propos du code minifié

Quand un projet est webpacké, le code source JavaScript devient minifié.
Cela signifie qu’il est compressé (suppression des espaces, renommage des variables, etc.) pour réduire la taille des fichiers.

 Cela rend la lecture du code plus difficile, et peut cacher certaines informations importantes.

Pour résoudre cela, Webpack ajoute souvent une ligne en bas du fichier minifié comme celle-ci :

`//# sourceMappingURL=app.a92c5074dafac0cb6365.js.map`

<img src="assets/img/posts/Rootme/Rootme_WebClient-8/sourcemappingurl.png" alt= "sourcemappingurl">

---

Dans l’onglet Headers du fichier app.js, on peut voir l’URL du fichier minifié :

`http://challenge01.root-me.org/web-client/ch27/static/js/app.a92c5074dafac0cb6365.js`

<img src="assets/img/posts/Rootme/Rootme_WebClient-8/sourcemap.png" alt= "sourcemap">

Il suffit alors de modifier cette URL par la sourceMappingURL présente à la fin du fichier app.js pour télécharger la source map :

`http://challenge01.root-me.org/web-client/ch27/static/js/app.a92c5074dafac0cb6365.js.map`

En ouvrant ce fichier .map, on peut voir le code source.

Pour la suite du challenge, il suffit de faire une recherche par mot-clé afin de trouver le flag.

<img src="assets/img/posts/Rootme/Rootme_WebClient-8/flag8.png" alt= "Flag">
---
