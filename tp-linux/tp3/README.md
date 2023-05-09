# TP 3 : We do a little scripting

# I. Script carte d'identité

Vous allez écrire **un script qui récolte des informations sur le système et les affiche à l'utilisateur.** Il s'appellera `idcard.sh` et sera stocké dans `/srv/idcard/idcard.sh`.


## Rendu

📁 **Fichier `/srv/idcard/idcard.sh`**

🌞 **Vous fournirez dans le compte-rendu**, en plus du fichier, **un exemple d'exécution avec une sortie**, dans des balises de code.
```
[edgy@localhost idcard]$ sudo vi idcard.sh
[edgy@localhost idcard]$ sudo chmod +x idcard.sh
[edgy@localhost idcard]$ ls -al
total 4
drwxr-xr-x. 2 root root   23 Dec 12 19:10 .
drwxr-xr-x. 3 root root   20 Dec 12 19:09 ..
-rwxr-xr-x. 1 root root 1425 Dec 12 19:10 idcard.sh
[edgy@localhost idcard]$ cat idcard.sh
#!/bin/bash

#id card

echo "Machine name : $(hostname)"
echo "OS $(cat /etc/redhat-release) and kernel version is $(uname -r)"
echo "IP : $(ip a | grep "inet " | grep "brd" | cut -d " " -f6 | head -n 1)"
echo "RAM : $(free -h | grep "Mem" | cut -d" " -f26) memory available on $(free -h | grep "Mem" | cut -d" " -f12) total memory"
echo "Disk : $(df -h --total | grep total | cut -d" " -f21) space left"

echo "Top 5 process by RAM usage :"
for i in $(seq 2 6)
do
        process=$(ps -e -o command= --sort=-%mem | head -n ${i} | tail -n 1)
        echo " - ${process}"
done


echo "Listening ports :"
for i in $(seq 1 $(ss -lnH4pu | tr -s " " | wc -l))
do
        port=$(ss -lnH4pu | tr -s " " | cut -d" " -f4 | cut -d":" -f2 | head -n ${i} | tail -n 1)
        process=$(ss -lnHu4p | tr -s " " | cut -d" " -f6 | cut -d":" -f2 | cut -d'"' -f2 | head -n ${i} | tail -n 1)
        echo " - ${port} UDP : ${process}"
done
for i in $(seq 1 $(ss -lnH4pt | tr -s " " | wc -l))
do
        port=$(ss -lnH4pt | tr -s " " | cut -d" " -f4 | cut -d":" -f2 | head -n ${i} | tail -n 1)
        process=$(ss -lnHt4p | tr -s " " | cut -d" " -f6 | cut -d":" -f2 | cut -d'"' -f2 | head -n ${i} | tail -n 1)
        echo " - ${port} TCP : ${process}"
done

curl https://cataas.com/cat -o /cat 2> /dev/null
fileName=cat.$(file /cat | cut -d" " -f2 | tr '[:upper:]' '[:lower:]')
mv ./cat ${fileName}
echo Here is your random cat : ./${fileName}
[edgy@localhost idcard]$
```
```
[edgy@localhost idcard]$ sudo ./idcard.sh
Machine name : localhost.localdomain
OS Rocky Linux release 9.0 (Blue Onyx) and kernel version is 5.14.0-70.26.1.el9_0.x86_64
IP : 10.3.1.2/24
RAM : 669Mi memory available on 960Mi total memory
Disk : 7.1G space left
Top 5 process by RAM usage :
 - /usr/sbin/NetworkManager --no-daemon
 - /usr/lib/systemd/systemd --switched-root --system --deserialize 28
 - /usr/lib/systemd/systemd --user
 - sshd: edgy [priv]
 - /usr/lib/systemd/systemd-udevd
Listening ports :
 - 323 UDP : chronyd
 - 22 TCP : sshd
Here is your random cat : ./cat.directory #affiche une erreur si absence de sudo car Permission denied
```

# II. Script youtube-dl

## Rendu

📁 **Le script `/srv/yt/yt.sh`**

📁 **Le fichier de log `/var/log/yt/download.log`**, avec au moins quelques lignes

🌞 Vous fournirez dans le compte-rendu, en plus du fichier, **un exemple d'exécution avec une sortie**, dans des balises de code.

# III. MAKE IT A SERVICE

