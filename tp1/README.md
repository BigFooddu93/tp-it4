# I. Exploration locale en solo

## 1. Affichage d'informations sur la pile TCP/IP locale

### En ligne de commande

En utilisant la ligne de commande (CLI) de votre OS : ipconfig /allcompartments /all

**🌞 Affichez les infos des cartes réseau de votre PC**

**- interface Wifi:**

- nom, adresse MAC : AC-82-47-F8-58-9A
- adresse IP de l'interface WiFi : 10.33.16.200

**- interface Ethernet**

- nom, adresse MAC : absence de carte éternet 
- adresse IP : absence de carte éternet

**🌞 Affichez votre gateway**
- gateway : 10.33.19.254

**🌞 Déterminer la MAC de la passerelle**

- 00-c0-e7-e0-04-4e

### En graphique (GUI : Graphical User Interface)

En utilisant l'interface graphique de votre OS :  

**🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)**
Panneau de configuration\Réseau et Internet\Centre Réseau et partage 
Wifi(Wifi@ynov) détail

## 2. Modifications des informations

### A. Modification d'adresse IP (part 1)  

🌞 Utilisez l'interface graphique de votre OS pour **changer d'adresse IP** :

Panneau de configuration\Réseau et Internet\Centre Réseau et partage 
Wifi(Wifi@ynov) propriétés \Protocole Internet verssion 4 


🌞 **Il est possible que vous perdiez l'accès internet.**

l'on peut perdre l'accées internet si l'on se connecte sur une adresse déjà utiliser 

# II. Exploration locale en duo

### 3. Modification d'adresse IP
####  Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau
- Connecter nos deuc ordinateurs en RJ45
- Comme à l'étape précédente je modifie manuelle l'adresse IP
- Je me met sur le même réseau en mettant 10.10.10.1 et 10.10.10.2
- Mettre le masque de sous-réseau à /24 soit 255.255.255.0

#### Vérifier à l'aide d'une commande que votre IP a bien été changée
Réalisé un Ipconfig /all et vérifié que l'IP soit bien présente dans la carte Ethernet

#### Vérifier que les deux machines se joignent

- puis ping vers l'IP de l'autre ordinateur
- ping 10.10.10.1
- Réponse renvoyé par le terminal

#### Déterminer l'adresse MAC de votre correspondant
Commande à utilisé : arp -a
La commande affiche l'IP de mon correspondant qui est 10.10.10.1 et donc je peux récupéré le mac de son ip qui est  
"50-a0-30-02-ef-59"

#### Tester l'accès internet
je réalise un ping 1.1.1.1 pour vérifié la connectivité à internet.
Le teste est fonctionnel.

#### Prouver que la connexion Internet passe bien par l'autre PC
Si nous fessons un traceroute vers 1.1.1.1 il renvoie dans le chemin d'accès notre gateway(passerelle)


#### Sur le PC serveur
Installer netcat
On créer un numéro de port avec la commande nc.exe -l -p 8888, le port est 8888 et devons ensuite attendre que le client ce connecte pour établir une connexion.
Le "l" signifie l'écoute et le port pour désigné un port.

####  Sur le PC client
Installer netcat
Le Client ce connecte a l'ip du serveur et au port qu'a désigné le serveur, a ce moment la notre client et serveur comunique il peuvent ce parler.

 #### Visualiser la connexion en cours

Avec la commande "netstat -a -n -b" le serveur visualise la connexion tcp en renvoyant l'ip et le port du serveur ainsi que l'IP et le port du client connecté et indique que la connexion est établie.

#### Pour aller un peu plus loin
Elle indique que le serveur écoute en attendant qu'un client ce connect
- nc.exe -1 -p "le numero de port" -s "IP_ADDRESS"
le serveur vas dire que toute les personne ayant la connaisance du port ici 8888 peut si connecter que si ils ont l IP

- nc.exe -1 -p "le numero de port"
alors qu en effectuer la commande sans le -s tout le monde peut si connecter grace au wifi apres en verifient 
nous verrons que notre adresse IP est a 0.0.0.8888, on indique l'IP car sinon tout le monde pourrait ce connecter directement en Wifi depuis le port 8888.

### 6. Firewall

#### Activez et configurez votre firewall

