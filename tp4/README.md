# TP4 : TCP, UDP et services réseau

Dans ce TP on va explorer un peu les protocoles TCP et UDP. 

**La première partie est détente**, vous explorez TCP et UDP un peu, en vous servant de votre PC.

La seconde partie se déroule en environnement virtuel, avec des VMs. Les VMs vont nous permettre en place des services réseau, qui reposent sur TCP et UDP.  
**Le but est donc de commencer à mettre les mains de plus en plus du côté administration, et pas simple client.**

Dans cette seconde partie, vous étudierez donc :

- le protocole SSH (contrôle de machine à distance)
- le protocole DNS (résolution de noms)
  - essentiel au fonctionnement des réseaux modernes

![TCP UDP](./pics/tcp_udp.jpg)

# Sommaire

- [TP4 : TCP, UDP et services réseau](#tp4--tcp-udp-et-services-réseau)
- [Sommaire](#sommaire)
- [0. Prérequis](#0-prérequis)
- [I. First steps](#i-first-steps)
- [II. Mise en place](#ii-mise-en-place)
  - [1. SSH](#1-ssh)
  - [2. Routage](#2-routage)
- [III. DNS](#iii-dns)
  - [1. Présentation](#1-présentation)
  - [2. Setup](#2-setup)
  - [3. Test](#3-test)

# 0. Prérequis

➜ Pour ce TP, on va se servir de VMs Rocky Linux. On va en faire plusieurs, n'hésitez pas à diminuer la RAM (512Mo ou 1Go devraient suffire). Vous pouvez redescendre la mémoire vidéo aussi.  

➜ Si vous voyez un 🦈 c'est qu'il y a un PCAP à produire et à mettre dans votre dépôt git de rendu

➜ **L'emoji 🖥️ indique une VM à créer**. Pour chaque VM, vous déroulerez la checklist suivante :

- [x] Créer la machine (avec une carte host-only)
- [ ] Définir une IP statique à la VM
- [ ] Donner un hostname à la machine
- [ ] Vérifier que l'accès SSH fonctionnel
- [ ] Vérifier que le firewall est actif
- [ ] Remplir votre fichier `hosts`, celui de votre PC, pour accéder au VM avec un nom
- [ ] Dès que le routeur est en place, n'oubliez pas d'ajouter une route par défaut aux autres VM pour qu'elles aient internet

> Toutes les commandes pour réaliser ces opérations sont dans [le mémo Rocky](../../cours/memo/rocky_network.md). Aucune de ces étapes ne doit figurer dan le rendu, c'est juste la mise en place de votre environnement de travail.

# I. First steps

Faites-vous un petit top 5 des applications que vous utilisez sur votre PC souvent, des applications qui utilisent le réseau : un site que vous visitez souvent, un jeu en ligne, Spotify, j'sais po moi, n'importe.

🌞 **Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP**

- avec Wireshark, on va faire les chirurgiens réseau
- déterminez, pour chaque application :
  - IP et port du serveur auquel vous vous connectez
  - le port local que vous ouvrez pour vous connecter

ROTMG (Realm of the mad god (serveur UsEast))  
ip du serveur : 54.234.226  
port du serveur :2050    
port local :59191  
[rotmg](./rotmg.pcapng)  

tor  
ip du serveur : 146.59.19.38  
port du serveur :55487    
port local :8080  
[tor](./tor.pcapng)  

nmprog  
ip du serveur : is  
port du serveur :ps    
port local :pl  
[](./)   

nmprog  
ip du serveur : is  
port du serveur :ps    
port local :pl  
[](./)  

nmprog  
ip du serveur : is  
port du serveur :ps  
port local :pl  
[](./)  


🌞 **Demandez l'avis à votre OS**

``` 
$netstat -o -n -b
  [RotMG Exalt Launcher.exe]
  TCP    192.168.215.95:56744   142.250.179.80:443     ESTABLISHED     12768
  [tor.exe]
  TCP    127.0.0.1:9151         127.0.0.1:50003        ESTABLISHED     17160
```
(les ponts tor on changer)

# II. Mise en place

## 1. SSH

🖥️ **Machine `node1.tp4.b1`**

- n'oubliez pas de dérouler la checklist (voir [les prérequis du TP](#0-prérequis))
- donnez lui l'adresse IP `10.4.1.11/24`

Connectez-vous en SSH à votre VM.

🌞 **Examinez le trafic dans Wireshark**

SSH utilise TCP 

[capture_wireshark_ssh](./sshVMgitbash.pcapng)

🌞 **Demandez aux OS**

- repérez, avec une commande adaptée (`netstat` ou `ss`), la connexion SSH depuis votre machine
```
$ netstat -o -n -b

Connexions actives

  Proto  Adresse locale         Adresse distante       État
  TCP    10.4.1.1:50024         10.4.1.11:22           ESTABLISHED     10204
 [ssh.exe]
  TCP    192.168.215.95:50044   20.67.219.150:443      TIME_WAIT       0
  TCP    192.168.215.95:50047   20.82.250.189:443      TIME_WAIT       0
  TCP    192.168.215.95:50069   20.67.219.150:443      TIME_WAIT       0
  TCP    192.168.215.95:50073   20.67.219.150:443      TIME_WAIT       0
  TCP    192.168.215.95:50088   20.67.219.150:443      TIME_WAIT       0
  TCP    192.168.215.95:50093   20.67.219.150:443      TIME_WAIT       0
  TCP    192.168.215.95:50100   20.67.219.150:443      TIME_WAIT       0
  TCP    192.168.215.95:50101   20.67.219.150:443      TIME_WAIT       0
  TCP    192.168.215.95:50104   20.67.219.150:443      TIME_WAIT       0
  TCP    192.168.215.95:50108   20.73.130.64:443       TIME_WAIT       0
  TCP    192.168.215.95:50111   20.73.130.64:443       TIME_WAIT       0
  TCP    192.168.215.95:50115   20.73.130.64:443       TIME_WAIT       0
  TCP    192.168.215.95:50120   20.73.130.64:443       TIME_WAIT       0
  TCP    192.168.215.95:50128   104.208.16.90:443      TIME_WAIT       0
  TCP    192.168.215.95:50129   40.79.141.154:443      TIME_WAIT       0
  TCP    192.168.215.95:50130   40.79.141.154:443      TIME_WAIT       0
  TCP    192.168.215.95:50131   40.79.141.154:443      TIME_WAIT       0
  TCP    192.168.215.95:50132   40.79.141.154:443      TIME_WAIT       0
  TCP    192.168.215.95:50135   40.79.141.152:443      TIME_WAIT       0
  TCP    192.168.215.95:50136   40.79.141.152:443      TIME_WAIT       0
  TCP    192.168.215.95:50137   40.79.141.152:443      TIME_WAIT       0
  TCP    192.168.215.95:50138   20.50.73.10:443        TIME_WAIT       0
  TCP    192.168.215.95:50139   20.50.73.10:443        TIME_WAIT       0
  TCP    192.168.215.95:50140   20.50.73.10:443        TIME_WAIT       0
  TCP    192.168.215.95:50142   13.69.239.74:443       ESTABLISHED     10540
```
- ET repérez la connexion SSH depuis votre VM
```
$ ss -tup
Netid      State      Recv-Q      Send-Q            Local Address:Port             Peer Address:Port       Process
tcp        ESTAB      0           0                     10.4.1.11:ssh                  10.4.1.1:57115
tcp        ESTAB      0           52                    10.4.1.11:ssh
```

🦈 **Je veux une capture clean avec le 3-way handshake, un peu de trafic au milieu et une fin de connexion**

[capture_wireshark_ssh](./sshVMgitbash.pcapng)

## 2. Routage

Ouais, un peu de répétition, ça fait jamais de mal. On va créer une machine qui sera notre routeur, et **permettra à toutes les autres machines du réseau d'avoir Internet.**

🖥️ **Machine `router.tp4.b1`**

- n'oubliez pas de dérouler la checklist (voir [les prérequis du TP](#0-prérequis))
- donnez lui l'adresse IP `10.4.1.254/24` sur sa carte host-only
- ajoutez-lui une carte NAT, qui permettra de donner Internet aux autres machines du réseau
- référez-vous au TP précédent

> Rien à remettre dans le compte-rendu pour cette partie.

# III. DNS

## 1. Présentation

Un serveur DNS est un serveur qui est capable de répondre à des requêtes DNS.

Une requête DNS est la requête effectuée par une machine lorsqu'elle souhaite connaître l'adresse IP d'une machine, lorsqu'elle connaît son nom.

Par exemple, si vous ouvrez un navigateur web et saisissez `https://www.google.com` alors une requête DNS est automatiquement effectuée par votre PC pour déterminez à quelle adresse IP correspond le nom `www.google.com`.

> La partie `https://` ne fait pas partie du nom de domaine, ça indique simplement au navigateur la méthode de connexion. Ici, c'est HTTPS.

Dans cette partie, on va monter une VM qui porte un serveur DNS. Ce dernier répondra aux autres VMs du LAN quand elles auront besoin de connaître des noms. Ainsi, ce serveur pourra :

- résoudre des noms locaux
  - vous pourrez `ping node1.tp4.b1` et ça fonctionnera
  - mais aussi `ping www.google.com` et votre serveur DNS sera capable de le résoudre aussi

*Dans la vraie vie, il n'est pas rare qu'une entreprise gère elle-même ses noms de domaine, voire gère elle-même son serveur DNS. C'est donc du savoir ré-utilisable pour tous qu'on voit ici.*

> En réalité, ce n'est pas votre serveur DNS qui pourra résoudre `www.google.com`, mais il sera capable de *forward* (faire passer) votre requête à un autre serveur DNS qui lui, connaît la réponse.

![Haiku DNS](./pics/haiku_dns.png)

## 2. Setup

🖥️ **Machine `dns-server.tp4.b1`**

- n'oubliez pas de dérouler la checklist (voir [les prérequis du TP](#0-prérequis))
- donnez lui l'adresse IP `10.4.1.201/24`

Installation du serveur DNS :

```bash
# assurez-vous que votre machine est à jour
$ sudo dnf update -y

# installation du serveur DNS, son p'tit nom c'est BIND9
$ sudo dnf install -y bind bind-utils
```

La configuration du serveur DNS va se faire dans 3 fichiers essentiellement :

- **un fichier de configuration principal**
  - `/etc/named.conf`
  - on définit les trucs généraux, comme les adresses IP et le port où on veu écouter
  - on définit aussi un chemin vers les autres fichiers, les fichiers de zone
- **un fichier de zone**
  - `/var/named/tp4.b1.db`
  - je vous préviens, la syntaxe fait mal
  - on peut y définir des correspondances `IP ---> nom`
- **un fichier de zone inverse**
  - `/var/named/tp4.b1.rev`
  - on peut y définir des correspondances `nom ---> IP`

➜ **Allooooons-y, fichier de conf principal**

```bash
# éditez le fichier de config principal pour qu'il ressemble à :
$ sudo cat /etc/named.conf
options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
[...]
        allow-query     { localhost; any; };
        allow-query-cache { localhost; any; };

        recursion yes;
[...]
# référence vers notre fichier de zone
zone "tp4.b1" IN {
     type master;
     file "tp4.b1.db";
     allow-update { none; };
     allow-query {any; };
};
# référence vers notre fichier de zone inverse
zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp4.b1.rev";
     allow-update { none; };
     allow-query { any; };
};
```

➜ **Et pour les fichiers de zone**

```bash
# Fichier de zone pour nom -> IP

$ sudo cat /var/named/tp4.b1.db

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns-server IN A 10.4.1.201
node1      IN A 10.4.1.11
```

```bash
# Fichier de zone inverse pour IP -> nom

$ sudo cat /var/named/tp4.b1.rev

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

;Reverse lookup for Name Server
201 IN PTR dns-server.tp4.b1.
11 IN PTR node1.tp4.b1.
```

➜ **Une fois ces 3 fichiers en place, démarrez le service DNS**

```bash
# Démarrez le service tout de suite
$ sudo systemctl start named

# Faire en sorte que le service démarre tout seul quand la VM s'allume
$ sudo systemctl enable named

# Obtenir des infos sur le service
$ sudo systemctl status named

# Obtenir des logs en cas de probème
$ sudo journalctl -xe -u named
```

🌞 **Dans le rendu, je veux**
```
$ sudo cat /etc/named.conf

//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
	listen-on port 53 { 127.0.0.1; any; };
	listen-on-v6 port 53 { ::1; };
	directory 	"/var/named";
	dump-file 	"/var/named/data/cache_dump.db";
	statistics-file "/var/named/data/named_stats.txt";
	memstatistics-file "/var/named/data/named_mem_stats.txt";
	secroots-file	"/var/named/data/named.secroots";
	recursing-file	"/var/named/data/named.recursing";
	allow-query     { localhost; any; };
	allow-query-cache { localhost; any; };

	/* 
	 - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
	 - If you are building a RECURSIVE (caching) DNS server, you need to enable 
	   recursion. 
	 - If your recursive DNS server has a public IP address, you MUST enable access 
	   control to limit queries to your legitimate users. Failing to do so will
	   cause your server to become part of large scale DNS amplification 
	   attacks. Implementing BCP38 within your network would greatly
	   reduce such attack surface 
	*/
	recursion yes;

	dnssec-validation yes;

	managed-keys-directory "/var/named/dynamic";
	geoip-directory "/usr/share/GeoIP";

	pid-file "/run/named/named.pid";
	session-keyfile "/run/named/session.key";

	/* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
	include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "tp4.b1" IN {
	type master;
	file "tp4.b1.db";
	allow-update { none; };
	allow-query { any; };
};

zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp4.b1.rev";
     allow-update { none; };
     allow-query { any; };
};


include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";
```
```
$ sudo cat /var/named/tp4.b1.db

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns-server IN A 192.168.57.4
node1      IN A 192.168.57.2
```
```
$ sudo cat /var/named/tp4.b1.db

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns-server IN A 192.168.57.4
node1      IN A 192.168.57.2
[rocky@dns-server ~]$ sudo cat /var/named/tp4.b1.rev
$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

;Reverse lookup for Name Server
4 IN PTR dns-server.tp4.b1.
2 IN PTR node1.tp4.b1.
```
```
$ systemctl status named

● named.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; vendor preset: disabled)
     Active: active (running) 
   Main PID: 1048 (named)
      Tasks: 5 (limit: 5908)
     Memory: 18.9M
        CPU: 70ms
     CGroup: /system.slice/named.service
             └─1048 /usr/sbin/named -u named -c /etc/named.conf

Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: zone 1.0.0.127.in-addr.arpa/IN: loaded serial 0
Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: network unreachable resolving './NS/IN': 192.58.128.30#53
Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: network unreachable resolving './DNSKEY/IN': 198.41.0.4#>
Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: network unreachable resolving './NS/IN': 198.41.0.4#53
Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: resolver priming query complete
Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: zone localhost.localdomain/IN: loaded serial 0
Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: managed-keys-zone: Unable to fetch DNSKEY set '.': failu>
Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: all zones loaded
Oct 25 13:56:02 dns-server.tp4.b1 systemd[1]: Started Berkeley Internet Name Domain (DNS).
Oct 25 13:56:02 dns-server.tp4.b1 named[1048]: running
```
🌞 **Ouvrez le bon port dans le firewall**

```
$ sudo firewall-cmd --list-all
  ports: 53/udp
```

## 3. Test

🌞 **Sur la machine `node1.tp4.b1`**
```
$ sudo cat /etc/resolv.conf
nameserver 10.4.1.201
```
```
$ dig node1.tp4.b1

; <<>> DiG 9.16.23-RH <<>> node1.tp4.b1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 5616
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 849d68f46027d60201000000635a5b81bac411d0c249cd99 (good)
;; QUESTION SECTION:
;node1.tp4.b1.                  IN      A

;; ANSWER SECTION:
node1.tp4.b1.           86400   IN      A       10.4.1.11

;; Query time: 3 msec
;; SERVER: 10.4.1.201#53(10.4.1.201)
;; WHEN: Thu Oct 27 12:20:48 CEST 2022
;; MSG SIZE  rcvd: 85

[murci@node1 ~]$ dig dns-server.tp4.b1

; <<>> DiG 9.16.23-RH <<>> dns-server.tp4.b1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 55639
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 0057cc7e9808a0d701000000635a5b0ea240e05eb5838564 (good)
;; QUESTION SECTION:
;dns-server.tp4.b1.             IN      A

;; ANSWER SECTION:
dns-server.tp4.b1.      86400   IN      A       10.4.1.201

;; Query time: 2 msec
;; SERVER: 10.4.1.201#53(10.4.1.201)
;; WHEN: Thu Oct 27 12:18:54 CEST 2022
;; MSG SIZE  rcvd: 90
```
```
$ dig www.google.com

; <<>> DiG 9.16.23-RH <<>> www.google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 30115
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 030762852b798f5101000000635a5bad644dbbe3e558cd31 (good)
;; QUESTION SECTION:
;www.google.com.                        IN      A

;; ANSWER SECTION:
www.google.com.         300     IN      A       142.250.179.100

;; Query time: 312 msec
;; SERVER: 10.4.1.201#53(10.4.1.201)
;; WHEN: Thu Oct 27 12:21:32 CEST 2022
;; MSG SIZE  rcvd: 87
```

🌞 **Sur votre PC**

- utilisez une commande pour résoudre le nom `node1.tp4.b1` en utilisant `10.4.1.201` comme serveur DNS

> Le fait que votre serveur DNS puisse résoudre un nom comme `www.google.com`, ça s'appelle la récursivité et c'est activé avec la ligne `recursion yes;` dans le fichier de conf.

🦈 **Capture d'une requête DNS vers le nom `node1.tp4.b1` ainsi que la réponse**
