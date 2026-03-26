# Strategie de groupe (GPO) 

 

---

## Question 1 : Pourquoi l'authentification par login et mot de passe est problématique pour se connecter avec un compte "Administrateur du domaine" ?

L’authentification par login / mot de passe pour le compte Administrateur du domaine est problématique parce que :

* Un simple mot de passe peut être volé (phishing, fuite AD…).
* Si ce mot de passe est compromis, l’attaquant obtient tous les droits sur tout le domaine.
* Elle ne vérifie pas vraiment l’identité, contrairement à un certificat.
* C’est la méthode la plus vulnérable pour **un des comptes les plus sensibles** d’Active Directory.

<br>

## Question 2 : Qu'est-ce qu'une authentification multifacteur (MFA) ?

Une authentification multifacteur forte (MFA) consiste à vérifier l’identité d’un utilisateur en demandant au moins deux facteurs différents parmi :

* **Ce qu’il sait :** mot de passe, code PIN
* **Ce qu’il possède :** smartphone, token, carte à puce
* **Ce qu’il est :** empreinte, visage, biométrie

<br>

## Question 3 : Quelles sont les fonctions du code PIN et du code PUK ?

* **Code PIN :** sert à déverrouiller et utiliser la carte SIM ou la carte à puce (accès normal).
* **Code PUK :** sert à réinitialiser le PIN quand il a été bloqué après plusieurs mauvaises tentatives.

<br>

## Question 4 : Pourquoi est-il obligatoire de les modifier avant de mettre en œuvre la nouvelle authentification ?

Pour éviter le blocage de la clé lors de la mise en œuvre de la nouvelle authentification.

<br>

### Question 5 : Démontrer en quoi l'authentification TOTP est jugée moins robuste que l'authentification par certificat X509 stocké sur une clé ?

**TOTP moins robuste car :**
* Peut être cloné (secret copiable), volé (smartphone) ou phishé (code saisi sur un faux site).
* Ne garantit pas la possession d’un matériel unique.

**Certificat X.509 plus robuste car :**
* Clé privée stockée dans un support matériel non extractible.
* Nécessite présence physique + PIN.
* Impossible à copier ou phisher.

#### Mise en place de la solution technique (Paramétrage AD)

<br>

![Paramétrage AD](../../medias/Aspose.Words.4d68b082-a97e-42d4-96c5-5d09b1f09955.004.png)

<br>

## Question 6 : Pourquoi le RSSI vous a-t-il conseillé de créer 4 comptes distincts (utilisateur standard, admin tier 2, admin tier 1 et admin tiers 0) pour le même collaborateur David Balny ?

On a créé 4 utilisateurs pour Balny car ils n’auront pas les mêmes droits pour limiter les risques si un compte est corrompu. 
* L’utilisateur Balny a les droits de base.
* Le premier niveau d'admin a certains droits d’admin, etc.

L'objectif est de limiter l’impact si l'un des comptes est compromis ou perdu.

<br>

## Question 7 : Qu'est-ce qu'un poste d'administration ?

Un poste d’administration est un poste qui va pouvoir effectuer des tâches sur les serveurs et autres, il a donc accès à des droits que les utilisateurs n’ont pas. Il est placé dans cette unité d'organisation car il sera impacté par des règles différentes des autres GPO.

<br>

## Question 8 : Quelle est la fonction d'une PKI ?

Une PKI (Public Key Infrastructure) gère, distribue et vérifie les certificats numériques et les paires de clés utilisées pour une cryptographie asymétrique.

<br>

## Question 9 : Quel est l'intérêt de disposer d'une PKI interne plutôt que de certificats auto-signés ?

Une PKI interne permet de centraliser la gestion des certificats, d’assurer une confiance automatique dans toute l’organisation, de faciliter leur renouvellement/révocation et d’éviter les avertissements de sécurité.

À l’inverse, les certificats auto-signés sont difficiles à gérer, non fiables à grande échelle et doivent être approuvés manuellement par chaque poste.

<br>

![Schéma PKI](../../medias/Aspose.Words.4d68b082-a97e-42d4-96c5-5d09b1f09955.005.png)

<br>

## Question 10 : Lors de la création de la clé privée de la PKI, est-il préférable de choisir RSA ou ECDSA ?

Si nous utilisons des **Yubikey**, il vaut mieux choisir **ECDSA** car :
Il offre une sécurité plus élevée avec des clés plus courtes, ce qui le rend plus rapide et plus efficace, notamment sur les clés matérielles comme les Yubikeys qui sont optimisées pour cet algorithme.

<br>

## Question 11 : Choisir SHA256 ou SHA1 ?

**SHA256**, car SHA1 est aujourd'hui obsolète et n'est plus sécurisé.

---
**SIO2 BLOC 2 – Contexte : CUB – Admin SYS**
![Footer](../../medias/Aspose.Words.4d68b082-a97e-42d4-96c5-5d09b1f09955.006.png)