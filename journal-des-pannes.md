# Journal des pannes

Heures relevées dans Zabbix, en **UTC** (Cameroun = UTC+1).
Équipement supervisé : `ALPINE-CLIENT` (`192.168.20.97`).

| N° | Début (UTC) | Fin (UTC) | Origine |
|---|---|---|---|
| 1 | 10:17:31 | 10:30:31 (durée 13 min) | Provoquée : `eth0` d'Alpine désactivée |
| 2 | 10:43:31 | Retour à `Up (1)` constaté après la correction | Provoquée : `eth0` d'Alpine désactivée, puis adresse différente de celle supervisée |

## Panne 1

| Rubrique | Détail |
|---|---|
| Symptôme dans Zabbix | Problème « ICMP Ping: Unavailable by ICMP ping », gravité High, hôte `ALPINE-CLIENT` |
| Vérification | `ip a` sur Alpine : `eth0` à l'état **DOWN**, aucune adresse `inet` |
| Cause | Interface réseau désactivée volontairement |
| Correction | `ip link set eth0 up`, puis `udhcpc -i eth0` |
| Contrôle | `udhcpc` : bail de `192.168.20.97` obtenu depuis `192.168.20.2`, durée 1800 s |
| Résultat | Alerte **RESOLVED** à 10:30:31 UTC |

## Panne 2

| Rubrique | Détail |
|---|---|
| Symptôme dans Zabbix | Même alerte, début 10:43:31 UTC |
| Vérification 1 | `ping 192.168.20.97` depuis Zabbix : 10 émis, 0 reçu, 100 % de perte, `Destination Host Unreachable` (réponse du serveur Zabbix lui-même, `192.168.20.100`) |
| Vérification 2 | `ip a` sur Alpine : `eth0` active avec `inet 192.168.20.99/24`, donc **une autre adresse que `.97`** |
| Cause | Bail DHCP dynamique : le routeur a attribué `.99` ; Zabbix testait toujours `.97` |
| Correction | Routeur : réservation de `192.168.20.97` pour l'adresse MAC d'Alpine (commentaire « Alpine - supervision »). Alpine : `ip addr flush dev eth0`, puis `udhcpc -i eth0` |
| Contrôle | `udhcpc` : `lease of 192.168.20.97 obtained from 192.168.20.2`, durée 1800 s |
| Résultat | `ICMP ping` = **Up (1)**, `ICMP loss` = 0 %, `ICMP response time` = 2,15 ms (Latest data) |

## Lecture du message « Destination Host Unreachable »

Le message vient du serveur Zabbix : il n'a trouvé aucune machine répondant à `192.168.20.97` sur son réseau local. Dans la panne 2, la machine était bien présente sur le réseau, mais avec une autre adresse.
