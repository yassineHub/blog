---
id: 195
title: 'Les indexes  dans SQL server'
date: 2016-06-29T14:36:17+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=195
permalink: /indexes-sql-server/
categories:
  - JAVA
---
## <u>Introduction</u>

SQl server stocke tous les données et métadata dans des unités de stockages appelé pages, chaque page est un bloc de 8Ko de données .

Les données sont stockeés selon 3 format :

  * Heap
  * Index clustred
  * Index non clustred

## <u>Un index , C’est quoi ?</u>

&nbsp;

Un **index** est une [structure de données](https://fr.wikipedia.org/wiki/Structure_de_donn%C3%A9es) utilisée par le [système de gestion de base de données](https://fr.wikipedia.org/wiki/Syst%C3%A8me_de_gestion_de_base_de_donn%C3%A9es) (SGBD) pour lui permettre de retrouver rapidement les données. L&rsquo;utilisation d&rsquo;un index simplifie et accélère les opérations de recherche, de tri, de jointure ou d&rsquo;agrégation effectuées par le SGBD

L’**index** placé sur une [table](https://fr.wikipedia.org/wiki/Table_(base_de_donn%C3%A9es)) va permettre au SGBD d&rsquo;accéder très rapidement aux enregistrements, selon la valeur d&rsquo;un ou plusieurs [champs](https://fr.wikipedia.org/wiki/Champ_(BDD)).

Regardons maintenant en détail les 3 formats de stockage des donénes

## <u>Heap : </u>

Une table est dite sous format Heap s’il ne contient aucun index .

Lorsqu’une nouvelle ligne de données est insérée, elle se positionne à la fin ,  Lorsqu’une ligne de données est supprimée, l’espace qu’elle occupait est déclaré vacant et peut étre remplie par la prochaine donnée qui sera insérée.

Du coup pour retrouver une donnée , la base est obligé de parcourir toutes les lignes , une à une pour vérifier si la condition recherché est vraie , ce qui est très couteux quand les données sont importantes .

Comme on peut le voir avec l’exemple suivant :

Créons une table qui s’appelle my_table :

Et insérons trois données :

<pre class="brush: sql; title: ; notranslate" title="">insert into person values (1 , 'TOTO1')

insert into person values (2 , 'TOTO2')

insert into person values (3 , 'TOTO3')

select * from person

</pre>

[<img class="size-full wp-image-205 aligncenter" src="http://boussoufiane.com/wp-content/uploads/2016/06/Capture.jpg" alt="Capture" width="162" height="132" />](http://boussoufiane.com/wp-content/uploads/2016/06/Capture.jpg)

&nbsp;

Supprimons TOTO2 et ajoutons TOTO4 :

<pre class="brush: sql; title: ; notranslate" title="">delete from person where name ='TOTO2'

insert into person values (4 , 'TOTO4')

select *  from person

</pre>

&nbsp;

[<img class="size-full wp-image-206 aligncenter" src="http://boussoufiane.com/wp-content/uploads/2016/06/Capture-1.jpg" alt="Capture" width="180" height="143" />](http://boussoufiane.com/wp-content/uploads/2016/06/Capture-1.jpg)

On constate que TOTO4 a pris la place et la position de TOTO2

Du coup lignes de données sont stockées dans une structure désordonnée  , cette structure est nommée segment

## <u>Index clustred </u>

Les données sont physiquement réordonnées en fonction des champs sur lesquelles on a mis des index , physiquement veut dire que les données sont réordonnées au niveau de l’espace disque .

Pour créer un clusterd index :

<pre class="brush: sql; title: ; notranslate" title="">CREATE  CLUSTERED INDEX IDX_PERSON ON person( age )

</pre>

Dans ce cas les lignes seront réordonnées selon la collone age :

Les caractéristiques des clustred index :

  * Utilisé généralement pour les colonnes uniques
  * Chaque table doit avoir un seul index
  * On peut créer l’index sur plusieurs colonnes en meme temps
  * Par défaut , la clé primaire est considéré comme index

## <u>Index Non-clustred </u>

Dans ce cas , les données ne sont pas réordonnées physiquement , mais un autre objet est créé qui contient les colonnes désigné pour être des indexes plus  une colonne qui contient des pointeurs vers les lignes de la table .

Ce type d’index sont des index simple qui sont utilisé pour récupérer des données plus facilement .

Alors que les index clustred sont utilisé pour les colonnes uniques et spécialement les clés primaires , les index non clustred sont utilisé pour les autres situation comme :

  * Pour les clés étrangers
  * pour les colonnes de jointure avec les autres tables
  * utilisé dans « Where » ou « Order By »

Pour créer un index non clustred :

<pre class="brush: sql; title: ; notranslate" title="">CREATE INDEX IX_name ON person(name)

</pre>

&nbsp;

## <span style="text-decoration: underline;">Les bonnes pratiques :</span>

-il faut essayer de mettre moins de colonnes possible dans un index , car plus on a de colonnes plus le temps de parcours est long , le mieux de mettre une seule colonne

-Créer un index clustred pour chaque table, il faut mettre dans l’index les colonnes les plus utilisés pour récupérer les données

&#8211; Créer un index clustred sur la colonne qui a moins de doublons

&#8211; Créer un index clustred sur les colonnes qui ne seront jamais modifier ou modifier fréquement

&#8211; dans  SQLServer , par défaut la clé primaire contient un clustred index , mais  dans certains cas , il peut être judicieux de mettre cet index sur une autre colonne .