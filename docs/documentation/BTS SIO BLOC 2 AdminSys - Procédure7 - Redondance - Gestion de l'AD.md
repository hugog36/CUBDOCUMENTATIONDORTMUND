# **Mise en place de la redondance des AD**
## **Désinstallation du deuxièmes AD DS**

Pour pouvoir effectuer la redondance il va falloir désinstaller le deuxièmes AD DS pour le réinstaller 

![](../../medias/AD_redondant1.png)

On va faire supprimer des rôles et fonctionnalité 


![](../../medias/AD_redondant2.png)


on sélectionne AD DS pour le supprimez 


![](../../medias/AD_redondant3.png)


on fait supprimez


![](../../medias/AD_redondant4.png)


Il faut faire rétrograder le contrôleur de domaine


![](../../medias/AD_redondant5.png)


on sélectionne forcer la suppression 


![](../../medias/AD_redondant6.png)


On fait procéder à la suppression


![](../../medias/AD_redondant7.png)


on mais le mot de passe admin et ensuite on va faire retrogration pour pouvoir le supprimez

## **On peut faire maintenant la configuration**


![](../../medias/AD_redondant8.jpeg)


On va dans le petit drapeau et on fait promouvoir 


![](../../medias/AD_redondant9.png)


On fait ajouter a un domaine existant et on mais le domaine et dans info identification on mais l’utilisateur admin et mot de passe


Pour identifiant mot de passe il faut mettre <Administrateur@local>,dortmund,cub,sioplc,fr CubCub\_007


![](../../medias/AD_redondant10.png)


On mais le mot de passe pour une récupération future


![](../../medias/AD_redondant11.jpeg)


On met suivant sur toute les étapes jusqu’à arriver à cette page et on fait installer et normalement il redémarre et sa fonctionne.


Sur le serveur primaire mettre le DNS du serveur de redondance et sur le serveur de redondance mettre ceelle du serveur primaire 


![](../../medias/AD_redondant12.png)
