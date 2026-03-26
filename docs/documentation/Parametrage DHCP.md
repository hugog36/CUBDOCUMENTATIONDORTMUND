# 🛠️ Infrastructure Active Directory & Services Réseaux
> **Documentation Technique Complète** | Administration, Sécurisation et Automatisation.

---

## 📖 Sommaire
1. [Redondance Active Directory (AD DS)](#1-redondance-active-directory-ad-ds)
2. [Sécurisation MFA & Modèle de Tiering](#2-sécurisation-mfa--modèle-de-tiering)
3. [Guide Complet : Configuration DHCP via PowerShell](#3-guide-complet--configuration-dhcp-via-powershell)
4. [Maintenance et Diagnostic Rapide](#4-maintenance-et-diagnostic-rapide)

---

## 1. Redondance Active Directory (AD DS)
*Procédure de nettoyage et de promotion du serveur secondaire.*

### 1.1. Désinstallation du rôle existant
Avant de promouvoir un serveur en tant que contrôleur de domaine secondaire, il faut supprimer l'ancien rôle proprement.

1. **Suppression :** Via le *Gestionnaire de serveur* > `Gérer` > `Supprimer des rôles et fonctionnalités`.
2. **Rétrogradation :** Cliquez sur le lien **Rétrograder ce contrôleur de domaine**.
3. **Forçage :** Si le partenaire est hors ligne, cochez **Forcer la suppression de ce contrôleur de domaine** pour nettoyer les métadonnées locales.

---

## 2. Sécurisation MFA & Modèle de Tiering
*Renforcement de l'authentification et isolation des privilèges.*

### 🔐 Authentification Forte (MFA)
L'utilisation de **certificats X.509** (via Yubikey ou Smart Cards) est privilégiée :
* **Clé privée non extractible :** Contrairement aux mots de passe, l'identité est liée à un objet physique.
* **Protection :** Immunise contre le phishing et les attaques par force brute.

### 🏛️ Modèle de Tiering
L'administration est segmentée en trois niveaux pour stopper la compromission totale en cas d'attaque sur un poste utilisateur :

| Tier | Périmètre | Cibles |
| :--- | :--- | :--- |
| **Tier 0** | Identité | Contrôleurs de domaine, PKI, AD Connect. |
| **Tier 1** | Applications | Serveurs de fichiers, SQL, serveurs métiers. |
| **Tier 2** | Utilisateurs | Postes de travail, PC portables. |

---

## 3. Guide Complet : Configuration DHCP via PowerShell
*Toutes les commandes pour administrer le service de A à Z.*

### 3.1. Installation et Initialisation
```powershell
# Installer le rôle et les outils d'administration
Install-WindowsFeature DHCP -IncludeManagementTools

# Autoriser le serveur dans l'Active Directory
Add-DhcpServerInDC -DnsName "SRV-DHCP-01.contoso.local"

# Créer les groupes de sécurité DHCP
Add-DhcpServerSecurityGroup

Gemini a dit
Voici l'intégralité de ta documentation technique regroupée en un seul bloc Markdown.

Ce fichier unique est conçu pour être ton README.md final sur GitHub. Il couvre la redondance AD DS, la sécurité avancée (MFA/Tiering) et le guide exhaustif des commandes PowerShell pour le DHCP.

Markdown
# 🛠️ Infrastructure Active Directory & Services Réseaux
> **Documentation Technique Complète** | Administration, Sécurisation et Automatisation.

---

## 📖 Sommaire
1. [Redondance Active Directory (AD DS)](#1-redondance-active-directory-ad-ds)
2. [Sécurisation MFA & Modèle de Tiering](#2-sécurisation-mfa--modèle-de-tiering)
3. [Guide Complet : Configuration DHCP via PowerShell](#3-guide-complet--configuration-dhcp-via-powershell)
4. [Maintenance et Diagnostic Rapide](#4-maintenance-et-diagnostic-rapide)

---

## 1. Redondance Active Directory (AD DS)
*Procédure de nettoyage et de promotion du serveur secondaire.*

### 1.1. Désinstallation du rôle existant
Avant de promouvoir un serveur en tant que contrôleur de domaine secondaire, il faut supprimer l'ancien rôle proprement.

1. **Suppression :** Via le *Gestionnaire de serveur* > `Gérer` > `Supprimer des rôles et fonctionnalités`.
2. **Rétrogradation :** Cliquez sur le lien **Rétrograder ce contrôleur de domaine**.
3. **Forçage :** Si le partenaire est hors ligne, cochez **Forcer la suppression de ce contrôleur de domaine** pour nettoyer les métadonnées locales.

---

## 2. Sécurisation MFA & Modèle de Tiering
*Renforcement de l'authentification et isolation des privilèges.*

### 🔐 Authentification Forte (MFA)
L'utilisation de **certificats X.509** (via Yubikey ou Smart Cards) est privilégiée :
* **Clé privée non extractible :** Contrairement aux mots de passe, l'identité est liée à un objet physique.
* **Protection :** Immunise contre le phishing et les attaques par force brute.

### 🏛️ Modèle de Tiering
L'administration est segmentée en trois niveaux pour stopper la compromission totale en cas d'attaque sur un poste utilisateur :

| Tier | Périmètre | Cibles |
| :--- | :--- | :--- |
| **Tier 0** | Identité | Contrôleurs de domaine, PKI, AD Connect. |
| **Tier 1** | Applications | Serveurs de fichiers, SQL, serveurs métiers. |
| **Tier 2** | Utilisateurs | Postes de travail, PC portables. |

---

## 3. Guide Complet : Configuration DHCP via PowerShell
*Toutes les commandes pour administrer le service de A à Z.*

### 3.1. Installation et Initialisation
```powershell
# Installer le rôle et les outils d'administration
Install-WindowsFeature DHCP -IncludeManagementTools

# Autoriser le serveur dans l'Active Directory
Add-DhcpServerInDC -DnsName "SRV-DHCP-01.contoso.local"

# Créer les groupes de sécurité DHCP
Add-DhcpServerSecurityGroup


###3.2. Gestion des Étendues (Scopes)

# Créer une étendue
Add-DhcpServerv4Scope -Name "VLAN_Prod" -StartRange 192.168.10.100 -EndRange 192.168.10.200 -SubnetMask 255.255.255.0 -State Active

# Modifier la durée du bail (ici 8 heures)
Set-DhcpServerv4Scope -ScopeId 192.168.10.0 -LeaseDuration 08:00:00

# Configurer la passerelle et le DNS
Set-DhcpServerv4OptionValue -ScopeId 192.168.10.0 -Router 192.168.10.1 -DnsServer 192.168.1.10

###3.3. Exclusions et Réservations

# Exclure une plage d'IP (ex: pour des serveurs en IP fixe)
Add-DhcpServerv4ExclusionRange -ScopeId 192.168.10.0 -StartRange 192.168.10.1 -EndRange 192.168.10.10

# Créer une réservation (IP fixe liée à la MAC)
Add-DhcpServerv4Reservation -ScopeId 192.168.10.0 -IPAddress 192.168.10.50 -ClientId "00-1A-2B-3C-4D-5E" -Name "Imprimante-RH"


###3.4. Sécurité et Filtres MAC

# Activer la liste blanche (seuls les PC listés auront une IP)
Set-DhcpServerv4FilterList -Allow $true

# Ajouter un appareil à la liste blanche
Add-DhcpServerv4Filter -List Allow -MemberId "00-11-22-33-44-55" -Description "PC-Direction"

###3.5. Sauvegarde et Export

# Exporter toute la configuration vers un fichier XML
Export-DhcpServer -File "C:\Backup\DHCP_Full.xml"

# Importer la configuration
Import-DhcpServer -File "C:\Backup\DHCP_Full.xml" -BackupPath "C:\Backup\Temp" -Force
