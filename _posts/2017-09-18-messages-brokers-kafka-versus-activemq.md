---
id: 312
title: 'Messages brokers : Kafka versus ActiveMQ'
date: 2017-09-18T13:53:22+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=312
permalink: /messages-brokers-kafka-versus-activemq/
categories:
  - JAVA
---
**ActiveMQ**

l&rsquo;architecture ActiveMQ a été dictée par la spécification JMS , La puissance de la spécification JMS est un moyen standard d&rsquo;interfaçage avec les courtiers de messages &#8211; et pour cette raison, JMS a été désigné comme l&rsquo;une des meilleures API d&rsquo;entreprise Java.
  
Une grande partie de JMS est préoccupée par des messages fiables et persistants sans mettre trop de fardeau sur les clients de messagerie. Par exemple, il appartient au courtier du message de conserver les messages jusqu&rsquo;à ce que les consommateurs les aient traités. Cela rend le courtier un peu plus complexe, et (au moins pour ActiveMQ) rend les performances dégradées à mesure que le nombre de consommateurs augmente. Notez que de nombreux courtiers JMS permettent de définir une durée de vie du message, après quoi les messages non consommés peuvent être supprimés.
  
Par conséquent, les courtiers de messages JMS comme ActiveMQ sont moins adaptés au modèle « pipeline de données universelles » &#8211; en fait, il est considéré comme un anti-modèle pour l&rsquo;architecture du courtier JMS. Et parce que le courtier conserve les messages pour les consommateurs, trop de consommateurs qui se déconnectent peuvent consommer un peu plus d&rsquo;espace disque .

**Kafka**

L&rsquo;objectif de la conception de Kafka était (selon le cas) permettre le «pipeline de données universel» à LinkedIn. Cela nécessitait un courtier différent car c&rsquo;est un anti-modèle dans JMS pour les raisons expliquées ci-dessus. Donc, où JMS et ActiveMQ sont syntonisés pour une messagerie persistante et fiable (et ne peuvent donc pas très bien supporter les pipelines de données), la conception de Kafka se concentre sur ces pipelines de données. Cela offre une persistance, mais ce n&rsquo;est pas aussi garanti que chez les courtiers basés sur JMS.

Pour atteindre ses objectifs, Kafka utilise un «modèle de destination unifiée» appelé «sujet» (quelque chose entre la notion de sujet JMS et une file d&rsquo;attente JMS). Les messages sont conservés pendant un certain temps (et peuvent être consommés plus d&rsquo;une fois via les pointeurs réinitialisés si vous le souhaitez). Cependant, la persistance du message est limitée dans le temps et il incombe au consommateur de consommer des messages pertinents avant que le courtier ne les supprime. Ainsi: les messages peuvent être perdus (en plus d&rsquo;être livrés plus d&rsquo;une fois).

Dans l&rsquo;ensemble, la conception du courtier Kafka augmente l&rsquo;utilisation du disque jusqu&rsquo;à 1000 fois ce que nécessitent les agents ActiveMQ (ou les courtiers JMS), sans aucune garantie pour les consommateurs qui ne parviennent pas à prendre leurs messages avant que le courtier décide de les supprimer. Cependant, c&rsquo;est très rapide et peut gérer jusqu&rsquo;à millions de messages par seconde.

Conclusion

Nous vous recommandons d&rsquo;utiliser Kafka pour des cas d&rsquo;utilisation de surveillance à plus haut rendement où la perte de message n&rsquo;est pas importante, comme les événements de journalisation de diagnostic, les paramètres de performance ou d&rsquo;autres types d&rsquo;événements statistiques. Mais si vous vous souciez d&rsquo;une livraison exacte, si vos messages sont précieux ou si vous n&rsquo;avez pas besoin d&rsquo;un «pipeline de données universel», il est préférable d&rsquo;adhérer à un courtier classique comme ActiveMQ.