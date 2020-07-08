---
layout: post
title:  "Lab Active Directory - Part 1 - Introduction"
date:   2020-07-06 21:40:14 +0200
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

> Nous simulerons le SI de la société **RED** qui souhaite déployer un réseau d'entreprise sécurisé basé sur Active Directory, et prévu pour facilement  s'agrandir afin de suivre les évolutions de l'entreprise.

L'objectif de cette série d'articles est d'étudier la mise en place d'un laboratoire basé sur VMWare ESXi, qui simulera un environnement Microsoft Active Directory d'entreprise avec ses clients, serveurs, utilisateurs et équipements réseaux. Il sera dès lors possible de s'en servir pour monter en compétence sur l'administration d'un SI basé sur Active Directory, mais également de travailler sur les différentes méthodes de compromission de ce type d'environnement.

L'installation de VMWare ESXi ne sera pas couverte par cette série d'articles, cependant, la configuration de celui-ci le sera (Configuration des vSwitch par exemple).

Le résultat final **devrait** ressembler à quelque chose comme:

![schema-reseau]({{ site.url }}/assets/lab-entreprise-introduction/schema_reseau.png){:style="border: 1px solid #1756a9;"}

> Serveur 1 - RAM: 8Go - AD / PKI / Filer / NPS / WSUS / SCCM
>
> Serveur 2 - RAM: 4Go - ATA / Exchange
>
> Serveur 3 - RAM: 4Go - Serveur de logs (Syslog / ELK / Splunk / Graylog)

## Infrastructure et services


Le laboratoire sera hebergé sur un serveur ESXi avec 1To de stockage et 32Go de RAM. Il se composera des services suivants:

- Microsoft Active Directory
- Network Policy Server (Contrôle d'accès)
- Active Directory Certificate Services (PKI)
- Microsoft Advanced Threats Analytics (Analyse des events sécurité d'Active Directory)
- SMB (Serveur de fichiers)
- Exchange (Serveur de mail)
- Windows Server Update Services (Serveur de mise à jour)
- Syslog
- Elasticsearch / Logstash / Kibana 
- Grafana
- Splunk
- PFSense




