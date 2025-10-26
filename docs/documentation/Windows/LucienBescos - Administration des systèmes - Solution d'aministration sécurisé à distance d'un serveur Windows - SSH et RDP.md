# Situation 1 – Solution d'aministration sécurisé à distance d'un serveur Windows - SSH et RDP

**Contexte :** CUB  
**Réalisé par :** Lucien BESCOS  

---


![Logo CUB](../../medias/logocub.png)


## Sommaire

- [Question 1 : Sysprep Windows 2019](#question-1-réaliser-un-sysprep-afin-de-réinitialiser-le-sid-de-los-windows-2019)
- [Question 2 : Changer le nom du serveur](#question-2-changer-le-nom-de-votre-serveur-seveurprimairex-x1-pour-létudiant-1-etc)
- [Question 3 : Modifier le mot de passe administrateur](#question-3-modifier-le-mot-de-passe-du-compte-administrateur-local-pour-respecter-les-recommandations-de-lansi)
- [Question 4 : Modifier le VLAN et l'adresse IP](#question-4-modifier-le-vlan-et-ladresse-ip-de-votre-serveur-192168yx)
- [Question 5 : Connexion sécurisée via SSH](#question-5-rédiger-la-procédure-dinstallation-de-la-connexion-sécurisée-à-distance-via-ssh)
- [Question 6 : Accès distant via OpenSSH](#question-6-installer-et-tester-laccès-à-distance-au-serveur-windows2019-via-openssh)
- [Question 7 : Sécurité SSH](#question-7-en-quoi-lutilisation-du-protocole-ssh-permet-til-dassurer-une-intégrité-des-communications)
- [Question 8 : Création d'un utilisateur](#question-8-créer-un-nouvel-utilisateur-sous-windows-2019)
- [Question 9 : Tester SSH et interdire administrateur](#question-9-tester-la-connexion-ssh-pour-ce-nouvel-utilisateur-puis-interdire-une-connexion-ssh-avec-lutilisateur-administrateur)
- [Question 10 : Modifier port SSH et pare-feu](#question-10-modifier-le-port-découte-par-défaut-du-service-ssh-en-222-et-réaliser-les-modifications-sur-le-pare-feu-windows-en-conséquences)
- [Question 11 : Procédure disponible](#question-11-procédure-réalisée-et-disponible)
- [Question 12 : Accès distant RDP](#question-12-installer-et-tester-laccès-à-distance-au-serveur-windows2019-via-le-protocole-rdp)
- [Question 13 : Sécurité RDP](#question-13-en-quoi-le-protocole-rdp-permet-dassurer-une-gestion-sécurisée-du-bureau-à-distance)

---

## Question 1 : Réaliser un « sysprep » afin de réinitialiser le SID de l'OS Windows 2019

- Se rendre dans `Disque local/Windows/System32/Sysprep`

  ![Situation 1](../../medias/sit1_1.png)



- Exécuter le fichier « sysprep »

  ![Situation 1](../../medias/sit1_2.png)

- Vérifier dans l’invite de commande:

```bash
whoami /user
```
 
![Situation 1](../../medias/sit1_3.png)


---

## Question 2 : Changer le nom de votre serveur : SeveurPrimaireX (X=1 pour l'étudiant 1, etc.)

- Dans le menu « Serveur local », cliquer sur le nom du serveur d’origine.

![Situation 1](../../medias/sit1_4.png)

- Nouveau nom : `SeveurPrimaire8` → faire `OK`.


---

## Question 3 : Modifier le mot de passe du compte administrateur local

- Aller dans les paramètres du compte local admin → section « mot de passe » → cliquer sur « modifier »  
- Nouveau mot de passe :  

```
Jesuisenbtssio2025*-
```



![Situation 1](../../medias/sit1_5.png)
---

## Question 4 : Modifier le VLAN et l'adresse IP de votre serveur : 192.168.Y.X

- Cliquer sur l’IP par défaut

  ![Situation 1](../../medias/sit1_6.png)

- Clic droit sur « Ethernet » → Propriété → IPV4


**Configurations réseau :**

- Lucien : 4.1  
- Hugo : 4.51

![Situation 1](../../medias/sit1_7.png)


**Résultats ping depuis la machine cliente Windows :**  

- Serveur Lucien  
- Serveur Hugo

![Situation 1](../../medias/sit1_8.png)

Resultats et test
Résultats ping de la machine cliente Windows vers les serveurs

- Serveur Lucien
  
  
![Situation 1](../../medias/sit1_9.png)

- Serveur Hugo

  
![Situation 1](../../medias/sit1_10.png)

---

## Question 5 : Rédiger la procédure d'installation de la connexion sécurisée à distance via SSH

- Procédure réalisée et disponible.

---

## Question 6 : Installer et tester l'accès à distance au serveur Windows2019 via OpenSSH


![Situation 1](../../medias/sit1_11.png)

- Ne pas oublier de changer le port par 222 avec :  

```bash
-p 222
```
![Situation 1](../../medias/sit1_12.png)



---

## Question 7 : En quoi SSH assure l’intégrité des communications

- SSH chiffre les données échangées, empêchant l’interception ou la modification par des tiers.  
- Première connexion SSH demande "yes/no" pour valider la clé publique du serveur afin d’éviter les attaques « man-in-the-middle ».  

---

## Question 8 : Créer un nouvel utilisateur sous Windows 2019

- Nom : `adminssh`  
- Mot de passe : `Cub_Admin_Ssh_007`


---

## Question 9 : Tester SSH pour ce nouvel utilisateur et interdire administrateur

- Tester la connexion SSH pour `adminssh`  
- Interdire la connexion SSH pour l’utilisateur `administrateur`.


![Situation 1](../../medias/sit1_13.png)
---

## Question 10 : Modifier le port SSH et ajuster le pare-feu

- Modifier le port d’écoute par défaut du service SSH en `222`  
- Adapter les règles sur le pare-feu Windows en conséquence.

  ![Situation 1](../../medias/sit1_14.png)
  ![Situation 1](../../medias/sit1_15.png)

---

## Question 11 : Procédure disponible

- Procédure réalisée et disponible.

---

## Question 12 : Installer et tester l'accès distant via RDP

- Procédure réalisée et testée.

![Situation 1](../../medias/sit1_16.png)

![Situation 1](../../medias/sit1_17.png)

![Situation 1](../../medias/sit1_18.png)

---

## Question 13 : Sécurité du protocole RDP

Le protocole RDP (Remote Desktop Protocol) permet une gestion sécurisée du bureau à distance en chiffrant les données échangées entre l’utilisateur et l’ordinateur distant. Cela protège les informations (écran, clavier, souris, etc.) contre les interceptions.
Il utilise aussi des mécanismes d’authentification (nom d’utilisateur, mot de passe, voire certificat ou double authentification) pour s'assurer que seule une personne autorisée peut accéder au bureau distant.

