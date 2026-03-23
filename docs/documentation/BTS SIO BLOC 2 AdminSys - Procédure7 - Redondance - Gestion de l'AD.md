|<p>**BTS SIO BLOC 2**</p><p>**Admin Sys**</p>|***Effectuer la redondance d’un AD avec un autre***|**Fiche de procédure 7**|
| :- | - | :-: |
## **Désinstallation du deuxièmes AD DS**
Pour pouvoir effectuer la redondance il va falloir déseinstaller le deuxiémes AD DS pour le réinstaller 

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.001.png)

On va faire supprimer des rôles et fonctionnalité 

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.002.png)

on sélectionne AD DS pour le supprimez 

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.003.png)

on fait supprimez

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.004.png)

Il faut faire rétrograder le contrôleur de domaine

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.005.png)

on sélectionne forcer la suppression 

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.006.png)

On fait procéder à la suppression

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.007.png)

on mais le mot de passe admin et ensuite on va faire retrogration pour pouvoir le supprimez
## **On peut faire maintenant la configuration**
![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.008.jpeg)

On va dans le petit drapeau et on fait promouvoir 

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.009.png)

On fait ajouter a un domaine existant et on mais le domaine et dans info identification on mais l’utilisateur admin et mot de passe

Pour identifiant mot de passe il faut mettre <Administrateur@local>,dortmund,cub,sioplc,fr CubCub\_007

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.010.png)

On mais le mot de passe pour une récupération future

![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.011.jpeg)

On met suivant sur toute les étapes jusqu’à arriver à cette page et on fait installer et normalement il redémarre et sa fonctionne.

Sur le serveur primaire mettre le DNS du serveur de redondance et sur le serveur de redondance mettre ceelle du serveur primaire 
BTS SIO – BLOC2 – *fiche procédure n°5 – Configuration Failover Windows Server 2012*Page 4![](C:\documentation_github\CUBDOCUMENTATIONDORTMUND\docs\medias\Aspose.Words.c3077208-d7ee-47e1-820b-912afcc4a01a.012.png)
