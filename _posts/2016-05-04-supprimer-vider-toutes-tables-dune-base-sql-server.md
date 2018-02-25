---
id: 102
title: 'Supprimer ou vider toutes les tables d&rsquo;une base SQL Server'
date: 2016-05-04T11:22:10+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=102
permalink: /supprimer-vider-toutes-tables-dune-base-sql-server/
categories:
  - SGBD
---
Pour supprimer toutes les tables d&rsquo;une base de données  SQL server , il suffit d&rsquo;exécuter les lignes de codes suivantes :

<pre class="brush: sql; title: ; notranslate" title="">EXEC sp_msforeachtable 'ALTER TABLE ? NOCHECK CONSTRAINT all'
EXEC sp_msforeachtable 'DROP TABLE ?'

</pre>

On peut avoir l&rsquo;erreur ci dessous si les tables sont reliées par des contraintes .

<pre class="brush: sql; title: ; notranslate" title=""> Could not drop object 'dbo.table' because it is referenced by a FOREIGN KEY constraint </pre>

&nbsp;

Il suffit d&rsquo;exécuter le code précédent plusieurs fois jusqu&rsquo;à ce que toutes les tables soient supprimées .

Pour vider toute les tables , il suffit d&rsquo;utiliser le meme code en remplacant &lsquo;DROP&rsquo; par &lsquo;DELETE&rsquo;

<pre class="brush: sql; title: ; notranslate" title="">EXEC sp_msforeachtable 'ALTER TABLE ? NOCHECK CONSTRAINT all'
EXEC sp_msforeachtable 'DELETE TABLE ?'

</pre>