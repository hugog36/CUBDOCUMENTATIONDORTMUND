# 🖥️ Situation 1 – Administration sécurisée à distance d’un serveur Windows (SSH et RDP)
![](../medias/logocub.png)

**Contexte :** CUB  
**Réalisé par :** Lucien BESCOS  
**BTS SIO – Bloc 2 Réseaux avancés**

---

## 📑 Sommaire

- [Contexte](#contexte)
- [Question 1 – Sysprep et réinitialisation du SID](#question-1--sysprep-et-réinitialisation-du-sid)
- [Question 2 – Changement du nom du serveur](#question-2--changement-du-nom-du-serveur)
- [Question 3 – Sécurisation du mot de passe administrateur](#question-3--sécurisation-du-mot-de-passe-administrateur)
- [Question 4 – Configuration du VLAN et de l’adresse IP](#question-4--configuration-du-vlan-et-de-ladresse-ip)
- [Question 5 – Procédure d’installation SSH](#question-5--procédure-dinstallation-ssh)
- [Question 6 – Test de la connexion SSH](#question-6--test-de-la-connexion-ssh)
- [Question 7 – Sécurité et intégrité du protocole SSH](#question-7--sécurité-et-intégrité-du-protocole-ssh)
- [Question 8 – Création d’un utilisateur SSH dédié](#question-8--création-dun-utilisateur-ssh-dédié)
- [Question 9 – Interdiction de la connexion SSH pour l’administrateur](#question-9--interdiction-de-la-connexion-ssh-pour-ladministrateur)
- [Question 10 – Modification du port SSH et pare-feu](#question-10--modification-du-port-ssh-et-pare-feu)
- [Question 11 – Procédure complémentaire](#question-11--procédure-complémentaire)
- [Question 12 – Installation et test du protocole RDP](#question-12--installation-et-test-du-protocole-rdp)
- [Question 13 – Sécurité du protocole RDP](#question-13--sécurité-du-protocole-rdp)

---

## 🧩 Contexte

Ce projet a pour but de **sécuriser l’administration à distance d’un serveur Windows Server 2019** dans le cadre du **contexte CUB**.  
Les protocoles **SSH** et **RDP** seront mis en place et testés pour garantir la sécurité et l’intégrité des communications.

---

## 🧱 Question 1 – Sysprep et réinitialisation du SID

📍 **Objectif :** Réinitialiser le SID du système pour éviter les doublons.

**Procédure :**

1. Se rendre dans :  
   `C:\Windows\System32\Sysprep`
2. Exécuter le fichier **sysprep.exe**
3. Choisir :
   - Action : `Entrer en mode OOBE (Out-of-Box Experience)`
   - Cocher : `Généraliser`
   - Option d’extinction : `Redémarrer`
4. Vérifier les changements avec la commande :
   ```bash
   whoami /user
   ```

---

## 🖥️ Question 2 – Changement du nom du serveur

1. Aller dans le menu **Serveur local**.  
2. Cliquer sur le nom du serveur d’origine.  
3. Choisir un nouveau nom, par exemple :  
   **SeveurPrimaire8**
4. Redémarrer le serveur pour appliquer les changements.

---

## 🔐 Question 3 – Sécurisation du mot de passe administrateur

Changer le mot de passe du compte **Administrateur local** conformément aux recommandations de l’**ANSI** :

> **Nouveau mot de passe :**  
> `Jesuisenbtssio2025*-`

📍 **Critères respectés :**
- Au moins 12 caractères  
- Minuscules / majuscules  
- Chiffres  
- Caractères spéciaux

---

## 🌐 Question 4 – Configuration du VLAN et de l’adresse IP

1. Ouvrir les **Propriétés réseau** → clic droit sur **Ethernet** → **Propriétés** → **IPv4**  
2. Configurer l’adresse IP manuellement :

| Élève | IP | Masque | Passerelle |
|--------|----|---------|-------------|
| Lucien | 192.168.4.1 | 255.255.255.128 | 192.168.4.126 |
| Hugo | 192.168.4.51 | 255.255.255.128 | 192.168.4.126 |

3. Tester la connectivité :
   ```bash
   ping 192.168.4.51
   ping 192.168.4.1
   ```

**Résultat :**
> Réponses obtenues avec un temps moyen inférieur à 1 ms, aucune perte de paquets.

---

## 🧠 Question 5 – Procédure d’installation SSH

1. Ouvrir **Windows PowerShell (Admin)**  
2. Installer le module SSH :
   ```powershell
   Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
   ```
3. Démarrer le service :
   ```powershell
   Start-Service sshd
   ```
4. Activer le démarrage automatique :
   ```powershell
   Set-Service -Name sshd -StartupType 'Automatic'
   ```

---

## 🧪 Question 6 – Test de la connexion SSH

Depuis une machine cliente Linux ou Windows :

```bash
ssh Administrateur@172.16.54.57 -p 222
```

📌 *Ne pas oublier le paramètre `-p 222` après modification du port.*

---

## 🔒 Question 7 – Sécurité et intégrité du protocole SSH

Le protocole **SSH (Secure Shell)** garantit :
- Le **chiffrement** des données échangées
- L’**authenticité** du serveur (via clé publique)
- L’**intégrité** des messages transmis

Lors de la première connexion, SSH demande une validation ("yes/no") pour approuver la clé du serveur.  
👉 Cela protège contre les attaques de type **Man-in-the-Middle**.

---

## 👤 Question 8 – Création d’un utilisateur SSH dédié

1. Ouvrir **Gestion de l’ordinateur → Utilisateurs et groupes locaux**
2. Créer un utilisateur :
   - Nom : `adminssh`
   - Mot de passe : `Cub_Admin_Ssh_007`
3. Ajouter l’utilisateur au groupe **Administrateurs** si nécessaire.

---

## 🚫 Question 9 – Interdiction de la connexion SSH pour l’administrateur

1. Éditer le fichier de configuration SSH :
   ```
   C:\ProgramData\ssh\sshd_config
   ```
2. Modifier / ajouter la ligne :
   ```
   DenyUsers Administrateur
   ```
3. Redémarrer le service :
   ```powershell
   Restart-Service sshd
   ```

---

## ⚙️ Question 10 – Modification du port SSH et pare-feu

1. Dans `C:\ProgramData\ssh\sshd_config`, modifier :
   ```
   Port 222
   ```
2. Ouvrir le port dans le pare-feu Windows :
   ```powershell
   New-NetFirewallRule -Name "SSH_222" -DisplayName "SSH Port 222" -Protocol TCP -LocalPort 222 -Action Allow
   ```

3. Redémarrer le service SSH :
   ```powershell
   Restart-Service sshd
   ```

---

## 🧾 Question 11 – Procédure complémentaire

Procédure réalisée et vérifiée : les tests de connexion SSH et RDP sont concluants sur les deux serveurs.

---

## 🖥️ Question 12 – Installation et test du protocole RDP

1. Activer le **Bureau à distance** :
   - Ouvrir : *Paramètres → Système → Bureau à distance*
   - Activer l’option « Autoriser les connexions à distance à cet ordinateur »
2. Depuis un poste client :
   - Ouvrir `Connexion Bureau à distance`
   - Entrer l’adresse IP : `172.16.54.57`
   - Se connecter avec un utilisateur autorisé

---

## 🧰 Question 13 – Sécurité du protocole RDP

Le protocole **RDP (Remote Desktop Protocol)** assure :
- Le **chiffrement complet** de la session
- La **protection des données** clavier/souris/écran
- Une **authentification forte** de l’utilisateur (mot de passe, certificat, etc.)

Ainsi, seule une personne autorisée peut accéder à distance au bureau de l’administrateur en toute sécurité.

---

✅ **Documentation complète – Situation 1 : Administration sécurisée d’un serveur Windows Server 2019 via SSH et RDP**  
**Auteur :** Lucien BESCOS  
**Contexte : CUB**
