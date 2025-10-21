# Situation 3 — Gestion des droits utilisateurs sur un dossier avec PowerShell

**Auteur :** Guignard Hugo  
**Contexte :** CUB  
**Formation :** BTS SIO SISR  
**Année :** 2025–2026  

---

## Sommaire

1. [Objectif](#1-objectif)  
2. [Pré-requis](#2-pré-requis)  
3. [Terminologie](#3-terminologie)  
4. [Sauvegarde des ACL (indispensable)](#4-sauvegarde-des-acl-indispensable)  
5. [Inspection des droits actuels](#5-inspection-des-droits-actuels)  
6. [Ajouter des droits](#6-ajouter-des-droits)  
7. [Supprimer des droits](#7-supprimer-des-droits)  
8. [Remplacer / réinitialiser les autorisations (opération destructive)](#8-remplacer--réinitialiser-les-autorisations-opération-destructive)  
9. [Gérer l’héritage](#9-gérer-lhéritage)  
10. [Modifier le propriétaire](#10-modifier-le-propriétaire)  
11. [Vérification et diagnostic](#11-vérification-et-diagnostic)  

---

## 1. Objectif

Permettre aux administrateurs de gérer de manière reproductible et sécurisée les autorisations **NTFS** d’un dossier via **PowerShell**, avec **journalisation** et **sauvegarde**.

---

## 2. Pré-requis

- Compte avec privilèges administrateur  
- PowerShell **5.1 ou supérieur** recommandé  
- Chemin complet du dossier (local ou UNC)  
- Connaissance des comptes/groupes (`DOMAIN\Groupe` ou `PC-NOM\Utilisateur`)

> 💡 **Remarque :** Les permissions effectives sur un partage = intersection entre les permissions du **partage** (Share Permissions) et les permissions **NTFS**.

---

## 3. Terminologie

- **NTFS ACL** : liste de contrôle d’accès sur fichiers/dossiers  
- **Allow / Deny** : autoriser / refuser  
- **InheritanceFlags** :  
  - `ContainerInherit` → sous-dossiers  
  - `ObjectInherit` → fichiers  
- **PropagationFlags** : contrôle la propagation aux enfants  

---

## 4. Sauvegarde des ACL (indispensable)

### PowerShell (Export-Clixml)

```powershell
$chemin = 'C:\Partage\Projets' # ou '\\Serveur\Partage'
$sauvegarde = "C:\Sauvegarde\ACL_Projets_$(Get-Date -Format yyyyMMdd_HHmmss).xml"

Get-Acl $chemin | Export-Clixml $sauvegarde
Write-Output "ACL sauvegardée dans : $sauvegarde"
```

### icacls (sauvegarde arborescente)

```powershell
icacls "C:\Partage\Projets" /save "C:\Sauvegarde\ACL_sauvegarde.acl" /T
```

---

## 5. Inspection des droits actuels

```powershell
$chemin = 'C:\Partage\Projets'
$acl = Get-Acl $chemin
$acl | Format-List

# pour un affichage plus lisible
$acl.GetAccessRules($true, $true, [System.Security.Principal.NTAccount]) |
Format-Table IdentityReference, FileSystemRights, AccessControlType, IsInherited, 
InheritanceFlags, PropagationFlags -AutoSize

# ou icacls pour un aperçu rapide
icacls "$chemin"
```

> **Explication :** `GetAccessRules($true,$true,[NTAccount])` convertit les SID en noms reconnaissables et indique si une règle est héritée.

---

## 6. Ajouter des droits

### 6.1 Méthode robuste (avec enums)

```powershell
$chemin = 'C:\Partage\Projets'
$utilisateur = 'DOMAIN\\Utilisateur1'

$acl = Get-Acl $chemin
$droits = [System.Security.AccessControl.FileSystemRights]::ReadAndExecute
$inheritance = [System.Security.AccessControl.InheritanceFlags]::ContainerInherit -bor `
[System.Security.AccessControl.InheritanceFlags]::ObjectInherit
$propagation = [System.Security.AccessControl.PropagationFlags]::None
$type = [System.Security.AccessControl.AccessControlType]::Allow

$regle = New-Object System.Security.AccessControl.FileSystemAccessRule($utilisateur, $droits, `
$inheritance, $propagation, $type)
$acl.AddAccessRule($regle)
Set-Acl -Path $chemin -AclObject $acl
```

> **Notes :**  
> - Pour `Modify` → `FileSystemRights::Modify`  
> - Pour `FullControl` → `FileSystemRights::FullControl`  
> - L’usage des **enums** évite les ambiguïtés liées à la langue et assure la précision.

### 6.2 Exemple : donner Full Control

```powershell
$droits = [System.Security.AccessControl.FileSystemRights]::FullControl
# puis création de la règle identique à l’exemple précédent
```

---

## 7. Supprimer des droits

```powershell
$chemin = 'C:\Partage\Projets'
$utilisateur = 'DOMAIN\\Utilisateur1'

$acl = Get-Acl $chemin
$rules = $acl.GetAccessRules($true, $true, [System.Security.Principal.NTAccount]) |
Where-Object { $_.IdentityReference -eq $utilisateur }

foreach ($r in $rules) {
    $acl.RemoveAccessRuleSpecific($r)
}
Set-Acl -Path $chemin -AclObject $acl
```

> 💡 **Remarque :** `RemoveAccessRuleSpecific` exige une correspondance exacte.  
> Si cela ne fonctionne pas, utiliser `RemoveAccessRule` ou `RemoveAccessRuleAll` **avec prudence**.

---

## 8. Remplacer / réinitialiser les autorisations (opération destructive)

> ⚠️ **Attention :** Cette opération efface toutes les ACL existantes sur l’objet (sauf la propriété).

```powershell
$chemin = 'C:\Partage\Projets'
$admin = 'DOMAIN\\Admin-Projets'

$acl = New-Object System.Security.AccessControl.DirectorySecurity
$rule = New-Object System.Security.AccessControl.FileSystemAccessRule($admin,
[System.Security.AccessControl.FileSystemRights]::FullControl,
[System.Security.AccessControl.InheritanceFlags]::ContainerInherit -bor `
[System.Security.AccessControl.InheritanceFlags]::ObjectInherit,
[System.Security.AccessControl.PropagationFlags]::None,
[System.Security.AccessControl.AccessControlType]::Allow)

$acl.AddAccessRule($rule)
Set-Acl -Path $chemin -AclObject $acl
```

---

## 9. Gérer l’héritage

### Activer / Protéger (désactiver héritage)

```powershell
$acl = Get-Acl $chemin
# true = protéger (désactiver héritage), false = ne pas copier les règles héritées
$acl.SetAccessRuleProtection($true, $false)
Set-Acl -Path $chemin -AclObject $acl
```

### Convertir les règles héritées en règles explicites

```powershell
$acl = Get-Acl $chemin
$acl.SetAccessRuleProtection($true, $true)
Set-Acl -Path $chemin -AclObject $acl
```

---

## 10. Modifier le propriétaire

```powershell
$acl = Get-Acl $chemin
$nouveauProprietaire = [System.Security.Principal.NTAccount] 'DOMAIN\\OwnerAccount'
$acl.SetOwner($nouveauProprietaire)
Set-Acl -Path $chemin -AclObject $acl
```

> 🔐 Seul le propriétaire ou un administrateur peut changer la propriété.

---

## 11. Vérification et diagnostic

### Affichage détaillé

```powershell
$acl = Get-Acl $chemin
$acl.GetAccessRules($true,$true,[System.Security.Principal.NTAccount]) |
Format-Table IdentityReference, FileSystemRights, AccessControlType, IsInherited, 
InheritanceFlags -AutoSize
```

### Vérification rapide

```powershell
icacls "$chemin"
```

### Erreurs fréquentes

- `Set-Acl` échoue → lancer PowerShell **en administrateur**  
- L’utilisateur n’existe pas → vérifier **nom et domaine**  
- Les règles réapparaissent → **héritage non désactivé**
