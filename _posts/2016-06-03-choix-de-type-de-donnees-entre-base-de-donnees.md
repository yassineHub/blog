---
id: 156
title: Choix de Type de données entre base de données
date: 2016-06-03T15:33:32+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=156
permalink: /choix-de-type-de-donnees-entre-base-de-donnees/
categories:
  - JAVA
---
* Règle du choix du type de la clé primaire sur Une base de donées :
  
&#8211; elle doit etre d&rsquo;un type de donnée le plus petit possible , il est préférable d&rsquo;utiliser les types numériques que les types caractères car les numériques prend moins de places . en effet les clés primaires sont utilisé en tant qu&rsquo;indexe dans les clés étrangère , du coup plus les types prend de places plus on utilisera de cache et plus les requettes seront moins performents
  
&#8211; les clés primaires ne doit jamais changer car elle peut etre utilisé comme des clés étrangères ( à eviter de mettre les numéro de sécurité social ou numéro de passport comme ID )

&#8211; du coup il est préférable d&rsquo;uitiliser Int pour les identités si on sait que nos données ne dépassèrent pas les 2 milliards . sinon il faut utiliser BigInt .
  
généralement l&rsquo;utilisation d&rsquo;un simple INT est suffisent .
  
Le choix des types jouent énormément dans les performences des requettes , car un INT prend 4 Byte alors que BigInt prend le double d&rsquo;espace 8 byte , donc pour chercher une valeur sql server doit chercher dans plus d&rsquo;espace et du coup il sera plus lent .

Cela est vrai pour tous les champs pas justes les clés primaaires , il faut choisir les types de données les plus petits possibles .

Différence entre Decimal , numeric and Float in SQL server
  
-> Decimal ou numeric sont équivalent
  
-> Decimal ou numeric sont utilisé pour les representations des chiffres après les virgules de facon axacte mais la précision (nombre de chiffre total y inclut ceux après la virgule) est limité au 38 chiffres max
  
&#8211; Les types de données float et real présentant un caractère approximatif, ne les utilisez pas lorsqu&rsquo;un comportement numérique exact est requis, comme dans les applications financières, les opérations comprenant des arrondis ou les vérifications d&rsquo;égalité. Utilisez plutôt les types de données integer, decimal, money ou smallmoney.
  
-> Évitez d&rsquo;utiliser les colonnes float ou real dans les conditions de recherche de la clause WHERE, plus particulièrement les opérateurs = et <>. L&rsquo;idéal est de limiter les colonnes float et real aux signes de comparaison > ou

&#8211; Un Int dans SQL server correspond à int, java.lang.Integer en Java
  
&#8211; BigInt dans SQL server correspond à long , Long
  
&#8211; numeric peut etre mapé

Coté java Double ou BigDecimal ?
  
BigDecimal est une représentation avec une précision exacte alors que Double est d&rsquo;une précision approximative , mais dans la plupart des cas le Double suffit .
  
BigDecimal offre plusieurs opérationpour arrondir et d&rsquo;autres opéation , mais il est couteux en terme de mémoire

Voici une idée sur les espaces mémoires des différents types

double: 8 bytes

Double: 16 bytes (8 bytes overhead for the class, 8 bytes for the contained double)

BigDecimal: 32 bytes

long: 8 bytes

Long: 16 bytes (8 bytes overhead for the class, 8 bytes for the contained long)

BigInteger: 56 bytes

  * Pour les pourcentage il est recommendé d&rsquo;utiliser le type numéric coté SqlServer et double coté java .