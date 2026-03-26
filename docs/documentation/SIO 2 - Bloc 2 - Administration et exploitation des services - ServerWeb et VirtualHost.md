**Redirection des dommaines via des VirtualHosts – TLS - HTTPS**Installation d'un serveur LAMP (Linux/Apache/MySQL/PHP)**
LAMP signifie :

- **L**inux (Debian 12)
- **A**pache2 (serveur web)
- **M**ariaDB (base de données)
- **P**HP (langage serveur)

*sudo apt install apache2*

*sudo apt install php mariadb-server php-mysql libapache2-mod-php*

**Notes** 

- apache2 → installe le serveur web.
- php et libapache2-mod-php → permettent à Apache d’interpréter les pages PHP.
- mariadb-server et php-mysql → ajoutent la base de données et le connecteur PHP.
##**Création des VirtualHost**
Héberger plusieurs sites web sur le même serveur grâce à des noms de domaine différents (ex. www0 et www1).

**sudoedit /etc/apache2/sites-available/www0.conf** 

<VirtualHost \*:80> #Ecoute sur le port 80 

`        `ServerName www0.dortmund.cub.sioplc.fr

`        `ServerAlias www0.dortmund.cub.sioplc.fr

`        `ServerAdmin etudiant@dortmund.cub.sioplc.fr

`        `ErrorLog /var/log/apache2/www0.dortmund.cub.sioplc.fr-error\_log

`        `CustomLog /var/log/apache2/www0.dortmund.cub.sioplc.fr-access\_log combined

`        `DocumentRoot "/var/www/html/www0" #Doit etre le meme chemin de **ton installation WordPress**, là où se trouvent tous les fichiers du site (wp-admin, wp-content, wp-config.php, etc.).

`        `<Directory "/var/www/html/www0"> #Idem

`                `Options Indexes FollowSymLinks

`                `AllowOverride All

`                `Require all granted

`        `</Directory>

</VirtualHost>

**Apres avoir configurer nos VHOST,** sudo a2ensite www0

sudo systemctl reload apache2
##**Installation de Wordpress**
URL : IP du serveur 

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.003.jpeg) ![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.004.jpeg)

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.005.png)

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.006.png)
##**Configurer le serveur DNS autoritaire** 
Dans le fichier de zone du serveur nous alons rajouter les enregitrement correctement afin de faire des correspondence IP et des Alias pour la redirection 

Dans : **var/cache/bind/db.dortmund.cub.sioplc.fr**

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.007.jpeg)

Ne pas oublier de redémarrer le service apres avoir modifier des enregistrement dans la configuration 

*sudo systemctl restart bind9*
##**Configuration du site via GIT Clone** 
Nous avons un depot git sut github, nous avons juste a recuperer le liens du depot ![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.008.png)

Puis nous allons dans 

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.009.png)

Pour introduire le clone du git dans le dossier pour « scanner0 » *mkdir scanner0*

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.010.png)

*sudo git clone « lien copié »* 

Puis nous pouvons voir un nouveau dossier crée avec le clone ainsi que toutes les dépendances 

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.011.png)

**Ne pas oublier de changer le chemin de notre VirtualHost** 

<VirtualHost \*:80>

`        `ServerName scanner0.dortmund.cub.sioplc.fr

`        `ServerAlias scanner0.dortmund.cub.sioplc.fr

`        `ServerAdmin etudiant@dortmund.cub.sioplc.fr

`        `ErrorLog /var/log/apache2/scanner0.dortmund.cub.sioplc.fr-error\_log

`        `CustomLog /var/log/apache2/scanner0.dortmund.cub.sioplc.fr-access\_log combined



|`  `DocumentRoot "/var/www/html/scanner0|/**command-attack**|
| - | - |
|||
`        `<Directory "/var/www/html/scanner0/**command-attack/**">                 Options Indexes FollowSymLinks

`                `AllowOverride All

`                `Require all granted

`        `</Directory>

</VirtualHost>

