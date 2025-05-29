---
title: "Analyse du Honeypot"
date: 2025-05-28 00:000 +0800
categories: [Projects, Security]
tags: [Honeypot, Networking]
toc: true
image: assets/img/posts/Analyse_Honeypot/honeypot-trap.png
---

# Analyse d'un Honeypot T-Pot après 5 jours d'activité:

## Dashboard général
<img src="assets/img/posts/Analyse_Honeypot/Dashboard.png" alt= "Dashboard">

Après seulement 5 jours en ligne, le serveur a enregistré plus de 257 000 intrusions. Le trio Cowrie, Dionaea et Honeytrap regroupe à lui seul plus de 80 % des attaques détectées.

En tête : Cowrie, avec près de 94 000 attaques, il sera la référence dans cette analyse. Ce honeypot simule un système UNIX exposé via SSH et Telnet, ce qui en fait une cible privilégiée pour les attaques automatisées.

## 1. 🧑‍💻 Qui attaque ?

### HASSH/IP : Empreintes SSH récurrentes
<img src="assets/img/posts/Analyse_Honeypot/HasshIP.png" alt= "GraphiqueHasshIP">

Le graphique ci-dessus montre que certaines empreintes **HASSH** sont partagées entre plusieurs IP sources, ce qui suggère l’utilisation d’**outils automatisés ou de botnets** pour mener les attaques.

Par exemple, l’empreinte `0a07365c...` est utilisée par **au moins 5 adresses IP différentes**.

### IP Attaquantes & Réputation
<img src="assets/img/posts/Analyse_Honeypot/IPattacker.png" alt= "GraphiqueAttackerIp">
De nombreuses adresses IP impliquées sont classées comme **"Known attackers"**.  
Cela renforce l’idée d’une **campagne automatisée massive** visant principalement le port **SSH**.

<img src="assets/img/posts/Analyse_Honeypot/AttacksByPort.png" alt= "Pass&UserName">

---

## 2. 🔐 Mots de passe et noms d’utilisateur tentés:
<img src="assets/img/posts/Analyse_Honeypot/MdpUsername.png" alt= "SSHattacksport">

---

## 3. 📜 Commandes exécutées & CVE exploitées

Une fois connectés, les attaquants tentent souvent d’exécuter des commandes.  
Ces commandes donnent des indications sur leurs objectifs : téléchargement de malware, persistance, ou exploitation de vulnérabilités.

### commandes:

| Commande                                                     | Occurrences |
|--------------------------------------------------------------|-------------|
| `uname -a`                                                   | 52          |
| `cd ~; chattr -ia .ssh; lockr -ia .ssh`                      | 48          |
| `lockr -ia .ssh`                                             | 48          |
| `cat /proc/cpuinfo \| grep name \| wc -l`                    | 47          |
| `cat /proc/cpuinfo \| grep model \| grep name \| wc -l`      | 46          |
| `cat /proc/cpuinfo \| grep name \| head -n 1 \| awk '{...}'` | 46          |
| `crontab -l`                                                 | 46          |
| `df -h \| head -n 2 \| awk 'FNR == 2 {print $2;}'`           | 46          |
| `free -m \| grep Mem \| awk '{...}'`                         | 46          |
| `ls -lh $(which ls)`                                         | 46          |

**Collecte d'informations système :**  
  Les commandes telles que `uname -a`, `cat /proc/cpuinfo`, `free -m`, et `df -h` sont utilisées pour identifier le système d'exploitation, le matériel, la mémoire et l'espace disque disponibles. Cela permet aux attaquants d'évaluer la valeur de la cible et de déterminer les charges utiles appropriées.

- **Préparation à l'installation de malwares :**  
  Les commandes `crontab -l` et `chattr` suggèrent des tentatives de persistance ou de modification des permissions de fichiers, souvent utilisées pour installer des malwares ou des mineurs de cryptomonnaie.

- **Évaluation de la cible :**  
  L'utilisation de `ls -lh $(which ls)` peut indiquer une vérification de la présence d'outils ou de modifications sur le système, potentiellement pour détecter des honeypots ou des environnements sécurisés.

  ### CVE détectées:

CVE-2019-12263 // CVE-2019-12261 // CVE-2019-12260 //CVE-2019-12255 // CVE-2020-11900

Les vulnérabilités CVE-2019-12255, CVE-2019-12260, CVE-2019-12261 et CVE-2019-12263 font partie de l'ensemble URGENT/11

**Risques associés :**
- Exécution de code à distance (RCE) 
- Déni de service (DoS)
- Propagation silencieuse

La vulnérabilité CVE-2020-11900 fait partie de l'ensemble Ripple20

**Risques associés :**
- Double libération de mémoire (Double Free) 
- Compromission de dispositifs critiques

### ⚠️ Implications pour la sécurité
Ces vulnérabilités permettent à des attaquants de compromettre des dispositifs critiques sans nécessiter d'authentification ni d'interaction utilisateur.

---

## 4. 🧪 Analyse des fichiers téléchargés
<img src="assets/img/posts/Analyse_Honeypot/DownloadsPayloads.png" alt= "PayloadDl">

### Fichier cat.sh
Le fichier `cat.sh` a été téléchargé une fois depuis l'adresse IP 106.248.251.189:33741. Ce fichier est un script shell (.sh) qui, lorsqu'il est exécuté, peut exécuter une série de commandes malveillantes sur le système compromis.

### Fichiers sshd
Cinq fichiers nommés `sshd` ont été téléchargés depuis différentes adresses IP. Ces fichiers sont des copies de l'exécutable SSH daemon (sshd),

### Fonction des malwares
Les fichiers téléchargés et exécutés ont pour objectif principal de :

- **Maintenir l'accès :** En remplaçant les services légitimes comme sshd, les attaquants peuvent revenir sur le système même après un redémarrage ou une tentative de nettoyage.
- **Exécuter des actions malveillantes :** Les scripts comme `cat.sh` peuvent être utilisés pour télécharger et exécuter d'autres malwares, collecter des informations ou perturber le fonctionnement normal du système.

---

## 5. 🔍 Conclusion:
Les attaquants semblent utiliser des outils automatisés pour mener des attaques par Brute Force sur le port SSH, ciblant des identifiants courants. Une fois l'accès obtenu, ils exécutent des commandes pour collecter des informations système, installer des malwares et maintenir l'accès, notamment en remplaçant le service sshd.
L'exploitation de vulnérabilités connues, telles que celles de l'ensemble URGENT/11, permet aux attaquants d'exécuter du code à distance ou de provoquer des dénis de service, facilitant ainsi la compromission de dispositifs critiques sans interaction utilisateur.

Les fichiers malveillants téléchargés, tels que des scripts shell (cat.sh) ou des exécutables ELF (sshd), ont pour objectif de maintenir l'accès au système, d'exécuter des actions malveillantes comme le minage de cryptomonnaies, ou d'alimenter  un reseau de Botnet.

