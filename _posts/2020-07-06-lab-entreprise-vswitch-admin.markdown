---
layout: post
title:  "Lab Active Directory - Part 2 - vSwitch ESXi (SW-ADM)"
date:   2020-07-06 21:44:14 +0200
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

Dans cette première partie, nous allons créer le switch dédié aux administrateurs de l'entreprise (SW-ADM). Il servira à complètement isoler ce réseau critique du réseau des utilisateurs classiques.

Le réseau d'administration sera également découpé en **deux VLAN**: 
- Le **premier** servira à l'**administration des serveurs et routeurs** de l'entreprise.
- Le **second** servira pour l'**administration des postes clients** de l'entreprise. 

L'idéal serait d'également créer un VLAN dédié à l'administration des équipements réseaux de l'entreprise. Mais dans le cadre de ce lab, nous nous contenterons de deux VLAN d'administration distincts.

L'objectif de ce découpage est de limiter le nombre de privilèges que peut récupérer un attaquant si il arrive à compromettre la machine d'un administrateur. Si le poste de l'administrateur des postes de travail est compromis, l'attaquant ne pourra pas s'en servir pour rebondir sur les serveurs ou les équipements réseaux.

En ce qui concerne les règles de filtrage, elles ressembleront à quelque chose comme:

- **Admin postes utilisateurs**: Possède l'autorisation de se connecter aux postes clients via RDP pour de l'administration/debug.
- **Admin serveurs**: Possède l'autorisation d'accéder aux ports RDP, SSH, HTTPS, etc des serveurs pour l'administration/debug.
- **Admin réseaux**: Possède l'autorisation d'accéder aux ports SSH et HTTPS des switchs et routeurs pour l'administration/debug.

## Création d'un nouveau vSwitch

Sur l'interface d'administration de ESXi, cliquer sur **mise en réseau** puis **Commutateurs virtuels** et enfin **Ajouter un commutateur virtuel standard**:

![creer-vswitch]({{ site.url }}/assets/lab-entreprise-vswitch-admin/mise-en-reseay-1.png){:style="border: 1px solid #1756a9;"}


Je laisse les options par défaut, et je nomme ce vSwitch **SW-ADM**:

![creer-vswitch2]({{ site.url }}/assets/lab-entreprise-vswitch-admin/ajouter-new-vswitch-adm.png){:style="border: 1px solid #1756a9;"}

On va maintenant pouvoir créer deux groupes de ports sur ce vSwitch afin d'y placer nos deux VLAN dédiés à l'administration.
- Le **VLAN 10** servira à l'**administration des serveurs et du réseau**.
- Le **VLAN 20** servira à l'**administration des postes clients**.

On retourne sur **Mise en réseau**, puis **Groupes de ports** et enfin **Ajouter un groupe de ports**:

![creer-groupe-ports1]({{ site.url }}/assets/lab-entreprise-vswitch-admin/creation-groupe-de-ports-1.png){:style="border: 1px solid #1756a9;"}

On configure le groupe de ports afin qu'il prenne place sur le vSwitch **SW-ADM**, on lui donne un nom (Par exemple: **VLAN-ADM-SRV-AND-NETWORK** afin de rapidement identifier son rôle) et on renseigne le numéro de **VLAN 10**:

![creer-groupe-ports2]({{ site.url }}/assets/lab-entreprise-vswitch-admin/creation-groupe-de-ports-2.png){:style="border: 1px solid #1756a9;"}

On fait la même chose pour le VLAN d'administration dédié aux postes de clients:

![creer-groupe-ports3]({{ site.url }}/assets/lab-entreprise-vswitch-admin/creation-groupe-de-ports-3.png){:style="border: 1px solid #1756a9;"}

Si l'on retourne sur **Mise en réseau**, puis **SW-ADM**, on doit avoir cette configuration réseau:

![creer-groupe-ports4]({{ site.url }}/assets/lab-entreprise-vswitch-admin/creation-groupe-de-ports-4.png){:style="border: 1px solid #1756a9;"}

C'est tout pour le moment, nous allons maintenant pouvoir installer une machine Windows 10 qui servira de machine d'administration pour configurer PFSense.