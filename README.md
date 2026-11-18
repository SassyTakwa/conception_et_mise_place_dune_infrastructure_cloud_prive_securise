Conception et mise en place d'une infrastructure Cloud privé sécurisé

Projet de fin d'études (licence en ingénierie des systèmes informatiques, ISSAT Mateur – Université de Carthage), réalisé lors d'un stage technique chez Standard Sharing Software (3S), Tunis (06/2026 – 07/2026).

L'objectif : concevoir et déployer, à partir d'outils 100 % open source, un cloud privé sécurisé qui regroupe virtualisation, segmentation réseau, pare-feu, accès distant VPN et supervision.

Mots-clés : Proxmox VE · pfSense · VLAN · DHCP · OpenVPN · Zabbix · SNMP · Squid · Ubuntu Server · Windows · virtualisation imbriquée · cloud privé

Architecture

L'infrastructure fonctionne en virtualisation imbriquée : VMware Workstation héberge un serveur Proxmox VE, qui héberge à son tour toutes les machines virtuelles du projet. Le pare-feu pfSense assure le routage entre les VLAN, le DHCP, le filtrage et l'accès VPN.

                        Poste hôte (VMware Workstation)
                                      │
                               Proxmox VE (vmbr0, VLAN-aware)
                                      │
                    ┌─────────────────┴─────────────────┐
                    │         pfSense (pare-feu / routeur)
                    │   DHCP · règles inter-VLAN · OpenVPN
                    └───┬─────────┬─────────┬─────────┬──┘
                        │         │         │         │
                    VLAN 10   VLAN 20   VLAN 30   VLAN 40
                      RH        IT      Finance   Serveurs
                       │         │         │      ┌──┴──────────────┐
                    PC-RH     PC-IT   PC-Finance   Ubuntu Server    Zabbix
                                                   192.168.40.10    192.168.40.20
