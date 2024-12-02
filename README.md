# Installation-du-firewall-pfSense

Version installée : V2.6.0. Paramétrage dans VirtualBox. Télécharger l'ISO, créer nouvelle VM : `Type : BSD`, `Version : FreeBSD (64bits)`
Dans VirtualBox, attribuer 2 cartes réseaux :
* Carte 1 : Pour le WAN accès par pont ( elle nous permettra de sortir sur internet )
* Carte 2 : Pour le LAN, en réseau interne. Elle permettra à une machine isolée d'internet de passer par cette passerelle pour aller sur le net avec la config du pare feu.

Lancer l'installation en fraçais, etc...
Au redémarrage, il faut vérifier la config des cartes réseau. Il faut modifier celle du LAN pour la changer de réseau. (Initialement en 192.168.1.1/24, il faut lui donner par exemple 192.168.2.1/24.
Pour la passerelle, laisser vide, car on utilisera l'interface WAN pour accéder à Internet. Laissez vide également pour la partie IPv6
