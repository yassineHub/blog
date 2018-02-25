---
id: 162
title: 'Lazy loading  à la demande en Hibernate et en JPA'
date: 2016-06-09T11:14:51+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=162
permalink: /lazy-loading-a-demande-hibernate-jpa/
categories:
  - JAVA
---
On a deux modes d&rsquo;initialisation des association des objets :
  
-LAZY: l&rsquo;association est initialisé quand l&rsquo;objet est chargé , cette association peut etre chargé à la demande par un getter tant que l&rsquo;objet est état persistent , c&rsquo;est à dire que la session n&rsquo;est pas encore fermé .
  
-EAGER: l&rsquo;association est initialisé quand l&rsquo;objet est chargé 

Pour des question de performmence Il est recommandé d&rsquo;utiliser le mode « LAZY » sur l&rsquo;association , et charger l&rsquo;association à la demande .

Mais qu&rsquo;est ce qui se passe si jamais on veut charger l’association quand l&rsquo;objet est détaché (en dehors de la session ) ?

Une LazyInitializationException est levé qu&rsquo;on veut accéder à une association d&rsquo;un objet en mode Lazy . 

On a 3 options : 

&#8211; Créer une méthode qui retourne une entité initialisé , mais dans certains cas on peut pas savoir le nombre des niveaux de cascade des associations , donc on ne peut pas utiliser cette option

&#8211; Une autre option est de conserver la Session ouverte jusqu&rsquo;à ce que toutes les collections et tous les proxies nécessaires aient été chargés , Cela peut être utilisé dans une architecture applicative web en utilisant le modèle « Open Session in View », , un filtre de servlet peut être utilisé pour fermer la Session uniquement lorsque la requête a été entièrement traitée, lorsque le rendu de la vue est fini (il s&rsquo;agit du modèle Vue de la session ouverte) , en effet spring fournit le fitre OpenEntityManagerInViewFilter qui permet de gardee la session ouverte pour l&rsquo;ensemble de la requette particulièrement dans la partie du rendu et présentation . cette option ne peut pas être utilisé dans certaines architectures multicouche comme pour du swing par exemple.

&#8211; Si on utilise hibernate : Les méthodes statiques Hibernate.initialize() et Hibernate.isInitialized() fournissent à l&rsquo;application un moyen de forcer l&rsquo;initialisation de toutes les association d&rsquo;une entité
  
&#8211; Par contre pour JPA et jusqu&rsquo;à JPA2 , on n&rsquo;a pas un moyen simple pour forcer le chargement des association comme le fait Hibernate , Il faut le faire manuellement mais ce n&rsquo;est pas évident et ca demande plus de code .