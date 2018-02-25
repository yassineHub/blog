---
id: 210
title: Table temporaires et variables dans SQL Server
date: 2016-07-28T14:06:29+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=210
permalink: /table-temporaires-variable-de-type-table-sql-server/
categories:
  - SGBD
---
On a quatre facon de créer les tables dans Sql Server :

  * Tables normaux
  * Tables temporaires locales
  * tables temporaires globals
  * Table variable

#### <span style="text-decoration: underline;">Table temporaire locale  :</span>

  * Les tables temporaires sont crées dans la base « tempdb » de Sql server
  * Meme si elle sont appelées ainsi , ces tables temporaires ont quand meme sauvegardé physiquement  sur le disque comme des tables normaux
  * Une table temporaire s&rsquo;elle a été crée à l&rsquo;intérieur d&rsquo;une procédure stocké elle est détruite automatiquement dès qu&rsquo;on sort de cette procédure .
  * elle est visible que pour qu&rsquo;à l&rsquo;intérieur d&rsquo;une session  , c&rsquo;est à dire qu&rsquo;à l&rsquo;utilisateur courant
  * des utilisateurs différents peuvent créent la même table  #Table portant le même nom , ces tables seront bien utilisés séparément  sans aucun conflit . cela est possible car Sql Server crée un identifiant unique pour chaque table
  * Ces tables sont géré comme des tables normaux dans le sens ou on peut ajouter des indexes et contraintes

Pour créer une table temporaire local  , Ci dessous la syntaxe à utiliser(nom de table avec le suffixe #) :

<pre class="brush: sql; title: ; notranslate" title="">CREATE TABLE #MyTable 
   (   
   id int NOT NULL, 

   )
</pre>

&nbsp;

#### <span style="text-decoration: underline;">Table temporaire globale</span>

Pour créer une table temporaire golobale , Ci dessous la syntaxe à utiliser (nom de table avec le suffixe ##) :

<pre class="brush: sql; title: ; notranslate" title="">CREATE TABLE ##MyTable 
   (   
   id int NOT NULL, 

   )
</pre>

  * Ce type de table diffère avec les tables temporaires locale par le fait qu&rsquo;elles sont visibles sur toutes les sessions .
  * elle sera détruite quand elle n&rsquo;est plus référencé par aucune session .
  * Le nom de table doit être unique sinon une erreur est levée .

#### <span style="text-decoration: underline;">Table variable</span>

la syntaxe pour créer une variable table est similaire à celle de la création des tables ordinaires ou des tables temporaires :

<pre class="brush: sql; title: ; notranslate" title="">DECLARE @MyTable table 
   ( 
   id int NOT NULL, 

   )
</pre>

  * Les tables variables ne sont pas crées mais déclarées
  * Elles sont limitées au scope dans lequel elles sont déclarées
  * On peut créer des clustred index
  * on peut faire toutes les opérations dessus : SELECT, INSERT, UPDATE et DELETE.

&nbsp;

Les variable variables ont des limitations :

* elles n&rsquo;ont pas des non-clustred-indexes

  * on peut pas mettre des contraintes
  * on ne peut pas mettre des valeurs par défauts aux colonnes
  * Ils sont insensibles aux transactions : les commit et rollback ne fonnctionnent pas

&nbsp;

Utilisation

  * Les tables temporaires sont utilisé généralement pour des besoins de faire des modification sur une table sans affecter la table d&rsquo;origine
  * Comme les tables variables sont stockées en mémoire ,  ils sont plus performant en terme d&rsquo;accès , elles doivent être utilisées pour des faibles volumétries
  * Par contre les tables temporaires sont à utiliser en cas d’importante volumétries
  * Si on a besoin d&rsquo;index et de statistique dans ce cas on doit choisir les tables temporaires
  * utiliser les tables  variables dans une fonction pour retourner un résultat

&nbsp;

&nbsp;