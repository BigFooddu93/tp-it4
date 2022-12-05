# TP3 : On va router des trucs
## 0. Prérequis


## I. ARP

### 1. Echange ARP

🌞**Générer des requêtes ARP**

- effectuer un `ping` d'une machine à l'autre
```
$ ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=64 times=1.06
```
- observer les tables ARP des deux machines
```
$ ip neight show //from marcel
10.3.1.11 dev enp0s8 lladdr 08:00:27:51:f2:b5 STALE
```
``` 
$ ip neigh show //from john
10.3.1.12 dev enp0s8 lladdr 08:00:27:cb:e1:e9 STALE
```
- prouvez que l'info est correcte (que l'adresse MAC que vous voyez dans la table est bien celle de la machine correspondante)
  - une commande pour voir la MAC de `marcel` dans la table ARP de `john`
  ```
  $ ip neigh show 10.3.1.12
  10.3.1.12 dev enp0s8 lladdr 08:00:27:cb:e1:e9 STALE
  ```
  - et une commande pour afficher la MAC de `marcel`, depuis `marcel`
  ```
  $ ip a //in 2: enp0s8
  link/ether 08:00:27:cb:e1:e9 brd ff:ff:ff:ff:ff:ff
  ```

### 2. Analyse de trames

🌞**Analyse de trames**

```
dropped privs to tcpdump
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on enp0s8, link-type EN10MB (Ethernet), snapshot length 262144 bytes
10:59:39.100677 ARP, Request who-has localhost.localdomain tell 10.3.1.12, length 46
10:59:39.100694 ARP, Reply localhost.localdomain is-at 08:00:27:51:f2:b5 (oui Unknown), length 28
10:59:39.100892 IP 10.3.1.12 > localhost.localdomain: ICMP echo request, id 4, seq 1, length 64
10:59:39.100937 IP localhost.localdomain > 10.3.1.12: ICMP echo reply, id 4, seq 1, length 64
10:59:40.147782 IP 10.3.1.12 > localhost.localdomain: ICMP echo request, id 4, seq 2, length 64
10:59:40.147820 IP localhost.localdomain > 10.3.1.12: ICMP echo reply, id 4, seq 2, length 64
10:59:41.172525 IP 10.3.1.12 > localhost.localdomain: ICMP echo request, id 4, seq 3, length 64
10:59:41.172572 IP localhost.localdomain > 10.3.1.12: ICMP echo reply, id 4, seq 3, length 64
10:59:42.196760 IP 10.3.1.12 > localhost.localdomain: ICMP echo request, id 4, seq 4, length 64
10:59:42.196817 IP localhost.localdomain > 10.3.1.12: ICMP echo reply, id 4, seq 4, length 64
```

## II. Routage

Vous aurez besoin de 3 VMs pour cette partie. **Réutilisez les deux VMs précédentes.**

| Machine  | `10.3.1.0/24` | `10.3.2.0/24` |
|----------|---------------|---------------|
| `router` | `10.3.1.254`  | `10.3.2.254`  |
| `john`   | `10.3.1.11`   | no            |
| `marcel` | no            | `10.3.2.12`   |

```schema
   john                router              marcel
  ┌─────┐             ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     ├────┤ho2├────┤     │
  └─────┘    └───┘    └─────┘    └───┘    └─────┘
```

### 1. Mise en place du routage

🌞**Activer le routage sur le noeud `router`**

```
$ sudo firewall-cmd --list-all
public (active)
  interfaces: enp0s3 enp0s8
  forward: yes
  masquerade: yes
```

🌞**Ajouter les routes statiques nécessaires pour que `john` et `marcel` puissent se `ping`**
```
$ ip a //router
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:60:44:b4 brd ff:ff:ff:ff:ff:ff
    inet 10.3.2.254/24 brd 10.3.2.255 scope global noprefixroute enp0s3
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe60:44b4/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:39:c7:51 brd ff:ff:ff:ff:ff:ff
    inet 10.3.1.254/24 brd 10.3.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe39:c751/64 scope link
       valid_lft forever preferred_lft forever
### 2. Analyse de trames
```
```
$ ip a //john
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:51:f2:b5 brd ff:ff:ff:ff:ff:ff
    inet 10.3.1.11/24 brd 10.3.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe51:f2b5/64 scope link
       valid_lft forever preferred_lft forever

$ sudo ip r add 10.3.2.12 via 10.3.1.254
```
``` 
$ ip a //marcel
2: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:cb:e1:e9 brd ff:ff:ff:ff:ff:ff
    inet 10.3.2.12/24 brd 10.3.2.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fecb:e1e9/64 scope link
       valid_lft forever preferred_lft forever

$ sudo ip r add 10.3.1.11 via 10.3.2.254

$ ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=63 time=0.843 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=63 time=1.51 ms
64 bytes from 10.3.1.11: icmp_seq=3 ttl=63 time=1.84 ms
^C
--- 10.3.1.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2017ms
rtt min/avg/max/mdev = 0.843/1.394/1.835/0.412 ms
```

🌞**Analyse des échanges ARP**

- videz les tables ARP des trois noeuds
- effectuez un `ping` de `john` vers `marcel`
- regardez les tables ARP des trois noeuds
- essayez de déduire un peu les échanges ARP qui ont eu lieu
- répétez l'opération précédente (vider les tables, puis `ping`), en lançant `tcpdump` sur `marcel`
- **écrivez, dans l'ordre, les échanges ARP qui ont eu lieu, puis le ping et le pong, je veux TOUTES les trames** utiles pour l'échange

