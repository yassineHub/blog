---
id: 264
title: Limitation de JSON-P en java
date: 2016-10-04T10:35:30+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=264
permalink: /limitation-de-json-p-java/
categories:
  - JAVA
---
Le strandard JSON-P est mal conçu et l&rsquo;implémentation de référence n&rsquo;est pas assez souple . EN effet , Il n&rsquo;est pas bien conçu puisque les deux API du standard ne supporte pas le parsing et la sérialisation des POJO java en objets Json  .

Les parsers et les serializers Json codés sont énormes et nécessitent plusieurs appels API , en plus de cela ils sont fragiles dans le sens ou tout changement de la structure du Json nécessite la modification des parsers et les serializers .

La conversion devient problématique quand on a un grand nombre d&rsquo;objets contenant beaucoup de champs à parser . Dans ce cas les API comme Jackson et GSON sont 10 fois plus rapide .

Cette limitation a été abordée dans un article de JavaMagazine de Septembre/Octobre 2016 .

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;