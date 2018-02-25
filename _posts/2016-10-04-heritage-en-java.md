---
id: 266
title: 'comprendre l&rsquo;héritage en Java'
date: 2016-10-04T14:34:57+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=266
permalink: /heritage-en-java/
categories:
  - JAVA
---
<span style="text-decoration: underline;"><strong>Utilisation de l&rsquo;héritage </strong></span>

On utilise l&rsquo;héritage pour :

  * Pour réutiliser du code qu&rsquo;on a écrit avant
  * Pour séparer et créer des classes de types différents
  * Pour séparer la spécification de l&rsquo;implémentation
  * Pour spécifier un contrat entre différentes parties  d&rsquo;un système
  * Pour d&rsquo;autres taches &#8230;etc

<span style="text-decoration: underline;"><strong>Héritage du code et du type </strong></span>

L&rsquo;héritage en java permet d&rsquo;hériter du code mais aussi le type .

Il est important de séparer ces deux notions parce que le stndard java les mélange.

En java , à chaque fois que je crée une classe je définie un type , du coup je peux créer des classes qui ont ce type , prenons un exemple pour bien comprendre :

Si Student est une sous classe de la classe Person , alors Student a un type Student mais aussi un type Person . Un student est un Person .

Donc le code et le type sont hérités en java . Ce qui n&rsquo;est pas le cas du C++ qui ne fait que l&rsquo;héritage du code .

<span style="text-decoration: underline;"><strong>Héritage multiple en java </strong></span>

L&rsquo;idée est de pouvoir hériter de plusieurs classes , prenons l&rsquo;exemple d&rsquo;une classe « Phd Sudent » , un Phd Student est à la fois étudient à l&rsquo;université mais aussi professeur .

donc on peut modéliser cela par un héritage multiple d&rsquo;une classe « Student » est une classe « Teacher » . donc la classe « Phd Student » va avoir les attributs et les méthodes de la classe « Student » et « Teacher » .

Mais cela nous mène vers une problématique : Comment peut on gérer cet héritage si les deux superclasse ont des attributs ayant le même nom mais deux implémentations différentes  ?

un scénario plus compliqué connu sous le nom de « diamond inheretence » , quand la classe « Phd Student » hérite des deux classes « Student » et « Teacher » qui ont eux meme une super classe commune « Person » , le graphe d&rsquo;héritage a une forme de diamant .

Dans ce cas la classe « Phd student »  tout en bas du graphe doit avoir une seule copie d&rsquo;un champ définie dans la super classe « Person » , mais avec l&rsquo;héritage multiple elle a deux provenant de la classe « Student » et « Teacher » . C&rsquo;est la cas du champ « name » par exemple.

Donc on voit bien que ça devient plus compliqué à gérer . En fait les lagunages qui permettent  l&rsquo;héritage multiple doivent avoir des règles  compliqué pour gérer ces situations .

Quand on analyse ces problématiques d&rsquo;héritage multiple on constate qu&rsquo;elles sont tous liées à l&rsquo;héritage du code : c&rsquo;est à dire les champs et les implémentations des méthodes .

L&rsquo;héritage du code est compliqué alors que l&rsquo;héritage de type ne pose aucun problème .

L&rsquo;héritage du code est compliqué mais pas très important car il peut être remplacé par la délégation en utilisant des références vers d&rsquo;autres objets . Par contre l&rsquo;héritage multiple des types est très pratique et il n&rsquo;est pas facile de trouver un autre moyen plus propre pour le remplacer .

C&rsquo;est pour cela les concepteurs de Java ont proposé  une solution plus pragmatique : Permettre un seul héritage de code , Par contre permettre un héritage multiple de types.

Et pour avoir des règles différentes pour le code et les types  , Java a besoin de spécifier les types sont spécifier du code . C&rsquo;est ce que les interfaces Java ont pour rôle !!

&nbsp;

**<span style="text-decoration: underline;">Interfaces </span>**

Les interfaces permettent de spécifier un type java sans spécifier aucune implémentation . aucun champs ni aucun corps de méthode n&rsquo;est spécifié . Seul les déclarations des méthodes et les constantes sont permises .

On peut ne pas spécifier les  modifiers ils sont implicitement ajouté par la JVM ( **public static final** pour les contsantes et **public** pour les méthodes ) .

