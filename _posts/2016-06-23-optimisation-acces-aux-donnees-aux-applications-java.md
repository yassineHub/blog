---
id: 181
title: Optimisation des accès aux données aux applications java
date: 2016-06-23T14:59:31+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=181
permalink: /optimisation-acces-aux-donnees-aux-applications-java/
categories:
  - JAVA
---
**Introduction** 

Une application Java peut voir ses performances rapidement baisser si la quantité des données manipulé est assez importante .
  
Dans cet article on fera une passe sur les bonne pratique pour pour optimiser l&rsquo;accès aux données en utilisant le driver JDBC , JPA et Hibernate .
  
1 .Procédure stockés (language SQL précompilé ):
  
Quand on a beaucoup de calculs et d&rsquo;analyse à faire sur une grande quantité de données , il est parfois préférable de déporter ces traitement dans la base de données en utilisant les procédure stockées , cela peut permet à notre application de gagner énormément en performence .
  
La plupart des base de données relationnelles offrent la possibilité de faire des procédures stockées ce qui n&rsquo;est pas le cas des base NoSql , donc cette solution dépendras du type de base de données utilisé .
  
Parmi les avantages de cette solution est qu&rsquo;on peut ouvrir une seule connexion à la base de données pour appeler une seule procédure stocké qui va faire un ensemble de traitement et nous retourner des résultats , au lieu de faire plusieurs appels à la base directement depuis l&rsquo;application .
  
cela permet également de limiter le traffic réseau et les aller-retour entre notre application et la base de données . Au lieu d&rsquo;envoyer une requête entière ou un lot de codes sur le réseau, vous aurez seulement besoin d&rsquo;envoyer une seule
  
Elles permettent également de sécuriser une base de données. Par exemple, il est possible de restreindre les droits des utilisateurs de façon à ce qu&rsquo;ils puissent uniquement exécuter des procédures

Les requêtes envoyées à un serveur SQL font l&rsquo;objet d&rsquo;une &lsquo;analyse syntaxique&rsquo; puis d&rsquo;une interprétation avant d&rsquo;être exécutées. Ces étapes sont très lourdes si l&rsquo;on envoie plusieurs requêtes complexes.

Les procédures stockées répondent à ce problème : une requête n&rsquo;est envoyée qu&rsquo;une unique fois sur le réseau puis analysée, interprétée et stockée sur le serveur sous forme exécutable (précompilée). Pour qu&rsquo;elle soit exécutée, le client n&rsquo;a qu&rsquo;à envoyer une requête comportant le nom de la procédure stockée.
  
en effet , les procédure stocké quand ils sont compilé lors de la première exécution , elle génère un plan de requette qui est stocké en mémoire et réutilisé pour les prochaines exécutions . 

Attention : dans certains cas, le plan de requête que l&rsquo;optimiseur de requête choisi lorsque la
  
procédure stockée a été d&rsquo;abord exécuté peut ne pas être le meilleur plan pour les exécutions ultérieures avec différentsparamètres et en fait provoque une baisse de performance

Les procédures stockées améliorent la sécurité en vous permettant d&rsquo;accorder des droits pour exécuter la procédure au lieu de donner accès directement aux tables de base. Aussi, en utilisant requêtes paramétrées, vous réduisez les chances d&rsquo;une attaque par injection SQL.

2. configuration correcte

Configuration spécifique 

&#8211; Par défaut le driver JDBC essaie de récupérer un nombre fixe de ligne pour chaque appel à la base de données , par exemple , Pour la base de données Oracle , le nombre de ligne par défaut récupérées est 10 . cela veut dire si on veut récupérer 100 ligne de la base , le driver JDBC va faire 10 appel à la base de données .
  
Le nombre de ligne récupéré par défaut peut etre paramétré par le JDBC avec la méthode setFetchSize() .
  
Cette méthode permet de controller le nombre d&rsquo;appel réseau effectué par driver JDBC depuis la JVM à la base de donnée , et par la suite de controller la taille de la mémoire
  
utiliser pour retourné les résultats , donc il faut trouver la valuer optimal pour une bonne performence , car il faut garder en tete que plus la taille de ce paramètre est grande plus
  
on alloue de mémoire .
  
Cette méthode n&rsquo;est pas supporté par tous les base de données , donc avant de l&rsquo;utiliser il faut s&rsquo;assurer que la base de données ne l&rsquo;ignore pas .
  
ce paramètre est modifiable dans JDBC , JPA et Hibernate

Gestion des connexions 

L&rsquo;ouverture de connexion à une base de données pour chaque utilisateur connecté ou chaque appel à la base est très couteux en terme de performance , c&rsquo;est pour cela la plupart des serveur d&rsquo;application (tomcat , glassfish..) propose des pools de connexion qui consistent à maintenir en cache plusieurs connexions avec la base de données . Pour chaque connexion crée , elle est placé dans un pool comme ca elle pourra etre réutilisé pour une nouvelle requette . si tous les connextion sont utilisé , une nouvelle connexion est crée et mis dans le pool pour que lorsque un nouvel utilisateur se connecte il n&rsquo;attend pas qu&rsquo;une connexion soit libéré pour qu&rsquo;il l&rsquo;utilise .
  
donc la bonne configuration des pool de connextion est important pour les performences d&rsquo;un application .

3. Code d&rsquo;accès aux données 

&#8211; Mettre les donénes en cache
  
Quand un application manipule les mêmes données plusieurs fois , il est parfois judicieux de mettre ces données en cache afin d’éviter des appels à la base de données .
  
on peut utiliser la solotion JDBC javax.sql.CachedRowSet pour mettre les données en cache , d&rsquo;autres solutions externe peuvent etre utilisé pour cela .
  
Avec Hibernate on a deux types de cache :
  
* Cache de premier niveau : il dépend de la session et il est vidé une fois la session est fermé , les entités manipulé sont sans mis en cache et sont synchronisé avec la base lors d&rsquo;un flush par exemple .
  
* cache de 2éme niveau : qui ne dépend pas d&rsquo;une session mais qui est partagé par toute l&rsquo;application , c&rsquo;est celui ci qui peut être utilisé pour augmenter les performances . les objets qu&rsquo;on veut mettre en cache doivent etre annoté avec l&rsquo;annotation @Cache (Pour plus de détail voir dans la documentation officiel de Hibernate) 

Encore une fois , il faut faire attention à l&rsquo;utilisation de cette solution en s&rsquo;assurant que notre environement a suffisamment de mémoire .

&#8211; JDBC PrepareStatement VS Statement

il est préférable d&rsquo;utiliser Prepared statements , car ils sont plus rapides que les Statements dans le cas ou on veut exécuter a la meme statement plusieurs fois avec des données différents , car prepare statement est compilé et validé une seule fois et exécutés directement les prochaines fois sans etre recompilé , alors que statement est compilé à chaque fois qu&rsquo;on l&rsquo;exécute.
  
Un autre avantage de Prepared statements c&rsquo;est qu&rsquo;il permet d&rsquo;éviter les injection sql dans les requettes 

&#8211; Priviligier le mode transactionnel 

Quand on a un ensemble d&rsquo;opérations de modification et de suppression des lignes de la base de données , il est préférable de les traiter en un seul coup . cela consiste à traiter plusieurs opérations dans une seule transaction , comme ça on a une seule connexion et une seule session qui est ouverte pour l&rsquo;ensemble des opérations .
  
c&rsquo;est le principe qui est utilisé dans les modules de batch , on taite les données par lots , et chaque lot est géré dans une seule transaction .