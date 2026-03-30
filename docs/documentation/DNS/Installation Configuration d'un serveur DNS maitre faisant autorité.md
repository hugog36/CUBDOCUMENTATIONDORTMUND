# Mise en place d’un serveur DNS Maitre faisant autorité

 

---

## Objectif

Mettre en place un **serveur DNS autoritaire** sous **Debian 12** (solution Dortmund), permettant de gérer une zone de domaine interne et d'assurer la résolution locale pour ce domaine.

---

## 1. Préparation de l’environnement

### Étape 1 – Mise à jour du système
Avant toute installation, on met à jour le serveur :

```bash
sudo apt update && sudo apt upgrade -y
```


---
Cette commande met à jour la liste des paquets disponibles (apt update) et installe les dernières versions (apt upgrade).

### Étape 2 – Installation des paquets nécessaires
Installer le service **BIND9** (Berkeley Internet Name Domain), utilisé pour la gestion DNS :

```bash
sudo apt install bind9 dnsutils rsylog
```


---
bind9 : installe le service principal DNS.

Cela installe tout ce qu’il faut pour faire fonctionner un serveur DNS.


### Étape 3 – définir les paramètres réseaux   
Pour accéder à la config réseau :

```bash
sudoedit /etc/network/interfaces
```
La configuration utiliser par le serveur :


```bash
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

The loopback network interface
auto lo
iface lo inet loopback

The primary network interface
allow-hotplug ens3
iface ens3 inet static
    address 192.36.4.10/24
    gateway 192.36.4.254
    # dns-* options are implemented by the resolvconf package, if installed
    dns-nameservers 172.16.20.11,9.9.9.9

```



---

### Étape 4 – définir le serveur DNS récursif à utiliser 

La commande pour y accéder : 
 
```bash
sudoedit /etc/resolv.conf
```
La config effectuer : 

```bash
nameserver 86.54.11.100
nameserver 9.9.9.9
```
Après sa on effectue la commande pour relancer le service réseau : 

```bash
sudo systemctl restart networking
```
---
### Étape 5 – Configurer correctement les fichiers /etc/hostname et /etc/hosts
Le fichier hostname sert à donner un nom à votre serveur.
Donc pour le changer on fait : 

```bash
sudoedit /etc/hostname
```
Et dedans on va mettre le nom que l'on souhaite : 

```bash
ns0
```
Ensuite on va configurer le fichier hosts qui fait la correspondance entre un nom et une IP.
il va falloir entrer la commande pour aller dans le fichier :

```bash
sudoedit /etc/hosts
```
Et ensuite le configurer selon nos information : 

```bash
127.0.0.1       localhost
127.0.1.1       ns0.dortmund.cub.sioplc.fr     ns0

The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

```
---

## 2. Configuration du serveur DNS autoritaire

### Étape 6 – Configuration du fichier named.conf 
il va falloir entrer la commande pour aller dans le fichier named.conf :

```bash
sudo cat /etc/bind/named.conf
```
Et ensuite le configurer selon nos information : 

```bash

include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
include "/etc/bind/named.conf.default-zones";

```
---
### Étape 7 – Configuration du fichier named.conf.option 
il va falloir entrer la commande pour aller dans le fichier named.conf.option :

```bash
sudo cat /etc/bind/named.conf.option
```
Et ensuite le configurer selon nos information : 

```bash
options {
        directory "/var/cache/bind";
        listen-on port 53 {127.0.0.1; 192.36.4.10; };
        recursion no;
        version none;
};

```

---
### Étape 8 – Configuration du fichier named.conf.local 
il va falloir entrer la commande pour aller dans le fichier named.conf.local :

```bash
sudo cat /etc/bind/named.conf.local
```
Et ensuite le configurer selon nos information : 

```bash
zone "tours.tierslieux86.fr" {
    type master;
    allow-transfer { 172.16.3.11; };
    file "/var/cache/bind/db.tours.tierslieux86.fr";
};
```


---


### Étape 9 – Configuration du fichier db.dortmund.cub.sioplc.fr

il va falloir accéder au fichier :

```bash
sudo nano /etc/bind/db.dortmund.cub.sioplc.fr

```
on va y mettre les information demander : 

```bash
$TTL 43200

dortmund.cub.sioplc.fr. IN SOA ns0.dortmund.cub.sioplc.fr. postmaster.dortmund. (
        2025100902 ; Serial
        1D ; Refresh
        1H ; Retry
        1W ; Expire
        3H ) ; Negative Cache TTL

dortmund.cub.sioplc.fr. IN NS ns0.dortmund.cub.sioplc.fr.
dortmund.cub.sioplc.fr. IN NS ns1.dortmund.cub.sioplc.fr.

ns0 IN A 192.36.4.10
ns1 IN A 192.36.4.11

extranet IN CNAME www.dortmund.cub.sioplc.fr.};
```
Et une fois modifier on effectue cette commande :

```bash
sudo chown bind:bind /var/cache/bind/db.tours.tierslieux86.fr
```
---
### Étape 10 – journalisation des évènements du service DNS

accéder au fichier de log :

```bash
sudoedit /etc/bind/named.conf.log
```
Puis dedans il faut y mettre les information : 

```bash
logging {

    channel bind_log {
                    file "/var/log/bind.log" versions 3 size 100m;
                   severity info;
                    print-category  yes;
                    print-severity  yes;
                    print-time      yes;
    };

    category default { bind_log; };

};
```
On active les log :
```
sudo touch /var/log/bind.log
sudo chown bind:bind /var/log/bind.log
```
---

### Étape 11 – déclarer ce nouveau fichier de configuration

La commande pour accéder au fichier :

```bash
sudoedit /etc/bind/named.conf
```
Les données pour le remplir :
```bash
// This is the primary configuration file for the BIND DNS server named.
// If you are just adding zones, please do that in /etc/bind/named.conf.local
include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
include "/etc/bind/named.conf.default-zones";

// Ajout du fichier de parametrage de la journalisation du service BIND
include "/etc/bind/named.conf.log";
```


## 3. Vérification et validation

### Étape 12 – Vérifier la configuration BIND

```bash
sudo named-checkconf
sudo named-checkzone dortmund.local /etc/bind/db.dortmund.local
sudo named-checkzone 1.168.192.in-addr.arpa /etc/bind/db.192
```
named-checkconf : vérifie la syntaxe des fichiers de config.
named-checkzone : teste si les fichiers de zone sont valides.
Ces tests permettent de repérer des erreurs avant de redémarrer le service.


---

### Étape 13 – Redémarrer le service

```bash
sudo systemctl restart bind9
sudo systemctl enable bind9
```
restart : recharge la configuration.
enable : lance automatiquement BIND9 au démarrage.
Cela applique les changements et garantit la disponibilité du service.


---

## 4. Tests de fonctionnement

### Étape 14 – Tester la résolution locale

Sur le serveur :

```bash
dig @localhost dortmund.local
dig @localhost ns.dortmund.local
```
dig interroge le serveur DNS local pour vérifier les réponses.
Si tout est bon, les enregistrements “A” s’affichent.



---

### Étape 15 – Tester depuis un client

Sur un poste client configuré avec le DNS Dortmund :

```bash
nslookup dortmund.local 192.168.1.10
```
On interroge le serveur DNS Dortmund (192.168.1.10).
Si le client reçoit la bonne IP, la configuration est fonctionnelle.


---


## Conclusion

La solution **Dortmund** assure une gestion complète et stable d’un serveur DNS autoritaire sous Debian.  
Le service **BIND9** permet la gestion des zones directes et inverses, la traçabilité via les logs, et la compatibilité avec les clients du réseau interne.

---


