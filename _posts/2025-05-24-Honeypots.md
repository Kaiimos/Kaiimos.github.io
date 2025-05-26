---
title: "Honeypot"
date: 2025-05-25 00:000 +0800
categories: [Projects, Security]
tags: [Honeypot, Networking]
toc: true
image: assets/img/posts/Honeypot/honeypot.png
---

Dans ce projet, je mets en place un **honeypot** complet à l'aide de la solution open source **TPOTCE** développée par Telekom Security. Ce honeypot permet de **détecter, capturer et analyser des tentatives d'intrusion** en temps réel sur un serveur exposé à Internet.

➡️ <a href="https://github.com/telekom-security/tpotce" target="_blank"> **T-Pot** </a>

---

# 🛠️ Déploiement de TPOTCE sur un serveur Linux

Le HoneyPot est un All In One Multi Honeypot Platform hébergée sur un serveur Linux chez Linode. J'ai utilisé MobaXterm pour la mise en place et l'installation de T-Pot, en suivant la documentation du projet GitHub."

## 1. Création du serveur

Le honeypot est déployé sur un serveur **Linux distant**, hébergé chez Linode.  
Connexion en SSH 
<img src="assets/img/posts/Honeypot/Serveur.png" alt= "Serveur">

## 2. Mise à jour du serveur

```bash
apt update -y && apt upgrade -y
apt install git -y
```
## 3. Installation de Docker et Docker Compose

Pour simplifier l’installation, j’utilise un script proposé par kitpro :
```bash
nano install-docker.sh
```
Copier le contenu du script depuis :
https://wiki.kitpro.us/en/articles/docker-script

```bash
#!/bin/bash

# Quick Install Script for Docker and Docker Compose on Ubuntu Server 24.04 LTS

# Update the package index and install prerequisites
echo "Updating package index and installing prerequisites..."
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release

# Add Docker's official GPG key and set up the stable repository
echo "Adding Docker's official GPG key and setting up repository..."
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update the package index again and install Docker Engine
echo "Installing Docker Engine..."
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin

# Add the current user to the Docker group
echo "Adding current user to the Docker group..."
sudo usermod -aG docker $USER

# Install Docker Compose
echo "Installing Docker Compose..."
DOCKER_COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
sudo curl -L "https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Verify Docker and Docker Compose Installation
echo "Verifying Docker installation..."
docker --version
echo "Verifying Docker Compose installation..."
docker-compose --version

echo "Docker and Docker Compose installation completed successfully!"
echo "You need to log out and back in for group changes to take effect."
```

```bash
chmod +x install-docker.sh
./install-docker.sh
```

## 4. Clonage du dépôt TPOTCE
```bash
git clone https://github.com/telekom-security/tpotce.git
cd tpotce
```

## 5. Choix de la configuration
```bash
./install.sh
```
J’ai choisi le mode standard (hive).
<img src="assets/img/posts/Honeypot/Config.png"alt= "Config">

Pendant l'installation, on définit aussi :
- Un mot de passe d’administration pour l’interface web
- Le port SSH personnalisé (64295)
- Le port web d'administration (64297)

<img src="assets/img/posts/Honeypot/Install.png" alt= "Install">

## 6. Redémarrage du serveur

## 7. 🌐 Accès à l'interface web TPOT

Une fois le serveur redémarré, on peut accéder à l’interface via :
https://<adresse ip>:64297
Je me connecte avec les identifiants définis plus tôt :

<img src="assets/img/posts/Honeypot/Web.png" alt= "Web">
<img src="assets/img/posts/Honeypot/tpot.png" alt= "tpot">


Nous pouvons désormais accéder à plusieurs options, comme par exemple la carte des attaques :
<img src="assets/img/posts/Honeypot/Mapattack.png" alt="Carte d’attaque Mapattack">

## 8. 🔒 À venir : Analyse des données collectées

Une fois le honeypot en production, je prévois une seconde partie apres plusieurs jours / semaines pour :
- Analyser les logs
- Visualiser les attaques
- Identifier les comportements suspects