| ordre | type trame  | IP source | MAC source              | IP destination | MAC destination            |
|-------|-------------|-----------|-------------------------|----------------|----------------------------|
| 1     | Requête ARP | 10.3.1.11         | `john` `08:00:27:39:c7:51`  | 10.3.1.254              | Broadcast `FF:FF:FF:FF:FF` |
| 2     | Réponse ARP | 10.3.1.254         | `router ` ` 08:00:27:39:c7:51`                       | 10.3.1.11              | `john` `08:00:27:39:c7:51`     |
| 3   | Ping         | 10.3.1.11       | `john` `08:00:27:39:c7:51`             |10.3.2.12                | `router ` ` 08:00:27:39:c7:51`                            |
| 4   | Requête ARP  | 10.3.2.254          | `router ` ` 08:00:27:60:44:b4`                        |10.3.2.12                     | Broadcast `FF:FF:FF:FF:FF`|
| 5   | Réponse ARP  | 10.3.2.12           | `marcel` ` 08:00:27:cb:e1:e9`                  |10.3.2.254                     | `router ` ` 08:00:27:60:44:b4`     |
| 6   | Ping         | 10.3.2.254           | `router ` ` 08:00:27:60:44:b4`                        |10.3.2.12                      | `marcel` ` 08:00:27:cb:e1:e9`                              |
| 7     | Pong        | 10.3.2.12         | `marcel` ` 08:00:27:cb:e1:e9`                        | 10.3.2.254              | `router ` ` 08:00:27:60:44:b4 `                          |
| 8     | Pong        | 10.3.2.12        | `router ` ` 08:00:27:39:c7:51`                       | 10.3.1.11              | `john` `08:00:27:39:c7:51`                           |



### 3. Accès internet

🌞**Donnez un accès internet à vos machines**

- ajoutez une carte NAT en 3ème inteface sur le `router` pour qu'il ait un accès internet
- ajoutez une route par défaut à `john` et `marcel`
  - vérifiez que vous avez accès internet avec un `ping`
  - le `ping` doit être vers une IP, PAS un nom de domaine
- donnez leur aussi l'adresse d'un serveur DNS qu'ils peuvent utiliser
  - vérifiez que vous avez une résolution de noms qui fonctionne avec `dig`
  - puis avec un `ping` vers un nom de domaine

🌞**Analyse de trames**

- effectuez un `ping 8.8.8.8` depuis `john`
- capturez le ping depuis `john` avec `tcpdump`
- analysez un ping aller et le retour qui correspond et mettez dans un tableau :

| ordre | type trame | IP source          | MAC source              | IP destination | MAC destination |     |
|-------|------------|--------------------|-------------------------|----------------|-----------------|-----|
| 1     | ping       | `marcel` `10.3.1.12` | `marcel` `AA:BB:CC:DD:EE` | `8.8.8.8`      | ?               |     |
| 2     | pong       | ...                | ...                     | ...            | ...             | ... |

🦈 **Capture réseau `tp3_routage_internet.pcapng`**

## III. DHCP

On reprend la config précédente, et on ajoutera à la fin de cette partie une 4ème machine pour effectuer des tests.

| Machine  | `10.3.1.0/24`              | `10.3.2.0/24` |
|----------|----------------------------|---------------|
| `router` | `10.3.1.254`               | `10.3.2.254`  |
| `john`   | `10.3.1.11`                | no            |
| `bob`    | oui mais pas d'IP statique | no            |
| `marcel` | no                         | `10.3.2.12`   |

```schema
   john               router              marcel
  ┌─────┐             ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     ├────┤ho2├────┤     │
  └─────┘    └─┬─┘    └─────┘    └───┘    └─────┘
   dhcp        │
  ┌─────┐      │
  │     │      │
  │     ├──────┘
  └─────┘
```

### 1. Mise en place du serveur DHCP

🌞**Sur la machine `john`, vous installerez et configurerez un serveur DHCP** (go Google "rocky linux dhcp server").

- installation du serveur sur `john`
- créer une machine `bob`
- faites lui récupérer une IP en DHCP à l'aide de votre serveur

> Il est possible d'utilise la commande `dhclient` pour forcer à la main, depuis la ligne de commande, la demande d'une IP en DHCP, ou renouveler complètement l'échange DHCP (voir `dhclient -h` puis call me et/ou Google si besoin d'aide).

🌞**Améliorer la configuration du DHCP**

- ajoutez de la configuration à votre DHCP pour qu'il donne aux clients, en plus de leur IP :
  - une route par défaut
  - un serveur DNS à utiliser
- récupérez de nouveau une IP en DHCP sur `bob` pour tester :
  - `bob` doit avoir une IP
    - vérifier avec une commande qu'il a récupéré son IP
    - vérifier qu'il peut `ping` sa passerelle
  - il doit avoir une route par défaut
    - vérifier la présence de la route avec une commande
    - vérifier que la route fonctionne avec un `ping` vers une IP
  - il doit connaître l'adresse d'un serveur DNS pour avoir de la résolution de noms
    - vérifier avec la commande `dig` que ça fonctionne
    - vérifier un `ping` vers un nom de domaine

### 2. Analyse de trames

🌞**Analyse de trames**

- lancer une capture à l'aide de `tcpdump` afin de capturer un échange DHCP
- demander une nouvelle IP afin de générer un échange DHCP
- exportez le fichier `.pcapng`

🦈 **Capture réseau `tp3_dhcp.pcapng`**
