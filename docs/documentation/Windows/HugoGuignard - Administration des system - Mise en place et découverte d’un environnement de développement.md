# Mise en place et découverte d’un environnement de développement
**Auteur :** Hugo Guignard  
**Contexte :** CUB  
**Formation :** BTS SIO SISR 2025/2026  

---

## Sommaire

1. [Initialisation de l’environnement Git](#partie-1--initialisation-de-lenvironnement-git)
2. [Configuration initiale de Git](#partie-2--configuration-initiale-de-git)
3. [Gestion des versions en local](#partie-3--gestion-des-versions-en-local)
4. [Gestion des dépôts GitHub à distance](#partie-4--gestion-des-dépôts-github-à-distance)

---

## Partie 1 : Initialisation de l’environnement Git

### Question 1 : Installer la version Windows de Git sur votre serveur
Pour l’installation, il faut se rendre sur le site officiel de Git et l’installer.  
C’est simple et rapide.


---

### Question 2 : Aller dans le Git Bash et tester une première commande
Cette commande permet de savoir la version de Git installée :
```bash
git --version
```

---

### Question 3 : Créer un dossier par le Git Bash sur la racine de votre disque local
On se place dans le disque C puis :
```bash
mkdir git_cub
ls
```
Cela permet de vérifier que le dossier a bien été créé.


---

### Question 4 : Créer un compte à votre nom sur le site GitHub
La création du compte s’est faite sans problème.


---

## Partie 2 : Configuration initiale de Git

### Question 5 : Préparer la configuration initiale de Git en local
```bash
git config --global user.name "Votre Nom"
git config --global user.email "votre@mail.com"
git config --global --list
```
Ces commandes permettent d’enregistrer le nom et l’adresse e-mail et de vérifier que cela à était enregistrer.


---

### Question 6 : Initialiser l’environnement Git local pour un dépôt
```bash
cd C:\git_cub
mkdir premierdepot
cd premierdepot
git init
```
Cela initialise le dépôt local.


---

## Partie 3 : Gestion des versions en local

### Question 7 : Créer le premier script `script0.bat`
Le script contient les actions à exécuter.
```bash
@echo off
echo Ceci est mon premier script sous git
netstat -ano
pause
```
Le contenus du script donc les action qu’il va effectuer.


---

### Question 8 : Valider la première version du script dans la zone tampon
```bash
git add .
```

La commande permet de valider la version du script

---

### Question 9 : Vérifier le statut du dépôt
```bash
git status
```
Cela va permettre de voir le statu du dépôt

---

### Question 10 : Réaliser le premier commit
```bash
git commit -m "Première version du script"
```

La commande va permettre de faire le premier commit donc sauvegarde

---

### Question 11 : Vérifier le statut à nouveau
```bash
git status
```

Aucun problème signalé, on vérifie le dépôt.

---

### Question 12 : Lister les versions du dépôt
```bash
git log
```
On observe la version du dépôt.


---

### Question 13 : Modifier le script
Ajout de la commande :
```bat
ipconfig
```
On ajoute ipconfig au script

---

### Question 14 : Valider la deuxième version du script
```bash
git add .
git commit -m "Ajout de la commande ipconfig"
```

On entre la commande pour enregistrer la modification et les envoyer au dépôt distant

---

### Question 15 : Vérifier le statuts de votre dépôt
La commande pour voir la situation du document :
```bash
git status
```

---

### Question 16 : Réaliser la deuxièmes version du dépôt
Enregistrement de la deuxième version du document : 
```bash
git commit -m "mon deuxieme commit"
```

---

### Question 17 : vérifier le statuts à nouveau de votre dépôt
On vérifie le statuts du dépôt : 
```bash
git status
```

---

### Question 18 : Lister les version de votre dépôt
On vérifie la version des dépôt : 
```bash
git log
```

---

### Question 19 : Revenir à la première version du script
on va voir l'identifiant du depot : 
```bash
git checkout --oneline
```

on tape git checkout et l’identifant pour choisir la version à utiliser : 
```bash
git checkout "identifiant"
```


---

### Question 20 : Vérifier le contenu du script
la commande pour voir le contenus de la première version :
```bash
nano script0.bat
```

---

### Question 21 : Revenir à la dernière version
la commande pour la première version
```bash
git checkout master
```

---

### Question 22 : Lister les versions du dépôt
Deux versions sont présentes on va taper cette commande pour le voir :
```bash
git log
```


---

## Partie 4 : Gestion des dépôts GitHub à distance

### Question 23 : Créer un dépôt sur GitHub
Le dépôt **premierdepot** est créé sur GitHub, On le voit apparaitre sur nos dépôt.

---

### Question 24 : Cloner ce dépôt en local
La commande pour cloner le dépôt en local :
```bash
git clone https://github.com/<utilisateur>/premierdepot.git
```

---

### Question 25 : Vérifier le contenu du dossier cloné
on peut verifier le contenus du dossier
```bash
ls
```
Le dossier est vide.


---

### Question 26 : Copier le script vers ce nouveau dossier
La commande pour copier : 
```bash
cp script0.bat /c/git_cub/premierdepotgithub
```



---

### Question 27 : Vérifier le statut
la commande pour le status et pour voir le contenus :
```bash
git status
ls
```

---

### Question 28 : Réaliser la première version du dépôt distant
La commande pour la première version : 
```bash
git add .
git commit -m "Ajout du script initial"
```

---

### Question 29 : Vérifier à nouveau
La commande pour le statuts : 
```bash
git status
```

Aucun problème détecté.

---

### Question 30 : Pousser et valider le contenu du dépôt
la commande pour pousser la config : 
```bash
git push origin main
```


---

### Question 31 : Réaliser la deuxiémes version de votre dépôt
la commande pour la version : 
```bash
git commit -m "mon deuxieme commit"
```

---

### Question 32 :vérifier le statuts à nouveau de votre dépôt
la commande pour le status : 
```bash
git status
```

---

### Question 33 :Pousser et valider le contenus de votre dépôt
la commande pour le pousser est : 
```bash
git push -u origin main
```



**Fin du document**  
_BTS SIO SISR 2025/2026 - Hugo Guignard_
