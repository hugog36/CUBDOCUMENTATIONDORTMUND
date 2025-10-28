# Documentation BTS SIO SISR  
### Réalisée par **Hugo GUIGNARD** & **Lucien BESCOS**

Lycée Paul-Louis Courier – Tours  
Années scolaires **2023 – 2025**

---

## Introduction

Bienvenue sur notre site de documentation réalisé dans le cadre du **BTS SIO (Services Informatiques aux Organisations)**, option **SISR** (*Solutions d’Infrastructure, Systèmes et Réseaux*).  
Ce site regroupe l’ensemble de nos **documentations pédagogiques, projets et travaux pratiques** réalisés au cours de nos deux années de formation.

L’objectif est de proposer une **bibliothèque technique complète et organisée** accessible depuis n’importe où, présentant nos **acquis, nos compétences et nos réalisations**.

La navigation se fait grâce au **menu situé à gauche ou en haut de la page** :  
- Chaque catégorie correspond à un **thème abordé en cours ou en atelier** (réseau, système, sécurité, projets, etc.),  
- Les sous-pages contiennent les **fiches techniques, captures d’écran, explications et commandes** associées à chaque sujet.

---

## 🧠 Objectifs du projet

Ce site a plusieurs finalités :

- Centraliser toutes nos **productions techniques** (TP, AP, projets, configurations).  
- Faciliter la **révision** pour les épreuves E4 et E5 du BTS SIO.  
- Mettre en avant nos **compétences professionnelles** auprès d’entreprises ou recruteurs.  
- Offrir une **documentation claire et accessible** à d’autres étudiants souhaitant comprendre les manipulations réseau et système.

---

## ⚙️ Comment nous avons créé nos documentations

L’ensemble de nos documents ont été rédigés à partir des **travaux réalisés en atelier de professionnalisation, TP, et projets personnels**.  
Chaque sujet (ex : DNS, Active Directory, routage, sécurité) a été traité de manière **structurée et reproductible** :

1. **Réalisation technique** sur environnement réel ou virtualisé :  
   - Serveurs Windows et Linux (Debian, Ubuntu Server, Windows Server 2019).  
   - Outils de supervision, DNS, DHCP, routage, Active Directory, etc.

2. **Documentation pédagogique** :  
   Chaque manipulation a été décrite dans un document Markdown comprenant :
   - Le **contexte** du travail,  
   - Les **commandes utilisées**,  
   - Les **captures d’écran** des résultats,  
   - Les **analyses techniques et explications**.

3. **Mise en forme en Markdown** :  
   Nous avons utilisé la **syntaxe Markdown** (`.md`) pour rédiger des fichiers simples à maintenir et lisibles aussi bien dans un éditeur que sur GitHub.

4. **Organisation du contenu** :  
   Toutes les documentations ont été regroupées dans un dossier `docs/` selon leur thématique :
   ```
   docs/
   ├── DNS.md
   ├── Windows.md
   ├── Parefeu.md
   ├── ActiveDirectory.md
   └── ...
   ```

---

## 🌐 Création et hébergement du site

Une fois les documentations rédigées, nous avons voulu les rendre **accessibles en ligne sous forme de site web**.  
Pour cela, nous avons utilisé une **chaîne complète d’outils modernes** :

### 🔧 Outils utilisés
- **MkDocs** → Générateur de site statique à partir de fichiers Markdown.  
- **Material for MkDocs** → Thème moderne et responsive, idéal pour la documentation technique.  
- **GitHub Actions** → Automatisation du déploiement du site à chaque mise à jour.  
- **GitHub Pages** → Hébergement gratuit et sécurisé du site web.

### 🧩 Étapes techniques
1. Création du fichier **`mkdocs.yml`** à la racine du projet pour définir la structure du site et le menu de navigation.  
2. Ajout d’un **workflow GitHub Actions (`ci.yml`)** permettant de :
   - Installer MkDocs et ses dépendances,
   - Générer le site,
   - Déployer automatiquement sur la branche `gh-pages`.  
3. Chaque fois que nous faisons un `git push` sur la branche `main`, le site est **reconstruit et mis à jour automatiquement**.  
4. Le site final est hébergé sur **GitHub Pages**, accessible via une URL publique.

---

## 📚 Contenu du site

Le site est divisé en plusieurs grandes parties accessibles depuis le menu :

- 📡 **Documentation Réseau** : DNS, pare-feu, routage, plan d’adressage, etc.  
- 🖥️ **Systèmes** : configuration et gestion de serveurs Windows, Active Directory, GPO, partages, etc.  
- 🧑‍💻 **Projets BTS** : nos projets d’atelier de professionnalisation et réalisations concrètes.  
- ℹ️ **À propos** : notre parcours, nos outils et nos compétences principales.

---

## 🤝 Collaboration et gestion de version

Tout le travail a été réalisé **en collaboration entre Hugo Guignard et Lucien Bescos**, avec une utilisation intensive de **Git et GitHub** pour :
- Travailler à deux sur les mêmes fichiers,
- Sauvegarder chaque version,
- Éviter les pertes de données,
- Suivre l’évolution du projet au fil du BTS.

Chaque commit représente une étape concrète dans l’évolution de nos documentations.

---

## 🙌 Remerciements

Nous remercions nos enseignants et encadrants pour leur accompagnement, ainsi que nos camarades de classe pour les échanges techniques et le travail d’équipe.  

Ce projet représente l’aboutissement de **deux années de formation**, marquées par la pratique, la rigueur et la passion pour l’informatique.

---

> _« Documenter, c’est transmettre. Apprendre, c’est partager. »_  
> — **Lucien BESCOS & Hugo GUIGNARD**
