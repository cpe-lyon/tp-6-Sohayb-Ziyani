# TP 6 - Services réseau 

## Exercice 1. Adressage IP 

On a 7 sous-réseaux et le sous-réseau avec le plus grand nombres d'hôtes est le 3 (52 hôtes) donc il faut se baser sur ce dernier pour commencer le VLSM. 
On a donc besoin d'un /26 puisque 2^6 - 2 = 62 hôtes : 

- 172.16.0.0/26 -> sous réseaux 3 (52 machines) /
adresse de broadcast : 172.16.0.63/26 /
première adresse : 172.16.0.1/26 /
dernière adresse : 172.16.0.62/26 //
- 172.16.0.64/26 -> sous réseaux 1 (38 machines) /
adresse de broadcast : 172.16.0.127/26 /
première adresse : 172.16.0.65/26 /
dernière adresse : 172.16.0.126/26 //
- 172.16.0.128/26 -> sous réseaux 6 (37 machines) /
adresse de broadcast : 172.16.0.191/26 /
première adresse : 172.16.0.129/26 /
dernière adresse : 172.16.0.190/26 //
- 172.16.0.192/26 -> sous réseaux 4 (35 machines) /
adresse de broadcast : 172.16.0.255/26 /
première adresse : 172.16.0.193/26 /
dernière adresse : 172.16.0.254/26 //
- 172.16.1.0/26 -> sous réseaux 5 (34 machines) /
adresse de broadcast : 172.16.1.63/26 /
première adresse : 172.16.1.1/26 /
dernière adresse : 172.16.1.62/26 //
- 172.16.1.64/26 -> sous réseaux 2 (33 machines) /
adresse de broadcast : 172.16.1.127/26 /
première adresse : 172.16.1.65/26 /
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

## Exercice 3. Installation du serveur DHCP

### 1. Sur le serveur, installez le paquet isc-dhcp-server. La commande systemctl status isc dhcp-server devrait vous indiquer que le serveur n’a pas réussi à démarrer, ce qui est normal puisqu’il n’est pas encore configuré (en particulier, il n’a pas encore d’adresses IP à distribuer).

On peut voir qu'il y a bien une erreur :

