---
id: 76
title: 'La programation concurrente : Threads'
date: 2016-04-26T21:48:05+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=76
permalink: /programation-concurrente-threads/
post_views_count:
  - "35"
categories:
  - JAVA
---
La programmation concurrente a pour objectif de simplifier la  programmation des applications parallèles et l&rsquo;optimisation les performances par une meilleure utilisation des ressources . La programmation concurrente en java repose sur la notion d’activité, cette dernière est composé d&rsquo;une tache et une ressource exécutant cette  tache.

**Une  activité = tache + ressource d&rsquo;éxécution **

  * Une tache correspond à la description du travail à réaliser : la définition d&rsquo;une tache en java consiste à implémenter l&rsquo;interface  java.lang.Runnable
  * La  ressource d&rsquo;exécution  correspond à une instance de la classe java.lang.Thread
  * Une tache peut être exécuté par plusieurs threads simultanément
  * Un process correspond à un ensemble de ressource (thread)

# <span style="text-decoration: underline;"><strong>Thread avant java 5</strong></span>

Une tache peut être définit en implémentant l’interface **Runnable**

<pre class="brush: java; title: ; notranslate" title="">class Task implements Runnable { 
   public void run() { 
   System.out.println("Doing task ..."); 
 } 
}
</pre>

<pre class="lang:java decode:true"></pre>

La tache peut être exécuté par plusieurs thread comme suit :

<pre class="brush: java; title: ; notranslate" title="">public class ExecutionTask {
    public static void main(String args[]) throws Exception {
        Task task = new Task();

        Thread thread1 = new Thread(task);
        thread1.start();

        Thread thread2 = new Thread(task);
    thread2.start();
    }
}
</pre>

Cette manière de faire permet de découpler la tache de l&rsquo;exécuteur qui est le thread .

On peut également créer des des activités en étendant la classe **Thread comme suit :**

<pre class="brush: java; title: ; notranslate" title="">class ExtendsThread extends Thread {
public void run() {    
    System.out.println("Execution of ExtendsThread ...");
 }

public static main (String[] args){
  new ExtendsThread().start();
  new ExtendsThread().start();
}
}
}
</pre>

Le fait d&rsquo;hériter de la classe thread et de surcharger la méthode run n&rsquo;est pas recommandé  , vous trouverz dans ce lien plus de détail sur la comparaison entre implémenter Runnable et étendre la classe Thread .

# <span style="text-decoration: underline;">Les états d&rsquo;un thread</span>

Il existe plusieurs états d&rsquo;un thread :

  1. **Etat de création** : quand on crée une nouvelle instance du thread avec new Thread()
  2. **Runnable** : Quand on appelle la méthode start() , , cet état dit que le thread est candidat pour etre exécuté
  3. **Running** : quand il est en cours d&rsquo;éxecusion
  4. **Bloqué **: quand on appelle les méthodes sleep() , wait()
  5. **Dead** : quand le thread est mort , cela arrive quand on sort de la méthode run() , ou bien quand on appelle les méthode stop() ou  destroy()

# <span style="text-decoration: underline;">Types de threads</span>

Il existe deux types :

  * **Thread utilisateur** : c&rsquo;est un thread normale
  * **Thread deamon** : c&rsquo;est un thread qui s’exécute en tache de fond

# <span style="text-decoration: underline;">Thread en Java 5</span>

  * ### <span style="text-decoration: underline;">Avant Java 5</span>
    
      * une tache est reprenté par l&rsquo;interface Runnuble et ne retourne pas de résultats et on ne peut  lancer aucune exception
      * une ressource d&rsquo;éxécution : thread
  * ### <span style="text-decoration: underline;">Depuis Java 5</span>
    
      *  l&rsquo;interface Callable : Une tache peut retourner un résultat et lancer une exception
      * L&rsquo;interface Future : ticket donnant accès au resultats de l&rsquo;exécution d&rsquo;une tache
      *  L&rsquo;interface Executor Service : encapsule les ressources d’exécution et une politique de gestion de ces ressources

L&rsquo;interface Callable :

<pre class="brush: java; title: ; notranslate" title="">public interface Callable&lt;V&gt; {
 public V call() throws Exception;
}
</pre>

De la même manière que les « Runnables », on  peut utiliser les Executors pour soumettre des Callables. Cependant, nous n&rsquo;utiliserons pas la méthode **EXECUTE** mais **SUBMIT**

<pre class="brush: java; title: ; notranslate" title="">ExecutorService execute = Executors.newSingleThreadExecutor();	
Future&lt;Integer&gt; future = execute.submit(new MonCallable());
System.out.println("Resultat du Callable " + future.get());
execute.shutdown();
</pre>

Le code ci dessus montre comment on peut créer un executor mono-thread depuis le service executor  et récupérer le résultat après l’exécution de la tache .

