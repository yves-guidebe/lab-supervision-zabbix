# Commandes utilisées

## Alpine Linux (équipement supervisé)

| Commande | Rôle |
|---|---|
| `ip a` | Affiche les interfaces et leurs adresses |
| `ip link set eth0 down` | Désactive l'interface (provoque la panne) |
| `ip link set eth0 up` | Réactive l'interface |
| `ip addr flush dev eth0` | Supprime les adresses de l'interface |
| `udhcpc -i eth0` | Demande une adresse au serveur DHCP |
| `df -h /` | Affiche le système de fichiers racine (`tmpfs` = mode live) |

## Serveur Zabbix (console)

| Commande | Rôle |
|---|---|
| `ping -c 3 192.168.20.97` | Teste la joignabilité d'Alpine (`-c 3` : trois paquets) |
| `ip a` | Vérifie l'adresse du serveur |

## Routeur MikroTik

| Commande | Rôle |
|---|---|
| `/ip dhcp-server lease print` | Liste les baux DHCP (adresses attribuées) |
| `/ip dhcp-server lease remove [find mac-address=<MAC>]` | Supprime les anciens baux d'une machine |
| `/ip dhcp-server lease add address=192.168.20.97 mac-address=<MAC> server=dhcp-lan comment="Alpine - supervision"` | Réserve une adresse à une machine |

## Interface web Zabbix

| Menu | Usage |
|---|---|
| Data collection > Hosts | Créer et modifier les hôtes |
| Monitoring > Latest data | Voir ICMP ping, perte de paquets, temps de réponse |
| Monitoring > Problems | Voir les alertes et leur résolution |
