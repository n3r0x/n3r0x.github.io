---
layout: post
title:  "Lab Active Directory - Part 4 - Client Windows 10 (SW-ADM)"
date:   2020-07-06 21:54:14 +0200
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

Dans ce chapitre, nous allons configurer une machine Windows 10 qui appartiendra à un administrateur de l'entreprise, et à partir de laquelle nous allons pouvoir configurer les réseaux et serveurs du lab.

Pour commencer, nous allons sur **Machines virtuelles**, puis **Créer/Enregistrer une machine virtuelle**:

![creer-win10-adm1]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-1.png){:style="border: 1px solid #1756a9;"}

On prépare une machine pour **Microsoft Windows 10 (64 bits)** qu'on va nommer **Win10-admin-network-system**:

![creer-win10-adm2]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-2.png){:style="border: 1px solid #1756a9;"}

Pour le stockage, on laisse les options de base:

![creer-win10-adm3]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-3.png){:style="border: 1px solid #1756a9;"}

On configure le hardware de notre machine virtuelle:

- **CPU**: 2
- **Disque**: 32Go
- **RAM**: 2048Mo
- **Adaptateur réseau**: VLAN-ADM-SRV-AND-NETWORK
- **Lecteur de CD/DVD 1**: Notre ISO Windows 10

![creer-win10-adm4]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-4.png){:style="border: 1px solid #1756a9;"}

Afin d'éviter un bug lors du boot de la VM, on va dans **Options VM** et dans **Options de démarrage** / **Microprogramme** on sélectionne **BIOS**:

![creer-win10-adm5]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-5.png){:style="border: 1px solid #1756a9;"}

On va également créer un compte **admin_local** sur le poste:

![creer-win10-adm6]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-6.png){:style="border: 1px solid #1756a9;"}

Une fois terminé, on peut configurer le réseau avec les paramètres suivants:

- **IP**: 10.0.1.2
- **Masque**: 24
- **Gateway**: 10.0.1.1
- **DNS**: 10.0.1.1

![creer-win10-adm7]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-7.png){:style="border: 1px solid #1756a9;"}

Une fois configuré, on peut accéder à l'interface d'administration de **PFSense** via [https://10.0.1.1](https://10.0.1.1) avec le login **admin** et le mot de passe **pfsense**:

![creer-win10-adm8]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-8.png){:style="border: 1px solid #1756a9;"}

On laisse l'interface **WAN** en **DHCP**:

![creer-win10-adm9]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-9.png){:style="border: 1px solid #1756a9;"}

Le reste de la configuration est classique. On change le mot de passe, et on reload la configuration du routeur.

Une fois terminé, on peut vérifier qu'on a bien accès à Internet depuis la machine Windows 10 de l'administrateur:

![creer-win10-adm10]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-10.png){:style="border: 1px solid #1756a9;"}

Au niveau de notre vSwitch **SW-ADM** nous avons désormais cette configuration:

![creer-win10-adm11]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-11.png){:style="border: 1px solid #1756a9;"}

Selon le schéma disponible dans [Introduction]({% post_url 2020-07-06-lab-entreprise-intoduction %}), voici où on en est actuellement:

![creer-win10-adm12]({{ site.url }}/assets/lab-entreprise-win10-admin/create-win-10-admin-12.png){:style="border: 1px solid #1756a9;"}

On va maintenant pouvoir s'attaquer au **contrôleur de domaine**, mais avant ça, configurons le vSwitch **SW-SRV** qui connectera nos serveurs à notre routeur.
