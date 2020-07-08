---
layout: post
title:  "Lab Active Directory - Part 3 - PFSense"
date:   2020-07-06 21:52:14 +0200
categories: Microsoft Active Directory
---


## Sommaire

0. [Introduction]({% post_url 2020-07-06-lab-entreprise-intoduction %})
1. [vSwitch ESXi (SW-ADM)]({% post_url 2020-07-06-lab-entreprise-vswitch-admin %})
2. [PFSense]({% post_url 2020-07-06-lab-entreprise-config-pfsense %})
3. [Client Windows 10 (SW-ADM)]({% post_url 2020-07-06-lab-entreprise-win10-admin %})
4. [vSwitch ESXi (SW-SRV)]({% post_url 2020-07-06-lab-entreprise-vswitch-server %})
5. [Active Directory]({% post_url 2020-07-06-lab-entreprise-active-directory %})
6. vSwitch ESXi (SW-CLI)
7. Client Windows 10 (SW-CLI)
8. WSUS
9. Serveur de fichiers
10. Microsoft ATA
11. PKI d'entreprise
12. Network Policy Server
13. Exchange
14. Serveur de logs

## Introduction

Dans ce chapitre, nous allons configurer **PFSense**. Celui-ci servira de routeur principal pour l'ensemble du réseau. C'est sur celui-ci que viendront se connecter les différents sous-réseaux (clients, admins, serveurs).
Nous allons dans un premier temps configurer deux pattes sur celui-ci: 
- **WAN** (Qui sera reliée à notre interface physique afin de fournir Internet aux VMs)
- **LAN_ADM** (Qui sera notre sous-réseau dédié aux administrateurs, sur le vSwitch du chapitre précédent).

Nous configurerons les pattes suivantes (**LAN_CLI** et **LAN_SRV**) quand elles seront nécessaires.

Seule la patte **LAN_ADM** pourra se connecter sur l'interface d'administration du routeur.

## Création de la machine virtuelle PFSense

Conectez-vous en tant qu'administrateur ESXi, puis cliquez sur **Machines virtuelles**, puis **Créer/Enregistrer une machine virtuelle**:

![creation-pfsense-1]({{ site.url }}/assets/lab-entreprise-pfsense-part3/create-vm-pfsense-1.png){:style="border: 1px solid #1756a9;"}

Donnez lui un nom, et choississez **FreeBSD 12 ou version ultérieures (64 bits)**:

![creation-pfsense-2]({{ site.url }}/assets/lab-entreprise-pfsense-part3/create-vm-pfsense-2.png){:style="border: 1px solid #1756a9;"}

Vous pouvez configurer une machine virtuelle avec peu de ressources. Sélectionnez l'ISO d'installation de PFSense, cliquez ensuite sur **Ajouter un adapteur réseau** puis configurez les deux adaptateurs de cette façon:
![creation-pfsense-3]({{ site.url }}/assets/lab-entreprise-pfsense-part3/create-vm-pfsense-3.png){:style="border: 1px solid #1756a9;"}

Vous pouvez maintenenant terminer la configuration de cette nouvelle machine, et la lancer afin d'installer PFSense.

Une fois prêt, nous pouvons configurer les interfaces disponibles sur le routeur:

- **Should VLANs be set up now**: n
- **Enter the WAN interface name**: vmx0
- **Enter the LAN interface name**: vmx1
- **Do you want to proceed ?**: y

On entre **2** afin d'assigner une IP sur l'interface **vmx1** (Le LAN d'administration):

![creation-pfsense-4]({{ site.url }}/assets/lab-entreprise-pfsense-part3/create-vm-pfsense-4.png){:style="border: 1px solid #1756a9;"}

On sélectionne notre interface **vmx1**:

![creation-pfsense-5]({{ site.url }}/assets/lab-entreprise-pfsense-part3/create-vm-pfsense-5.png){:style="border: 1px solid #1756a9;"}


- L'IP de cette interface: **10.0.1.1**
- Masque: **24**
- Activer le DHCP ? **n**
- Revenir au protocole HTTP ? **n**

![creation-pfsense-6]({{ site.url }}/assets/lab-entreprise-pfsense-part3/create-vm-pfsense-6.png){:style="border: 1px solid #1756a9;"}

Notre configuration est prête, nous allons maintenant installer le poste d'un administrateur afin de prendre la main sur l'interface web d'administration du PFSense.