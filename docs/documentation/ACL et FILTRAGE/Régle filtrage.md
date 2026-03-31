# Mise en place des régles de filtrage



## La table de filtrage du Switch Layer 3

### Le vlan client

|N°|Interface|Sens|@IP src|Port src|@IP dest|Port dest|Protocole|Statut|Action|
|-|-|-|-|-|-|-|-|-|-|
|1|192.168.6.190|Sortie|\*|\*|\*|\*|\*|\*|Autoriser|
|2|192.168.6.190|Entrée|\*|\*|\*|\*|\*|Établie|Autoriser|
|3|192.168.6.190|Entrée|\*|68 (DHCP)|255.255.255.255|67 (DHCP)|UDP|Nouvelle|Autoriser|
|4|192.168.6.190|Entrée|192.168.6.128/26|\*|192.168.6.10 / 192.168.6.11|53 (DNS)|UDP|Nouvelle|Autoriser|
|5|192.168.6.190|Entrée|192.168.6.128/26|\*|192.168.6.0/25|22 / 3389 (SSH/RDP)|TCP|Nouvelle|Bloquer|
|6|192.168.6.190|Entrée|192.168.6.128/26|\*|192.168.6.1 / 192.168.6.2 / 192.168.6.110 / 192.168.6.111|\*|\*|Nouvelle|Autoriser|
|7|192.168.6.190|Entrée|192.168.6.128/26|\*|192.168.6.192/28|\*|\*|Nouvelle|Bloquer|
|8|192.168.6.190|Entrée|\*|\*|192.168.6.0/25|\*|\*|Nouvelle|Bloquer|
|9|192.168.6.190|Entrée|\*|\*|\*|\*|\*|Nouvelle|Autoriser|



### Le Vlan Production



|N°|Interface|Sens|@IP src|Port src|@IP dest|Port dest|Protocole|Statut|Action|
|-|-|-|-|-|-|-|-|-|-|
|1|192.168.6.126|Sortie|\*|\*|\*|\*|\*|\*|Autoriser|
|2|192.168.6.126|Entrée|\*|\*|\*|\*|\*|Établie|Autoriser|
|3|192.168.6.126|Entrée|192.168.6.0/25|\*|192.168.6.192/28|\*|\*|Nouvelle|Bloquer|
|4|192.168.6.126|Entrée|192.168.6.0/25|\*|192.168.6.128/26|\*|\*|Nouvelle|Bloquer|
|5|192.168.6.126|Entrée|192.168.6.0/25|\*|\*|\*|\*|Nouvelle|Autoriser|



### Le Vlan Admin

|N°|Interface|Sens|@IP src|Port src|@IP dest|Port dest|Protocole|Statut|Action|
|-|-|-|-|-|-|-|-|-|-|
|1|192.168.6.206|Sortie|\*|\*|\*|\*|\*|\*|Autoriser|
|2|192.168.6.206|Entrée|\*|\*|\*|\*|\*|Établie|Autoriser|
|3|192.168.6.206|Entrée|192.168.6.192/28|\*|192.168.6.0/25, 192.168.6.128/26|\*|ICMP|Nouvelle|Autoriser|
|4|192.168.6.206|Entrée|192.168.6.192/28|\*|192.168.6.128/26|\*|\*|Nouvelle|Autoriser|
|5|192.168.6.206|Entrée|192.168.6.192/28|\*|192.168.6.0|22 / 3389 (SSH/RDP)|TCP|Nouvelle|Bloquer|
|6|192.168.6.206|Entrée|192.168.6.192/28|\*|192.168.6.1, 192.168.6.2, 192.168.6.110, 192.168.6.111|\*|\*|Nouvelle|Autoriser|
|7|192.168.6.206|Entrée|192.168.6.192/28|\*|192.168.6.10, 192.168.6.11|53|UDP|Nouvelle|Bloquer|
|8|192.168.6.206|Entrée|192.168.6.192/28|\*|192.168.6.125|443|TCP|Nouvelle|Bloquer|
|9|192.168.6.206|Entrée|192.168.6.192/28|\*|192.168.6.0/25|\*|\*|Nouvelle|Autoriser|
|10|192.168.6.206|Entrée|192.168.6.192/28|\*|\*|\*|\*|Nouvelle|Autoriser|



