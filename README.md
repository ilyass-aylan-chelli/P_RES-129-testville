# P_RES-129 – Conception et simulation d'un réseau local communal

Projet réalisé à l'**ETML** (École Technique des Métiers de Lausanne) dans le cadre du module **I129**.
Réseau LAN de la commune fictive de **Testville** (groupe 1), conçu et simulé sous Cisco Packet Tracer.

## Auteurs

- Ilyass Chelli
- Quentin Dubard


## Contexte

La commune de Testville modernise son réseau : environ 80 postes fixes et 30 utilisateurs
mobiles répartis sur 3 bâtiments, avec un Wi-Fi invité séparé.

| Bâtiment | Distance | Particularité |
|---|---|---|
| Hôtel de ville (principal) | – | 3 niveaux + sous-sol, serveurs internes |
| Bibliothèque municipale | 150 m | Postes publics, 2 bornes Wi-Fi |
| Atelier des services techniques | 80 m | 8 postes fixes |

## Contraintes du cahier des charges

- Un routeur par bâtiment, liaisons dédiées entre les bâtiments
- **Routage statique uniquement** (RIP, OSPF et EIGRP interdits)
- Routage inter-VLAN totalement ouvert (aucune ACL)
- Configuration 100 % en CLI dans Packet Tracer

## Solution

### VLANs

| VLAN | Nom | Réseau | Passerelle |
|---|---|---|---|
| 10 | serveur | 192.168.10.0/29 | 192.168.10.1 |
| 20 | bibliothèque | 192.168.20.0/28 | 192.168.20.1 |
| 30 | finance | 192.168.30.0/26 | 192.168.30.1 |
| 40 | accueil | 192.168.40.0/26 | 192.168.40.1 |
| 50 | direction | 192.168.50.0/26 | 192.168.50.1 |
| 60 | servicetech | 192.168.60.0/27 | 192.168.60.1 |
| 70 | guest | 192.168.70.0/27 | 192.168.70.1 |

### Liaisons entre routeurs

| Liaison | Réseau |
|---|---|
| Routeur 1 ↔ Routeur 2 | 10.0.0.0/30 |
| Routeur 2 ↔ Routeur 3 | 10.0.0.4/30 |

### Topologie

- **Routeur 1** : bibliothèque (VLAN 20 et 70)
- **Routeur 2** : Hôtel de ville (VLAN 10, 30, 40, 50), routeur central
- **Routeur 3** : atelier des services techniques (VLAN 60)
- Un serveur DHCP dans le VLAN 10 (`192.168.10.3`), atteint via `ip helper-address`

## Contenu du dépôt

| Dossier | Contenu |
|---|---|
| `cdc/` | Cahiers des charges général et du groupe 1 |
| `shéma réeseau/` | Schéma réseau (draw.io) |
| `plan_adressage/` | Plan d'adressage IP (Excel) |
| `commande-configuration/` | Simulation Packet Tracer (.pkt) et commandes CLI de chaque équipement |
| `journal de travaille-ilyass-chelli/` | Journal de travail |
| `présenation/` | Présentation pour l'oral (PDF), (pptx) aidée par l'ia pour faire une belle présenation (je précise que l'ia n'a pas été utiliser pour le projet sauf pour la présentation) |
| `version-acl/` | Version avec ACL : schéma, simulation Packet Tracer et configurations |
## Utilisation

1. Ouvrir `packet-tracer/SANS_ACL.pkt` avec Cisco Packet Tracer.
2. Tester les `ping` entre VLANs, entre bâtiments et vers les serveurs.
3. Les configurations complètes sont dans `configs/`.

## Pour aller plus loin

Version avec ACL pour restreindre certains accès entre VLANs (facultatif) : à venir.

## Licence

Projet scolaire – usage pédagogique.