YES. Yet again. **On va en faire un [service](../../cours/notions/serveur/README.md#ii-service).**

L'idée :

➜ plutôt que d'appeler la commande à la main quand on veut télécharger une vidéo, **on va créer un service qui les téléchargera pour nous**

➜ le service devra **lire en permanence dans un fichier**

- s'il trouve une nouvelle ligne dans le fichier, il vérifie que c'est bien une URL de vidéo youtube
  - si oui, il la télécharge, puis enlève la ligne
  - sinon, il enlève juste la ligne

➜ **qui écrit dans le fichier pour ajouter des URLs ? Bah vous !**

- vous pouvez écrire une liste d'URL, une par ligne, et le service devra les télécharger une par une

---

Pour ça, procédez par étape :

- **partez de votre script précédent** (gardez une copie propre du premier script, qui doit être livré dans le dépôt git)
  - le nouveau script s'appellera `yt-v2.sh`
- **adaptez-le pour qu'il lise les URL dans un fichier** plutôt qu'en argument sur la ligne de commande
- **faites en sorte qu'il tourne en permanence**, et vérifie le contenu du fichier toutes les X secondes
  - boucle infinie qui :
    - lit un fichier
    - effectue des actions si le fichier n'est pas vide
    - sleep pendant une durée déterminée
- **il doit marcher si on précise une vidéo par ligne**
  - il les télécharge une par une
  - et supprime les lignes une par une

➜ **une fois que tout ça fonctionne, enfin, créez un service** qui lance votre script :

- créez un fichier `/etc/systemd/system/yt.service`. Il comporte :
  - une brève description
  - un `ExecStart` pour indiquer que ce service sert à lancer votre script
  - une clause `User=` pour indiquer que c'est l'utilisateur `yt` qui lance le script
    - créez l'utilisateur s'il n'existe pas
    - faites en sorte que le dossier `/srv/yt` et tout son contenu lui appartienne
    - le dossier de log doit lui appartenir aussi
    - l'utilisateur `yt` ne doit pas pouvoir se connecter sur la machine

```bash
[Unit]
Description=<Votre description>

[Service]
ExecStart=<Votre script>
User=yt

[Install]
WantedBy=multi-user.target
```

> Pour rappel, après la moindre modification dans le dossier `/etc/systemd/system/`, vous devez exécuter la commande `sudo systemctl daemon-reload` pour dire au système de lire les changements qu'on a effectué.

Vous pourrez alors interagir avec votre service à l'aide des commandes habituelles `systemctl` :

- `systemctl status yt`
- `sudo systemctl start yt`
- `sudo systemctl stop yt`

![Now witness](./pics/now_witness.png)

## Rendu

📁 **Le script `/srv/yt/yt-v2.sh`**

📁 **Fichier `/etc/systemd/system/yt.service`**

🌞 Vous fournirez dans le compte-rendu, en plus des fichiers :

- un `systemctl status yt` quand le service est en cours de fonctionnement
- un extrait de `journalctl -xe -u yt`

> Hé oui les commandes `journalctl` fonctionnent sur votre service pour voir les logs ! Et vous devriez constater que c'est vos `echo` qui pop. En résumé, **le STDOUT de votre script, c'est devenu les logs du service !**

🌟**BONUS** : get fancy. Livrez moi un gif ou un [asciinema](https://asciinema.org/) (PS : c'est le feu asciinema) de votre service en action, où on voit les URLs de vidéos disparaître, et les fichiers apparaître dans le fichier de destination

# IV. Bonus

Quelques bonus pour améliorer le fonctionnement de votre script :

➜ **en accord avec les règles de [ShellCheck](https://www.shellcheck.net/)**

- bonnes pratiques, sécurité, lisibilité

➜  **fonction `usage`**

- le script comporte une fonction `usage`
- c'est la fonction qui est appelée lorsque l'on appelle le script avec une erreur de syntaxe
- ou lorsqu'on appelle le `-h` du script

➜ **votre script a une gestion d'options :**

- `-q` pour préciser la qualité des vidéos téléchargées (on peut choisir avec `youtube-dl`)
- `-o` pour préciser un dossier autre que `/srv/yt/`
- `-h` affiche l'usage

➜ **si votre script utilise des commandes non-présentes à l'installation** (`youtube-dl`, `jq` éventuellement, etc.)

- vous devez TESTER leur présence et refuser l'exécution du script

➜  **si votre script a besoin de l'existence d'un dossier ou d'un utilisateur**

- vous devez tester leur présence, sinon refuser l'exécution du script

➜ **pour le téléchargement des vidéos**

- vérifiez à l'aide d'une expression régulière que les strings saisies dans le fichier sont bien des URLs de vidéos Youtube
