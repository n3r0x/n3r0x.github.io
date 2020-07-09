---
layout: post
title:  "Lab Active Directory - Part 6 - Active Directory"
date:   2020-07-09 01:03:14 +0200
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

Dans ce chapitre, nous allons configurer le **contrôleur de domaine** du lab. C'est donc cette machine qui embarquera **Active Directory**.

Pour commencer, nous allons sur **Machines virtuelles**, puis **Créer/Enregistrer une machine virtuelle**:

![creer-dc1]({{ site.url }}/assets/lab-entreprise-active-directory/creer-dc-1.png){:style="border: 1px solid #1756a9;"}

On prépare une machine pour **Microsoft Windows Server 2016 ou version ultérieure (64 bits)** qu'on va nommer **Win2019-DC-01**:

![creer-dc2]({{ site.url }}/assets/lab-entreprise-active-directory/creer-dc-2.png){:style="border: 1px solid #1756a9;"}

Pour le stockage, on laisse les options de base:

![creer-dc3]({{ site.url }}/assets/lab-entreprise-active-directory/creer-dc-3.png){:style="border: 1px solid #1756a9;"}

On configure le hardware de notre machine virtuelle:

- **CPU**: 2
- **Disque**: 150Go
- **RAM**: 4096Mo
- **Adaptateur réseau**: SW-SRV
- **Lecteur de CD/DVD 1**: Notre ISO Windows Server 2019 64 bits

![creer-dc4]({{ site.url }}/assets/lab-entreprise-active-directory/creer-dc-4.png){:style="border: 1px solid #1756a9;"}

Pour éviter le problème au démarrage sur EFI, il faut aller dans **Options VM**, puis dans **Options de démarrage** et sélectionner **Microprogramme: BIOS** à la place de **EFI**:

![creer-dc5]({{ site.url }}/assets/lab-entreprise-active-directory/creer-dc-5.png){:style="border: 1px solid #1756a9;"}

On peut maintenant installer Windows Server 2019 de façon classique.

## Configuration de Windows Server 2019

### Nom de la machine

Pour commencer, nous allons changer le nom du serveur, puis lui configurer une adresse IP fixe:

Ouvrez **Paramètres Windows**, puis **Système**, **Informations système**:

![creer-dc-rename]({{ site.url }}/assets/lab-entreprise-active-directory/conf-dc-rename.png){:style="border: 1px solid #1756a9;"}

Cliquez sur **Renommer ce PC** et entrez **DC01**:

![creer-dc-rename2]({{ site.url }}/assets/lab-entreprise-active-directory/conf-dc-rename2.png){:style="border: 1px solid #1756a9;"}

Cliquez sur **Suivant**, puis **Redémarrer plus tard**.

Nous allons maintenant configurer une IP fixe pour le contrôleur de domaine. 

### Configuration réseau

On se rend sur **Paramètres Ethernet** afin de configurer notre interface:

![conf-ip1]({{ site.url }}/assets/lab-entreprise-active-directory/conf-ip1.png){:style="border: 1px solid #1756a9;"}

Notre contrôleur de domaine possedera l'IP **10.0.2.2**. Sa **gateway** et son **dns** seront tous les deux **10.0.2.1**:

![conf-ip2]({{ site.url }}/assets/lab-entreprise-active-directory/conf-ip2.png){:style="border: 1px solid #1756a9;"}

On redémarre pour prendre en compte le changement de nom de machine.

Inutile d'essayer de ping le contrôleur de domaine depuis le poste Windows 10. Le firewall du serveur bloque l'ICMP. Vous pouvez désactiver le firewall sur la zone **privée** pour avoir une réponse. Ca peut-être utile pour du debug.

### Configuration PFSense

Nous allons devoir configurer le firewall de PFSense afin qu'il laisse passer le trafic venant de l'interface **SRV**.
Cette configuration dangereuse est temporaire et ne servira qu'à l'installation initiale du lab.
Une fois terminé, le contrôle d'accès empêchera les serveurs de communiquer directement avec Internet pour plus de sécurité.

Nous nous rendons depuis le poste Windows 10 en zone d'administration sur l'interface web de PFSense.

On clique sur **Firewall**, puis **SRV**

![regle-pfsense]({{ site.url }}/assets/lab-entreprise-active-directory/regle-pfsense.png){:style="border: 1px solid #1756a9;"}

On peut maintenant ajouter la règle suivante:

![autoriser-net-srv]({{ site.url }}/assets/lab-entreprise-active-directory/autoriser-net-srv.png){:style="border: 1px solid #1756a9;"}

Il ne reste plus qu'à tester que le flux est ouvert côté serveur:

![ping-srv]({{ site.url }}/assets/lab-entreprise-active-directory/ping-srv.png){:style="border: 1px solid #1756a9;"}


### Installation du rôle AD DS

> Todo...