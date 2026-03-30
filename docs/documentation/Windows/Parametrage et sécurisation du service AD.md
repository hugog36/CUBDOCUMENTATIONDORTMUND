
# Contexte : CUB
**Réaliser par** : Lucien BESCOS

---

## Sommaire

- [Context : CUB](#context--cub)
- [Question 1 : Installation des services ADDS et promouvoir le serveur en contrôleur de domaine](#question-1--installation-des-services-adds-et-promouvoir-le-serveur-en-contrôleur-de-domaine)
- [Question 2 : Ajouter une nouvelle forêt correspondant à votre nom de domaine](#question-2--ajouter-une-nouvelle-forêt-correspondant-à-votre-nom-de-domaine)
- [Question 3 : Mot de passe pour la restauration des services d'annuaire](#question-3--mot-de-passe-pour-la-restauration-des-services-dannuaire)
- [Question 4 : Afficher le script Powershell pour visualiser les commandes utilisées](#question-4--afficher-le-script-powershell-pour-visualiser-les-commandes-utilisées)
- [Question 5 : Nouveau mot de passe pour le compte « administrateur »](#question-5--nouveau-mot-de-passe-pour-le-compte-administrateur)
- [Question 6 : Nouveau → Unité d’organisation](#question-6--nouveau--unité-dorganisation)
- [Question 7 : Intégrer un ordinateur client Windows au domaine (nom : posteA)](#question-7--intégrer-un-ordinateur-client-windows-au-domaine-nom--postea)
- [Question 8 : Déplacer l'ordinateur « posteA » dans l'unité d'organisation « Salle002 »](#question-8--déplacer-lordinateur-postea-dans-lunité-dorganisation-salle002)
- [Question 9 : Créer dans le conteneur « Users » les utilisateurs suivants](#question-9--créer-dans-le-conteneur-users-les-utilisateurs-suivants)
- [Question 10 : Créer les groupes suivants dans le conteneur « Users »](#question-10--créer-les-groupes-suivants-dans-le-conteneur-users)
- [Question 11 : Ajouter des membres aux groupes créés](#question-11--ajouter-des-membres-aux-groupes-créés)
- [Question 12 : Retrouver les commandes PowerShell essentielles pour gérer l'AD](#question-12--retrouver-les-commandes-powershell-essentielles-pour-gérer-lad)
- [Question 13 : Réaliser à l'aide de commandes Powershell diverses actions](#question-13--réaliser-à-laide-de-commandes-powershell-diverses-actions)
- [Question 14 : Mise en place d'une redondance de l'Active Directory](#question-14--mise-en-place-dune-redondance-de-lactive-directory)

---

# A Installation des services ADDS

## Question 1 : Installation des services ADDS et promouvoir le serveur en contrôleur de domaine

## Question 2 : Ajouter une nouvelle forêt correspondant à votre nom de domaine
local.anvers.cub.sioplc.fr (pour l'agence d'Anvers...)

## Question 3 : Mot de passe pour la restauration des services d'annuaire
Cub_007

## Question 4 : Afficher le script Powershell pour visualiser les commandes utilisées lors de l'installation des services ADDS

## Question 5 : Nouveau mot de passe pour le compte « administrateur »
Nouveau mot de passe : CubCub_007

# B Paramétrage de l'Active Directory

## Question 6 : Nouveau → Unité d’organisation

## Question 7 : Intégrer un ordinateur client Windows au domaine (nom : posteA)
Dans « Computer » → Nouveau → Ordinateur

## Question 8 : Déplacer l'ordinateur « posteA » dans l'unité d'organisation « Salle002 »
Clic droit sur posteA → déplacer

## Question 9 : Créer dans le conteneur « Users » les utilisateurs suivants

## Question 10 : Créer les groupes suivants dans le conteneur « Users » (étendue: Globale; Type: Sécurité)
- Production
- Clients
- Administration

Exemple

## Question 11 : Ajouter des membres aux groupes créés
- M Balny et M Demouliere : groupe Production
- Mme Ferreira : groupe Administration
- Mme Caramigeas : groupe Clients

## Question 12 : Retrouver les commandes PowerShell essentielles permettant de gérer l'Active Directory et rédiger la fiche de procédure 6: Powershell - Gestion de l'AD
Fiche faite

## Question 13 : Réaliser à l'aide de commandes Powershell les actions suivantes
- Création d'une unité d'organisation : Salle005
- Déplacer l'ordinateur « posteA » vers l'unité d'organisation « Salle001 »
- Créer l'utilisatrice suivante dans le conteneur « Users » :  
  Nom : delouche; Prénom: patricia; mot de passe (à changer à la première connexion) : Provisoire_007  
  ```powershell
  New-ADUser -Name "patricia delouche" -GivenName "patricia" -Surname "delouche" -SamAccountName "pdelouche" -UserPrincipalName "pdelouche@local.dortmund.cub.sioplc.fr" -AccountPassword (ConvertTo-SecureString "Provisoire_007" -AsPlainText -Force) -ChangePasswordAtLogon $true -Enabled $true -Path "CN=Users,DC=local,DC=dortmund,DC=cub,DC=sioplc,DC=fr"
  ```
- Créer le groupe « Developpeurs » dans le conteneur « Users »
- Ajouter Mme Delouche au groupe « Developpeurs »
- Lister tous les comptes de l'Active Directory avec le détail des informations

## Question 14 : Mise en place d'une redondance de l'Active Directory
Voir fiche de procédure 7
