# Mise en place de la redondance des contrôleurs de domaine (AD DS)

Ce guide détaille la procédure de désinstallation et de reconfiguration d'un second contrôleur de domaine pour assurer la haute disponibilité.

---

## 1. Désinstallation de l'ancien rôle AD DS

Pour mettre en place une redondance propre, il est nécessaire de désinstaller le rôle AD DS existant sur le serveur secondaire afin de le réinstaller correctement.

### Étapes de suppression :

1. Ouvrez le **Gestionnaire de serveur**, cliquez sur **Gérer** puis sur **Supprimer des rôles et fonctionnalités**.

<br>

![Accès au menu de suppression](../../medias/AD_redondant1.png)
*Accès au menu Gérer du Gestionnaire de serveur*

<br>

2. Dans la liste des rôles, décochez la case **Services AD DS**.

<br>

![Décocher AD DS](../../medias/AD_redondant2.png)
*Désélection du rôle Services AD DS*

<br>

3. Une fenêtre de confirmation s'affiche, cliquez sur **Supprimer des fonctionnalités**.

<br>

![Confirmer la suppression](../../medias/AD_redondant3.png)

<br>

4. Cliquez sur le lien bleu **Rétrograder ce contrôleur de domaine** (étape obligatoire avant la suppression totale).

<br>

![Lien de rétrogradation](../../medias/AD_redondant5.png)
*Cliquer sur "Rétrograder ce contrôleur de domaine"*

<br>

5. Cochez la case **Forcer la suppression de ce contrôleur de domaine** et cliquez sur **Suivant**.

<br>

![Forcer la suppression](../../medias/AD_redondant6.png)

<br>

6. Cliquez sur **Procéder à la suppression**. Le serveur demandera le mot de passe de l'administrateur local avant de redémarrer.

<br>

![Lancer la rétrogradation](../../medias/AD_redondant7.png)

---

## 2. Configuration de la redondance (Promotion)

Une fois le serveur redémarré et sain, nous allons l'ajouter au domaine existant.

### Étapes de configuration :

1. Cliquez sur le drapeau de notification en haut à droite et sélectionnez **Promouvoir ce serveur en contrôleur de domaine**.

<br>

![Promouvoir le serveur](../../medias/AD_redondant8.jpeg)

<br>

2. Sélectionnez l'option **Ajouter un contrôleur de domaine à un domaine existant**.
3. Renseignez le nom de votre domaine et cliquez sur **Modifier** pour entrer les identifiants administrateur.

> **Format requis :** `Administrateur@votre-domaine.local`

<br>

![Infos identification](../../medias/AD_redondant10.png)
*Saisie du domaine et des informations d'identification*

<br>

4. Définissez un mot de passe pour la restauration des services d'annuaire (DSRM).

<br>

![Mot de passe DSRM](../../medias/AD_redondant11.jpeg)
*Ce mot de passe est vital en cas de crash de l'AD*

<br>

5. Poursuivez en cliquant sur **Suivant** jusqu'à l'étape finale, puis cliquez sur **Installer**. Le serveur redémarrera automatiquement pour finaliser la promotion.

---

## 3. Optimisation DNS (Post-installation)

Pour une redondance efficace, les serveurs doivent pointer l'un vers l'autre pour la résolution DNS.

* **Sur le serveur primaire :** Configurez l'IP du serveur secondaire en tant que DNS auxiliaire.
* **Sur le serveur secondaire :** Configurez l'IP du serveur primaire en tant que DNS préféré.

<br>

![Configuration des cartes réseau](../../medias/AD_redondant12.png)
*