![image](https://user-images.githubusercontent.com/80455771/193038369-93d445d5-3acd-446a-8165-b54f3888330e.png)

### 2. Un serveur DHCP a besoin d’une IP statique. Attribuez de manière permanente l’adresse IP 192.168.100.1 à l’interface réseau du réseau interne. Vérifiez que la configuration est correcte.

Dans le fichier `etc/netplan/50-cloud-init.yaml` on ajoute quelques lignes pour mettre notre serveur en IP fixe : 

![image](https://user-images.githubusercontent.com/80455771/193457779-5bf533b9-8d14-4564-bb09-1596bd9cb673.png)

et on peut ensuite voir que le changement a bien été fait : 

![image](https://user-images.githubusercontent.com/80455771/193457802-cdf4b796-f41f-41fd-9aba-38dfecd57bf7.png)

### 3. La configuration du serveur DHCP se fait via le fichier /etc/dhcp/dhcpd.conf. Faites une sauvegarde du fichier existant sous le nom dhcpd.conf.bak puis éditez le fichier dhcpd.conf avec les informations suivantes :

Modification du fichier `dhcpd.conf.bak` :

![image](https://user-images.githubusercontent.com/80455771/193048668-b111eade-588d-4f4f-b5e7-0abd3610b943.png)

`Default-lease-time` est le temps par défault pendant lequel la machine peut garder l'adresse IP et `max-lease-time` est son maximum 

### 4. Editez le fichier /etc/default/isc-dhcp-server afin de spécifier l’interface sur laquelle le serveur doit écouter

![image](https://user-images.githubusercontent.com/80455771/193051052-4539a543-8274-4f6f-8d11-fef1bc2540a8.png)

### 5. Validez votre fichier de configuration avec la commande dhcpd -t puis redémarrez le serveur DHCP (avec la commande systemctl restart isc-dhcp-server) et vérifiez qu’il est actif.

On peut voir que le serveur est bien actif

![image](https://user-images.githubusercontent.com/80455771/193055668-1dd946f4-da3a-492f-a256-cb40f244fdc7.png)

### 6. 

Renommage de la machine client : 

![image](https://user-images.githubusercontent.com/80455771/193061403-e8a548e4-8f95-4687-b90e-54bc08053a01.png)

### 7. La commande tail -f /var/log/syslog affiche de manière continue les dernières lignes du fichier de log du système (dès qu’une nouvelle ligne est écrite à la fin du fichier, elle est affichée à l’écran). Lancez cette commande sur le serveur, puis connectez la carte réseau du client et observez les logs sur le serveur. Expliquez à quoi correspondent les messages DHCPDISCOVER, DHCPOFFER, DHCPREQUEST, DHCPACK. Vérifiez que le client reçoit bien une adresse IP de la plage spécifiée précédemment.

Voici ce qu'on obtient avec la commande `tail -f /var/log/syslog` : 

![image](https://user-images.githubusercontent.com/80455771/193458746-fd38faea-0acb-4ad6-87f8-b2d1aaf15a36.png)

DHCPDISCOVER correspong au message du client pour voir s'il y a bien un serveur DHCP disponible sur le réseau et avoir sa première configuration, DHCPOFFER est la réponse du serveur au message du client avec les premiers paramètres, DHCPREQUEST est une réquête du client pour nimporte quel bail, DHCPACK est la réponse du serveur qui contient l'adresse IP du client.

### 8. Que contient le fichier /var/lib/dhcp/dhcpd.leases sur le serveur, et qu’afficle la commande dhcp-lease-list ?

Le fichier dhcpd.leases stocke automatiquement les informations d'attribution des IP 

La commande `dhcp-lease-list` affiche les informations des différents client à qui on a attribué une addresse IP dans notre cas qu'une seule ligne :

![image](https://user-images.githubusercontent.com/80455771/193460226-575707a3-f1f9-435f-85f8-2d8d52646e4a.png)

### 9. Vérifiez que les deux machines peuvent communiquer via leur adresse IP, à l’aide de la commande ping

Le ping marche

![image](https://user-images.githubusercontent.com/80455771/193460303-c2b8b90a-cf7f-4e4f-ab6b-50992cc13cf0.png)

### 10. Modifiez la configuration du serveur pour que l’interface réseau du client reçoive l’IP statique 192.168.100.20 

On effectue les changements dans le fichier conf du serveur :

![image](https://user-images.githubusercontent.com/80455771/193461743-b3c6a223-78f0-472e-bac0-0c16ed40eadb.png)

Et on peut voir quel la machine cliente a bien reçu son IP fixe : 

![image](https://user-images.githubusercontent.com/80455771/193461724-175eeda0-3999-4208-83f1-452dd3140374.png)

## Exercice 4. Donner un accès à Internet au client

### 1. La première chose à faire est d’autoriser l’IP forwarding sur le serveur (désactivé par défaut, étant donné que la plupart des utilisateurs n’en ont pas besoin). Pour cela, il suffit de décommenter la ligne net.ipv4.ip_forward=1 dans le fichier /etc/sysctl.conf. Pour que les changements soient pris en compte immédiatement, il faut saisir la commande sudo sysctl -p /etc/sysctl.conf.

Décommentation de la ligne : 

![image](https://user-images.githubusercontent.com/80455771/193464908-953f10a0-6d7e-4a71-b6b2-78984f079c26.png)

### 2. Ensuite, il faut autoriser la traduction d’adresse source (masquerading) en ajoutant la règle iptables suivante :

On tape la commande en indiquant le bon port : 

![image](https://user-images.githubusercontent.com/80455771/193465437-8b493ff1-ec0b-475d-bdd3-3b8f5a54f986.png)

Et on voit que le client ping bien à l'éxterieur et on prend par exemple 1.1.1.1 :

![image](https://user-images.githubusercontent.com/80455771/193465473-f416a865-9c92-46f4-84a1-5d498aeff41e.png)

## Exercice 5. Istallation du serveur DNS

### 2. Nous allons donc modifier son fichier de configuration : /etc/bind/named.conf.options. Dans ce fichier, décommentez la partie forwarders, et à la place de 0.0.0.0, renseignez les IP de DNS publics comme 1.1.1.1 et 8.8.8.8 (en terminant à chaque fois par un point virgule). Redémarrez le serveur bind9

Décommentation et on ajoute les IPs de DNS :

![image](https://user-images.githubusercontent.com/80455771/193465815-1e28ada1-701f-477e-b97a-e74c38a5ff6b.png)

### 3. Sur le client, retentez un ping sur www.google.fr. Cette fois ça devrait marcher ! On valide ainsi la configuration du DHCP effectuée précédemment, puisque c’est grâce à elle que le client a trouvé son serveur DNS.

On peut voir que le ping vers google marche bien : 

![image](https://user-images.githubusercontent.com/80455771/193466153-650399c7-5e1d-453b-9966-72c00b49f98c.png)

### 4. Sur le client, installez le navigateur en mode texte lynx et essayez de surfer sur fr.wikipedia.org (bienvenue dans le passé...)

Après avoir installer le paquet Lynx avec la commande `apt install lynx` et fait une recherche avec la commande `lynx fr.wikipédia.org` on tombe sur ça :

![image](https://user-images.githubusercontent.com/80455771/193466411-876fdc00-741c-4289-9440-3cdedf7e9e40.png)

## Exercice 6. Configuration du serveur DNS pour la zone tpadmin.local

### 1. Modifiez le fichier /etc/bind/named.conf.local et ajoutez les lignes suivantes 

Modification du fichier : 

![image](https://user-images.githubusercontent.com/80455771/193466637-7983cc7f-8e53-4e99-ac04-61eb2488c942.png)

### 2. Créez une copie appelée db.tpadmin.local du fichier db.local. Ce fichier est un fichier configuration typique de DNS, constitué d’enregistrements DNS (cf. cours). Dans le nouveau fichier, remplacez toutes les références à localhost par tpadmin.local, et l’adresse 127.0.0.1 par l’adresse IP du serveur.

On effectue les remplacements dans le fichier : 

![image](https://user-images.githubusercontent.com/80455771/193469434-5cfd0d56-0b08-41a3-a259-f5e6975fd0db.png)

### 3. Maintenant que nous avons configuré notre fichier de zone, il reste à configurer le fichier de zone inverse, qui permet de convertir une adresse IP en nom.

Modification du fichier `named.conf.local` :

![image](https://user-images.githubusercontent.com/80455771/193467213-d5c54d13-c15d-49aa-8271-729e5e87881b.png)

Création du fichier db.192.168.100 et sa modification :

![image](https://user-images.githubusercontent.com/80455771/193469364-68b3420b-cd5a-4d20-93df-dae1780c2624.png)

### 4. Utilisez les utilitaires named-checkconf et named-checkzone pour valider vos fichiers de configuration :

On peut voir que tous les fichiers sont bons : 

![image](https://user-images.githubusercontent.com/80455771/193469766-25111770-6bee-473d-a6c0-bfa11a7edf60.png)

![image](https://user-images.githubusercontent.com/80455771/193470397-d1fa133b-6582-43ca-81bb-4229fedd2caf.png)

### 5. Redémarrer le serveur Bind9. Vous devriez maintenant être en mesure de « pinguer »les différentes machines du réseau

On peut ping les différentes machines sur le réseau et dans ce cas un autre serveur sur le réseau :

![image](https://user-images.githubusercontent.com/80455771/193470501-de33326d-7c17-4b9c-9a2c-ff5e2325bfc1.png)