Donc cela nous fournit deux types d&rsquo;héritages :

  * Un héritage de code et de type : héritage d&rsquo;une classe en utilisant **extends  **
  * Un héritage de type seulement : implémentation d&rsquo;une interface en utilisant **implements **

Java permet l&rsquo;héritage multiple pour les types (interfaces) mais un seul héritage pour les classes (héritage du code )

**<span style="text-decoration: underline;">Classes abstraites </span>**

Une classe abstraite est une classe à mi chemin entre une classe et une interface : elle définit un type et contient du code comme une classe normale mais contient aussi des méthodes abstraites , des méthodes qui sont juste déclaré mais pas implémentées .

On ne peut pas faire de l&rsquo;héritage multiple avec les classes abstraites .

<span style="text-decoration: underline;"><strong>Héritage en Java 8</strong></span>

Les interfaces en Java 8 ont été étendues pour être similaire à des classes abstraites mais avec des différences subtiles .

L&rsquo;un des problématiques de Java 8 est de faire évoluer les interface (en ajoutant les expression lambda par exemple )  sans casser le système existant  .

Supposons maintenant qu&rsquo;on a une interface implémenté par plusieurs classes dans plusieurs projets . si on veut ajouter d&rsquo;autres méthodes à cette interface , il faut que toutes les classes implémentant cette interface , implémente les méthodes ajouté sinon le code ne compilera pas . Cela devient problématique car on veut ajouter des méthodes à l&rsquo;interface sans toucher et modifier les classes implémentant cette interface .

Pour régler ce problème Java 8 a jouté une nouvelle notion dans les interface : les méthodes par défaut (defaults methods).

c&rsquo;est des méthodes qui sont implémentées au niveau de l&rsquo;interface et sont marqué par un modifier « default » . les classes implémentant cette interface ont le choix de surcharger la méthode ou de laisser celle par défaut .

En java 8 on peut définir des méthodes statiques dans les interfaces .

Mais qu&rsquo;en est il du problème du graphe de diamant ?

Comme vous pouvez le voir maintenant , les interfaces en java 8 et les classes abstraites sont similaires ; s peuvent contenir des méthodes abstraites et des implémentations de méthodes avec des syntaxes différentes . Mais en fait il y a des différences  , par exemple les classes abstraites peuvent avoir des instances alors que les interfaces ne peuvent pas .

Mais il reste encore un point essentiel : A partir de java 8 on peut faire de l&rsquo;héritage multiple via les interfaces qui contient du code ( héritage multiple de type et de code qui devrait normalement être interdit ) !!!!!!

En fait , comme abordé plus haut , l&rsquo;héritage multiple du code à été interdit à cause de l&rsquo;héritage du code qui pose des problèmes d&rsquo;héritage plusieurs fois du même attribut et le conflit de nommage qui peut  poser . Alors que devient la situation maintenant avec java 8 ?

Les concepteurs Java ont pu régler ces problèmes en suivant les règles suivants

  * L&rsquo;héritage multiple de plusieurs méthodes abstraites ayant le nom n&rsquo;est pas un problème car c&rsquo;est vu comme étant la même méthode
  * Le problème d&rsquo;héritage des attributs connu sous le problème de graphe de diamant est évité car les interfaces ne permettent de déclaration d’attributs qui ne sont pas des constantes
  * L&rsquo;héritage des méthodes static et les constantes ( qui sont aussi static par définition ) ne pose pas de problème , vu qu&rsquo;ils sont appelées en les préfixant avec le nom de l&rsquo;interface , donc il n y&rsquo;a pas de conflit des noms
  * **L&rsquo;héritage multiple de plusieurs interfaces des méthodes de type <span style="text-decoration: underline;">default</span> ayant le même nom mais des implémentations différentes pose un grand problème . Mais  Java a choisit une autre solution plus pragmatique que les autres langages : Le compilateur affiche juste une erreur et  c&rsquo;est au développeur de régler le problème  .**

<span style="text-decoration: underline;"><strong>Conclusion</strong></span>

Les interfaces sont très pratiques en Java :

  * Ils permettent de définir un contrat entre différent partie d&rsquo;un programme
  * définir des types pour dispatcher des traitement de façon dynamique
  * Séparer la définition des types avec leurs implémentations
  * Permettre l&rsquo;héritage multiple en java

Les nouveaux fonctionnalités des interfaces en Java 8 comme les default méthodes sont utilisés plus pour l’écriture des librairies et moins utilisées pour l&rsquo;ecriture du code des applications métiers .

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;