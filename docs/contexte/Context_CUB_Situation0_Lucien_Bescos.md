# Documentation Réseau – CUB

**Contexte :** CUB  
**Réalisé par :** Lucien BESCOS  

---

##  Sommaire

- [Contexte : CUB](#contexte--cub)
- [Plan d’adressage](#plan-dadressage)
- [Table de routage – Pare-feu](#table-de-routage--pare-feu)
- [Table de routage – Multilayer](#table-de-routage--multilayer)
- [Table de routage NAT](#table-de-routage-nat)
- [Commandes effectuées sur les équipements réseau](#commandes-effectuées-sur-les-équipements-réseau)
  - [Switch Layer 2](#switch-layer-2)
  - [Switch Layer 3](#switch-layer-3)
  - [Pare-feu](#pare-feu)

---

##  Contexte : CUB

Ce projet consiste à la mise en place d’une infrastructure réseau complète pour le contexte **CUB**, incluant la configuration du routage, des VLANs, du NAT, du pare-feu et de l’accès distant SSH.

---

##  Plan d’adressage

Pour le VLAN **Production**, nous avons besoin de **120 hôtes**.  
Nous déterminons donc le masque de sous-réseau approprié :

> Calcul : 2⁷ – 2 = 126 hôtes disponibles  
> Masque : `/25` (soit `255.255.255.128`)

| VLAN | @Réseau        | Masque            | @Diffusion     | @1ère IP        | @Passerelle     |
|------|----------------|-------------------|----------------|-----------------|-----------------|
| Production 54 | 192.168.4.0   | 255.255.255.128 | 192.168.4.127  | 192.168.4.1     | 192.168.4.126   |
| Client 10     | 192.168.4.128 | 255.255.255.192 | 192.168.4.191  | 192.168.4.129   | 192.168.4.190   |
| Admin Sys 20  | 192.168.4.192 | 255.255.255.240 | 192.168.4.207  | 192.168.4.193   | 192.168.4.206   |

---

##  Table de routage – Pare-feu

| Protocole | Réseau de destination | Masque | Passerelle | Interface |
|------------|-----------------------|---------|-------------|------------|
| Connected (C) | 192.36.4.0 | 255.255.255.0 (/24) | 192.168.4.254 | 192.168.4.254 |
| Connected (C) | 192.168.44.0 | 255.255.255.0 (/24) | 192.168.44.254 | 192.168.44.254 |
| Connected (C) | 192.36.253.0 | 255.255.255.0 (/24) | 192.36.253.40 | 192.36.253.40 |
| Static (S) | 192.168.4.0 | 255.255.255.0 (/24) | 192.168.44.253 | 192.168.44.254 |
| Static (S) | 0.0.0.0 | 0.0.0.0/0 | 192.36.253.254 | 192.36.253.40 |

---

##  Table de routage – Multilayer

| Protocole | Réseau de destination | Masque | Passerelle | Interface |
|------------|-----------------------|---------|-------------|------------|
| Connected (C) | 192.168.4.0 | 255.255.255.128 | 192.168.4.126 | 192.168.4.126 |
| Connected (C) | 192.168.4.128 | 255.255.255.192 | 192.168.4.190 | 192.168.4.190 |
| Connected (C) | 192.168.4.192 | 255.255.255.240 | 192.168.4.206 | 192.168.4.206 |
| Connected (C) | 192.168.44.248 | 255.255.255.240 | 192.168.44.253 | 192.168.44.253 |
| Static (S) | 0.0.0.0 | 0.0.0.0/0 | 192.168.44.254 | 192.168.44.253 |

---

##  Table de routage NAT

| Table NAT avant translation | Table NAT après translation |
|-----------------------------|------------------------------|
| **IP Src** | **Port Src** | **IP Dest** | **Port Dst** | **IP Src** | **Port Src** | **IP Dest** | **Port Dst** |
| 192.168.4.0/24 | * | * | * | 192.36.253.0/24 | * | * | * |

---

##  Commandes effectuées sur les équipements réseau

---

###  Switch Layer 2

```bash
enable
configure terminal

# VLAN 10 : interfaces FastEthernet 0/1 à 0/11
interface range fastEthernet0/1 - 11
 switchport mode access
 switchport access vlan 10
exit

# VLAN 20 : interfaces FastEthernet 0/12 à 0/16
interface range fastEthernet0/12 - 16
 switchport mode access
 switchport access vlan 20
exit

# VLAN 54 : interface FastEthernet 0/18
interface fastEthernet0/18
 switchport mode access
 switchport access vlan 54
exit

# Interface trunk : FastEthernet 0/24
interface fastEthernet0/24
 switchport mode trunk
exit
```

---

### Switch Layer 3

```bash
enable
configure terminal

# VLAN10
interface range fastEthernet0/1 - 11
 switchport mode access
 switchport access vlan 10
exit

# VLAN20
interface range fastEthernet0/12 - 16
 switchport mode access
 switchport access vlan 20
exit

# VLAN54
interface fastEthernet0/18
 switchport mode access
 switchport access vlan 54
exit

# Trunk
interface fastEthernet0/24
 switchport mode trunk
exit

# Route par défaut
ip route 0.0.0.0 0.0.0.0 192.168.44.254
```

#### Configuration SSH

```bash
username etudiant privilege 15 password etudiant_007
ip domain name sio.local
crypto key generate rsa
2042
ip ssh time-out 60
ip ssh authentication-retries 2 
line vty 0 4 
transport input ssh
login local
```

---

###  Pare-feu

```bash
enable
configure terminal

# Nom du routeur
hostname Router

# Interfaces
interface GigabitEthernet0/0
 ip address 192.168.44.254 255.255.255.240
 no shut 
 ip nat inside
exit

interface GigabitEthernet0/1
 ip address 192.36.4.254 255.255.255.0
 no shut 
exit

interface GigabitEthernet0/2
 ip address 192.36.253.40 255.255.255.0
 no shut 
 ip nat outside
exit

# NAT Overload
ip nat inside source list 4 interface GigabitEthernet0/2 overload

# Routes statiques
ip route 192.168.4.0 255.255.255.0 192.168.44.253

# Classless routing
ip classless

# NetFlow export
ip flow-export version 9

# ACL pour NAT
access-list 4 permit 192.168.0.0 0.0.255.255
```

####  Configuration SSH

```bash
username etudiant privilege 15 password etudiant_007
ip domain name sio.local
crypto key generate rsa
2042
ip ssh time-out 60
ip ssh authentication-retries 2 
line vty 0 4 
transport input ssh
login local
```


