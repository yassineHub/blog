---
id: 179
title: Tester une méthode privée
date: 2016-06-23T12:55:01+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=179
permalink: /tester-methode-privee/
categories:
  - JAVA
---
Parfois certaines méthode privée qu&rsquo;on crée contiennent du code complexe qu&rsquo;on souhaite tester .
  
La problématique qui se pose est qu&rsquo;on peut pas accéder à une méthode privée depuis la classe de Test vu que celle ci n&rsquo;est pas visible en dehors de sa classe .

**La réflexion**

Cette solution utilise la réflexion pour accéder et modifier la visibilité d&rsquo;une méthode privée .
  
Cette solution n&rsquo;est pas du tout recommandé car la reflexion se base sur la signature de la méthode (nom de la méthode , type de retour &#8230;) et tout changement de la signature de la méthode après l’écriture d&rsquo;un test utilitaire fera échouer le test . 

**Refactoring**

Lorsqu&rsquo;une méthode privée devient si complexe que le besoin de la tester en isolation se fait sentir, cela signifie que la classe fait trop de choses .
  
Extraire cette méthode dans une classe externe permet de tester complètement ce code tout en améliorant le design en rendant le code plus solide.
  
Dès qu&rsquo;une méthode privée est créée afin d&rsquo;effectuer un traitement, il peut être utile de se demander si elle ne devrait pas être extraite, tout en restant pragmatique.