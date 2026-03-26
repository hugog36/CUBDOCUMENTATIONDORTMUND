# Dossier Technique : Administration Systèmes et Réseaux (CUB)
**Auteur :** Lucien BESCOS  
**Formation :** SIO2 BLOC 2

---

## 1. Redondance Active Directory (AD DS)
*Procédure de nettoyage et de promotion du serveur secondaire.*

### Désinstallation du rôle existant
Avant de promouvoir le serveur, il faut supprimer l'ancien rôle AD DS.

1. **Suppression via le Gestionnaire de serveur :** Allez dans `Gérer` > `Supprimer des rôles et fonctionnalités`.
<br>

![Menu Gérer](../../medias/AD_redondant1.png)

<br>

2. **Rétrogradation du contrôleur de domaine :** Cliquez sur le lien bleu **Rétrograder ce contrôleur de domaine**.
<br>

![Rétrograder](../../medias/AD_redondant5.png)

<br>

3. **Forçage de la suppression :** Cochez la case **Forcer la suppression de ce contrôleur de domaine** puis validez.
<br>

![Forcer](../../medias/AD_redondant6.png)

---

## 2. Stratégies de Groupe (GPO) & Sécurité MFA
*Mise en place de l'authentification forte via certificats X.509.*

### Questions de réflexion
* **Pourquoi le MFA ?** Pour pallier la faiblesse des mots de passe (vol, phishing).
* **Certificat X.509 :** Plus robuste que le TOTP car la clé privée est stockée physiquement dans la Yubikey (non extractible).
* **Modèle de Tiering :** Séparation des comptes (Tier 0 à Tier 2) pour limiter les privilèges.

<br>

![Schéma PKI](../../medias/Aspose.Words.4d68b082-a97e-42d4-96c5-5d09b1f09955.005.png)
*Architecture de la PKI Interne*

---

## 3. Paramétrage et Sécurisation du Service DHCP
*Configuration via PowerShell et sécurisation contre les attaques réseau.*

### Configuration des Étendues (PowerShell)

```powershell
# --- CRÉATION DES PLAGES D'ADRESSES ---

# Étendue pour le VLAN Clients
Add-DhcpServerv4Scope -Name "Clients" -StartRange 192.168.4.129 -EndRange 192.168.4.189 -SubnetMask 255.255.255.192

# Étendue pour l'Administration Système
Add-DhcpServerv4Scope -Name "AdministrationSystemeReseau" -StartRange 192.168.4.193 -EndRange 192.168.4.205 -SubnetMask 255.255.255.240

# --- EXCLUSIONS ET RÉSERVATIONS ---

# Exclusion d'une plage d'IP fixes
Add-DHCPServerV4ExclusionRange -ScopeId 192.168.4.192 -StartRange 192.168.4.195 -EndRange 192.168.4.200

# Réservation pour un poste spécifique (MAC : 08-00-27-2D-AB-B4)
Add-DhcpServerv4Reservation -ScopeId 192.168.4.192 -IPAddress 192.168.4.201 -ClientId "08-00-27-2D-AB-B4" -Description "Poste Specifique"

# --- PARAMÈTRES RÉSEAU ---

# Configuration de la durée du bail à 4 heures
Set-DhcpServerv4Scope -ScopeId 192.168.4.192 -LeaseDuration 04:00:00