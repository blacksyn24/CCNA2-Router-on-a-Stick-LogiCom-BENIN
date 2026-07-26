# 🌐 CCNA2 — Router-on-a-Stick | LogiCom Bénin

![Cisco](https://img.shields.io/badge/Cisco-CCNA2-blue?style=for-the-badge&logo=cisco&logoColor=white)
![PacketTracer](https://img.shields.io/badge/Packet%20Tracer-8.x-orange?style=for-the-badge&logo=cisco)
![Status](https://img.shields.io/badge/Status-✅%20Completed-brightgreen?style=for-the-badge)
![Protocol](https://img.shields.io/badge/Protocol-Router%20on%20a%20Stick-purple?style=for-the-badge)
![VLANs](https://img.shields.io/badge/VLANs-5-red?style=for-the-badge)
![PCs](https://img.shields.io/badge/PCs-15-yellow?style=for-the-badge)
![Switches](https://img.shields.io/badge/Switches-2-green?style=for-the-badge)

---

## 📋 Description

Ce TP simule le réseau de l'entreprise **LogiCom Bénin** 🇧🇯.
L'entreprise possède plusieurs services (RH, Comptabilité, Commercial,
Informatique, Direction) qui doivent être isolés les uns des autres
pour des raisons de sécurité et de performance, tout en pouvant
communiquer entre eux.

L'objectif est de segmenter le réseau en **5 VLANs** répartis sur
**2 switches**, puis de configurer le **routage inter-VLAN** via la
méthode **Router-on-a-Stick** : un seul routeur, un seul câble trunk,
plusieurs sous-interfaces.

### Objectifs
- ✅ Créer et nommer **5 VLANs** sur 2 switches
- ✅ Configurer des **liens trunk** (switch↔switch et switch↔routeur)
- ✅ Configurer des **sous-interfaces** sur le routeur (encapsulation 802.1Q)
- ✅ Permettre à **15 PC** répartis sur 5 VLANs de communiquer entre eux
- ✅ Vérifier le routage inter-VLAN de bout en bout

---

## 🖥️ Équipements

| Équipement | Modèle | Nom | Rôle |
|-----------|--------|-----|------|
| 🌐 Routeur | Cisco 1941 | R1 | Routage inter-VLAN (Router-on-a-Stick) |
| 🔌 Switch | Cisco 2960-24TT | SW1 | Switch principal (relié au routeur) |
| 🔌 Switch | Cisco 2960-24TT | SW2 | Switch secondaire |
| 💻 PC | PC-PT | PC1 → PC15 | Postes des employés (5 services) |

---

## 🏢 VLANs de l'entreprise

| VLAN ID | Nom | Service | Sous-réseau |
|---------|-----|---------|--------------|
| 10 | RH | Ressources Humaines | 192.168.10.0/24 |
| 20 | COMPTABILITE | Comptabilité | 192.168.20.0/24 |
| 30 | COMMERCIAL | Service Commercial | 192.168.30.0/24 |
| 40 | INFORMATIQUE | Service IT | 192.168.40.0/24 |
| 50 | DIRECTION | Direction Générale | 192.168.50.0/24 |

---

## 🗺️ Topologie

```
                         ┌───────────────────┐
                         │        R1          │
                         │  (Router-on-a-Stick)│
                         └─────────┬───────────┘
                                   │ G0/0 (Trunk 802.1Q)
                                   │ .10 .20 .30 .40 .50
                              ┌────┴────┐
                              │  Fa0/1  │
                             [ SW1 ]────Fa0/2────[ SW2 ]
                            (Trunk)              (Trunk)
                          /   |   |   \            /  |  \
                       VLAN VLAN VLAN VLAN       VLAN VLAN VLAN
                        10   20  30   50          10  20   30
                       PC1  PC2 PC4  PC7          PC9 PC11 PC12
                       ...  PC3 PC5  PC8          PC10 ... ...
```

<img width="1919" height="1080" alt="topo" src="https://github.com/user-attachments/assets/6ea14c28-5b22-4235-8dc3-aaabbdf95efb" />


---

## 🔌 Câblage

| De | Port | Vers | Port | Type de lien |
|----|------|------|------|---------------|
| R1 | G0/0 | SW1 | Fa0/1 | Trunk (Router-on-a-Stick) |
| SW1 | Fa0/2 | SW2 | Fa0/1 | Trunk inter-switch |
| SW1 | Fa0/3 → Fa0/10 | PC1 → PC8 | Fa0 | Accès (8 PC) |
| SW2 | Fa0/2 → Fa0/8 | PC9 → PC15 | Fa0 | Accès (7 PC) |

---

## 📊 Plan d'adressage complet

### VLAN 10 — RH

| PC | Switch | Port | IP | Masque | Passerelle |
|----|--------|------|-----|--------|------------|
| PC1 | SW1 | Fa0/3 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC9 | SW2 | Fa0/2 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 |
| PC10 | SW2 | Fa0/3 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 |

### VLAN 20 — Comptabilité

| PC | Switch | Port | IP | Masque | Passerelle |
|----|--------|------|-----|--------|------------|
| PC2 | SW1 | Fa0/4 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC3 | SW1 | Fa0/5 | 192.168.20.11 | 255.255.255.0 | 192.168.20.1 |
| PC11 | SW2 | Fa0/4 | 192.168.20.12 | 255.255.255.0 | 192.168.20.1 |

### VLAN 30 — Commercial

| PC | Switch | Port | IP | Masque | Passerelle |
|----|--------|------|-----|--------|------------|
| PC4 | SW1 | Fa0/6 | 192.168.30.10 | 255.255.255.0 | 192.168.30.1 |
| PC5 | SW1 | Fa0/7 | 192.168.30.11 | 255.255.255.0 | 192.168.30.1 |
| PC12 | SW2 | Fa0/5 | 192.168.30.12 | 255.255.255.0 | 192.168.30.1 |

### VLAN 40 — Informatique

| PC | Switch | Port | IP | Masque | Passerelle |
|----|--------|------|-----|--------|------------|
| PC6 | SW1 | Fa0/8 | 192.168.40.10 | 255.255.255.0 | 192.168.40.1 |
| PC13 | SW2 | Fa0/6 | 192.168.40.11 | 255.255.255.0 | 192.168.40.1 |
| PC14 | SW2 | Fa0/7 | 192.168.40.12 | 255.255.255.0 | 192.168.40.1 |

### VLAN 50 — Direction

| PC | Switch | Port | IP | Masque | Passerelle |
|----|--------|------|-----|--------|------------|
| PC7 | SW1 | Fa0/9 | 192.168.50.10 | 255.255.255.0 | 192.168.50.1 |
| PC8 | SW1 | Fa0/10 | 192.168.50.11 | 255.255.255.0 | 192.168.50.1 |
| PC15 | SW2 | Fa0/8 | 192.168.50.12 | 255.255.255.0 | 192.168.50.1 |

---

## ⚙️ Configuration complète

### 🔧 SW1 — Switch principal

```cisco
enable
configure terminal
hostname SW1

! Création des 5 VLANs
vlan 10
 name RH
vlan 20
 name COMPTABILITE
vlan 30
 name COMMERCIAL
vlan 40
 name INFORMATIQUE
vlan 50
 name DIRECTION
exit

! Port vers le routeur R1 = trunk (Router-on-a-Stick)
interface fa0/1
 switchport mode trunk
exit

! Port vers SW2 = trunk inter-switch
interface fa0/2
 switchport mode trunk
exit

! Ports d'accès - VLAN 10 (RH)
interface fa0/3
 switchport mode access
 switchport access vlan 10
exit

! Ports d'accès - VLAN 20 (Comptabilité)
interface range fa0/4-5
 switchport mode access
 switchport access vlan 20
exit

! Ports d'accès - VLAN 30 (Commercial)
interface range fa0/6-7
 switchport mode access
 switchport access vlan 30
exit

! Port d'accès - VLAN 40 (Informatique)
interface fa0/8
 switchport mode access
 switchport access vlan 40
exit

! Ports d'accès - VLAN 50 (Direction)
interface range fa0/9-10
 switchport mode access
 switchport access vlan 50
exit

end
write
```

---

### 🔧 SW2 — Switch secondaire

```cisco
enable
configure terminal
hostname SW2

! Mêmes VLANs que SW1 (obligatoire pour que le trunk fonctionne)
vlan 10
 name RH
vlan 20
 name COMPTABILITE
vlan 30
 name COMMERCIAL
vlan 40
 name INFORMATIQUE
vlan 50
 name DIRECTION
exit

! Port vers SW1 = trunk
interface fa0/1
 switchport mode trunk
exit

! Port d'accès - VLAN 10 (RH)
interface range fa0/2-3
 switchport mode access
 switchport access vlan 10
exit

! Port d'accès - VLAN 20 (Comptabilité)
interface fa0/4
 switchport mode access
 switchport access vlan 20
exit

! Port d'accès - VLAN 30 (Commercial)
interface fa0/5
 switchport mode access
 switchport access vlan 30
exit

! Ports d'accès - VLAN 40 (Informatique)
interface range fa0/6-7
 switchport mode access
 switchport access vlan 40
exit

! Port d'accès - VLAN 50 (Direction)
interface fa0/8
 switchport mode access
 switchport access vlan 50
exit

end
write
```

---

### 🔧 R1 — Routeur (Router-on-a-Stick)

```cisco
enable
configure terminal
hostname R1

! Activer l'interface physique (jamais d'IP dessus !)
interface g0/0
 no shutdown
exit

! Sous-interface VLAN 10 - RH
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit

! Sous-interface VLAN 20 - Comptabilité
interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
exit

! Sous-interface VLAN 30 - Commercial
interface g0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
exit

! Sous-interface VLAN 40 - Informatique
interface g0/0.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.0
exit

! Sous-interface VLAN 50 - Direction
interface g0/0.50
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
exit

end
write
```

---

## 🔍 Commandes de vérification

```cisco
! Sur les switches
SW1# show vlan brief
SW1# show interfaces trunk
SW1# show interfaces fa0/1 switchport
SW1# show running-config

! Sur le routeur
R1# show ip interface brief
R1# show ip route
R1# show interfaces g0/0.10
R1# show running-config
```

---

### 📊 Résultat attendu — show vlan brief (SW1)

```
VLAN Name                 Status    Ports
---- ---------------------- --------- -------------------------------
1    default                active    Fa0/11, Fa0/12, ...
10   RH                     active    Fa0/3
20   COMPTABILITE           active    Fa0/4, Fa0/5
30   COMMERCIAL             active    Fa0/6, Fa0/7
40   INFORMATIQUE           active    Fa0/8
50   DIRECTION              active    Fa0/9, Fa0/10
```

### 📊 Résultat attendu — show ip route (R1)

```
C  192.168.10.0/24 is directly connected, GigabitEthernet0/0.10
C  192.168.20.0/24 is directly connected, GigabitEthernet0/0.20
C  192.168.30.0/24 is directly connected, GigabitEthernet0/0.30
C  192.168.40.0/24 is directly connected, GigabitEthernet0/0.40
C  192.168.50.0/24 is directly connected, GigabitEthernet0/0.50
```

---

## 🧪 Tests de connectivité

```
✅ PC1  (VLAN10/SW1) → ping 192.168.10.11 (PC9, VLAN10/SW2)   → Même VLAN
✅ PC1  (VLAN10/SW1) → ping 192.168.20.10 (PC2, VLAN20/SW1)   → Inter-VLAN via R1
✅ PC7  (VLAN50/SW1) → ping 192.168.40.11 (PC13, VLAN40/SW2)  → Inter-VLAN + inter-switch
✅ PC15 (VLAN50/SW2) → ping 192.168.10.10 (PC1, VLAN10/SW1)   → Inter-VLAN + inter-switch
✅ Tous les PC       → ping 192.168.X.1 (passerelle)          → Toutes passerelles OK
```

---

## 🛠️ Dépannage

| Problème | Cause | Solution |
|---------|-------|---------|
| PC ne ping pas sa passerelle | IP/masque/passerelle mal configurés sur le PC | Vérifier `ipconfig` du PC |
| PC ne ping pas un autre VLAN | Sous-interface routeur down ou mal encapsulée | `show ip interface brief` sur R1 |
| Trunk ne monte pas | Un des deux ports n'est pas en mode trunk | Vérifier `switchport mode trunk` des 2 côtés |
| VLAN absent sur un switch | VLAN non créé sur ce switch | Recréer le VLAN avec `vlan X` / `name ...` |
| G0/0 down sur R1 | Interface physique non activée | `no shutdown` sur g0/0 (pas besoin sur les sous-interfaces) |

```cisco
! Diagnostic rapide
SW1# show interfaces trunk
R1#  show ip interface brief
R1#  show vlans
```

---

## 💡 Points clés à retenir

| 🔑 Commande | 📖 Rôle |
|-------------|---------|
| `vlan 10` / `name RH` | Créer et nommer un VLAN |
| `switchport access vlan 10` | Affecter un port d'accès à un VLAN |
| `switchport mode trunk` | Faire passer tous les VLANs sur un lien |
| `interface g0/0.10` | Créer une sous-interface sur le routeur |
| `encapsulation dot1Q 10` | Lier la sous-interface au VLAN 10 |
| `ip address 192.168.10.1 255.255.255.0` | Passerelle du VLAN 10 |
| `no shutdown` sur G0/0 | Activer l'interface physique du routeur |

---

## 📊 Comparatif avant/après

| | Avant (réseau à plat) | Après (Router-on-a-Stick) |
|---|---|---|
| **Segmentation** | ❌ Tous les PC dans le même domaine | ✅ 5 VLANs isolés |
| **Sécurité** | ❌ Aucune séparation des services | ✅ Trafic RH/Compta/Direction séparé |
| **Câblage routeur** | 5 câbles nécessaires (1 par VLAN) | ✅ 1 seul câble trunk |
| **Communication inter-services** | Impossible sans routage | ✅ Routage centralisé sur R1 |

---

## 🛠️ Outils

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-8.x-orange?style=flat-square&logo=cisco)
![Cisco IOS](https://img.shields.io/badge/Cisco%20IOS-15.x-blue?style=flat-square)
![GitHub](https://img.shields.io/badge/GitHub-black?style=flat-square&logo=github)

---

## 👨‍💻 Auteur

**Urbain Sedami Landjidé**
🎓 Étudiant en 2ème année — Licence Professionnelle
📡 Réseaux Informatique Mobilité Sécurité (RMS)
🏫 Cisco Networking Academy
📍 Cotonou, Bénin 🇧🇯

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connecter-blue?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/urbain-sedami-landjide-9b49043a8/)

---

## 📄 Licence

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Libre d'utilisation pour l'apprentissage et la formation réseau.

---

## 🏷️ Topics GitHub à ajouter

```
cisco  ccna  ccna2  vlan  inter-vlan-routing
router-on-a-stick  subinterfaces  trunk
packet-tracer  networking  switching  cisco-ios
```
