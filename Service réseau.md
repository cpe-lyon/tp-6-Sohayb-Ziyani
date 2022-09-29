# TP 6 - Services réseau 

## Exercice 1. Adressage IP 

On a 7 sous-réseaux et le sous-réseau avec le plus grand nombres d'hôtes est le 3 (52 hôtes) donc il faut se baser sur ce dernier pour commencer le VLSM. 
On a donc besoin d'un /26 puisque 2^6 - 2 = 62 hôtes : 

- 172.16.0.0/26 -> sous réseaux 3 (52 machines)
adresse de broadcast : 172.16.0.63/26
première adresse : 172.16.0.1/26
dernière adresse : 172.16.0.62/26
- 172.16.0.64/26 -> sous réseaux 1 (38 machines)
adresse de broadcast : 172.16.0.127/26
première adresse : 172.16.0.65/26
dernière adresse : 172.16.0.126/26
- 172.16.0.128/26 -> sous réseaux 6 (37 machines)
adresse de broadcast : 172.16.0.191/26
première adresse : 172.16.0.129/26
dernière adresse : 172.16.0.190/26
- 172.16.0.192/26 -> sous réseaux 4 (35 machines)
adresse de broadcast : 172.16.0.255/26
première adresse : 172.16.0.193/26
dernière adresse : 172.16.0.254/26
- 172.16.1.0/26 -> sous réseaux 5 (34 machines)
adresse de broadcast : 172.16.1.63/26
première adresse : 172.16.1.1/26
dernière adresse : 172.16.1.62/26
- 172.16.1.64/26 -> sous réseaux 2 (33 machines)
adresse de broadcast : 172.16.1.127/26
première adresse : 172.16.1.65/26
dernière adresse : 172.16.1.126/26

Pour le dernier sous-réseau nous n'avons plus besoin d'un /26 mais d'un /27 puisque 2^5 - 2 = 30 hôtes : 

- 172.16.1.128/27 -> sous réseaux 7 (25 machines)
adresse de broadcast : 172.16.1.159/27
première adresse : 172.16.1.129/27
dernière adresse : 172.16.1.158/27

## Exercice 2. Préparation de l'environnement 

### 1. VM éteintes, utilisez les outils de configuration de VirtualBox pour mettre en place l’environnement décrit ci-dessus.

On connecte le client au réseau local :

![image](https://user-images.githubusercontent.com/80455771/193028077-38b02377-5757-4a66-addf-43c1151e2327.png)

On connecte le serveur au réseau local et on lui rajoute une deuxième carte réseau pour qu'il puisse se connecter à internet :

![image](https://user-images.githubusercontent.com/80455771/193028818-69e5bc43-a88e-40ab-9e33-d8449163c26c.png)

### 2. Démarrez le serveur et vérifiez que les interfaces réseau sont bien présentes. A quoi correspond l’interface appelée lo ?

L'interface 'lo' est l'interface de loopback avec comme ip 127.0.0.1

### 3. Désinstallez complètement ce paquet 

Désinstallation du paquet sur le serveur avec la commande `apt remove could-init`:

![image](https://user-images.githubusercontent.com/80455771/193032199-efa717e8-0d16-4702-b6d3-8c65841377dc.png)

Désinstallation du paquet sur le client avec la commande `apt remove could-init` :

![image](https://user-images.githubusercontent.com/80455771/193032916-e9cff7a5-9e37-4899-b7f7-e62f5c7435db.png)

### 4. Les deux machines serveur et client se trouveront sur le domaine tpadmin.local. A l’aide de lacommande hostnamectl renommez le serveur (le changement doit persister après redémarrage, donc cherchez les bonnes options dans le manuel !). On peut afficher le nom et le domaine d’une machine avec les commandes hostname et/ou dnsdomainname ou en affichant le contenu du fichier /etc/hostname.

On modifie le nom de la machine avec la commande `hostanmectl` :

![image](https://user-images.githubusercontent.com/80455771/193036720-48a8b231-3c6a-432f-ac96-7b6929b2a271.png)
