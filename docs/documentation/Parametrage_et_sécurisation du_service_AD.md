# 📂 Situation 5 : Paramétrage et sécurisation du service AD
**Contexte :** CUB (Agence d'Anvers)  
**Réalisé par :** Lucien BESCOS  
**Système :** Windows Server 2019

---

## 🏗️ A. Installation des services AD DS

### Question 1 : Installation et promotion
L'installation du rôle **AD DS** (Active Directory Domain Services) a été réalisée via le Gestionnaire de serveur, suivie de la promotion du serveur en tant que contrôleur de domaine.

### Question 2 : Configuration de la forêt
* **Action :** Ajouter une nouvelle forêt.
* **Nom de domaine :** `local.anvers.cub.sioplc.fr`

### Question 3 : Restauration d'annuaire (DSRM)
* **Configuration :** Définition du mot de passe de secours pour la restauration des services d'annuaire.
* **Mot de passe :** `Cub_007`

### Question 4 : Scripting PowerShell
* **Action :** Exportation et visualisation du script PowerShell généré par l'assistant avant l'installation finale pour archivage technique.

### Question 5 : Sécurisation du compte Administrateur
* **Action :** Modification du mot de passe par défaut du compte administrateur du domaine.
* **Nouveau mot de passe :** `CubCub_007`

---

## ⚙️ B. Paramétrage de l'Active Directory

### Question 6 : Unités d'Organisation (OU)
* **Action :** Création d'une nouvelle Unité d'Organisation pour structurer les ressources.
* **Nom :** `Salle002`

### Question 7 : Intégration d'un ordinateur
* **Action :** Création de l'objet ordinateur dans le domaine.
* **Nom :** `posteA`

### Question 8 : Déplacement d'objets
* **Action :** Déplacement de l'ordinateur `posteA` depuis le conteneur `Computers` vers l'unité d'organisation `Salle002`.

### Question 9 : Création des utilisateurs
* **Action :** Création des comptes utilisateurs suivants dans le conteneur `Users` :
  * M. Balny
  * M. Demouliere
  * Mme Ferreira
  * Mme Caramigeas

### Question 10 : Création des groupes de sécurité
* **Type :** Sécurité / **Étendue :** Globale
* **Groupes créés :** `Production`, `Clients`, `Administration`.

### Question 11 : Affectation des membres
* **Groupe Production :** M. Balny et M. Demouliere.
* **Groupe Administration :** Mme Ferreira.
* **Groupe Clients :** Mme Caramigeas.

---

## 💻 C. Gestion de l'AD via PowerShell (Questions 12 & 13)

### 1. Gestion des Unités d'Organisation et Ordinateurs
```powershell
# Création de l'OU Salle005
New-ADOrganizationalUnit -Name "Salle005" -Path "DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr"

# Déplacement de l'ordinateur posteA vers l'OU Salle001
Move-ADObject -Identity "CN=posteA,OU=Salle002,DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr" `
              -TargetPath "OU=Salle001,DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr"

###2. Création de l'utilisatrice Patricia Delouche

$Password = ConvertTo-SecureString "Provisoire_007" -AsPlainText -Force

New-ADUser -Name "patricia delouche" `
           -GivenName "patricia" `
           -Surname "delouche" `
           -SamAccountName "pdelouche" `
           -UserPrincipalName "pdelouche@local.anvers.cub.sioplc.fr" `
           -AccountPassword $Password `
           -ChangePasswordAtLogon $true `
           -Enabled $true `
           -Path "CN=Users,DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr"


###3. Gestion du groupe Développeurs

# Création du groupe
New-ADGroup -Name "Developpeurs" -GroupCategory Security -GroupScope Global -Path "CN=Users,DC=local,DC=anvers,DC=cub,DC=sioplc,DC=fr"

# Ajout de l'utilisateur au groupe
Add-ADGroupMember -Identity "Developpeurs" -Members "pdelouche"

###4. Audit du domaine

# Lister tous les comptes avec le détail des informations
Get-ADUser -Filter * -Properties * | Select-Object Name, SamAccountName, UserPrincipalName, Enabled


D. Redondance de l'Active Directory (Question 14)
Procédure 7 : Configuration d'une redondance AD DS sous Windows Server 2019

Préparation : Attribution d'une adresse IP fixe et configuration du DNS pointant vers le premier contrôleur de domaine (DC1).

Installation : Ajout du rôle "Services de domaine Active Directory" sur le second serveur.

Promotion : Sélection de l'option "Ajouter un contrôleur de domaine à un domaine existant".

Réplication : Vérification de la synchronisation de la base NTDS et du dossier SYSVOL.

Validation : Exécution de la commande repadmin /showrepl pour confirmer le succès de la réplication.