<h1 id="r-1278215" class="foldable__button secondTitle js-foldable-button hoveredCourseElement" data-claire-element-id="4290247">
  <span style="text-decoration: underline;">Les pools de threads :</span>
</h1>

<p class="foldable__button secondTitle js-foldable-button hoveredCourseElement" data-claire-element-id="4290247">
  Un pool d&rsquo;objets est une collection d&rsquo;objets créés et initialisés puis mis en mémoire. On les utilise quand le coût de création et d&rsquo;initialisation d&rsquo;un objet est assez important. Le fait de mettre certains objets en mémoire peut augmenter les performances dans certain cas .
</p>

<p class="foldable__button secondTitle js-foldable-button hoveredCourseElement" data-claire-element-id="4290247">
  Le but ici est de ne pas utiliser le même thread pour toutes les tâches mais d&rsquo;utiliser un thread différent pour chacune. Donc il n&rsquo;y a plus besoin d&rsquo;attendre qu&rsquo;une tâche soit finie pour exécuter la suivante .
</p>

<p class="foldable__button secondTitle js-foldable-button hoveredCourseElement" data-claire-element-id="4290247">
  Pour cela java nous offre l&rsquo;object suivant  :
</p>

<pre class="brush: java; title: ; notranslate" title="">ExecutorService execute = Executors.newFixedThreadPool(10) </pre>

&nbsp;

&nbsp;

**Synchronisation** 

L&rsquo;exécution des taches en parallèles avec les thread pose le problème de l&rsquo;acccès sumiltané à une ressource ou tout simplement à un bloc de code .
  
La spécification java permet de gérer ce problème avec mot clé synchronized qui permet de surveiller (et protéger) l&rsquo;utilisation d&rsquo;une instance .
  
Lorsqu&rsquo;un thread entre dans un bloc synchronized, il acquiert un « moniteur » sur l&rsquo;objet concerné

<pre class="brush: java; title: ; notranslate" title="">Object verrou = new Object();

new Thread() {
  public void run() {
    Logger.println("Pause");
    Thread.sleep(200);
    Logger.println("Verrouillage");
    synchronized (verrou) {
      Logger.println("Verrou posé");
    }
    Logger.println("Verrou libéré");
  }
}.start();

new Thread() {
  public void run() {
    Logger.println("Verrouillage");
    synchronized (verrou) {
      Logger.println("Verrou posé");
      Logger.println("Pause");
      Thread.sleep(1500);
    }
    Logger.println("Verrou libéré");
  }
}.start();
</pre>

Il est possible de vérifier si un thread détient le moniteur via la méthode Thread.holdsLock(verrou)
  
les méthodes wait et notify pour respectivement mettre en attente et réveiller un thread. Celles-ci ne peuvent être appelées que depuis un thread qui détient le moniteur, dans le cas contraire une IllegalMonitorStateException sera levée
  
Les méthodes wait permettent de mettre le thread courant en attente (limitée ou indéterminée) et libèrent le moniteur de l&rsquo;objet concerné (mais pas les autres moniteurs !). Ainsi un autre thread est autorisé à détenir le verrou (et entrer dans un bloc synchronized). Un thread demeure en attente jusqu&rsquo;à : 

  * l&rsquo;expiration du délai d&rsquo;attente (si spécifié et strictement positif) verrou.wait(timeOut)
  * son réveil suite à un appel à la méthode notify de l&rsquo;objet sur lequel il est en attente verrou.notify(); 
  * son interruption myThread.interrupt(); 
Donc , Les blocs synchronized permettent de protéger très simplement l&rsquo;accès à une référence tandis que les méthodes wait et notify permettent de mettre en attente un thread jusqu&rsquo;à ce qu&rsquo;une condition soit remplie

notify : reveille un seul thread , le thread est choisi selon l&rsquo;algorithme de la JVM
  
notifyAll : reveille tous les thread 

Attention , la methode sleep du thread bloque ce dernier pendant un laps de temps sans libérer le moniteur (verrou)

**Volatile** 
   
le mot clé volatile permet de signifier à la JVM qu’une variable sera modifiée par plusieurs Threads. En déclarant une variable volatile, sa valeur ne sera jamais placée dans le cache local au Thread courant d’exécution. Chaque lecture et chaque écriture passeront forcément par la mémoire partagée entre les Threads. De fait, l’accès à la variable elle-même devient implicitement synchronisé. Et ce, bien plus efficacement qu’avec un marqueur synchronized sur des méthodes.

Contrairement à synchronized, volatile s&rsquo;applique à une variable. nous pourrions dire que synchronized s’applique uniquement à des Objets alors que volatile peut s’utiliser aussi sur des types primitifs.
  
Il est donc bien important de noter que avec synchronized c&rsquo;est l&rsquo;instance qui est protégée et non la variable ! De fait, prenez garde à ne pas utiliser les blocs synchronized avec une valeur null. alors que avec volatile la valeur Null est autorisée