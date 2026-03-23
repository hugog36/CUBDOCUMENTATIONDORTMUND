**Table de filtrage du Switch de layer 3**

![]((../../medias/Aspose.Words.d263a690-b1e7-4b10-b312-25eb7d50543e.001.jpeg)

![]((../../medias/Aspose.Words.d263a690-b1e7-4b10-b312-25eb7d50543e.002.jpeg)

![]((../../medias/Aspose.Words.d263a690-b1e7-4b10-b312-25eb7d50543e.003.jpeg)

**Règles ACL appliqués de filtrage du Switch de layer 3**
### **[action] [protocol] [source] [wildcard] [destination] [wildcard] [options] Expliqué élément par élément :**
- **action** → permit ou deny
- **protocol** → ip, tcp, udp, icmp, gre, etc.
- **source** → IP ou any ou host x.x.x.x
- **wildcard source** → masque générique (inverse du masque réseau)
- **destination** → IP ou any ou host x.x.x.x
- **wildcard destination** → masque générique
- **options** (selon protocole)
  - eq 80 (port HTTP)
  - eq 443 (HTTPS)
  - range 1000 2000
  - log
  - established

Vlan client 

n1 pas a mettre 

n2 permit tcp any any established

n3 permit udp any eq 68 any eq 67

n3 permit udp any eq 67 any eq 68

n4  permit udp 192.168.4.128 0.0.0.63 host 192.168.4.10 eq 53

n4 permit udp 192.168.4.128 0.0.0.63 host 192.168.4.11 eq 53

n5 permit tcp  192.168.4.128 0.0.0.63 192.168.4.0 0.0.0.127 eq 22 n5 permit tcp  192.168.4.128 0.0.0.63 192.168.4.0 0.0.0.127 eq 3389 n6 permit ip 192.168.4.128 0.0.0.63 host 192.168.4.1 

n6 permit ip 192.168.4.128 0.0.0.63 host 192.168.4.2 

n6 permit ip 192.168.4.128 0.0.0.63 host 192.168.4.110 

n6 permit ip 192.168.4.128 0.0.0.63 host 192.168.4.111 

n7 permit ip 192.168.4.128 0.0.0.63 192.168.4.192 0.0.0.15 

n8 permit ip 192.168.4.128 0.0.0.63 192.168.4.0 0.0.0.127 

n9 permit ip 192.168.4.128 0.0.0.63 any

Vlan Production

n1 pas a mettre 

n2 permit tcp any any established

n3 deny ip 192.168.4.0 0.0.0.127 192.168.4.192 0.0.0.127 n4 deny ip 192.168.4.0 0.0.0.127 192.168.4.128 0.0.0.63 n5 permit ip 192.168.4.0 0.0.0.127 any 

Vlan AdminSYS

n1 pas a mettre 

n2 permit tcp any any established

n3  permit icmp 192.168.4.192 0.0.0.15  192.168.4.0 0.0.0.127

n3  permit icmp 192.168.4.192 0.0.0.15  192.168.4.128 0.0.0.63 n4 deny ip 192.168.4.192 0.0.0.15 192.168.4.128 0.0.0.63

n5 deny tcp 192.168.4.192 0.0.0.15 192.168.4.0 0.0.0.127 eq 22 n5 deny tcp 192.168.4.192 0.0.0.15 192.168.4.0 0.0.0.127 eq 3389 n6 permit ip 192.168.4.192 0.0.0.15 host 192.168.4.1 

n6 permit ip 192.168.4.192 0.0.0.15 host 192.168.4.2

n6 permit ip 192.168.4.192 0.0.0.15 host 192.168.4.110

n6 permit ip 192.168.4.192 0.0.0.15 host 192.168.4.111

n7 deny udp 192.168.4.192 0.0.0.15 host 192.168.4.10 eq 53

n7 deny udp 192.168.4.192 0.0.0.15 host 192.168.4.11 eq 53 ![]((../../medias/Aspose.Words.d263a690-b1e7-4b10-b312-25eb7d50543e.004.png)n8 deny tcp 192.168.4.192 0.0.0.15 host  192.168.4.125 eq 443 n9 permit ip 192.168.4.192 0.0.0.15 192.168.4.0 0.0.0.127 

n10  permit ip 192.168.4.192 0.0.0.15 any  