**Apres avoir configurer nos VHOST,** sudo a2ensite www0

sudo systemctl reload apache2

**Pour tester, idem nous allons maintenant taper l’URL** <http://scanner0.dortmund.cub.sioplc.fr/command-attack/>

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.012.png)

Maintenant il est question de sécurise l’accès a ce Vhost car celui-ci contrairement au WordPress est priver, nous allons essayer de demander un login et un mot de passe pour accéder au service 
1. ## **Activer<a name="_page7_x56.70_y363.90"></a> les modules Apache requis**
sudo a2enmod auth\_basic sudo a2enmod authz\_host sudo systemctl restart apache2
2. ## **Créer<a name="_page7_x56.70_y445.80"></a> le fichier htpasswd**
sudo htpasswd -c /etc/apache2/.htpasswd admin

(Un mot de passe sera demandé)

Identifiant : **admin** 

Mot de passe : **etudiant\_007**

3. **Adapter<a name="_page8_x56.70_y109.55"></a> le Virtualhost**

<VirtualHost \*:80>

`    `ServerName scanner0.dortmund.cub.sioplc.fr

`    `ServerAlias scanner0.dortmund.cub.sioplc.fr

`    `ServerAdmin etudiant@dortmund.cub.sioplc.fr

`    `DocumentRoot "/var/www/html/scanner0/command-attack/"

`    `<Directory "/var/www/html/scanner0/command-attack/">         Options Indexes FollowSymLinks

`        `AllowOverride All

`        `AuthType Basic

`        `AuthName "Accès restreint - Scanner Réseau"         AuthUserFile /etc/apache2/.htpasswd

`        `<RequireAll>

`            `Require ip 192.168.4.192/28             Require valid-user

`        `</RequireAll>

`    `</Directory>

`    `ErrorLog ${APACHE\_LOG\_DIR}/scanner0-error.log

`    `CustomLog ${APACHE\_LOG\_DIR}/scanner0-access.log combined </VirtualHost>

<a name="_page9_x56.70_y231.50"></a>**Port monitoring** 

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.013.png)

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.014.png)

<a name="_page10_x56.70_y121.55"></a>**Mise en place de HTTPS**

Généré mon certificat auto-signé Attention ! Un certificat utilise = un site 

sudo mkdir certs

cd certs/

sudo openssl req -newkey rsa:4096 -keyout docs.key -x509 -days 365 -out docs.crt

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.015.png)

Explication : 

- **sudo** : exécute la commande avec les droits administrateur.
- **openssl req** : lance la création d’une demande de certificat (CSR) ou d’un certificat auto-signé.
- **-newkey rsa:4096** : génère une nouvelle clé privée RSA de 4096 bits.
- **-keyout docs.key** : enregistre la clé privée générée dans le fichier *docs.key ( « docs » = mettre nom de notre serveur web)*
- **-x509** : crée directement un certificat X509 auto-signé (pas seulement une demande).
- **-days 365** : définit la durée de validité du certificat à 365 jours.
- **-out docs.crt** : enregistre le certificat généré dans le fichier *docs.crt*. *( « docs » = mettre nom de notre serveur web)*

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.016.png)

**Passphrase :** etudiant\_007

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.017.png)

Modification de mes virtualhosts

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.018.jpeg)

<VirtualHost \*:443> 

|/|443 = port HTTP|
| - | - |
|||
**Aj**outé la configuration SSL obligatoire SSLEngine on

SSLCertificateFile /etc/apache2/certs/scanner0.crt //Elles servent à activer HTTPS + 

charger ton certificat auto-signé.

SSLCertificateKeyFile /etc/apache2/certs/scanner0.key sudo service apache2 reload // Besoins de la Passphrase 

![](../../medias/Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.019.png)
**SIO 2 - Bloc 2 - Administration et exploitation des services - Contexte : CUB ![](Aspose.Words.b02e3dcb-e62d-4140-b14e-28d5333d7eb5.020.png)**
