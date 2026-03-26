# 🛠️ Administration & Sécurisation Infrastructure Windows Server

> **Projet :** Redondance AD DS, Stratégies GPO (MFA) et Automatisation DHCP.
> **Date :** 26 Mars 2026
> **Auteur :** [Ton Nom/Pseudo]

---

## 📖 Sommaire
1. [Redondance Active Directory (AD DS)](#1-redondance-active-directory-ad-ds)
2. [Sécurisation MFA & Modèle de Tiering](#2-sécurisation-mfa--modèle-de-tiering)
3. [Configuration et Automatisation DHCP](#3-configuration-et-automatisation-dhcp)
4. [Maintenance et Diagnostic](#4-maintenance-et-diagnostic)

---

## 1. Redondance Active Directory (AD DS)
*Procédure de nettoyage et de promotion du serveur secondaire.*

### 1.1. Désinstallation du rôle existant
Avant de promouvoir un nouveau contrôleur de domaine (DC) en remplacement d'un ancien, il est nécessaire de procéder à un nettoyage propre de l'ancien rôle.

* **Suppression des rôles :** Via le *Gestionnaire de serveur*, allez dans `Gérer` > `Supprimer des rôles et fonctionnalités`.
* **Rétrogradation :** Cliquez impérativement sur le lien bleu **Rétrograder ce contrôleur de domaine** avant de supprimer le binaire du rôle.
* **Forçage (si nécessaire) :** En cas de perte de connectivité avec le DC principal, cochez **Forcer la suppression de ce contrôleur de domaine** pour nettoyer la base locale.

---

## 2. Sécurisation MFA & Modèle de Tiering
*Mise en place de l'authentification forte via certificats X.509 et isolation des privilèges.*

### 🔐 Authentification Forte (MFA)
Pour pallier la vulnérabilité des mots de passe simples (phishing, attaques par dictionnaire), nous implémentons une solution basée sur la PKI (Public Key Infrastructure) :
* **Certificat X.509 :** Utilisation de clés de sécurité physiques (type Yubikey). 
* **Avantage :** La clé privée est générée dans la puce et ne peut pas être extraite, contrairement à un code SMS ou TOTP.

### 🏛️ Modèle de Tiering (Architecture de Sécurité)
Afin de limiter les mouvements latéraux en cas d'attaque, l'administration est segmentée en trois niveaux :

| Tier | Périmètre | Description |
| :--- | :--- | :--- |
| **Tier 0** | Identité | Contrôleurs de domaine, PKI, serveurs de fédération. |
| **Tier 1** | Applications | Serveurs membres, SQL, IIS, serveurs de fichiers. |
| **Tier 2** | Utilisateurs | Postes de travail et périphériques finaux. |

> **Règle d'or :** Un administrateur Tier 0 ne doit jamais se connecter sur une machine Tier 1 ou Tier 2.

---

## 3. Configuration et Automatisation DHCP
*Déploiement des étendues réseau via PowerShell.*

Ce script permet une configuration reproductible et limite les erreurs humaines lors de la création des scopes pour les différents VLANs.

### Script PowerShell de configuration

```powershell
<#
.SYNOPSIS
    Configuration des étendues et de la sécurité du serveur DHCP.
#>

# --- 1. VARIABLES D'ADRESSAGE ---
$ScopeClients = @{
    Name      = "VLAN_Clients"
    Start     = "192.168.4.129"
    End       = "192.168.4.189"
    Mask      = "255.255.255.192"
}

$ScopeAdmin = @{
    Id        = "192.168.4.192"
    Name      = "VLAN_Admin_Systeme"
    Start     = "192.168.4.193"
    End       = "192.168.4.205"
    Mask      = "255.255.255.240"
}

# --- 2. CRÉATION DES ÉTENDUES ---
Write-Host "[*] Création des étendues DHCP..." -ForegroundColor Cyan

Add-DhcpServerv4Scope -Name $ScopeClients.Name -StartRange $ScopeClients.Start -EndRange $ScopeClients.End -SubnetMask $ScopeClients.Mask
Add-DhcpServerv4Scope -Name $ScopeAdmin.Name -StartRange $ScopeAdmin.Start -EndRange $ScopeAdmin.End -SubnetMask $ScopeAdmin.Mask

# --- 3. EXCLUSIONS ET RÉSERVATIONS ---
Write-Host "[*] Configuration des exclusions et réservations..." -ForegroundColor Yellow

# Exclusion des IPs statiques serveurs (195 à 200)
Add-DhcpServerv4ExclusionRange -ScopeId $ScopeAdmin.Id -StartRange "192.168.4.195" -EndRange "192.168.4.200"

# Réservation pour le poste d'administration principal
$ResaAdmin = @{
    ScopeId   = $ScopeAdmin.Id
    IPAddress = "192.168.4.201"
    ClientId  = "08-00-27-2D-AB-B4"
    Name      = "PC-ADMIN-SECURE"
    Description = "Réservation statique poste administrateur"
}
Add-DhcpServerv4Reservation @ResaAdmin

# --- 4. PARAMÈTRES RÉSEAU (BAIL) ---
# Configuration d'un bail court (4h) pour le VLAN Admin
Set-DhcpServerv4Scope -ScopeId $ScopeAdmin.Id -LeaseDuration 04:00:00

Write-Host "[OK] Configuration DHCP appliquée." -ForegroundColor Green