#### autoriser les ping
- Appuyez sur Démarrer, tapez «pare-feu Windows avec», puis lancez «Pare-feu Windows avec sécurité avancée».
- Vous allez créer une nouvelle règles: une pour autoriser les requêtes ICMPv4
- Vous allez créer deux nouvelles règles: une pour autoriser les requêtes ICMPv4
- Dans la fenêtre « Assistant Nouvelle règle entrante », sélectionnez « Personnalisé », puis cliquez sur « Suivant »
- Sur la page suivante, assurez-vous que «Tous les programmes» est sélectionné, puis cliquez sur «Suivant».
- Sur la page suivante, choisissez «ICMPv4» dans la liste déroulante «Type de protocole», puis cliquez sur le bouton «Personnaliser».
- Dans la fenêtre «Personnaliser les paramètres ICMP», sélectionnez l’option «Types ICMP spécifiques». Dans la liste des types ICMP, activez « Demande d’écho », puis cliquez sur « OK ».
- De retour dans la fenêtre « Assistant Nouvelle règle entrante », vous êtes prêt à cliquer sur « Suivant ».
- Sur la page suivante, il est plus simple de s’assurer que les options «Toute adresse IP» sont sélectionnées pour les adresses IP locales et distantes puis faite suivant
- Sur la page suivante, assurez-vous que l’option « Autoriser la connexion » est activée, puis cliquez sur « Suivant ».
- La page suivante vous permet de contrôler le moment où la règle est active- Tous sélectionner et suivant
- Et enfin donner le nom que l'on souhaite donc ping ICMP

#### autoriser le traffic sur le port qu'utilise nc

- Retourner au même endroit pour créer une nouvelle règle dans le trafic entrant
- Dans la page Type de règle de l’Assistant Nouvelle règle de trafic entrant, cliquez sur Personnalisé, puis sur Suivant.
- Dans la page Programme , cliquez sur Tous les programmes, puis sur Suivant
- Dans la page Protocole et ports , sélectionnez le type de protocole que vous souhaitez autoriser. Pour limiter la règle à un numéro de port spécifié, vous devez sélectionner TCP ou UDP. Dans notre cas nous pouvons spécifié notre port et alors une echelle de port.
- Une fois que vous avez configuré les protocoles et les ports, cliquez sur Suivant.
- Dans la page Étendue cliquez sur Suivant
- Dans la page Action , sélectionnez Autoriser la connexion, puis cliquez sur Suivant.
- Dans la page Profil , sélectionnez les types d’emplacement réseau auxquels cette règle s’applique, puis cliquez sur Suivant. Pour nous on sélectionne tous.
- Dans la page Nom , tapez un nom et une description pour votre règle, puis cliquez sur Terminer, Dans notre cas je l'ai appelé TCP.

# III. Manipulations d'autres outils/protocoles côté client

## 1. DHCP

🌞**Exploration du DHCP, depuis votre PC**

IP serveur DHCP : 10.33.19.254

duré du bail: 24h

commande : ipconfig /allcompartments /all

## 2. DNS

🌞** Trouver l'adresse IP du serveur DNS que connaît votre ordinateur**

ip DNS : 8.8.8.8

ip DNS : 8.8.4.4

ip DNS : 1.1.1.1

🌞 Utiliser, en ligne de commande l'outil `nslookup` (Windows, MacOS) ou `dig` (GNU/Linux, MacOS) pour faire des requêtes DNS à la main

comande : nslookup 
```
lookup google.com :
Serveur :   dns2.proxad.net
Address:  212.27.40.241
Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:80e::200e
          142.250.178.142

lookup ynov.com :
Serveur :   dns2.proxad.net
Address:  212.27.40.241

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::681a:ae9
          2606:4700:20::ac43:4ae2
          2606:4700:20::681a:be9
          104.26.11.233
          104.26.10.233
          172.67.74.226

lookup 231.34.113.12 :
Serveur :   dns2.proxad.net
Address:  212.27.40.241

*** dns2.proxad.net ne parvient pas à trouver 231.34.113.12 : Non-existent domain

lookup 78.34.2.17 :
Serveur :   dns2.proxad.net
Address:  212.27.40.241
Nom :    cable-78-34-2-17.nc.de
Address:  78.34.2.17
````
# IV. Wireshark

🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

n'as pas été réalise en raison d'une absence de port Ethernet sur ma machine (même si j'ai pue le voir avec Yohan et Baptiste qui eux pouvais le faire)

![cap](https://github.com/BigFooddu93/tp-reso/blob/main/capture/1.png)

![cap](https://github.com/BigFooddu93/tp-reso/blob/main/capture/2.png)

Essai nslookup blendermarket.com :

![capture](https://github.com/BigFooddu93/tp-reso/blob/main/capture/3.png)
