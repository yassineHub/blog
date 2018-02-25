---
id: 281
title: 'com.microsoft.sqlserver.jdbc.SQLServerException: Incorrect syntax near &lsquo;@P0&rsquo;'
date: 2016-10-24T16:28:20+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=281
permalink: /com-microsoft-sqlserver-jdbc-sqlserverexception-incorrect-syntax-near-p0/
categories:
  - JAVA
---
**<span style="text-decoration: underline;">Problème</span> 1**
  
Sur un des projets sur lequel j&rsquo;ai travaillé , j&rsquo;utilise spring data avec du jpa pour interroger la base de données SQL server .
  
Dès qu&rsquo;il y a une requette qui génére une requete sql avec le mot clé TOP (qui permet de récupérer les premiers résultats d&rsquo;un requete sql) j&rsquo;avais l&rsquo;erreur suivante :

<pre class="brush: java; title: ; notranslate" title="">com.microsoft.sqlserver.jdbc.SQLServerException: Incorrect syntax near '@P0'
</pre>

en creusant plus j&rsquo;ai constaté que la requete sql générée par spring data contient **TOP?** et non pas **TOP(?)** , ce qui pose problème à SQL Server parce que lui s&rsquo;attend à des parenthèses .

<pre class="brush: java; title: ; notranslate" title="">SELECT TOP (?)
</pre>

<span style="text-decoration: underline;"><strong>Solution</strong></span>
  
Pour cela il suffit de s&rsquo;assurer que le dialect utilisé dans les propriétés d&rsquo;hibernate est bien celui de sqlServer **org.hibernate.dialect.SQLServerDialect**

&nbsp;

**<span style="text-decoration: underline;">Problème</span> 2**

j&rsquo;ai eu la même erreur en en faisant migrer SQLServer 2012 en SqlServer 2014 .

Cette fois ci le dialect **org.hibernate.dialect.SQLServerDialect** ne marche pas **.**

j&rsquo;ai en plus le warning suivant :

<pre class="brush: java; title: ; notranslate" title="">Unknown Microsoft SQL Server major version [12] using SQL Server 2000 dialect
</pre>

Cela veut dire que Hibernate utilise par défaut SqlServer 2000 car la version de la base n&rsquo;est pas connu de hibernate
  
<span style="text-decoration: underline;"><strong>Solution</strong></span>

Le contourenement est d&rsquo;utiliser le dialect  **org.hibernate.dialect.SQLServer2012Dialect . **

car c&rsquo;est le meme pour la version SQLServer 2014 .

&nbsp;