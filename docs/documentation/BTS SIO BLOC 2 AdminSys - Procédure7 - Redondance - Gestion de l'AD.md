# Mise en place de la redondance Active Directory (AD DS)

Ce document explique comment préparer et configurer un second contrôleur de domaine pour assurer la haute disponibilité de l'annuaire.

---

## 1. Désinstallation de l'ancien rôle AD DS
*Note : Pour une redondance propre, il est parfois nécessaire de repartir sur une base saine en désinstallant le rôle existant sur le futur serveur secondaire.*

### Étapes de suppression :
1. Dans le **Gestionnaire de serveur**, cliquez sur **Gérer** > **Supprimer des rôles et fonctionnalités**.
   ![Capture d'écran](../../medias/AD_redondant1.png)

2. Sélectionnez le serveur cible et décochez **Services AD DS**.
   ![Capture d'écran](../../medias/AD_redondant2.png)
   ![Capture d'écran](../../medias/AD_redondant3.png)

3. Cliquez sur **Supprimer**. Un message d'erreur apparaîtra car le serveur est encore un contrôleur de domaine : cliquez sur **Rétrograder ce contrôleur de domaine**.
   ![Capture d'écran](../../medias/AD_redondant4.png)
   ![Capture d'écran](../../medias/AD_redondant5.png)

4. Cochez la case **Forcer la suppression de ce contrôleur de domaine**, puis cliquez sur **Suivant**.
   ![Capture d'écran](../../medias/AD_redondant6.png)

5. Confirmez en cliquant sur **Procéder à la suppression**.
   ![Capture d'écran](../../medias/AD_redondant7.png)

6. Saisissez le mot de passe Administrateur local, puis lancez la **Rétrogradation**. Le serveur va redémarrer.

---

## 2. Configuration de la redondance (Promotion)

Une fois le serveur prêt, nous allons le promouvoir comme réplica du domaine existant.

### Étapes de promotion :
1. Dans le **Gestionnaire de serveur**, cliquez sur le drapeau de notification (triangle jaune) et choisissez **Promouvoir ce serveur en contrôleur de domaine**.
   ![Capture d'écran](../../medias/AD_redondant8.jpeg)
   ![Capture d'écran](../../medias/AD_redondant9.png)

2. Sélectionnez **Ajouter un contrôleur de domaine à un domaine existant**.
3. Renseignez le nom du domaine et les informations d'identification :
   * **Utilisateur :** `Administrateur@votre-domaine.local`
   * **Mot de passe :** `CubCub_007` (par exemple)
   ![Capture d'écran](../../medias/AD_redondant10.png)

4. Saisissez le mot de passe de **récupération des services d'annuaire (DSRM)** pour une utilisation future.
   ![Capture d'écran](../../medias/AD_redondant11.jpeg)

5. Cliquez sur **Suivant** sur toutes les étapes restantes, puis sur **Installer**. Le serveur redémarrera automatiquement.

---

## 3. Configuration DNS (Post-Installation)
Pour que la réplication et le basculement fonctionnent, les serveurs doivent pointer l'un vers l'autre pour la résolution DNS :

* **Sur le serveur primaire :** Mettre l'IP du serveur secondaire en DNS secondaire.
* **Sur le serveur secondaire :** Mettre l'IP du serveur primaire en DNS primaire.

![Configuration DNS](../../medias/AD_redondant12.png)