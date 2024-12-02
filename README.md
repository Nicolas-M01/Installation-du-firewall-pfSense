# Installation-du-firewall-pfSense

Version installée : V2.6.0. Paramétrage dans VirtualBox. Télécharger l'ISO, créer nouvelle VM : `Type : BSD`, 
Dans VirtualBox, attribuer 2 cartes réseaux :
* Carte 1 : Pour le WAN accès par pont ( elle nous permettra de sortir sur internet )
* Carte 2 : Pour le LAN, en réseau interne. Elle permettra à une machine isolée d'internet de passer par cette passerelle pour aller sur le net avec la config du pare feu.
