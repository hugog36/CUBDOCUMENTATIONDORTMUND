# 2ème Situation : Mise en place et découverte d’un environnement de développement GitHub

**Contexte : CUB**  
**Réalisé par : Lucien BESCOS**  
**SIO2 BLOC 2 Réseaux avancé – Contexte : CUB – Réseau avancé**

---

## Sommaire
- Contexte
- Partie 1 : Installation de l’environnement Git
- Partie 2 : Initialisation de l’environnement
- Partie 3 : Gérer les versions en local
- Partie 4 : Gérer les dépôts sur GitHub à distance

---

## Partie 1 : Installation de l’environnement Git

### Question 1 : Installer la version Windows de Git sur votre serveur
Installation de Git pour Windows.

### Question 2 : Aller dans le Git Bash et tester une première commande
Cette commande permet de savoir la version de Git installée.

### Question 3 : Créer un dossier par le Git Bash sur la racine de votre disque local
Dans le dossier `C:`, on utilise :
```bash
mkdir git_cub
ls
```
Cela permet de vérifier la création du dossier.

### Question 4 : Créer un compte à votre nom sur le site GitHub
Création du compte personnel GitHub pour héberger les dépôts.

---

## Partie 2 : Initialisation de l’environnement

### Question 5 : Préparer la configuration initiale de Git en local
On configure le nom et l’email utilisateur pour tous les projets :
```bash
git config --global user.name "Lucien BESCOS"
git config --global user.email "exemple@mail.com"
```

### Question 6 : Initialiser l’environnement Git local pour un dépôt
```bash
git init
```

---

## Partie 3 : Gérer les versions en local

### Question 7 : Créer le premier script0.bat dans le dossier premierdepot
Création du fichier `script0.bat`.

### Question 8 / 9 : Valider la première version du script et vérifier le statut du dépôt
```bash
git add script0.bat
git status
```

### Question 10 : Réaliser la première version de votre dépôt
```bash
git commit -m "Première version du script0.bat"
```

### Question 11 : Vérifier le statut à nouveau de votre dépôt
```bash
git status
```

### Question 12 : Lister les versions de votre dépôt
```bash
git log
```

### Question 13 : Réaliser et enregistrer une modification du script
Modification de `script0.bat` puis :
```bash
git add script0.bat
git commit -m "Deuxième version du script0.bat"
```

### Question 14 : Valider la deuxième version du script
Fait via le commit ci-dessus.

### Question 15 : Vérifier le statut de votre dépôt
```bash
git status
```

### Question 16 / 17 : Réaliser la deuxième version du dépôt et vérifier le statut
Commit et vérification effectués.

### Question 18 : Lister les versions de votre dépôt
```bash
git log --oneline
```

### Question 19 : Revenir à la première version du script
```bash
git checkout <ID_commit_version1>
```

### Question 20 : Vérifier que le contenu correspond à la première version

### Question 21 : Revenir à la dernière version du script
```bash
git checkout main
```

### Question 22 : Lister les versions de votre dépôt
```bash
git log
```

---

## Partie 4 : Gérer les dépôts sur GitHub à distance

### Question 23 : Revenir sur votre compte GitHub et créer un dépôt
Création du dépôt nommé `premierdepot`.

### Question 24 : Cloner ce nouveau dépôt vers votre machine locale
```bash
git clone <url_du_depot>
```

### Question 25 : Vérifier le contenu du nouveau dossier sur votre machine

### Question 26 : Copier le script0.bat vers ce nouveau dossier
```bash
cp script0.bat premierdepot/
```

### Question 27 : Vérifier le statut à nouveau de votre dépôt
```bash
git status
```

### Question 28 : Réaliser la première version de votre dépôt
```bash
git add .
git commit -m "Première version distante"
```

### Question 29 : Vérifier le statut à nouveau de votre dépôt
```bash
git status
```

### Question 30 : Pousser et valider le contenu de votre dépôt
```bash
git push origin main
```

### Question 31 / 32 : Réaliser la deuxième version de votre dépôt et vérifier le statut
Modification du script, `git add .`, `git commit -m "Deuxième version"` et `git status`.

### Question 33 : Pousser et valider le contenu de votre dépôt
```bash
git push origin main
```

### Question 34 : Finalisation
Le dépôt est à jour sur GitHub.
