---
id: 151
title: Equals et HashCode dans Hibernate
date: 2016-05-24T10:59:58+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=151
permalink: /equals-hashcode-hibernate/
categories:
  - JAVA
---
En java on compare deux objets en utilisant l&rsquo;opérateur == , la comparaison retourne vrai si les deux objets ont la même adresses mémoires .
  
La méthode equals de Java si elle n&rsquo;est redéfinie , utilse par défaut == par la comparaison .
  
Le but de equals est de comparer deux objets du point de vue métier.

Dans Hibernate ?
  
La question qui se pose ; Doit on rédéfinir les deux méthodes tous le temps ? En fait ca dépend :
  
&#8211; les clées composés doivent toujours les redéfinir pour permettre à Hibernate de comparer les id , généralement on utilise tous les champs de la classe pour redifinir les méthodes
  
&#8211; Si on enregistre ou modifier ou merger un objet en base de données avant de l&rsquo;ajouter à un Set ou Map , on a pas besoin de les redéfinir
  
&#8211; quand on veut comparer des objets ou les ajouter à Map ou Set quand la session Hibernate est fermé et les objets sont détaché alors il faut les redifinir sinon ce n&rsquo;est pas nécessaire

Ce qui est recommandé , c&rsquo;est de sauvegarder un objet en base avant de l&rsquo;ajouter à un Set , car l&rsquo;objet contienderas un id et on n&rsquo;aura pas besoin de redéfinir les deux méthodes .

Car le problème qui se pose généralement dans les ORM particulièrement pour Hibenrate , c&rsquo;est que on peut avoir plusieurs d&rsquo;insance d&rsquo;objet qui représente la meme ligne de base de données . qui logiquement doivent considéré comme égaux , mais vu du coté java et la JVM sont différent car ont des adresse mémoire différents (on utilise l&rsquo;implémentation par défaut de eqauls).
  
Du coup pour la JVM un objet pas encore enregistré dans la base est different de celui qui a été retourné apres la lecture de la base . dans Hibernate c&rsquo;est important de redéfinir ces deux méthodes pour comparer les entités .surtout si on utilise les collections (Set , Map , List)