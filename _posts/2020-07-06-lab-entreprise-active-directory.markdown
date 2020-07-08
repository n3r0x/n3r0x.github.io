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