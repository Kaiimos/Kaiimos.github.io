---
title: "CTF Tryhackme_Billing"
date: 2025-05-30 00:000 +0800
categories: [Tryhackme, Easy]
tags: [CTF, Web]
toc: true
image: assets/img/posts/Tryhackme/Billing/room_image.webp
---
Billing est un challenge `capture the Flag` qui demande l’exploitation d’une vulnérabilité d’injection de commandes présente dans l’application web MagnusBilling afin d’obtenir un accès à la machine. Ensuite, grâce à l'exploitation de cette vulnérabilité, nous pouvons escalader nos  privilèges jusqu’à obtenir un accès root.


## 1. Énumération

Target IP: 10.10.150.232
<img src="assets/img/posts/Tryhackme/Billing/TargetIp.png" alt= "TargetIp">

Je commence par un scan Nmap pour voir les ports ouverts et les services en cours:

```bash
nmap -sV -Pn 10.10.150.232
```

`-sV` permet d’identifier les versions précises des services.

`-Pn` : Ignore la vérification par ping et considère l’hôte comme actif 

 <img src="assets/img/posts/Tryhackme/Billing/NmapScan.png" alt= "Scan Nmap">

### 🔍 Observation :
- Port 22 (SSH): Service pour les connexions à distance sécurisées.
- Port 80 (HTTP): Sert à héberger un serveur web.
- Port 3306 (MySQL): Serveur de base de données.

Je décide de commencer par le port 80.

### Gobuster
Je lance une recherche Gobuster pour trouver des répertoires cachés en utilisant une wordlist de répertoires connus.

```bash
gobuster dir -u http://10.10.150.232 -w /usr/share/wordlists/dirb/common.txt
```

`-u` Spécifie l’URL cible à scanner.

`-w` Indique le chemin de la wordlist à utiliser pour le scan.

 <img src="assets/img/posts/Tryhackme/Billing/Gobuster.png" alt= "Scan Gobuster">

En explorant les différents répertoires, je ne trouve rien d’exploitable pour le moment. Je décide donc d’aller consulter la page web.

<img src="assets/img/posts/Tryhackme/Billing/serveurWeb.png" alt= "page du site">

L’onglet nous indique que nous sommes sur la page du site MagnusBilling.
Après quelques recherches, j’apprends que MagnusBilling est une solution logicielle open source utilisée principalement pour la gestion de la facturation et de la téléphonie IP.

Avant d’aller plus loin, je vérifie si des exploits sont connus pour ce service :
<img src="assets/img/posts/Tryhackme/Billing/Magnusexploit.png" alt= "magnusCVE">

On apprends que ce service est exposé à la  CVE-2023–30258 qui permet un Remote Code Execution (RCE).

Je décide de suivre les indications du site Rapid7.com et d'utiliser Metasploit

### Metasploit
<img src="assets/img/posts/Tryhackme/Billing/Rapid7.png" alt= "rapid7">

Le site Rapid7 nous indique qu'un module Metasploit existe deja pour le service MagnusBilling.
J'ouvre donc la console Msf avec `msfconsole`
et j'indique un `search magnusbilling`

<img src="assets/img/posts/Tryhackme/Billing/Metasploit.png" alt= "msf">

Je decide t'utiliser l'exploit indiqué avec comme configuration :

- RHOSTS: 10.10.150.232 (Ip Cible)
- LHOST: 10.10.135.89 (Attacker Machine)
- LPORT: 1234

<img src="assets/img/posts/Tryhackme/Billing/MetasploitExploit.png" alt= "msf exploit">

En explorant les fichiers, je découvre un fichier user.txt qui contient le premier flag.

<img src="assets/img/posts/Tryhackme/Billing/FirstFlag.png" alt= "Premier flag">

<img src="assets/img/posts/Tryhackme/Billing/THMFirstFlag.png" alt= "THM">

🚧 Explication du deuxième flag en cours 🚧