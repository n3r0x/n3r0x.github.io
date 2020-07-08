---
layout: post
title:  "Lab Active Directory - Part 5 - vSwitch ESXi (SW-SRV)"
date:   2020-07-08 22:44:14 +0200
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

Nous allons créer le switch dédié aux serveurs de l'entreprise (**SW-SRV**). 

On va rester simple pour le moment, et se contenter d'un réseau à plat sur cette zone (Pas de VLAN)

## Création d'un nouveau vSwitch

Sur l'interface d'administration de ESXi, cliquer sur **mise en réseau** puis **Commutateurs virtuels** et enfin **Ajouter un commutateur virtuel standard**:

![creer-vswitch]({{ site.url }}/assets/lab-entreprise-vswitch-server/mise-en-reseay-1.png){:style="border: 1px solid #1756a9;"}


Je laisse les options par défaut, et je nomme ce vSwitch **SW-SRV**:

![creer-vswitch2]({{ site.url }}/assets/lab-entreprise-vswitch-server/ajouter-new-vswitch-srv.png){:style="border: 1px solid #1756a9;"}

On retourne sur **Mise en réseau**, puis **Groupes de ports** et enfin **Ajouter un groupe de ports**:

![creer-groupe-ports1]({{ site.url }}/assets/lab-entreprise-vswitch-server/creation-groupe-de-ports-1.png){:style="border: 1px solid #1756a9;"}

On configure le groupe de ports afin qu'il prenne place sur le vSwitch **SW-SRV**, on lui donne un nom (Par exemple: **SW-SRV** afin de rapidement identifier son rôle) et on laisse le numéro de **VLAN 0** par défaut:

![creer-groupe-ports2]({{ site.url }}/assets/lab-entreprise-vswitch-server/creation-groupe-de-ports-2.png){:style="border: 1px solid #1756a9;"}

Si l'on retourne sur **Mise en réseau**, puis **SW-SRV**, on doit avoir cette configuration réseau:

![creer-groupe-ports4]({{ site.url }}/assets/lab-entreprise-vswitch-server/creation-groupe-de-ports-4.png){:style="border: 1px solid #1756a9;"}

## Configuration de PFSense

Depuis ESXi, on coupe PFSense puis nous ajoutons une carte réseau supplémentaire pour y connecter le switch **SW-SRV**.

![ajouter-carte-reseau]({{ site.url }}/assets/lab-entreprise-vswitch-server/ajouter-carte-reseau.png){:style="border: 1px solid #1756a9;"}

On peut ensuite redémarrer le routeur et utiliser le Windows 10 présent dans le LAN d'administration, pour se connecter sur l'interface web du routeur et configurer cette nouvelle interface:

![interface-assignment]({{ site.url }}/assets/lab-entreprise-vswitch-server/interface-assignment.png){:style="border: 1px solid #1756a9;"}

On clic sur **OPT1** afin de modifier la configuration de cette nouvelle interface:

- **Enable**: Clic
- **IPv4 Configuration Type**: Static IPv4
- **IPv4 Address**: 10.0.2.1

![config-opt1]({{ site.url }}/assets/lab-entreprise-vswitch-server/config-opt1.png){:style="border: 1px solid #1756a9;"}

On valide, puis on recharge la configuration en cliquant sur **Apply Changes**.

On va également en profiter pour renommer l'interface **LAN** en **ADM** pour coller avec la réalité.
Pour ça, on clique sur **Interfaces**, puis **LAN** et on change la valeur du champ **Description**:

![change-nom-lan]({{ site.url }}/assets/lab-entreprise-vswitch-server/change-nom-lan.png){:style="border: 1px solid #1756a9;"}

On valide, on recharge, et on se retrouve avec les interfaces suivantes:

![liste-interfaces]({{ site.url }}/assets/lab-entreprise-vswitch-server/liste-interfaces.png){:style="border: 1px solid #1756a9;"}

Nous allons maintenant pouvoir installer le contrôleur de domaine du laboratoire.