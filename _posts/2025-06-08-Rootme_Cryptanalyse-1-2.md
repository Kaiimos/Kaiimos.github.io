---
title: "Challenge Rootme Cryptanalyse 1 à 2"
date: 2025-06-09 00:000 +0800
categories: [Rootme, Cryptanalyse]
tags: [CTF, Cryptanalyse]
toc: true
image: assets/img/posts/Rootme/Rootme_Cryptanalyse-1-2/Cryptanalyse.png
---

Sur cette page, je détail la résolution des challenges Root Me, catégorie Cryptanalyse :

- **Encodage - ASCII**
- **Encodage - UU**

--- 

## 1. **Encodage - ASCII**


en fichier source, on nous fournit une table de déchiffrement ASCII :

<img src="assets/img/posts/Rootme/Rootme_Cryptanalyse-1-2/ASCIITable.gif" alt= "ASCIITable">

Voici le code à déchiffrer :

`4C6520666C6167206465206365206368616C6C656E6765206573743A203261633337363438316165353436636436383964356239313237356433323465`

D’après le titre du challenge et le fichier source, le code est encodé en ASCII. Il faut maintenant déterminer si ce code est en hexadécimal, décimal ou octal.

- l'Hexadécimal : Utilise les chiffres 0-9 et les lettres A-F (ou a-f). Chaque octet est représenté par deux caractères hex (ex: "4C", "65", "66")
- Décimal: Utilise uniquement les chiffres 0-9, souvent en groupes de 3 chiffres (ex: "065", "108")
- Octal : Utilise uniquement les chiffres 0-7

La chaîne ne contient que des caractères valides en hexadécimal (0-9, a-f), donc le message est encodé en ASCII hexadécimal.

Grâce à la table ASCII, il suffit de remplacer chaque valeur hexadécimale par son caractère correspondant, par exemple :

4C = L

65 = e

etc.

ce qui nous donne :

`Le flag de ce challenge est: 2ac376*********b91275d324e`

---

## 2. **Encodage - UU**

En fichier source, on nous fournit un PDF qui explique plusieurs formats populaires pour encoder des données binaires en texte, notamment uuencoding, xxencoding, Base64 et BinHex.

Pour ce challenge, voici le code à déchiffrer :

```bash
_=_ 
_=_ Part 001 of 001 of file root-me_challenge_uudeview
_=_ 

begin 644 root-me_challenge_uudeview
B5F5R>2!S:6UP;&4@.RD*4$%34R`](%5,5%)!4TE-4$Q%"@``
`
end
```

À l’aide du fichier source, je comprends qu’il s’agit de **UUencoding**.

<img src="assets/img/posts/Rootme/Rootme_Cryptanalyse-1-2/FichierSource.png" alt= "FichierSource">

Le fichier source nous montre aussi la table de déchiffrement :

<img src="assets/img/posts/Rootme/Rootme_Cryptanalyse-1-2/Table.png" alt= " table de déchiffrement">

Mais je décide d’utiliser un décodeur en ligne, ce qui nous donne le flag :

<img src="assets/img/posts/Rootme/Rootme_Cryptanalyse-1-2/Flag2.png" alt= "Flag">
