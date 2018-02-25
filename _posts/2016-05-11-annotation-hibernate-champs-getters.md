---
id: 144
title: Annotation Hibernate sur les champs ou sur les getters
date: 2016-05-11T17:21:19+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=144
permalink: /annotation-hibernate-champs-getters/
categories:
  - JAVA
---
En fait Hibernate utilise une notion du type d&rsquo;acccès « @AccessType » pour déterminer si on correspondent les colonnes de la base de données aux champs ou plutot aux méthodes getters .
  
Donc AccessType a deux valeur possible , AccessType.FIELD pour les mappage des champs et AccessType.PROPERTY pour le mappage des getter 

par défaut le type d&rsquo;accès d&rsquo;une classe est défini par rapport à la position des annotation « @Id » ou « @EmbeddedId » , si ces annotations sont mis sur un champ alors seuls les champs sont considéré pour le mappage de la persistence et les états de persistence sont accessible via ces champs , et pareil pour les getteres .

On peut mixer les deux approches , mais dans ce cas il faut faire attention , car il y a une subtilité : Hibernate va d&rsquo;abord regarder la ou on a mis le @Id ou @EmbeddedId , si c&rsquo;est sur les champs donc automatiquement va utiliser l&rsquo;approche des champs mais si on veut combiner , par exemple lui imposer d&rsquo;utiliser l&rsquo;aproche des getter sur un seul getter à ce moment la on doit annoté ce getter par @AccessType en précisant la valeur AccessType.PROPERTY . sinon ca ne marchera pas .

Il n&rsquo;est pas recommadé de combiner les deux approches car ce n&rsquo;est pas très propre.

Y en a qui prèfère d&rsquo;utiliser une approche qu&rsquo;une autre , mais ce qui est recommandé est d&rsquo;utiliser les annotation sur les getters , car c&rsquo;est plus propre meme si ca rend la lecture de la classe plus difficile ,
  
En fait le fait d&rsquo;utiliser l&rsquo;approche des annotations sur les champs , ca permet aux getters d&rsquo;avoir plus de logique au lieu de retourner juste le champs , ce qui n&rsquo;est pas très recommandé car un getter dans l&rsquo;entité ne doit retourner que le champs et c&rsquo;est au niveau des beaans DTO qu&rsquo;il faut mettre plus de logique ( les DTO sont mappé aux entités )