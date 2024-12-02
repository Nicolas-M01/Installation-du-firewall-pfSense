# Installation-du-firewall-pfSense

Version installée : V2.6.0. Paramétrage dans VirtualBox. Télécharger l'ISO, créer nouvelle VM : `Type : BSD`, `Version : FreeBSD (64bits)`
Dans VirtualBox, attribuer 2 cartes réseaux :
* Carte 1 : Pour le WAN accès par pont ( elle nous permettra de sortir sur internet )
* Carte 2 : Pour le LAN, en réseau interne. Elle permettra à une machine isolée d'internet de passer par cette passerelle pour aller sur le net avec la config du pare feu.

Lancer l'installation en fraçais, etc...  

## Configuration carte réseau LAN de la machine Pfsense  

Au re démarrage, il faut vérifier la config des cartes réseau. Il faut modifier celle du LAN pour la changer de réseau. (Initialement en 192.168.1.1/24, il faut lui donner par exemple 192.168.2.1/24.
Pour la passerelle, laisser vide, car on utilisera l'interface WAN pour accéder à Internet. Laissez vide également pour la partie IPv6
Ne pas accepter le DHCP sur cette carte et ne pas accepter le HTPP (pour rester en HTTPS).

## Configuration carte réseau LAN de la machine cliente (Win10)  
Créer une VM Win10 :
* 1 carte réseau : Réseau interne, le même réseau interne que la machine FireWall.
Il faut attribuer une adresse statique sur le réseau : 192.168.2.0/24 comme la carte du LAN du Pfsense. Par exemple 192.168.2.2/24 c'est parfait. Et lui ajouter la passerelle de la carte réseau LAN de Pfsense : 192.168.2.1.
Le DNS pour accéder à internet : 8.8.8.8  
-> Faire un ping vers l'interface LAN de Pfsense.  
Si c'est ok, ouvrir page internet  

## Configuration initiale via l'assistant Pfsense (par interface Web)
Barre d'URL : 192.168.2.1  
Par défaut :  
`Username : admin`  
`PassWord : pfsense`  

Paramétrer la suite, Hôte, Domaine, DNS (externes), puis Heure...  
Décocher ces cases sinon, tout le trafic entrant sur l'interface WAN en provenance de réseaux privés (comme le réseau 192.168.1.0/24) sera bloqué.  
![image](https://github.com/user-attachments/assets/e13fcb0d-b779-49b6-b340-cf7af03e7aff)

Ensuite au step 5/9 (LAN interface), nous avons déjà configuré l'IP statique sur le client donc pas la peine, laisser 192.168.2.1/24.
Ensuite, changer le password.

Le pare feu est prêt à être configuré.

La VM connectée au réseau interne "LAN_VM" et que nous utilisons pour administrer le pare-feu Pfsense dispose d'un accès à Internet. Cela s'explique par le fait que le NAT (via la méthode du PAT) est configuré par défaut sur le pare-feu Pfsense. Pour le vérifier, accédez au menu `Firewall`>`NAT`>`Outbound`, le mode est paramétré sur automatic. Il faudra le mettre en "Manuel" pour modifier les règles du parefeu.

Tester la machine cliente, en ouvrant une page internet et aller sur un site. ça fonctionne.

## Bloquer la machine cliente pour qu'elle ne sorte pas du réseau LAN
Il faut éditer dans Pfsense en allant dans : `Firewal`>`Rules`>`LAN`  

![Capture d'écran 2024-12-02 165954](https://github.com/user-attachments/assets/3151a8c4-7a1d-4536-8a17-ba5069b189f1)  
* La première règle nommée "Anti-lockout Rule" sert à autoriser explicitement l'accès à l'interface de gestion du Pfsense, afin d'éviter de perdre la main si une règle trop restrictive est créée.  
* La deuxième règle sert à autoriser tous les flux du LAN vers le WAN, en IPv4
* La troisième règle sert à autoriser tous les flux du LAN vers le WAN, en IPv6

Dans ce menu, sur la 2ème ligne (lignes ds IPv4), cliquer sur le "crayon" à droite dans la colonne "Actions". Pour bloquer les machines du LAN en IPv4, mettre sur "Block", sauvegarder, recherger l'interface.
Ouvrir une page internet et tester, on voit que l'on obtient un message d'erreur :  
![Capture d'écran 2024-12-02 170909](https://github.com/user-attachments/assets/29ab79d3-986d-480c-be04-0737d1c4cb18)  

Puis quelques secondes après :  
![Capture d'écran 2024-12-02 170948](https://github.com/user-attachments/assets/deeb0ba3-a241-4ae3-91ec-2e7be002f4ee)


### Notre Pare Feu PfSense fonctionne !


